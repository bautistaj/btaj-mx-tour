# BTAJ MX — Street Tour 3D

Experiencia inmersiva 3D construida con Three.js para explorar murales de arte urbano de [@btaj_mx](https://www.instagram.com/btaj_mx/).

## Estructura

```
btaj-tour/
├── nginx.conf
└── www/
    ├── index.html
    ├── manifest.json
    └── imgs/
```

## Despliegue en VPS

### 1. Clonar el repo en el servidor

```bash
cd /home/admin/persist
git clone <repo-url> btaj-mx-tour
```

### 2. Copiar o enlazar la config de nginx

```bash
sudo cp /home/admin/persist/btaj-mx-tour/nginx.conf /etc/nginx/sites-available/bautistaj.dev
sudo ln -s /etc/nginx/sites-available/bautistaj.dev /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl reload nginx
```

### 3. HTTPS (si aún no está configurado)

```bash
sudo certbot --nginx -d bautistaj.dev
```

---

## Actualizar contenido

```bash
cd /home/admin/persist/btaj-mx-tour
git pull
```

nginx sirve los archivos directo del repo — no hace falta reiniciar nada.
