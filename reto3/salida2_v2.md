Criterio 1:
Dado que el curso ha alcanzado su cupo máximo de 25 estudiantes,
Cuando intento realizar una inscripción directa,
Entonces el sistema me notificará que no hay cupos disponibles y me colocará en la lista de espera.

Criterio 2:
Dado que estoy inscrito en dos cursos diferentes para el mismo periodo,
Cuando intente inscribirme en un tercer curso,
Entonces el sistema rechazará la inscripción indicando que el límite de 2 cursos por estudiante ya ha sido superado.

Criterio 3:
Dado que existe un curso con horarios cruzados respecto a mis cursos actuales,
Cuando intente inscribirme en ese nuevo curso,
Entonces el sistema bloqueará la inscripción para evitar conflictos de horario.

Criterio 4:
Dado que mi deuda financiera está vencida,
Cuando intente inscribirme en cualquier curso disponible,
Entonces el sistema impedirá la inscripción y mostrará un mensaje indicando que debo regularizar mi situación antes de poder participar.

Criterio 5:
Dado que el día actual es igual al inicio del curso menos tres días,
Cuando intente realizar una inscripción para ese curso,
Entonces el sistema me informará de que la inscripción ya ha cerrado y no será posible registrarme.

