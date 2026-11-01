# Reto 4 · Parte B · La instrucción que se olvida (InscribeFET)

Modelo: qwen-demo (`--think=false`). Ventana: `/set parameter num_ctx 512`.

**Regla de formato usada:** empieza SIEMPRE tu respuesta con «VEREDICTO InscribeFET:».

**Pregunta 1:** Analiza en al menos 400 palabras si el sistema InscribeFET debería permitir que un estudiante se inscriba en un curso el mismo día en que empieza. Considera ventajas, riesgos y casos borde.

**Pregunta 2:** Explica en al menos 400 palabras cómo debería manejar InscribeFET una lista de espera cuando un curso llega a su cupo máximo, y qué problemas podrían aparecer.

## Resultados

| Ejecución | Dónde estaba la regla | Respuesta | ¿Empieza con «VEREDICTO InscribeFET:»? | Observación |
|-----------|-----------------------|-----------|-----------------------------------------|-------------|
| Versión A | Mensaje del chat (con «¿Entendido?») | «¿Entendido?» | V | Confirmó la regla y la aplicó. |
| Versión A | Mensaje del chat | Pregunta 1 | V | Cuatro párrafos; la regla seguía dentro de la ventana. |
| Versión A | Mensaje del chat | Pregunta 2 | F | Empezó con «La gestión de una lista de espera…», usó negritas que antes no aparecían y se cortó a mitad de frase. |
| Versión B, intento 1 | `/set system` (ventana sin confirmar en 512) | Pregunta 1 | V | Respuesta completa. |
| Versión B, intento 1 | `/set system` (ventana sin confirmar en 512) | Pregunta 2 | V | Respuesta completa. No concluyente: no se verificó `num_ctx 512`. |
| Versión B, intento 2 | `/set system` con `num_ctx 512` | Pregunta 1 | V | Cerca de 500 palabras; usó la frase y cerró con un veredicto. |
| Versión B, intento 2 | `/set system` con `num_ctx 512` | Pregunta 2 | V | Respetó la regla, pero la respuesta se cortó a mitad de frase. |

(Verificar el conteo de palabras de cada respuesta con `(Get-Content archivo.md | Measure-Object -Word).Words` y anotarlo en la tabla.)

## Explicación

**Qué es la ventana de contexto.** `num_ctx` indica cuántos tokens puede tener el modelo «a la vista» en una conversación. Esa cuenta incluye todo: los mensajes del usuario, las respuestas anteriores y la respuesta que está generando. Con `num_ctx 512`, una sola respuesta de 400 palabras en español ocupa buena parte de la ventana, y dos respuestas seguidas la desbordan.

**Versión A: la regla como mensaje del chat.** La regla era un mensaje más de la conversación y además el más antiguo. La primera respuesta aún la tenía a la vista y la cumplió. Al llegar la pregunta 2, el historial ya no cabía: lo más antiguo se descartó, y con ello se perdió la regla. El modelo respondió sin la frase, con otro estilo y con la respuesta cortada al final. No parece imitación del inicio de la respuesta anterior, porque esa respuesta era larga y no cabía completa en la ventana.

**Versión B: la regla como system prompt.** Cuando la ventana se llena, el recorte del contexto descarta mensajes antiguos del chat, pero conserva el system prompt. Por eso la regla siguió vigente en las dos respuestas, incluso cuando la ventana se desbordó (la pregunta 2 también salió cortada al final). El system prompt funciona como una instrucción permanente, y un mensaje del chat solo dura mientras siga dentro de la ventana.

**Sobre el intento 1 de la versión B.** En esa ejecución las respuestas salieron completas, sin cortes, lo que indica que la ventana era mayor y no se había fijado `num_ctx 512`. Por eso se repitió. Es un ejemplo de por qué conviene fijar los parámetros antes de comparar.

**Limitaciones.** Es una sola ejecución con dos preguntas por versión, y los resultados dependen del modelo y de la longitud de cada respuesta. Que la versión B cumpla la regla no prueba que el modelo la «recuerde», solo que el system prompt sigue en la ventana. Y que el system prompt sobreviva no evita el corte de la respuesta: la longitud máxima sigue limitada por la ventana.

**Conclusión práctica.** Las reglas de formato y de comportamiento que deben cumplirse durante toda la conversación van en el system prompt. Lo que se escribe como un mensaje más del chat puede perderse cuando el historial crece, y por eso conviene no depender de él.