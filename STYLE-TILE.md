# Francisco’s Club · Style tile

## Dirección

Una identidad deportiva y social con contraste editorial: negro carbón para la energía nocturna del club, crema para la pausa y dorado para el escudo, el movimiento y los llamados a la acción.

| Elemento | Decisión |
| --- | --- |
| Logo | Escudo oficial entregado por el usuario, conservado como `public/escudo-franciscos-club.png` |
| Paleta | Carbón `#101112` · Crema `#f3efe5` · Papel `#faf8f2` · Dorado `#c99a3b` · Dorado claro `#e7c06a` |
| Tipografía | Manrope para interfaz y lectura · Playfair Display italic para énfasis · DM Mono para etiquetas |
| Botón primario | Píldora carbón con texto crema y flecha de avance |
| Superficie | Líneas finas, textura de grano y campos amplios; la imagen del escudo es siempre la fuente oficial |
| Imagen | El escudo funciona como objeto central; la cancha abstracta y el resplandor dorado acompañan el relato sin competir con la marca |
| 3D | La historia del club usa Three.js para crear una cancha interactiva local con césped, franjas, líneas reglamentarias, metas, postes de luz, balón, personajes tipo futbolito y un panel con el escudo |

## Visual Story

| Scene | Visual story | Website copy |
| --- | --- | --- |
| 01 — Opening | El escudo aparece como objeto hero sobre un campo crema con órbitas sutiles. | “Juega. Comparte. Pertenece.” + “Descubre el club”. |
| 02 — Development | Una cancha abstracta, un centro dorado y la palabra FRANCISCO’S sostienen el scroll mientras la copy cambia. | “Entra al juego.” → “Quédate por la experiencia.” |
| 03 — Resolution | La narrativa termina en identidad, datos reales y una llamada clara para visitar Paucarpata. | “Tu próxima buena historia.” + dirección y “Abrir en Maps”. |

## Notas de producción

- El contenido editable vive en `src/content/club.js`.
- La historia usa scroll nativo, un escenario `sticky` de 2500 px y tres cambios de copy; la cancha 3D vive aquí, no en “Un lugar para quedarte”. No secuestra la rueda y funciona hacia adelante y atrás.
- `prefers-reduced-motion` desactiva las transiciones suaves. El contenido principal no depende de la animación.
- La dirección y actividades provienen de la información entregada. No se añadieron testimonios, premios, métricas ni horarios no confirmados.
- La imagen del escudo proviene del archivo adjunto del usuario y se conserva como raster; no se redibujó.
