## Análisis de la salida 3 (variante: prompt nuevo sin el bloque de restricciones)
 
Variante ejecutada: el mismo prompt de la salida 2, quitando únicamente el bloque `# RESTRICCIONES` (solo librería estándar, no inventar reglas, sin `print`/`input`, usar `# SUPUESTO:` si algo no está especificado, nombres en español).
 
### 1. Qué cambió frente a la salida 2
 
| Aspecto | Salida 2 (con restricciones) | Salida 3 (sin restricciones) |
|---------|------------------------------|------------------------------|
| Reglas 1, 2, 4 y 5 | Correctas | Correctas (sin cambios) |
| Regla 3 (horarios cruzados) | Lógica de solapamiento correcta, pero ignora el día | Lógica correcta y ahora **sí compara el día** (`c1['dia'] != c2['dia']`) |
| Acceso a los datos | Mezcla: atributos (`est.deuda_vencida`) y diccionarios (`h1['inicio']`) | Consistente: todo con diccionarios (`est['deuda_vencida']`), aunque el prompt hablaba de «atributos» |
| Anotaciones de tipo | No | Sí (`est: dict`, `curso: dict`, `-> bool`) |
| Comentario `# SUPUESTO:` | Lo usó (y contradecía el código) | **No lo usó** |
| Línea final de reglas | «5» después del bloque | `# Reglas implementadas: 5/5` como comentario dentro del bloque |
| Reglas inventadas | No | No |
| Librerías externas, `print`, `input` | No | No |
| Orden de las reglas en el código | 1, 5, 4, 2, 3 | 1, 5, 2, 4, 3 |
 
### 2. Qué sigue fallando
 
1. **La regla 3 sigue mal conectada, con el mismo tipo de error que en la salida 2.** La función principal llama `son_horarios_cruzados(curso, c)` pasando los cursos completos, pero el helper accede a `c1['dia']`, `c1['inicio']`, etc. Esas claves están dentro de `curso['horario']`, no en el curso. Con datos reales, esto produciría un `KeyError` en cuanto el estudiante tuviera un curso inscrito. Lo correcto habría sido pasar `curso['horario']` y `c['horario']`.
2. **La autoevaluación vuelve a ser incorrecta.** Declara «5/5» reglas implementadas, pero la regla 3 no funciona.
3. **Cambió el tipo de dato sin decirlo.** El prompt describía `est` y `curso` como objetos con atributos y el modelo los trató como diccionarios, sin dejarlo indicado en un supuesto.
### 3. Qué mejoró sin el bloque de restricciones
 
- Corrigió un error de la salida 2: ahora la regla 3 considera el **día** de la semana, algo que el prompt pedía en el contexto.
- El código es más consistente internamente (un solo estilo de acceso a datos) y más legible gracias a las anotaciones de tipo.
### 4. Qué se perdió
 
- Desapareció el comentario `# SUPUESTO:`. Con la restricción, el modelo al menos dejaba constancia de lo que asumía (aunque en la salida 2 el supuesto se contradecía con el código). Sin ella, tomó la decisión de usar diccionarios en silencio, que es justo el tipo de adivinanza que el reto busca reducir.
- La línea final de reglas quedó dentro del bloque de código, y no como línea aparte posterior.
### 5. Interpretación
 
- El bloque de restricciones **no cambió el cumplimiento de las reglas de negocio**: ambas versiones aplican bien las reglas 1, 2, 4 y 5 y ambas fallan en la conexión de la regla 3. Esto sugiere que lo que más pesó fue el contexto y las reglas dentro de `<reglas>`, no las restricciones.
- Las restricciones sí tuvieron efecto en **el comportamiento declarativo**: el uso de `# SUPUESTO:` y el estilo de la respuesta.
- Los dos errores de la regla 3 (estructura de `horario` y día) tienen un origen común: el contexto no definía la estructura exacta de los datos. Ninguna restricción compensa un contexto ambiguo.
- Una advertencia sobre el alcance: es una sola ejecución por prompt (temperatura 0 y semilla 42). Cambiar el prompt, aunque sea quitando un bloque, puede alterar detalles del código (como el uso de diccionarios o la mejora del día) que no necesariamente se deban a la restricción en sí. Para afirmarlo con más seguridad habría que repetir la comparación con otros casos.
### 6. Conclusión del reto 1
 
Pasamos de **0 de 5 reglas** (prompt malo) a **4 de 5 bien implementadas** con el prompt completo y con la variante, y la quinta (horarios cruzados) quedó con la lógica correcta pero mal conectada en ambas. La técnica con más efecto fue entregar el **contexto y las reglas de negocio dentro de `<reglas>`**; el bloque de restricciones aportó sobre todo transparencia (`# SUPUESTO:`) y no más reglas cumplidas. Una mejora pendiente es describir con exactitud la estructura de datos de `curso` y `horario`, para eliminar el error de la regla 3 y la adivinanza entre objetos y diccionarios. Además, la autoevaluación del modelo («5/5») no es confiable: siempre hay que revisar el código.
 