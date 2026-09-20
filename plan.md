# Plan vigente de rediseño · Francisco’s Club

## Principio rector

“Juega. Comparte. Pertenece.” pertenece únicamente al Hero. Es la promesa de marca y no debe repetirse como título, índice, sistema de pilares ni cierre de las demás secciones.

Después del Hero, la página avanza como una experiencia natural:

**emoción → acción → recorrido → permanencia → oferta → comunidad**

## Arquitectura

### 1. Hero

- Mantener el escudo, el mensaje principal y el texto introductorio.
- CTA: “Descubre la cancha”.
- Objetivo: reconocimiento inmediato y deseo de seguir explorando.

### 2. La cancha te espera

- Título: “Haz que pase”.
- Cancha Three.js como protagonista visual e interactivo.
- Precio visible: **S/ 50**, rotulado como “Precio de cancha”.
- No inventar duración, promociones o condiciones; acompañar con “Consulta condiciones y disponibilidad”.
- CTA externo hacia los canales oficiales.

### 3. Más que un partido

- Secuencia: **Antes → Durante → Después**.
- Antes: llegar, reconocer personas y dejar la rutina afuera.
- Durante: movimiento, concentración y próxima jugada.
- Después: comida, conversación y recuerdo.

### 4. Un lugar para quedarse

- Explicar el lado social del club.
- Destacar mesa, comida, barra, conversación y ambiente cercano.
- Composición editorial con una fotografía principal y una imagen secundaria superpuesta.

### 5. Lo que encuentras aquí

- Cancha y actividades deportivas.
- Mesa, comida y barra.
- Ambiente de club.
- Mantener visibles las actividades declaradas por la empresa.

### 6. Únete a la comunidad

- Linktree, TikTok, WhatsApp y Google Reviews.
- Cierre: “Nos vemos en la cancha”.
- CTA hacia la ubicación en Google Maps.

### 7. Footer

- Dirección completa.
- Razón social, RUC, condición y fecha de inicio.
- Navegación interna.
- Aviso de imágenes conceptuales.

## Plan visual por sección

| Sección | Recurso | Sensación | Ubicación y tratamiento |
|---|---|---|---|
| Hero | Escudo oficial y órbitas gráficas | Identidad, orgullo y expectativa | Escudo dominante a la derecha; fondo crema y dorado |
| La cancha te espera | Escena Three.js | Movimiento, control y acción | Ocupa el lado derecho sobre fondo carbón; interacción con cursor o toque |
| Antes | `public/assets/pilar-pertenece.jpg` | Llegada, familiaridad y anticipación | Primera tarjeta vertical del recorrido |
| Durante | `public/assets/pilar-juega-v2.jpg` | Velocidad, concentración y competencia | Tarjeta central más grande y ligeramente desplazada |
| Después | `public/assets/pilar-comparte-v3.jpg` | Amistad, descanso y conversación | Última tarjeta del recorrido |
| Un lugar para quedarse | Imágenes “Comparte” y “Pertenece” | Calidez, cercanía y permanencia | Fotografía social a página parcial con una llegada nocturna superpuesta |
| Lo que encuentras aquí | Gráficos orbitales CSS | Claridad y continuidad de marca | Tarjetas informativas sobre fondo dorado; sin fotografías innecesarias |
| Comunidad | Tipografía y tarjetas de enlaces | Acceso, continuidad y convocatoria | Fondo verde oscuro; enlaces claros y cierre con CTA |
| Footer | Escudo pequeño | Confianza y formalidad | Fondo carbón, información legal y ubicación |

## Criterios para imágenes

- Usar imágenes aspiracionales, pero cercanas a una realidad urbana peruana sencilla.
- No presentar imágenes generadas como fotografías documentales del establecimiento o de clientes reales.
- Evitar estadios profesionales, lujo, uniformes oficiales y publicidad inventada.
- Mantener luz cálida, contraste cinematográfico y paleta negra, verde oscura y dorada.
- Conservar textos alternativos descriptivos.
- Usar carga diferida fuera del Hero.

## Criterios técnicos

- Three.js debe cargarse en un chunk diferido.
- Pausar el render 3D fuera del viewport.
- Respetar `prefers-reduced-motion`.
- Mantener fallback si WebGL no está disponible.
- No hacer que información comercial dependa del canvas.
- No guardar claves de mmx ni otros tokens en el repositorio.

## Verificación

- El lema completo aparece solamente en el Hero.
- **S/ 50** es legible y visualmente dominante en “La cancha te espera”.
- La secuencia Antes/Durante/Después se entiende sin explicación adicional.
- Los enlaces externos abren en una pestaña nueva.
- Dirección y datos legales están presentes en el footer.
- `npm run build` y `npm run lint` terminan sin errores.
- Desktop, tablet y móvil conservan jerarquía, contraste y navegación.

## Resultado esperado

Una página menos repetitiva y más narrativa: primero despierta emoción, luego muestra la cancha y el precio, acompaña al usuario por los momentos de la visita, presenta el lado social y termina facilitando el contacto y la ubicación.
