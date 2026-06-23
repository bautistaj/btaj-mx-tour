# BTAJ MX — Street Tour 3D

## Estructura
```
btaj-tour/
├── docker-compose.yml   # Configuración del contenedor
├── nginx.conf           # Servidor web dentro del contenedor
├── deploy.sh            # Script de despliegue
└── www/
    ├── btaj-3d-tour.html
    └── imgs/
        ├── IMG_2030.PNG
        ├── IMG_2097.PNG
        ├── IMG_2135.PNG
        ├── IMG_2140.PNG
        ├── IMG_2141.PNG
        ├── IMG_2144.PNG
        └── IMG_2146.PNG
```

## Deploy inicial

### 1. Subir archivos al servidor
```bash
# Desde tu máquina local
scp -r btaj-tour/ usuario@IP_SERVIDOR:~/
```

### 2. En el servidor
```bash
cd ~/btaj-tour
./deploy.sh
```

### 3. Verificar que funciona
```bash
curl http://localhost:8080
# Debe devolver el HTML del tour
```

---

## Configurar subdominio con Nginx reverse proxy

Si ya tienes Nginx instalado en el servidor (fuera de Docker):

```nginx
# /etc/nginx/sites-available/btaj-tour
server {
    listen 80;
    server_name tour.bautistaj.dev;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

```bash
sudo ln -s /etc/nginx/sites-available/btaj-tour /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### HTTPS gratis con Certbot
```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d tour.bautistaj.dev
# Sigue las instrucciones — certbot configura HTTPS automáticamente
```

---

## Actualizar el tour

Cuando tengas cambios en el HTML o imágenes:

```bash
# Desde tu máquina local — reemplaza los archivos
scp www/btaj-3d-tour.html usuario@IP:~/btaj-tour/www/
scp www/imgs/* usuario@IP:~/btaj-tour/www/imgs/

# En el servidor — reiniciar (opcional, nginx sirve directo desde el volumen)
docker compose restart
```

---

## Namecheap DNS

En Advanced DNS de bautistaj.dev agregar:

| Type     | Host | Value          | TTL       |
|----------|------|----------------|-----------|
| A Record | tour | TU_IP_SERVIDOR | Automatic |

Propagación: 5 min a 2 horas.

---

## Comandos útiles Docker

```bash
docker compose ps          # Ver estado
docker compose logs -f     # Ver logs en tiempo real
docker compose down        # Parar el contenedor
docker compose up -d       # Levantar de nuevo
docker stats btaj-tour     # Ver uso de CPU/memoria
```
# btaj-mx-tour
