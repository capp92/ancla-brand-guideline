# Prompts de imagen IA — banco de assets Ancla

Sistema para generar con **nano banana** (Gemini Image) un banco de imágenes
reutilizable en los templates de RRSS del guideline (§08 Social Media, §10 Piezas
Gráficas, §14 Ads Paid).

Última revisión: **2026-08-12** · SSoT del sync: [sync-design.md](sync-design.md)

---

## La regla que hace que esto funcione

Las imágenes **no llevan texto, ni logo, ni CTA**. Eso lo pone el template encima.
Lo que genera la IA es el **objeto y su fondo**, con espacio vacío deliberado donde
después entra el copy. Una imagen que ya trae su propio titular solo sirve una vez;
una con espacio negativo bien puesto sirve en veinte piezas.

Y la restricción menos obvia, directo del guideline: **cero naranjo en las imágenes.**
`#ff8322` está reservado para botones de acción. Si el naranjo aparece en la foto, el
CTA deja de ser lo único naranjo de la pieza y pierde su trabajo.

## Flujo: ancla primero, después el catálogo

1. **Generar el ancla de estilo** con el prompt de abajo, sujeto `casa`. Iterar hasta
   que quede: es la imagen que define material, luz, cámara y paleta de todo el banco.
2. **Pasar esa imagen como referencia** en cada generación siguiente, con la frase
   `match the exact material, lighting, camera angle and palette of the reference image`.
   Es lo que hace nano banana bien y es la diferencia entre un banco y 30 imágenes sueltas.
3. **Generar por lotes**, un sujeto a la vez, variando solo `[SUJETO]` y `[COMPOSICIÓN]`.
4. **Pasar el QA** de más abajo antes de guardar.
5. **Guardar y sincronizar** con el skill `/brand-asset`.

## Prompt maestro

Reemplazar `[SUJETO]`, `[COMPOSICIÓN]` y `[FONDO]`. El resto no se toca.

```
A single [SUJETO], rendered as a minimal 3D object: matte clay-like surfaces,
softly rounded edges, no reflections, no glass, no chrome.

Strict palette — use only these colors and nothing else:
deep near-black navy #08071a, indigo #392dcf, cyan #2bd3f5, violet #c94cff, white #ffffff.
Absolutely no orange, no yellow, no red, no green, no warm tones anywhere.

Background: flat solid [FONDO], perfectly even, seamless, no vignette, no texture,
no gradient, no floor line, no horizon.

Lighting: one soft key light from the upper left, gentle ambient fill, one soft
contact shadow directly beneath the object. No dramatic shadows, no rim light,
no glow, no lens flare.

Camera: three-quarter view, slightly above eye level, straight-on, no tilt,
mild perspective, like a product shot.

Composition: [COMPOSICIÓN]

Mood: calm, precise, editorial, trustworthy. Simple silhouette, low visual noise,
generous empty space.

Do not include: any text, letters, numbers, labels, signage, logos, watermarks,
UI elements, arrows, sparkles, human faces, brand marks, currency symbols,
decorative particles, or busy detail.
```

### `[FONDO]` — elegir uno

| Valor | Cuándo |
|---|---|
| `#08071a` | Default. La mayoría de las piezas van sobre dark |
| `#392dcf` | Piezas de identidad fuerte, alternar para que el feed no sea monótono |
| `#f4f7fc` | Contenido denso, carruseles explicativos, documentos |

Nunca fondo blanco puro `#ffffff` — no es un fondo del sistema.

### `[COMPOSICIÓN]` — cuatro modos, según dónde va el texto

| Modo | Texto | `[COMPOSICIÓN]` a pegar |
|---|---|---|
| **A · lateral** | izquierda | `the object occupies the right third of a square frame; the left two thirds are completely empty flat background` |
| **B · zócalo** | arriba | `the object sits in the lower third of a 4:5 vertical frame; the upper two thirds are completely empty flat background` |
| **C · centro** | arriba y abajo | `the object is small and centered in a square frame, occupying about 40% of the height, with wide empty margins above and below` |
| **D · story** | centro | `the object is small, centered in the lower half of a 9:16 vertical frame, with the upper half completely empty` |

Agregar al final de la composición el ratio: `Square 1:1.` · `Vertical 4:5.` ·
`Vertical 9:16.` Si el modelo no respeta el encuadre, generar en 1:1 y recortar —
por eso todos los modos dejan el sujeto lejos de los bordes.

## Catálogo de sujetos

Un archivo por sujeto, en los modos que correspondan. Los primeros 8 son el set
mínimo para empezar a producir contenido.

### Crédito hipotecario y desgravamen
1. `simple modern two-story house with a gable roof` — la base del banco
2. `modern urban apartment building, six floors, balconies`
3. `house key with a simple house-shaped keyring`
4. `stack of three folded paper documents, blank pages, one corner curled`
5. `shield with a small house centered inside it`
6. `pocket calculator with blank keys`
7. `descending bar chart, four bars, each shorter than the last`
8. `bank building with four columns and wide steps`
9. `fountain pen resting on a blank folded document`
10. `closed padlock, thick rounded body`
11. `open umbrella seen from above, sheltering a small house beneath it`
12. `stopwatch with a blank dial`
13. `stack of coins, plain blank discs, no engravings`
14. `sealed envelope, blank front`
15. `map pin marker standing on a small flat plot`
16. `tower crane beside an unfinished concrete frame`

### Incendio y sismo
17. `wall-mounted smoke detector, round, seen at an angle`
18. `fire extinguisher, upright, blank cylinder`
19. `small house resting on a base of four spring dampers`
20. `single seismic wave line crossing beneath a small house`
21. `single stylized flame, contained inside a rounded shield`

Tono contenido: **nada de catástrofe.** Ni casas quemadas, ni escombros, ni grietas
dramáticas. El villano de Ancla es el desconocimiento, no el desastre.

### Comparar y cambiar de póliza
22. `two-pan balance scale, empty pans, level`
23. `three blank rounded cards floating in a staggered row`
24. `magnifying glass over a blank document`
25. `two curved arrows forming a swap loop`
26. `simple minimalist boat anchor` — motivo de marca, úsalo con moderación

### Dispositivos para mockup
27. `smartphone standing upright, screen completely blank and dark, no UI`
28. `open laptop, screen completely blank and dark, no UI`

El template pega el screenshot encima. Pedir la pantalla **vacía** ahorra el 90% de
los descartes.

### Fondos atmosféricos (sin objeto)
29. `an empty scene: smooth diagonal gradient from #08071a to #392dcf, subtle
    dark grid pattern overlay, nothing else in frame` — ya existe la versión vectorial
    en `assets/patterns/grid-dark.svg`; usar esa cuando alcance
30. `an empty scene: soft cyan #2bd3f5 to violet #c94cff light mesh on a #08071a
    background, blurred, no objects` — solo para acentos, no para el 100% del feed

## Familia fotográfica (secundaria)

Para piezas que necesitan realidad — una fachada, un interior, unas manos firmando.
**No mezclar en la misma pieza** con los 3D: son dos familias, no dos estilos.

Mismo flujo: generar primero la **fachada de edificio** como ancla, y desde ahí
referenciarla en todas las demás para que el grade y la luz no se muevan.

### Prompt maestro fotográfico

```
Photograph of [SUJETO]. Shot on a 35mm lens at f/4, natural overcast daylight,
editorial architectural photography, straight-on composition, sharp and clean.

Color grade: cool and desaturated, pushed toward indigo and cyan; shadows tinted
deep navy #08071a. No warm tones, no golden hour, no orange, no yellow, no green cast.

Chilean urban context: contemporary attached houses or apartment buildings, flat or
gable roofs, painted stucco or exposed concrete. No North American suburbia, no white
picket fences, no palm-lined driveways.

Composition: [COMPOSICIÓN FOTO]

Mood: calm, precise, trustworthy. Uncluttered frame, few objects, no props styling.
Candid and real, not staged. Photorealistic with believable imperfections —
not a 3D render, not an illustration, not a stock photo.

Do not include: any text, letters, numbers, house numbers, signage, brand names,
logos, watermarks, human faces, or UI on screens.
```

### `[COMPOSICIÓN FOTO]` — el espacio vacío lo da el cielo o un muro

| Modo | Texto | Pegar |
|---|---|---|
| **A · lateral** | izquierda | `the subject fills the right two thirds; the left third is calm and empty — flat overcast sky, a plain painted wall or bare floor. Square 1:1.` |
| **B · cielo** | arriba | `shot from below with the subject in the lower third and flat empty overcast sky filling the upper two thirds. Vertical 4:5.` |
| **D · story** | centro | `the subject occupies the lower half of the frame, the upper half is flat empty sky or a plain wall. Vertical 9:16.` |

### Catálogo fotográfico

1. `the façade of a contemporary six-story apartment building with balconies` ← ancla
2. `the front façade of a contemporary attached two-story house, painted stucco`
3. `a pair of house keys resting on a bare kitchen counter, close up`
4. `two hands signing a completely blank document on a plain table, seen from above, no faces`
5. `an empty living room in a newly handed-over apartment, daylight from a large window`
6. `a close-up of a modern front door handle and lock on a dark door`
7. `a laptop closed on a desk beside a plain mug and a blank notebook`
8. `a smartphone held in one hand, screen completely off and dark, no UI, no face`
9. `a round smoke detector mounted on a white ceiling, shot from below`
10. `an exposed concrete column and beam joint of a modern building, shot from below`
11. `a rooftop view over a dense residential neighbourhood on an overcast day`
12. `a person seen from behind, silhouetted, looking out of an apartment window`

Sin caras. Si hace falta gente: manos, espaldas, siluetas a contraluz o figuras
desenfocadas a distancia.

Nada de catástrofe acá tampoco: ni casas quemadas, ni escombros, ni grietas. Incendio y
sismo se cuentan por la **prevención** (el detector, la estructura), no por el daño.

## Familia vívida (fotografía a todo color)

Tercera familia, decidida el 2026-08-12: fotografía **sin** el grade frío de marca —
color rico, sol real, cordillera, cielo azul. Sirve para piezas aspiracionales donde la
casa o el barrio son el protagonista y el brand se pone en la capa de arriba.

**El costo de esta familia y cómo se paga.** Una foto cálida y saturada compite con el
naranjo del CTA y con el texto blanco. No se arregla desaturando la foto: se arregla
en el template, con un **scrim** entre imagen y texto.

```css
/* Scrim lateral — texto a la izquierda, foto a la derecha */
background: linear-gradient(to right, rgba(8,7,26,0.88) 0%, rgba(8,7,26,0.55) 45%, transparent 70%);

/* Scrim de zócalo — texto abajo */
background: linear-gradient(to top, rgba(8,7,26,0.92) 0%, rgba(8,7,26,0.40) 45%, transparent 75%);
```

Regla: **si la pieza lleva CTA naranjo, el CTA va sobre el scrim, nunca sobre la foto
desnuda.** Y si la foto tiene naranjos o rojos dominantes, esa foto no se usa en piezas
con CTA — sirve para awareness, no para conversión.

### Prompt maestro vívido

```
Photograph of [SUJETO]. Shot on a 35mm lens at f/2.8, [LUZ], vibrant editorial
photography, sharp and clean.

Colour: rich saturated colour, deep blue sky, warm sunlit highlights, strong natural
contrast, film-like depth. Vivid but believable — no HDR crunch, no neon oversaturation,
no heavy filter look.

Location: Santiago de Chile. [MARCADORES]
No North American suburbia, no picket fences, no palm-lined driveways,
no generic Latin American colonial architecture.

Composition: [COMPOSICIÓN FOTO]

Mood: warm, alive, aspirational but real. Candid, not staged, no props styling.
Photorealistic with believable imperfections — not a 3D render, not an illustration,
not a stock photo.

Do not include: any text, letters, numbers, house numbers, signage, brand names,
logos, watermarks, human faces, or UI on screens.
```

`[LUZ]` — elegir uno: `late afternoon golden sunlight with long soft shadows` ·
`bright midday sun under a clear deep blue sky` · `warm side light coming through a
large window`. Mantener **uno solo** en todo el lote: la hora del día es lo que hace
que las fotos se vean de la misma sesión.

Los `[COMPOSICIÓN FOTO]` son los mismos tres modos de la familia fotográfica de arriba.

### `[MARCADORES]` — lo que hace que se vea Chile y no Latinoamérica genérica

Decir "Santiago" no alcanza: el modelo devuelve una ciudad latinoamericana cualquiera.
Lo que ancla la imagen son los detalles. Elegir 2 o 3 por prompt, los que correspondan
al encuadre — **aplica igual a la familia fotográfica fría de arriba.**

| Contexto | Marcadores a nombrar |
|---|---|
| Exterior urbano | `the snow-capped Andes on the horizon` · `overhead electrical cables crossing the street` · `patterned concrete paving-stone sidewalks` · `plane trees lining the street` · `metal security fences and electric gates in front of the houses` |
| Casa | `continuous-façade attached houses` · `painted brick or stucco masonry` · `a small front garden behind a metal fence` · `a flowering bougainvillea against the wall` · `clay tile or corrugated metal roof` · `aluminium-framed windows` |
| Edificio | `brick-clad mid-rise apartment building` · `enclosed balconies` · `the Andes filling the end of the street` |
| Interior | `laminate flooring` · `white melamine kitchen cabinets` · `a *logia* laundry alcove visible off the kitchen` · `aluminium window frames` · `a hot water heater on the wall` |
| Barrio (nombrar) | `a residential street in Ñuñoa` · `in Providencia` · `in Maipú` · `in Las Condes` |

Nunca marcas, patentes de auto legibles, letreros de calle ni logos de bancos o
supermercados. Un edificio reconocible al fondo del skyline está bien; como sujeto, no.

**Fuera de Santiago**, para variedad de propiedades: `colourful houses stacked on a
hillside in Valparaíso` · `a wooden shingled house under rain in the Chilean south,
near Puerto Varas` · `a house with a volcano on the horizon`. Son piezas de awareness,
no de conversión: el 80% del negocio está en Santiago.

### Catálogo vívido

1. `a contemporary apartment building with the snow-capped Andes rising behind it` ← ancla
2. `the front of a contemporary house with a small green front garden and a flowering bougainvillea`
3. `a pair of house keys on a kitchen counter with a hard patch of sunlight across it`
4. `two hands signing a blank sheet on a warm wooden table, seen from above, no faces`
5. `an empty living room with strong late afternoon sunlight falling across the bare floor`
6. `a smartphone held in one hand outdoors, screen completely off, sunlit street blurred behind`
7. `a clean modern kitchen with warm natural light` — la prevención de incendio vive acá
8. `an unfinished concrete building frame against a deep blue sky`
9. `a rooftop view over a dense residential neighbourhood at golden hour`
10. `a person seen from behind against a bright window, warm backlight, no face`

## Íconos: por SVG, no por imagen IA

**Los íconos no se generan con nano banana.** Tres razones, en orden de importancia:

1. **No hay transparencia real.** Un modelo de imagen devuelve raster con fondo. Si le
   pedís "transparent background" te dibuja el patrón de cuadritos, o te deja un blanco
   sucio que hay que recortar a mano ícono por ícono.
2. **El grosor de línea no es consistente.** El sistema Ancla es `stroke-width: 1.75`
   exacto en los 30 íconos. Un modelo de imagen te da 1.4 en uno y 2.3 en el siguiente,
   y el set se ve barato aunque cada ícono por separado se vea bien.
3. **`currentColor` se pierde.** Los SVG actuales heredan el color del texto que los
   rodea: el mismo archivo sirve blanco sobre dark y `#08071a` sobre light. Un PNG queda
   fijo a un color y hay que mantener una copia por fondo.

Los íconos se piden a un modelo de **código**, en SVG. La transparencia sale gratis, el
grosor es un número y el color lo decide el CSS.

### Prompt para generar íconos SVG

```
Necesito íconos SVG nuevos para el sistema de Ancla Seguros. Ya existen 30 y los
nuevos tienen que ser indistinguibles de ellos en peso y construcción.

Formato exacto — copiar esta estructura, sin agregar ni quitar atributos:

<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
     fill="none" stroke="currentColor" stroke-width="1.75" stroke-linecap="round"
     stroke-linejoin="round">
  <path d="…"/>
</svg>

Reglas duras:
- Estilo Lucide: outline lineal, un solo grosor, sin rellenos, sin degradados,
  sin máscaras, sin filtros, sin <style>, sin atributos de color en los paths.
- Todo el dibujo dentro de x/y 2–22: 2px de margen óptico en los cuatro lados.
- Coordenadas en pasos de 0.5 donde se pueda; los contenedores rectangulares
  usan rx="2".
- Máximo 5 elementos por ícono. Si necesita más, la metáfora es demasiado compleja
  y hay que simplificarla.
- Sin texto, sin letras, sin números dentro del ícono.
- Peso óptico parejo entre íconos: que ninguno se vea más pesado o más chico que
  el resto puesto al lado.

Entregá cada ícono como un archivo aparte, con el nombre exacto que indico, en
kebab-case. Nada de un solo SVG con todos adentro.

Íconos a crear: [LISTA]

Al final, autoverificá y reportá: (1) que todos tengan el header idéntico,
(2) que ninguno tenga fill distinto de none, (3) cuántos elementos tiene cada uno,
(4) cuáles quedaron con peso óptico dudoso y por qué.
```

### Lista de íconos que faltan (24)

Ninguno duplica los 30 existentes.

| Grupo | Nombres de archivo |
|---|---|
| Seguro y propiedad | `house-shield` · `building` · `key` · `flame-shield` · `seismic` · `water-drop` · `umbrella` |
| Dinero y trámite | `scale-compare` · `percent-badge` · `signature` · `receipt` · `calendar-check` · `wallet` |
| IA y tecnología | `sparkles` · `chip` · `bot` · `scan-doc` · `nodes` · `fingerprint` · `qr` |
| UI que falta | `arrow-left` · `chevron-up` · `refresh` · `eye` |

### Verificación

```bash
# Header idéntico en los 54
grep -L 'stroke-width="1.75"' assets/icons/*.svg
# Ningún fill de color
grep -l 'fill="#' assets/icons/*.svg
```

Y la que importa: renderizar los 54 juntos en una grilla y mirarlos. El peso óptico
solo se juzga en conjunto, nunca de a uno.

## QA — descartar si

- [ ] Aparece **cualquier** naranjo, amarillo, rojo o verde
- [ ] Hay texto, letras, números o algo que parezca un logo
- [ ] El modelo inventó una "A" o un ancla que parece nuestro isotipo
- [ ] Hay degradado azul → naranjo (prohibido explícito del guideline)
- [ ] El fondo no es plano: tiene viñeta, textura, piso o horizonte
- [ ] El sujeto toca los bordes o no deja el espacio vacío que pide su modo
- [ ] Hay caras humanas
- [ ] Se ve como stock photo: apretón de manos, gente aplaudiendo, gráficos flotantes
- [ ] Puesto al lado del ancla de estilo, se nota que es de otra familia

Ese último es el que más importa y el único que no se chequea mirando una sola imagen.

## Dónde se guardan

```
assets/images/
├── 3d/      <sujeto>-3d-<modo>-<ratio>.png
│             ej. casa-3d-a-1x1.png · edificio-3d-b-4x5.png
├── foto/    <sujeto>-<familia>-<detalle>-<ratio>.png
└── fondos/  gradiente-dark-brand-<ratio>.png   (aún vacía)
```

Nombres en minúscula, sin tildes, sin espacios. Formato: PNG para todo por ahora —
nano banana entrega PNG nativo; el plan original de este doc decía JPG calidad 85
para fotografía, pero convertir habría sido un paso extra sin necesidad real
(las piezas de RRSS no son tan sensibles al peso de archivo). Si el peso se vuelve
un problema, se revisita.

Al agregar imágenes al banco: correr el skill `/brand-asset` — el checklist incluye
listarlas en §16 Descargas y en el README, y subirlas al proyecto de Design.

## Banco actual — inventario (2026-08-17)

Primera tanda: 21 imágenes generadas por Cristóbal con nano banana a partir de los
prompts de este doc, a 2048×2048 (una a 1856×2304 ≈ 4:5). Todas en modo A (objeto en
el tercio derecho, izquierda vacía) salvo la marcada 4:5.

**9 · Familia 3D** (`assets/images/3d/`)

| Archivo | Sujeto |
|---|---|
| `casa-3d-a-1x1.png` | Casa — ancla de estilo del lote |
| `edificio-3d-a-1x1.png` | Edificio de departamentos |
| `banco-3d-a-1x1.png` | Banco (columnas) |
| `calculadora-3d-a-1x1.png` | Calculadora |
| `llave-3d-a-1x1.png` | Llave con llavero de casa |
| `escudo-3d-a-1x1.png` | Escudo con casa |
| `grafico-descendente-3d-a-1x1.png` | Gráfico de barras descendente |
| `detector-humo-3d-a-1x1.png` | Detector de humo |
| `casa-sismo-3d-a-1x1.png` | Casa sobre amortiguadores |

**12 · Familia foto** (`assets/images/foto/`)

| Archivo | Sujeto |
|---|---|
| `edificio-vivido-cordillera-1x1.png` | Edificio de ladrillo con la cordillera detrás |
| `casa-vivida-buganvilia-1x1.png` | Casa con buganvilia y garage lila |
| `casa-vivida-buganvilia-alt-1x1.png` | Misma casa, encuadre con más hormigón visible |
| `living-vivido-cordillera-1x1.png` | Living vacío con ventanal a la cordillera |
| `cocina-vivida-logia-1x1.png` | Cocina con logia y calefón a la vista |
| `celular-vivido-calle-1x1.png` | Celular en mano, calle residencial arbolada |
| `celular-vivido-colorido-1x1.png` | Celular en mano, calle de casas de colores — ver nota |
| `llaves-vividas-mesa-1x1.png` | Llaves sobre mesa de madera con sol |
| `firma-vivida-1x1.png` | Manos firmando un documento en blanco |
| `casa-vivida-sur-1x1.png` | Casa de tejuelas bajo lluvia, volcán al fondo (sur) |
| `obra-vivida-cordillera-4x5.png` | Obra en construcción con grúa, vertical |
| `edificio-frio-1x1.png` | Edificio en grade frío — familia fotográfica fría, no vívida |

### Pendientes de esta tanda

- **Las 21 traen un destello (sparkle) en la esquina inferior derecha.** Es la firma
  visual de nano banana, no algo que el prompt pidió — de hecho el prompt lo prohíbe
  explícitamente (`no sparkles`) y el modelo lo puso igual. Antes de usar cualquiera
  de estas piezas en un template, recortar o clonar esa esquina. No se resolvió acá
  porque corregir 21 PNG a mano no es una tarea de nombrar-y-commitear.
- **`celular-vivido-colorido-1x1.png` no cumple el brief de locación.** El fondo son
  casas de colores en una calle que no se lee como Santiago (podría pasar por
  Valparaíso o un pueblo genérico, pero no trae los marcadores de la tabla de
  arriba). Usable como pieza "fuera de Santiago" tipo awareness; no usar donde se
  necesite que se lea como la capital.
- Ninguna de las 21 quedó todavía en `index.html` (§08/§10) ni en el proyecto de
  Claude Design — son la materia prima del banco, no el banco mostrado en el
  guideline. Ese es un paso de curación aparte: elegir cuáles entran a las
  plantillas, no simplemente listar las 21.
