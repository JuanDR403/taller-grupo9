# Decisiones que el modelo tuvo que adivinar con el prompt malo

Prompt malo: «Haz una función para inscribir estudiantes.»

| # | Decisión adivinada | Que hizo el modelo |
|---|--------------------|--------------------------------|
| 1 | Lenguaje de programación | Entrego la función en dos lenguaje de programación diferentes Pytho y JavaScript. |
| 2 |Adivino el contexto|Supuso un colegio (Primaria, Secundaria, Preparatoria) en lugar de cursos libres de la Universida (inglés, deportes, artes).|
| 3 | Reglas de negocio| Inventó las reglas: edad entre 5 y 100 años y nombre no vacío.|
| 4 | Manejo de supuestos | No pregunto nada ni marco supuestos con comentarios: decidio todo en silencio|
| 5 | Invento el formato| gregó introducción, emojis y explicaciones en lugar de solo código.|

## Observaciones
* La salida se corto a la mitad del código de JavaScript
* el modelo no preguntó nada ni marcó supuestos con un comentario, decidió todo en silencio.