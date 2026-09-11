# Clon de dgrade.mx

Copia local y estática del sitio <https://dgrade.mx/> (WordPress + tema Divi 4.27.1),
descargada el 2026-09-10. Funciona **completamente offline**: no queda ninguna
petición a servidores externos.

Está preparado para publicarse en GitHub Pages.

## Verlo en local

```bash
python3 -m http.server 8000
# abrir http://localhost:8000
```

Conviene servirlo por HTTP y no abrir `index.html` con `file://`, porque varios
recursos se referencian con rutas relativas.

## Publicar en GitHub Pages

```bash
git init -b main
git add .
git commit -m "Clon estático de dgrade.mx"
git remote add origin git@github.com:USUARIO/REPO.git
git push -u origin main
```

Después, en el repo: **Settings → Pages → Source: Deploy from a branch →
`main` / `/ (root)`**.

Queda en `https://USUARIO.github.io/REPO/`. Todas las rutas del sitio son
relativas, así que funciona igual en esa subruta que en un dominio propio (si
usas dominio propio, agrega un archivo `CNAME` en la raíz con el dominio).

### Por qué está el archivo `.nojekyll`

Le dice a Pages que no procese el sitio con Jekyll. Sin él, Jekyll puede
ignorar o transformar archivos y rutas. Es un archivo vacío: no lo borres.

## Estructura

| Ruta | Contenido |
|---|---|
| `index.html` | La landing page completa |
| `wp-content/themes/Divi/` | CSS y JS del tema Divi |
| `wp-content/et-cache/` | CSS generado por Divi Builder |
| `wp-content/uploads/` | Imágenes y videos (2024/09, 2024/11, 2024/12, 2025/01) |
| `wp-includes/js/` | jQuery y MediaElement.js |
| `fonts.googleapis.com/` | Hojas de estilo de Google Fonts (Khand, Open Sans) |
| `fonts.gstatic.com/` | Archivos `.woff2` de las fuentes |
| `assets/external/` | Íconos de redes sociales, localizados |

## Notas

- **Nombres de archivo saneados.** El mirror original guardaba los CSS y JS con
  el sufijo de versión de WordPress (`scripts.min.js@ver=4.27.1`) y las hojas de
  Google Fonts con `%3A`, `&` y comas en el nombre. Eso es frágil detrás del CDN
  de GitHub Pages, así que se renombraron a nombres simples y se reescribieron
  todas las referencias.
- **Íconos sociales.** LinkedIn y Facebook se cargaban desde Flaticon y
  Wikimedia; ahora están en `assets/external/`. El de Facebook se ve roto en el
  sitio en producción porque Wikimedia bloquea el hotlinking de esa URL; en esta
  copia sí se ve.
- **Peso.** 48 MB en total, de los cuales 32 MB son dos videos en
  `wp-content/uploads/2024/12/`. Está dentro de los límites de GitHub Pages
  (1 GB por sitio, 100 MB por archivo), pero cuenta contra el límite blando de
  100 GB de tráfico al mes. Si el sitio recibe mucha visita, conviene mover esos
  videos a otro lado.
- **Es estático.** Los formularios de contacto y cualquier función que dependiera
  de PHP/WordPress no operan. Hay que reconectarlos a un servicio externo
  (Formspree, Netlify Forms, etc.) si se necesitan.
