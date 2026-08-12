# Sync: repo ↔ Claude Design ↔ web

Documento canónico del flujo de sincronización de la marca Ancla. Si algo acá no
coincide con el código o con lo que muestra el proyecto de Claude Design, **el
repo tiene razón** y este doc se corrige en el mismo cambio.

Última revisión: **2026-08-12**

---

## Las tres superficies

| Superficie | Qué es | Cómo se actualiza |
|---|---|---|
| **Repo** `capp92/ancla-brand-guideline` | **SSoT.** Los archivos reales (SVG, tokens, `index.html`) | `git commit` + `git push origin main` |
| **Web** `brand.anclaseguros.cl` | GitHub Pages, rama `main`, raíz, build legacy | Automático con cada push a `main` (2–3 min) |
| **Claude Design** proyecto `Ancla Seguros Design System`<br>`b71b5aed-e084-4a4d-8178-53f817bdd792` | Espejo consumible por agentes: tokens, componentes React, cards del pane | Manual: herramienta `DesignSync` desde una sesión de Claude Code |

**Dirección de la verdad: repo → Design.** El proyecto de Design es un espejo, no
una fuente. Si algo nace en Design (pasó con los logos de Recupera y con los
tokens `--ancla-*-surface`), hay que bajarlo al repo y desde ahí volver a subirlo.

## Por qué el repo es la SSoT (y no al revés)

Razón técnica, no ideológica: **subir es gratis, bajar es caro.**

- `DesignSync.write_files` acepta `localPath` — la herramienta lee el archivo del
  disco y lo sube sin pasar por el contexto del modelo. Da lo mismo si el SVG pesa
  175 KB.
- `DesignSync.get_file` devuelve el contenido **al contexto**, y para escribirlo en
  disco el modelo tiene que re-emitirlo entero. Un SVG de Inkscape de 175 KB son
  ~120k tokens ida y vuelta, con riesgo real de corromper los `path`.

Corolario operativo: **crea el asset en el repo primero.** Si ya existe solo en
Design y también lo tienes en el disco (Drive, otro repo), copia el local y súbelo
para que ambos queden idénticos por construcción — no lo bajes por contexto.

## Checklist: agregar o cambiar un asset de marca

Nueve pasos. El que se salta siempre es el 4.

1. **Archivo al repo**: `assets/logos/`, `assets/icons/`, `assets/patterns/` o
   `assets/colors/`. Nombre `logo-ancla-<marca>-<contexto>.svg`, con contexto
   `on-dark` (texto blanco) u `on-light` (texto brand `#392dcf`). **No existen
   variantes monocromas** — decisión de 2026-08-12, ver más abajo.
2. **`index.html` §02 Logo** (o la sección que corresponda): card con
   `.logo-showcase` + `.chip-mini` de descarga.
3. **`index.html` §16 Descargas**: `.chip` en la columna "Logos".
4. **`README.md`**: agregar el archivo al árbol de `assets/`. ← el que se olvida.
5. **Ver la página**: abrir `index.html` y confirmar que el asset carga y contrasta
   contra su fondo (`naturalWidth > 0` no basta: mirar la pieza).
6. **Commit + push** a `main` → Pages reconstruye `brand.anclaseguros.cl`.
7. **Subir a Design**: `finalize_plan` con las rutas exactas, luego `write_files`
   con `localPath`. Rutas espejo: `assets/logos/…` igual que en el repo, y
   `reference/ancla-brand-guideline.html` ← `index.html`.
8. **Card del pane**: si el asset merece verse en el Design System pane, crear
   `guidelines/<tema>.card.html`. La primera línea **debe** ser el marcador:
   `<!-- @dsCard group="Brand" viewport="700x160" name="…" subtitle="…" -->`.
   Sin esa línea la card no aparece. El HTML linkea `../styles.css` y usa los
   tokens `--ancla-*`.
9. **`github.md` del proyecto Design**: actualizar fecha y commit del último sync.

Los pasos 1–9 están automatizados en el skill `/brand-asset` de este repo
(`.claude/skills/brand-asset/SKILL.md`).

## Mapa de equivalencias

| Proyecto Design | Repo |
|---|---|
| `tokens/*.css` | `assets/colors/ancla-tokens.css`, `ancla-tokens.json`, bloque de tokens de `index.html` |
| `components/core/*`, `components/forms/*` | `index.html` §06 Componentes |
| `components/patterns/*` | `index.html` §06 (navbar), §11 (trust bar, FAQ, pasos), §09 (KPI) |
| `ui_kits/landing/*` | `index.html` §11 Landing Pages |
| `ui_kits/social/index.html` | `index.html` §08 Social Media, §10 Piezas Gráficas |
| `slides/*.html` | `index.html` §09 Presentaciones |
| `guidelines/*.card.html` | `index.html` §02–§07 |
| `assets/**` | `assets/logos/*`, `assets/icons/*`, `assets/patterns/*` |
| `reference/ancla-brand-guideline.html` | `index.html` |

## Decisiones tomadas

**2026-08-12 · El sistema de logos son 2 archivos por marca.** Ni uno más:

| Fondo | Archivo | Color del texto |
|---|---|---|
| dark `#08071a` / brand `#392dcf` | `logo-ancla[-recupera]-on-dark.svg` | blanco `#ffffff` |
| light `#f4f7fc` / blanco | `logo-ancla[-recupera]-on-light.svg` | brand `#392dcf` |

- **El texto negro no se usa.** La primera versión de `logo-ancla-on-light.svg` tenía
  el wordmark en `#08071a` y estaba mal; se reemplazó por la de texto brand.
- **No hay variantes monocromas.** Se eliminaron `logo-ancla-mono-white.svg` y
  `logo-ancla-mono-dark.svg` del repo y del proyecto de Design. Para impresión a una
  tinta, bordado o grabado, el proveedor recibe el archivo `on-light`/`on-dark` o el
  isotipo y lo reproduce en un solo color. Razón: dos archivos más que mantener
  sincronizados en tres superficies para un caso que el proveedor resuelve igual.
- El isotipo a color sirve sobre los tres fondos — no necesita variante.

## Deuda y decisiones abiertas

- **Tokens de marca por superficie.** Design define
  `--ancla-seguros-surface` (= brand `#392dcf`) y `--ancla-recupera-surface`
  (= dark `#08071a`) en `tokens/colors.css`. El repo **no** los tiene en
  `ancla-tokens.css` / `.json`. Decidir si Recupera es una marca con color propio
  o solo una superficie distinta, y recién ahí bajar los tokens.
- **Nombres duplicados en Design.** `assets/logos/logo-ancla-on-dark.svg` y
  `logo-ancla-seguros-on-dark.svg` conviven y probablemente son el mismo arte;
  las cards `brand-logo-dark` y `brand-marcas` usan uno cada una. Elegir un nombre
  y borrar el otro.
- **Alias de nombre pendiente de matar.** El ZIP `Logo Ancla Seguros claro.zip`
  traía `logo-ancla-on-light.svg` y `logo-ancla-seguros-on-light.svg` **byte a byte
  idénticos** (md5 `8575fbab…`). Solo se agregó el primero: un archivo, un nombre.
  El proyecto de Design todavía arrastra el mismo problema con
  `logo-ancla-seguros-on-dark.svg`.
- **`logo_recupera.svg` vive en 5 lugares** (Drive + `ancla-web` ×2 +
  `ancla-web-propietarios` ×2, todos md5 `313ff96f…`). El canónico ahora es
  `assets/logos/logo-ancla-recupera-on-dark.svg` de este repo; los otros deberían
  apuntar acá.
- **`ancla-tokens.css` y `ancla-tokens.json`** duplican los mismos valores a mano.
  Si divergen, gana el `.css` (es el que se pega en producción).

## Cicatrices

- **Push rechazado por CNAME (2026-08-12).** El dominio `brand.anclaseguros.cl`
  se configuró desde la UI de GitHub, lo que creó el commit `8722941 Create CNAME`
  directo en `main`. Si trabajas local sin `fetch` previo, el push rebota.
  `git pull --rebase origin main` y listo — **nunca** `--force`: el archivo
  `CNAME` en la raíz es lo que sostiene el dominio.
- **SVG de Inkscape = 175 KB.** `logo-ancla-on-dark.svg` (174.889 bytes) y
  `logo-ancla-on-light.svg` (174.798) pesan eso porque Inkscape embute los glyphs de
  la tipografía. Los de Recupera, exportados de Figma con el mismo arte, pesan 14.8 KB.
  Para variantes nuevas, recolorear el SVG existente con `sed` sobre los `fill`
  (así se generó `logo-ancla-recupera-on-light.svg`: `fill="white"` → `#392dcf`),
  y verificar en pantalla que ningún detalle interno del isotipo se pierda.
- **`.DS_Store` quedó trackeado** en el primer commit y no está en `.gitignore`.
  Limpiarlo requiere `git rm --cached`; pendiente.
