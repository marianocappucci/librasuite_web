# librasuite_web

Sitio de **LibraSuite** (`librasuite.com.ar`): la familia de sistemas de gestión de
Compulibra. Presenta los ocho productos y publica las páginas legales comunes.

| Ruta | Qué es |
|---|---|
| `/` | Los ocho productos, por rubro, lo que comparten y la Copia externa |
| `/legal/privacidad` | Política de Privacidad (incluye el Uso Limitado de las API de Google) |
| `/legal/condiciones` | Condiciones del Servicio: remite a los Términos y agrega lo de la Copia externa |
| `/legal/terminos` | Términos y Condiciones del Servicio v1.0, el texto único de la familia |

Las tres URLs de arriba (`/`, `/legal/privacidad`, `/legal/condiciones`) son las que se
declaran en la **Marca** de la app OAuth de Google del proyecto LibraSuite. Si cambian,
Google vuelve a pedir la verificación.

## Términos

`public/legal/terminos.html` y `terminos-v1.0.md` son el mismo texto que publican las
ocho landings. El texto vive **una sola vez**, en `libraauth/legal/terminos_v1.md`; el
`.md` de acá tiene que dar el mismo sha256 que anuncia la página. librasuite todavía no
está en `libra-web-kit` (`SIDEBARS`), así que la página no se genera con
`generate_legal.py`: cuando entre, se regenera como las demás.

## Deploy

Mismo camino que las landings: `push` a `main` → reusable workflow de
`libra-web-kit` → rsync a `/root/librasuite_web/` → `docker compose build` + `up -d`.
Contenedor `librasuite-web`, puerto `8104`, red `stack_stack-net`; el proxy host de
`librasuite.com.ar` en Nginx Proxy Manager apunta a `librasuite-web:80`.

A diferencia de las landings, no hay `/docs/` ni contenedor de auth: la imagen es
`nginx:1.27-alpine` con su propio `nginx.conf`.
