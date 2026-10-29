# Nota de corrección · Reto 3 (InscribeFET)

## Dónde apareció el error
En el Prompt 1. La historia de salida1.md decía "...para poder inscribirme
automáticamente cuando se libere un cupo", pero el pedido solo hablaba de
quedar en lista de espera. La inscripción automática fue inventada por el modelo.

## Cómo se propagó
- Prompt 2 (salida2.md): el criterio 2 inscribe automáticamente al "primer
  estudiante de la lista", sin comprobar que cumpla las otras reglas; el
  criterio 1 es circular; ningún criterio cubre el cierre de inscripción.
- Prompt 3 (salida3.md): CP2-1 y CP2-2 esperan esa inscripción automática,
  CP1-1 y CP1-2 son idénticos, y el resumen afirma incluir un caso límite
  de cupo que no existe.

## Qué corregí
Agregué al Prompt 1 un bloque <restricciones> (usar solo lo que dice el
pedido, no agregar comportamientos, beneficio general) y repetí la cadena
con los prompts 2 y 3 sin cambios, salvo el insumo.

## Qué mejoró
Desapareció la inscripción automática y el orden de la fila; los criterios
cubren las 5 reglas; la tabla tiene 10 casos con límites de cupo y de fecha.

## Qué persiste
- El caso 10 espera inscripción exitosa en T-2, cuando la inscripción cierra
  3 días antes del inicio.
- Faltan los casos T-4 y 24/25 inscritos.
- Ningún caso cruza la lista de espera con las demás reglas.
- La auto-verificación del modelo no detectó estos fallos.

## Conclusión
Corregir el primer paso eliminó la invención principal, pero la tabla final
aún necesita revisión humana.