# Ancla Seguros — Brand Guideline v2.0

Guía de marca oficial de Ancla Seguros. Todo lo que necesita alguien para hacer
posts, presentaciones, piezas gráficas o landing pages que se sientan Ancla.

## Cómo usar esta carpeta

**Para verla como presentación:**
Abrir `index.html` en cualquier navegador moderno (Chrome, Safari, Firefox, Edge).
Doble click al archivo basta. No requiere instalación ni conexión (salvo la fuente
Plus Jakarta Sans, que se carga desde Google Fonts).

**Para compartirla con un diseñador, marketer o proveedor:**
Comprimir la carpeta completa en un ZIP y enviar. Todos los recursos están adentro.

## Qué hay acá

```
ancla-brand-guideline/
├── index.html                          ← La presentación completa (16 secciones)
├── README.md                           ← Este archivo
└── assets/
    ├── logos/                          ← 2 archivos por marca + isotipo
    │   ├── logo-ancla-on-dark.svg      ← Seguros sobre dark (#08071a) o brand (#392dcf)
    │   ├── logo-ancla-on-light.svg     ← Seguros sobre claro — texto en brand #392dcf
    │   ├── logo-ancla-recupera-on-dark.svg  ← Recupera sobre dark
    │   ├── logo-ancla-recupera-on-light.svg ← Recupera sobre claro — texto en brand
    │   └── isotipo-ancla.svg           ← Solo la "A" (favicon, avatar, watermark)
    │                                     No hay versiones monocromas: a 1 tinta se usa
    │                                     el mismo archivo en un solo color.
    ├── icons/                          ← 30 íconos custom en estilo Lucide
    │   └── (arrow-right, bank, calculator, chart-down, shield-check, …)
    ├── colors/
    │   ├── ancla-tokens.css            ← Variables CSS para pegar en web
    │   └── ancla-tokens.json           ← Design tokens para Figma / Storybook / etc
    └── patterns/
        └── grid-dark.svg               ← Patrón de grid para hero sections
```

## Secciones del index.html

```
01  Esencia               ← Propósito, propuesta, principios, personalidad
02  Logo                  ← 2 archivos por marca + isotipo, descarga inline por card
03  Color                 ← Sistema dual primary (dark ↔ brand) + acentos
04  Tipografía            ← Plus Jakarta Sans, escala canónica
05  Iconografía           ← Sistema Lucide, 30 íconos custom
06  Componentes           ← Botones, cards, badges, inputs, navbar, footer
07  Voz & Tono            ← Cómo habla Ancla
08  Social Media          ← Posts 1:1, 4:5, 9:16 + carruseles
09  Presentaciones        ← PPT/Slides — cover (dark + brand), contenido, KPI, cierre
10  Piezas Gráficas       ← Banners, flyers, ilustraciones, stories
11  Landing Pages         ← Hero anatomy, ritmo, FAQ, footer
12  Email · OG · Favicon  ← Firma email, social preview, favicon set
13  WhatsApp              ← Business profile, Status diarios, templates
14  Ads Paid              ← Meta (Feed + Stories), Google, LinkedIn
15  Co-brand · Press · Merch ← Lockups con bancos, press kit, polera/taza/sticker
16  Descargas             ← Todos los archivos en una vista
```

## Resumen del sistema

| Color              | HEX        | Uso                                                                |
|--------------------|------------|--------------------------------------------------------------------|
| Brand              | `#392dcf`  | Identidad, componentes, **fondo co-primario** (alternar con dark)  |
| CTA                | `#ff8322`  | **Solo** botones de acción primaria                                |
| BG Dark            | `#08071a`  | **Fondo co-primario** (alternar con brand) — texto blanco          |
| BG Brand           | `#392dcf`  | **Fondo co-primario** — identidad fuerte, texto blanco             |
| BG Light           | `#f4f7fc`  | Soporte: contenido denso, formularios (texto `#08071a`)            |
| Cyan acento        | `#2bd3f5`  | Highlights, íconos destacados                                      |
| Rose acento        | `#ca4bff`  | Gradientes, badges                                                 |

**Fuente:** Plus Jakarta Sans (200–800), exclusiva.
**Gradientes permitidos:** cyan → rose (acentos) y dark → brand (fondos atmosféricos). **Nunca** azul → naranjo.

## Reglas de oro

1. **Dual primary.** Dark (`#08071a`) y Brand (`#392dcf`) son co-protagonistas. Alterna entre ambos como fondo principal para que la marca no se vuelva monótona. Light (`#f4f7fc`) es soporte para contenido denso.
2. El naranjo **nunca** decora. Solo acción.
3. Sobre fondo dark o brand, el texto es blanco. Siempre. Sobre fondo claro, el logo va con
   texto en brand `#392dcf` — el texto negro no se usa, y no existen versiones monocromas.
4. Un solo CTA por pantalla.
5. Cifras concretas en el copy. "25–40% de ahorro" > "ahorra mucho".
6. El villano es el desconocimiento, no los bancos.
7. **Gradientes permitidos**: cyan→rose (acentos) y dark→brand (fondos atmosféricos). **Nunca** azul→naranjo.

## Contacto

Para dudas sobre el uso de la marca, contactar al equipo de Ancla Seguros.

---

Versión 2.0 · 2026 · anclaseguros.cl
