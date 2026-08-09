# Rastreador de Fricción de Lectura — FCEIA·UNR

Actividad de lectura para el curso **«Herramientas de Inteligencia Artificial»** (Facultad de Ciencias Exactas, Ingeniería y Agrimensura — UNR).

Los estudiantes marcan momentos de **fricción** en una lectura (confusión, acuerdo, **duda**, conexión, importancia o necesidad de un ejemplo), escriben una breve reflexión para cada uno **en la misma barra**, y al final generan un **resumen en texto** que copian y entregan en el campus virtual.

- **100% en español (voseo)**, pensado para estudiantes de la FCEIA·UNR.
- **Sin dependencias, sin framework, sin build.** Todo corre en la pestaña del navegador.
- **Sin datos:** no hay analítica, cookies, `localStorage` ni backend. Nada de lo que el estudiante escribe sale de su dispositivo (por eso debe **copiar y entregar su resumen** antes de cerrar).
- **Lecturas del curso precargadas** (selector desplegable), o pegar cualquier texto propio.

## Archivos

| Archivo | Para qué |
|---|---|
| `index.html` | La aplicación completa (HTML + CSS + JS). |
| `lecturas/` | Las lecturas del curso: `index.json` (índice) + un `.txt` por lectura. |
| `logo-fceia-blanco.png` / `logo-fceia-navy.png` | Logo FCEIA·UNR (blanco para el header navy, navy para el pie). |
| `README.md` / `LICENSE.txt` | Documentación y licencia. |

## Lecturas del curso

Las lecturas viven en la carpeta **`lecturas/`**:

- `lecturas/index.json` — el índice. Cada entrada tiene `id`, `titulo` y `archivo`:
  ```json
  [
    { "id": "tono-seguro", "titulo": "El tono seguro no es verdad", "archivo": "tono-seguro.txt" }
  ]
  ```
- `lecturas/<archivo>.txt` — el texto de la lectura. **Separá los párrafos con una línea en blanco.**

**Para agregar una lectura** (se puede hacer desde el editor web de GitHub, sin instalar nada):
1. Subí un nuevo `.txt` a `lecturas/` con el texto (párrafos separados por una línea en blanco).
2. Agregá una entrada en `lecturas/index.json` con su `titulo` y `archivo`.

> **Cómo se cargan:** la app lee la carpeta `lecturas/` con `fetch` (funciona hosteada en GitHub Pages / Vercel). Si por algún motivo no se pueden leer, cae en una lectura de respaldo embebida en el propio HTML (constante `EMBEDDED_FALLBACK` en el script).

## Publicarlo

### Opción A — GitHub Pages (organización `fceia-unr` o `chia-fceia-unr`)
1. Subí el contenido de esta carpeta (con `lecturas/`) a un repo en la organización, rama `main`.
2. **Settings → Pages → Source: Deploy from a branch → `main` / `root` → Save.**
3. Queda en `https://<organizacion>.github.io/<repo>/`.

### Opción B — Vercel
Subí el repo a Vercel, **Framework Preset: Other** (es estático, sin build). Queda en `https://<proyecto>.vercel.app/`.

En el aula virtual, agregalo como recurso **URL** o incrustalo:
```html
<iframe src="https://TU-URL-PUBLICADA/" title="Rastreador de fricción de lectura"
        width="100%" height="900" style="border:1px solid #DCE5EF;border-radius:8px"></iframe>
```

Como la actividad no guarda nada, el trabajo de cada estudiante vive solo en su pestaña: tiene que **copiar y entregar su resumen** antes de cerrarla.

## Personalización

- **Colores:** variables CSS en `:root` (bloque `/* Paleta FCEIA–UNR */`) y el objeto `COLORS` en el JavaScript (color por tipo de fricción). Paleta:

  | Token | Hex | Uso |
  |---|---|---|
  | `--um-blue` / `--ink` | `#15246E` | Navy FCEIA — títulos y texto principal |
  | `--um-blue-2` | `#2173A6` | Azul de marca — enlaces, acción, gradiente |
  | `--accent` | `#1FBF92` | Verde IA — barra de acento |
  | `--sky` / `--pale` | `#84B1D9` / `#C6D9B8` | Acentos / fondos suaves |
  | `--muted` / `--faint` | `#6B7A93` / `#9AA8BC` | Texto secundario / terciario |
  | `--line` | `#DCE5EF` | Bordes y divisores |

- **Logo:** reemplazá `logo-fceia-blanco.png` / `logo-fceia-navy.png`.
- **Textos de la interfaz:** viven en el objeto `I18N.es` dentro de `index.html`.
- **Lecturas:** en la carpeta `lecturas/` (ver arriba).

## Créditos y licencia

Obra original: **«Reading Friction Tracker»** de **Marc Watkins**, University of Mississippi — licenciada **CC BY 4.0**.

Esta es una **obra derivada**: adaptación al español (voseo) y a la identidad institucional de la **FCEIA·UNR** para el curso «Herramientas de Inteligencia Artificial» (paleta, logo, textos y lecturas). Se comparte bajo la misma licencia **[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.es)**.

**Sobre el uso de IA:** tanto la actividad original como esta adaptación se elaboraron con asistencia de IA generativa para redactar/estructurar el código y los textos; las decisiones pedagógicas y la revisión final son de la cátedra. La actividad en sí **no** contiene funciones de IA y no transmite datos.
