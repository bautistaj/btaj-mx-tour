# BTAJ MX — Street Tour 3D

Experiencia inmersiva 3D construida con Three.js para explorar murales de arte urbano de [@btaj_mx](https://www.instagram.com/btaj_mx/).

## Estructura

```
btaj-mx-tour/
├── nginx.conf
└── btaj-tour/
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

### 2. Agregar la config de nginx

Copia el bloque `location` de `nginx.conf` dentro del `server` block existente de `bautistaj.dev` y recarga:

```bash
sudo nginx -t && sudo systemctl reload nginx
```

El tour queda disponible en: `https://bautistaj.dev/btaj-tour/`

---

## Actualizar contenido

```bash
cd /home/admin/persist/btaj-mx-tour
git pull
```

nginx sirve los archivos directo del repo — no hace falta reiniciar nada.
