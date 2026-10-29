Aquí tienes la tabla de casos de prueba generada basándome en los criterios proporcionados, asegurando cobertura completa y incluyendo casos límite donde sea relevante.

| Caso | Entrada | Resultado esperado | Criterio que cubre |
| :--- | :--- | :--- | :--- |
| 1 | Usuario intenta inscribirse en un curso lleno (25/25). | Notificación de "Sin cupos disponibles" y colocación automática en la lista de espera. | Criterio 1 |
| 2 | Usuario intenta inscribirse en el **26º** estudiante de un curso lleno. | Notificación de "Sin cupos disponibles" y colocación automática en la lista de espera. | Criterio 1 (Límite) |
| 3 | Usuario intenta inscribirse en su **2º** curso del periodo. | Inscripción exitosa sin restricciones adicionales. | - |
| 4 | Usuario intenta inscribirse en su **3º** curso del periodo. | Rechazo de inscripción indicando que el límite de 2 cursos por estudiante ha sido superado. | Criterio 2 |
| 5 | Usuario intenta inscribirse en un curso con horario coincidente (ej. Lunes 10:00) con uno ya registrado. | Bloqueo de inscripción indicando conflicto de horario. | Criterio 3 |
| 6 | Usuario intenta inscribirse en un curso sin horarios cruzados. | Inscripción exitosa. | - |
| 7 | Usuario intenta inscribirse estando con deuda vencida. | Impedimento de inscripción y mensaje para regularizar situación financiera. | Criterio 4 |
| 8 | Usuario intenta inscribirse estando con deuda pagada a tiempo. | Inscripción exitosa (si cumplen otros requisitos). | - |
| 9 | Usuario intenta inscribirse el día **T-3** (inicio del curso menos 3 días). | Notificación de que la inscripción ya cerró y no es posible registrarse. | Criterio 5 |
| 10 | Usuario intenta inscribirse el día **T-2** (inicio del curso menos 2 días). | Inscripción exitosa si hay cupos y cumple otros requisitos. | - |

### Resumen de agregados:
No se agregaron casos nuevos, ya que la tabla original contenía una cobertura adecuada para cada criterio:
*   **Criterio 1:** Cubierto por el Caso 1 (caso normal) y el Caso 2 (caso límite del cupo).
*   **Criterio 2:** Cubierto por el Caso 4.
*   **Criterio 3:** Cubierto por el Caso 5.
*   **Criterio 4:** Cubierto por el Caso 7.
*   **Criterio 5:** Cubierto por el Caso 9 (caso límite de tiempo).

