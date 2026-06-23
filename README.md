# BTAJ MX — Street Tour 3D

Experiencia inmersiva 3D construida con Three.js para explorar murales de arte urbano de [@btaj_mx](https://www.instagram.com/btaj_mx/). Camina por un barrio virtual, descubre los murales y accede directo a cada post de Instagram.

## Estructura

```
btaj-tour/
├── nginx.conf
└── www/
    ├── btaj-3d-tour.html
    ├── manifest.json
    └── imgs/
```

## Despliegue en VPS con nginx

### 1. Copiar archivos al servidor

```bash
scp -r www/ usuario@IP_SERVIDOR:/var/www/btaj-tour/
```

### 2. Configurar nginx

```bash
sudo nano /etc/nginx/sites-available/btaj-tour
```

Pega el contenido de `nginx.conf` del repo y ajusta `server_name`:

```nginx
server {
    listen 80;
    server_name tour.tudominio.com;
    root /var/www/btaj-tour;
    index btaj-3d-tour.html;

    location ~* \.(png|jpg|jpeg|webp)$ {
        expires 30d;
        add_header Cache-Control "public, immutable";
    }

    location / {
        try_files $uri $uri/ /btaj-3d-tour.html;
    }
}
```

```bash
sudo ln -s /etc/nginx/sites-available/btaj-tour /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl reload nginx
```

### 3. HTTPS con Certbot

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d tour.tudominio.com
```

---

## DNS (Namecheap)

En Advanced DNS de tu dominio agregar:

| Type     | Host | Value          | TTL       |
|----------|------|----------------|-----------|
| A Record | tour | TU_IP_SERVIDOR | Automatic |

Propagación: 5 min a 2 horas.

---

## Actualizar contenido

Cuando haya cambios en el HTML o imágenes:

```bash
scp www/btaj-3d-tour.html usuario@IP:/var/www/btaj-tour/
scp www/imgs/* usuario@IP:/var/www/btaj-tour/imgs/
```

nginx sirve los archivos directo — no hace falta reiniciar nada.
