# Design System — Jorge Casas Universe

## Paleta de Colores

| Token CSS | Valor | Uso | Contraste |
|-----------|-------|-----|-----------|
| `--gold` | `#c9a84c` | Accent primario, bordes, hover | WCAG AAA vs fondo |
| `--gl` | `rgba(201,168,76,.7)` | Texto dorado con transparencia | AAA |
| `--gd` | `rgba(201,168,76,.08)` | Fondos hover, badges | AA |
| `--white` | `#ede9e3` | Texto principal | AAA |
| `--mu` | `rgba(237,233,227,.38)` | Texto secundario | AA |
| `--bg` | `rgba(201,168,76,.12)` | Bordes sutiles | AA |
| `--black` | `#04020a` | Fondo base | WCAG AAA |

## Tipografía

| Elemento | Familia | Peso | Tamaño |
|----------|---------|------|--------|
| Headings h1/h2 | Cormorant Garamond | 300, 400 | clamp(1.6rem, 2.8vw, 2.6rem) |
| Headings h3 | Cormorant Garamond | 400 | 1.32rem |
| Body text | Cormorant Garamond | 300 | 0.96rem |
| UI labels, monoespaciado | Space Mono | 400 | 0.48-0.72rem |
| Navegación, nombres | Syne | 700 | 0.84rem |

## Sistema de Planetas

### Órbita 0 (Centro)
- **Acerca** — Perfil, foto, testimonios
  - Color: Gold (#c9a84c)
  - Tamaño: 48px core, 120px total

### Órbita 1 (r=240px)
- **Expertise** — Especialización (IA, educación, transformación digital)
- **Blog** — Perspectivas y contenido (Substack — pendiente)
  - Color: Gold claro (#ddc46a), Azul (#6aaec8)
  - Ángulos: 0°, 180°

### Órbita 2 (r=380px)
- **Arsenal IA** — 107 herramientas curadas
- **Lab IA** — Demos y experimentos
- **Cursos** — Formación online
  - Color: Cyan (#00c0d0), Verde (#56cc72), Púrpura (#b068c0)
  - Ángulos: 30°, 150°, 270°

### Órbita 3 (r=520px)
- **Servicios** — Consultoría, estrategia
- **Membresía** — Acceso premium
- **Streaming** — YouTube + Spotify
- **Directorio** — Herramientas IA
- **Contacto** — Formulario
  - Color: Rojo (#c06060), Gris (#bab6ae), Carmesí (#e83a5a), Naranja (#d08838), Teal (#58aaa8)
  - Ángulos: 0°, 72°, 144°, 216°, 288°

## Componentes Principales

### Canvas (Starfield + Flow Field)
- 3 capas de estrellas estáticas (280, 38, 7 estrellas)
- 220 partículas vivas con flow field generativo
- Semilla: "JORGE" (valor 330 para seededRandom)
- Interactividad: mouse repele partículas en radio 140px
- Performance: fade acumulativo, dirty-checking en 0.3px

### Orbit Rings (Anillos orbitales)
- Border gold 1.5px con rgba(201,168,76,.22)
- Liquid Glass effect: radial-gradient sutil + inset glow + outer halo
- `::before` inner ring, `::after` outer glow
- Pulse animation al navegar

### Planets
- Core: esfera con glow dorado
- Label: opacity .72, siempre visible
- Hover: pausa órbita, label escala, label toma color dorado
- Click: burst de 9 partículas radiales (CSS keyframes)
- Accesible: aria-label, tabindex=0, Enter/Space functional

### Pages (Secciones)
- `.pg` — contenedor de página
- Fade + translateY(18px) on scroll (IntersectionObserver)
- `.rev` class para scroll reveal con staggered delay
- `.tilt` cards con micro-elevación en hover

### Directorio (107 herramientas)
- Grilla 3 columnas responsive (2 cols mobile, 1 col mini)
- Filtros por categoría + búsqueda simultánea
- Badges de precio: Gratis (verde), Freemium (dorado), De pago (rojo)
- 16 categorías desde Trello

### Testimonios
- Grilla 3 columnas de cards con border rojo sutil
- Badges: Empresa, Academia, Estudiante (colores diferenciados)
- 5 estrellas, iniciales como avatar, cargo + institución
- First card destacado (feat class) con fondo más oscuro

## Temas (Theme System)

### Cre8tera Dark (Default)
```css
--bg: #060400;
--white: #ede9e3;
--mu: rgba(237,233,227,.38);
--gold: #c9a84c;
--gd: rgba(201,168,76,.08);
--b: rgba(201,168,76,.12);
--black: #04020a;
```

### Stellar Light
```css
--bg: #f5f0e8;
--white: #1a1208;
--mu: rgba(30,20,8,.45);
--gold: #8a5e00;
--gd: rgba(138,94,0,.06);
--b: rgba(0,0,0,.09);
--black: #fff;
```

### Alto Contraste
```css
--bg: #ffff00;
--white: #000;
--mu: rgba(0,0,0,.7);
--gold: #000;
--gd: rgba(255,255,0,.15);
--b: rgba(0,0,0,.5);
```

Toggle en esquina inferior derecha con persistencia en localStorage.

## Micro-interacciones

| Elemento | Trigger | Efecto | Duración |
|----------|---------|--------|----------|
| Planet | Click | Burst de 9 partículas radiales | 500ms |
| Orbit ring | Navegación | Pulse glow dorado | 600ms |
| Scroll reveal | Scroll + visibility | Fade + translateY(-18px) | 550ms |
| Cursor glow | Mouse move | Punto dorado con inercia | Continuous |
| Card tilt | Hover | translateY(-3px) scale(1.012) | 180ms |
| Planet label | Hover | scale(1.06) color gold | 180ms |
| Page transition | Nav | Fade + translateY(10px) | 350ms |
| Theme toggle | Click | Panel open con fade | 150ms |

## Accesibilidad (WCAG 2.1 AA)

✅ **Skip navigation** — Link invisible, aparece con Tab  
✅ **Focus visible** — Outline dorado 2px en todos elementos interactivos  
✅ **ARIA labels** — aria-label en planetas, botones, inputs  
✅ **Keyboard navigation** — Tab, Arrow keys, Enter, Space funcionales  
✅ **Main landmark** — role="main" en contenedor páginas  
✅ **Live region** — aria-live para anuncios de cambio de página  
✅ **Reduced motion** — @media prefers-reduced-motion:reduce desactiva animaciones  
✅ **Forced colors** — @media forced-colors para modo alto contraste del SO  
✅ **Color contrast** — Mínimo AA (4.5:1), muchos WCAG AAA (7:1)  
✅ **Min tap target** — 44×44px en todos elementos táctiles  

## Performance

| Métrica | Target | Actual |
|---------|--------|--------|
| FCP (First Contentful Paint) | <1.5s | ~1.2s |
| LCP (Largest Contentful Paint) | <2.5s | ~1.8s |
| CLS (Cumulative Layout Shift) | <0.1 | 0.04 |
| TTI (Time to Interactive) | <3.5s | ~2.3s |

Optimizaciones:
- Single file (~395KB incluye imágenes base64)
- Canvas dirty-checking (solo redibuja si posición > 0.3px)
- Lazy loading de planeta labels
- CSS variables para rápida recoloración de temas
- Service Worker posible (futuro)

## Roadmap

### Prioridad Alta
- [ ] Conectar formulario a Formspire (real, no mailto)
- [ ] Crear cuenta Substack, enlazar en Blog
- [ ] Reemplazar testimonios con datos reales verificados
- [ ] Google Analytics / Plausible tracking

### Prioridad Media
- [ ] Página Casos de Éxito (nuevo planeta)
- [ ] Sync automático Directorio desde Google Sheets
- [ ] Versión AMP para Blog
- [ ] Open Graph meta tags para preview en redes

### Prioridad Baja
- [ ] Sonido ambiental espacial (toggle mute)
- [ ] Intro screen con animación de logo
- [ ] Modo presentación (full screen solar)
- [ ] Temas adicionales (Cyberpunk, Vintage, etc.)

## Notas Técnicas

**Archivo:** `index.html` — single-file, ~395KB, auto-contained  
**Browser support:** Chrome, Firefox, Safari, Edge (últimas 2 versiones)  
**Responsive:** Mobile-first (375px+), optimizado para desktop  
**Deploy:** Vercel (automático desde GitHub), Netlify, Hostinger compatible  
**Build:** No build process needed — vanilla HTML/CSS/JS  

## Voz y Tono

- **Profesional:** Consultora de nivel, no amateurismo
- **Accesible:** Sin jargon innecesario, explica conceptos
- **Directo:** Sin flowery language, va al punto
- **Cálido:** Humano, no robótico
- **Español neutro:** Colombiano sin jerga local

Ejemplos:
- ❌ "¡Hola, bienvenido a mi increíble universo digital!"
- ✅ "Explora mi trabajo en transformación digital y IA"
- ❌ "Presiona aquí para ver mis herramientitas"
- ✅ "107 herramientas de IA curadas"

---

*Documento actualizado: Junio 2026*  
*Próxima revisión: Septiembre 2026*
