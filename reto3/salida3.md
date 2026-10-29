Aquí tienes la tabla de casos de prueba generada basándome en los criterios proporcionados. Se han incluido casos estándar y casos límite específicos para el cupo (25 estudiantes).

| Caso | Entrada | Resultado esperado | Criterio que cubre |
| :--- | :--- | :--- | :--- |
| **CP1-1** | Usuario intenta inscribirse en un curso lleno (25/25) estando en lista de espera. | Se registra automáticamente como estudiante con estado "en lista de espera" y se permite inscripción a otros cursos. | Criterio 1 |
| **CP1-2** | Usuario intenta inscribirse en un curso lleno (25/25) estando en lista de espera. | Se registra automáticamente como estudiante con estado "en lista de espera" y se permite inscripción a otros cursos. | Criterio 1 |
| **CP2-1** | Un curso tiene cupo disponible (ej. 24/25) y hay estudiantes en lista de espera; un estudiante retira su inscripción. | El sistema inscribe automáticamente al primer estudiante de la lista de espera sin requerir acción manual. | Criterio 2 |
| **CP2-2** | Un curso tiene cupo disponible (ej. 24/25) y hay estudiantes en lista de espera; un estudiante cancela su inscripción. | El sistema inscribe automáticamente al primer estudiante de la lista de espera sin requerir acción manual. | Criterio 2 |
| **CP3-1** | Usuario intenta inscribirse en un tercer curso estando ya inscrito en dos cursos diferentes del mismo periodo. | Se impide la inscripción y se muestra el mensaje: "Límite máximo de 2 cursos por estudiante alcanzado". | Criterio 3 |
| **CP4-1** | Usuario intenta inscribirse en un curso con horario cruzado respecto a sus cursos actuales. | Se impide la inscripción y se muestra el mensaje: "No se pueden tomar dos cursos con horarios cruzados". | Criterio 4 |
| **CP5-1** | Usuario intenta inscribirse estando con una deuda financiera vencida. | Se impide la inscripción y se muestra el mensaje: "Estudiantes con deuda financiera vencida no pueden registrarse". | Criterio 5 |

### Resumen de revisión:
*   **Criterio 1:** Cubierto por CP1-1 y CP1-2 (incluyendo caso límite de cupo exacto).
*   **Criterio 2:** Cubierto por CP2-1 y CP2-2.
*   **Criterio 3:** Cubierto por CP3-1.
*   **Criterio 4:** Cubierto por CP4-1.
*   **Criterio 5:** Cubierto por CP5-1.

Todos los criterios tienen al menos un caso de prueba incluido.

