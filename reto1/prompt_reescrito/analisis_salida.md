### 1. Qué cambió frente al prompt malo

Con el prompt malo, el modelo cumplió 0 de las 5 reglas de negocio, porque nunca las recibió: inventó un colegio, reglas de edad y dos lenguajes. Con el prompt nuevo, que incluye los cinco bloques y las reglas dentro de `<reglas>…</reglas>`, la función quedó alineada con el sistema InscribeFET: una sola versión en Python, la firma pedida, retorno booleano, comentarios por regla y sin reglas inventadas.

### 2. Reglas cumplidas

| Regla | Resultado | Comentario |
|-------|-----------|------------|
| 1. Máx. 2 cursos por periodo | ✔ | Usa `len(est.cursos_periodo) >= 2`; el límite está bien (el código original usaba `> 2`). |
| 2. Cupo máx. 25 | ✔ | Usa `inscritos >= 25`; el código original usaba `< 26`. |
| 3. Sin horarios cruzados | ~ | La lógica de solapamiento es correcta, pero la implementación falla (ver punto 3). |
| 4. Cierre 3 días antes | ✔ | Calcula `fecha_inicio - timedelta(days=3)` y rechaza si `ahora` es posterior. |
| 5. Sin deuda vencida | ✔ | Rechaza si `est.deuda_vencida` es verdadero. |

Resultado: 4 de 5 reglas bien implementadas y 1 parcial.

### 3. Qué sigue fallando

1. **La regla 3 está mal conectada.** La función principal llama `son_horarios_cruzados(curso, c)` pasando los cursos completos, pero el helper accede a `h1['inicio']`, como si recibiera diccionarios. Si `curso` es un objeto, esto genera un error en cuanto el estudiante tiene un curso inscrito. Lo correcto habría sido pasar `curso.horario` y `c.horario`.
2. **Ignora el día de la semana.** El prompt indicaba que el horario incluye día, hora de inicio y hora de fin, pero la comparación solo usa las horas. Dos cursos a la misma hora en días distintos se marcarían como cruzados.
3. **El `# SUPUESTO:` contradice el código.** Dice que `horario` es un diccionario con `inicio`, `fin` y `dia`, pero el código nunca accede a `horario`. Además quedó fuera de la función, al final del bloque.
4. **La autoevaluación es incorrecta.** El modelo reportó «5» reglas implementadas, pero la regla 3 no funciona. Su declaración no coincide con lo que entregó.
5. **Detalles de formato.** Se pidió una línea que dijera cuántas reglas implementó y respondió solo «5»; además agregó una función auxiliar fuera de la función pedida.

### 4. Por qué creemos que pasó

- **Ambigüedad en el bloque de contexto.** El prompt decía que el curso tiene un atributo `horario` «con hora de inicio y de fin, y día de la semana», pero no indicaba si era un diccionario, un objeto o una tupla. El modelo tuvo que adivinar la estructura y no fue coherente: usó una en el helper y otra en el supuesto. Es una hipótesis, que se podría comprobar dándole la estructura exacta.
- **Autoverificación poco fiable.** Un modelo pequeño (qwen3.5:4b) puede contar las reglas que mencionó en comentarios en lugar de verificar que el código funcione. Contar y revisar son tareas distintas.
- **Las reglas simples quedaron bien porque eran explícitas.** Las reglas 1, 2, 4 y 5 se expresan con una comparación directa; la regla 3 exigía coordinar varias piezas (estructura de datos, bucle y helper).


### 5. Conclusión parcial

El bloque de **contexto y reglas dentro de etiquetas** fue el que más efecto tuvo: pasamos de 0 a 4 de 5 reglas bien implementadas. Aun así, un prompt mejor no garantiza una función correcta: el modelo puede decir que cumplió una regla y no haberla implementado bien, por lo que siempre hay que revisar el código.
