---
name: brand-asset
description: Agrega o actualiza un asset de marca Ancla (logo, ícono, patrón, token, variante) y lo sincroniza en las tres superficies — repo, brand.anclaseguros.cl y el proyecto de Claude Design. Úsalo cuando el usuario cree o cambie un logo, una variante (on-light, mono, nueva marca), un ícono, un token de color o una sección del guideline, o cuando pida "sincronizar con Design", "subir esto al design system" o "que se vea en la página".
---

# Sincronizar un asset de marca Ancla

Contexto y el porqué de cada regla: [docs/sync-design.md](../../../docs/sync-design.md).
Ese doc es la SSoT — si algo acá contradice el código, corrige ambos.

## Coordenadas

- Repo: `capp92/ancla-brand-guideline`, rama `main`
- Web: `brand.anclaseguros.cl` (GitHub Pages, `main`, raíz — reconstruye sola con cada push)
- Proyecto Claude Design: `Ancla Seguros Design System`
  `projectId = b71b5aed-e084-4a4d-8178-53f817bdd792`

## Regla de oro

**El asset nace en el repo.** Subir a Design es gratis (`write_files` con
`localPath` no pasa por el contexto); bajar es caro (`get_file` devuelve el
contenido al contexto y hay que re-emitirlo para escribirlo). Si el asset solo
existe en Design pero también está en el disco (Drive, `ancla-web`,
`ancla-web-propietarios`), busca el local con `mdfind` y copia ese — no lo bajes.

## Procedimiento

1. **Archivo al repo.** `assets/logos/` · `assets/icons/` · `assets/patterns/` ·
   `assets/colors/`. Convención: `logo-ancla-<marca>-<contexto>.svg`, con
   contexto ∈ `on-dark` · `on-light` · `mono-white` · `mono-dark`.
   Variante de color nueva = `sed` sobre los `fill` del SVG existente, no un
   export nuevo (los de Inkscape pesan 175 KB por los glyphs embutidos).

2. **`index.html` — mostrarlo.** Buscar por texto, no por línea:
   - §02 Logo: bloque `Variantes secundarias` (grid de 3) o
     `Las dos marcas` si es otra marca. Card = `.logo-showcase`
     (`-dark` / `-brand` / `-light`) + `.chip-mini` de descarga.
   - §16 Descargas: `<h3 class="sub">Logos</h3>` → agregar el `.chip`.
   - Si el asset cambia una regla de uso, actualizar el `.lead` de la sección.

3. **`README.md`** — agregar el archivo al árbol de `assets/`. **Este es el paso
   que siempre se olvida.** Si el asset no está en el README, no existe para
   quien reciba el ZIP.

4. **Verificar en pantalla.** Abrir `index.html` en el navegador, ir al bloque y
   mirar la pieza: que el SVG cargue *y* que contraste contra su fondo. Un
   `naturalWidth > 0` no prueba que el wordmark blanco no esté sobre fondo claro.

5. **Commit + push.** Mensaje en español, imperativo, qué y por qué.
   **Pedir autorización explícita antes de commitear o pushear** — es regla del
   usuario y no se hereda de una tarea a otra.
   Si el push rebota: `git pull --rebase origin main`. Nunca `--force`
   (el `CNAME` de la raíz sostiene el dominio).
   Pages tarda 2–3 min en reflejarlo.

6. **Subir al proyecto de Design.** Secuencia exacta:
   - `DesignSync.list_files` con el `projectId` → ver qué falta o difiere.
   - `DesignSync.finalize_plan` con `writes` (rutas exactas) y
     `localDir` = raíz del repo → devuelve `planId`.
   - `DesignSync.write_files` con ese `planId`, cada archivo con `localPath`
     (nunca `data` para binarios/SVG grandes).
   - Rutas espejo: `assets/logos/…` igual que en el repo;
     `reference/ancla-brand-guideline.html` ← `index.html` local.

7. **Card del pane** (si el asset merece verse en el Design System pane).
   Crear `guidelines/<tema>.card.html`. La **primera línea** debe ser el marcador
   o la card no aparece:
   ```html
   <!-- @dsCard group="Brand" viewport="700x160" name="Logo sobre light y mono" subtitle="Variantes secundarias" -->
   ```
   Luego un HTML mínimo que linkee `../styles.css` y use tokens `--ancla-*`
   (`--ancla-bg-dark`, `--ancla-bg-brand`, `--ancla-bg-light`,
   `--ancla-surface-white`, `--ancla-hairline-light`).
   `register_assets` es legacy: con el marcador basta.

8. **`github.md` del proyecto Design** — actualizar fecha y SHA del último sync.
   No copiar el mapa de equivalencias ahí: vive en `docs/sync-design.md` y
   `github.md` solo apunta.

9. **Traspaso.** Cerrar con: qué se subió, qué quedó pendiente, cómo verificarlo
   (URL + comando), y las decisiones abiertas que aparecieron.

## Trampas conocidas

- Un asset puede estar en el repo, en la página **y** faltar en Design (o al
  revés). Siempre `list_files` antes de asumir que están iguales.
- Sin el marcador `@dsCard` en la primera línea, el archivo se sube pero la card
  no se ve — y parece que el sync falló.
- `write_files` acepta máximo 256 archivos por llamada; el mismo `planId` sirve
  para varias llamadas.
- Si el asset es de **Ancla Recupera**, hoy solo existe la variante `on-dark`:
  no lo pongas sobre fondo brand ni claro hasta que exista la variante.
