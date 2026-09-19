# Plan de mejora · Francisco’s Club

## Objetivo

Convertir la página actual en una experiencia digital más completa para Francisco’s Club: presentar el club deportivo y social de Paucarpata, transmitir energía de juego y facilitar que una persona decida visitar, contactar o reservar.

La base actual ya cuenta con una identidad visual en crema, carbón y dorado, el escudo oficial, una historia con scroll y una cancha interactiva construida con Three.js. Este plan propone mejorarla sin perder claridad, rendimiento ni facilidad de mantenimiento.

## Estado actual

- Landing page en Vue + Vite.
- Escudo oficial conservado en `public/escudo-franciscos-club.png`.
- Datos editables en `src/content/club.js`.
- Historia de tres escenas con scroll nativo y `position: sticky`.
- Cancha interactiva Three.js con jugadores simples tipo futbolito, balón, metas y luces.
- Asset generado con mmx en `public/futbol-franciscos-club.jpg`.
- Diseño responsive y fallback para `prefers-reduced-motion`.

## Prioridad 1 · Conversión y contenido real

### 1.1 Llamada principal

- Definir una acción principal real: reservar cancha, escribir por WhatsApp o solicitar información.
- Reemplazar el enlace genérico de Maps por un CTA de contacto cuando exista el número oficial.
- Añadir un formulario solo cuando tenga un destino real de envío.
- Mantener una alternativa visible para quienes solo desean visitar el club.

### 1.2 Información del club

- Confirmar horarios de atención.
- Confirmar deportes, servicios y capacidad real de las instalaciones.
- Añadir ubicación exacta de Google Maps cuando se valide el punto.
- Añadir redes sociales oficiales si existen.
- No publicar testimonios, premios, precios o métricas sin confirmación.

### 1.3 SEO básico

- Añadir Open Graph y Twitter Cards.
- Crear favicon optimizado a partir del escudo.
- Añadir `application/ld+json` de tipo `SportsActivityLocation` o `LocalBusiness` cuando se confirmen los datos públicos.
- Revisar títulos, descripciones, jerarquía de encabezados y textos alternativos.

## Prioridad 2 · Assets generados con mmx

Se puede usar `mmx image generate` para crear imágenes de apoyo, siempre que la API key se mantenga fuera del repositorio. La configuración debe vivir en `C:\Users\USER\.mmx\config.json` o en la variable de entorno `MINIMAX_API_KEY`.

### Assets recomendados

1. **Hero atmosférico**
   - Campo nocturno con iluminación dorada y espacio negativo para el titular.
   - Uso: fondo auxiliar del hero o transición hacia la historia.

2. **Vida social del club**
   - Escena editorial de amigos compartiendo después del partido.
   - Uso: sección de gastronomía y comunidad.
   - Evitar rostros identificables o presentarlos como imágenes conceptuales, no como clientes reales.

3. **Detalle deportivo**
   - Balón, textura de césped, redes o luces en formato horizontal y vertical.
   - Uso: tarjetas, separadores y responsive mobile.

4. **Texturas de marca**
   - Metal dorado, líneas de cancha, grano oscuro y patrones inspirados en el escudo.
   - Uso: fondos ligeros, nunca detrás de textos pequeños con poco contraste.

### Flujo seguro de generación

```powershell
mmx image generate `
  --prompt "Descripción específica, sin texto, sin logos falsos y con espacio negativo" `
  --aspect-ratio 16:9 `
  --out public/assets/nombre-del-asset.jpg `
  --response-format url `
  --non-interactive
```

- Guardar los originales y registrar prompt, fecha y finalidad en `assets/README.md`.
- Revisar cada imagen antes de incorporarla.
- No pedir que mmx redibuje el escudo oficial; el escudo entregado por el usuario debe seguir siendo la fuente de marca.
- Optimizar las imágenes para web con WebP/AVIF cuando el pipeline esté definido.
- Mantener una versión fallback si una imagen tarda en cargar o falla.

## Prioridad 3 · Evolución de Three.js

### 3.1 Interacción de la cancha

- Mantener jugadores geométricos simples para que la escena cargue rápido.
- Añadir estados claros: reposo, pase, remate y gol.
- Hacer que la pelota responda al click/tap con una trayectoria corta y predecible.
- Añadir una pequeña leyenda accesible: “Mueve el cursor” y “Toca para patear”.
- Desactivar la animación continua cuando `prefers-reduced-motion` esté activo.
- No hacer que la información del club dependa de WebGL.

### 3.2 Rendimiento

- Cargar Three.js y la cancha con `defineAsyncComponent` o `import()` para reducir el bundle inicial.
- Limitar el pixel ratio a un máximo razonable.
- Reducir geometrías y materiales en móviles.
- Pausar el render cuando la cancha esté fuera del viewport usando `IntersectionObserver`.
- Liberar geometrías, materiales y texturas al desmontar el componente.
- Medir el bundle y el FPS en desktop y móvil.

### 3.3 Dirección artística

- Integrar el dorado del escudo en luces y líneas de cancha.
- Usar jugadores de dos equipos con tonos crema/dorado y azul grisáceo.
- Mantener la cámara en una vista isométrica legible.
- Evitar que las animaciones compitan con los titulares de la historia.

## Prioridad 4 · Estructura de la página

1. **Inicio**: propuesta de valor, escudo y CTA principal.
2. **Historia del club**: cancha Three.js y tres momentos narrativos.
3. **Experiencia**: deporte, mesa/barra y comunidad; sin otra cancha 3D.
4. **Galería**: assets mmx y fotografías reales cuando estén disponibles.
5. **Información práctica**: dirección, horarios, contacto y mapa.
6. **Footer**: razón social, RUC, enlaces legales y redes oficiales.

## Prioridad 5 · Accesibilidad y responsive

- Comprobar contraste de textos dorados sobre fondos crema y carbón.
- Añadir focus states visibles a botones, enlaces y navegación.
- Asegurar que el canvas tenga una descripción accesible.
- Mantener navegación usable sin mouse.
- Probar anchos de 320 px, 390 px, 768 px, 1024 px y desktop amplio.
- Probar con movimiento reducido y conexión lenta.

## Verificación antes de publicar

- `npm run build` sin errores.
- `npm run lint` sin errores.
- Consola del navegador sin errores WebGL o de carga.
- Imágenes con rutas válidas y tamaños razonables.
- Navegación hacia cada sección funcionando.
- Scroll hacia adelante y atrás sin saltos.
- Cancha 3D visible en desktop y con fallback legible en móvil/dispositivos sin WebGL.
- No hay claves, tokens ni URLs privadas dentro del repositorio.
- Todo dato comercial publicado está confirmado por el club.

## Orden recomendado de ejecución

### Fase 1 · Base comercial

Confirmar contacto, horarios, servicios y CTA principal. Implementar SEO y metadatos.

### Fase 2 · Assets

Generar con mmx el hero atmosférico, la escena social y texturas. Revisar, optimizar y documentar cada asset.

### Fase 3 · Three.js

Separar la cancha en carga diferida, añadir estados de juego, pausar fuera del viewport y validar el fallback.

### Fase 4 · Galería y experiencia

Incorporar los assets generados en una galería editorial y conectar cada tarjeta con el contenido real del club.

### Fase 5 · QA y publicación

Ejecutar build, lint, revisión responsive, accesibilidad, rendimiento y validación final del contenido antes de publicar.

## Resultado esperado

Una web memorable pero clara: la cancha 3D atrae y explica la energía del club, los assets generados amplían la atmósfera y los datos reales convierten la visita en una acción concreta.
