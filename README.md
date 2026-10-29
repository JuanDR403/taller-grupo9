# Taller: ingeniería de prompts y de contexto · Grupo [N]

Diplomado en Ingeniería de Software con IA y Sistemas Agénticos · Sesión 03 · Módulo 1 · Fundación Escuela Tecnológica de Neiva (FET)

## 1. Integrantes

| Integrante | Nombre | Retos a cargo |
|---|---|---|
| 1 | Miguel Angel Medina Ibarra | Reto 2 |
| 2 | Juan David Rico Cerquera | Reto 3 y Reto 4 (Parte A) |
| 3 | Juan Pablo Macias Cortes | Reto 1, Reto 4 (Parte B) y este README |

> Ajusta la tabla a como se repartieron realmente los retos.

## 2. Caso elegido: InscribeFET

Sistema ficticio de la FET para la inscripción a cursos libres (inglés, deportes, artes). Reglas de negocio:

- Máximo 2 cursos por estudiante en cada periodo.
- Cupo máximo de 25 estudiantes por curso.
- No se pueden inscribir dos cursos con horarios cruzados.
- La inscripción cierra 3 días antes del inicio del curso.
- Los estudiantes con deuda financiera vencida no pueden inscribirse.

## 3. Modelos y equipo usados

**Modelos (con Ollama):**

| Modelo | Nombre en Ollama | Configuración | Uso |
|---|---|---|---|
| `qwen3.5:4b` | `qwen-demo` | temperatura 0, semilla 42, `--think=false` | Retos 1, 2 y 3 y Parte B del reto 4 |
| `phi4-mini` | `phi-demo` | sin `--think=false` | Reto 4, Parte A |

**Equipo:**

| Integrante | Equipo | Sistema operativo |
|---|---|---|
| 1 | [completar: procesador, RAM y GPU] | [completar] |
| 2 | Lenovo IdeaPad Gaming 3 (AMD Ryzen 5 4600H, NVIDIA GTX 1650 Ti, RAM: [completar]) | CachyOS Linux |
| 3 | [completar: procesador, RAM y GPU] | [completar] |

## 4. Estructura del repositorio

```
taller-grupoN/
├── README.md
├── reto1/   # prompt malo, prompt nuevo, variante, salidas y comparación
├── reto2/   # Modelfile v1 y v2, revisiones comparadas y tabla de ataques
├── reto3/   # cadena de 3 prompts (v1 y v2), salidas y nota_correccion.md
└── reto4/   # Parte A (phi-demo) y Parte B (contexto) con su explicación
```

Cada prompt está en un archivo `.txt` y cada salida en un `.md`. Las salidas con códigos de control del terminal se conservan sin cambios en archivos `_raw.md`, y la versión limpia lleva el nombre normal.

## 5. Resumen de resultados

| Reto | Qué se hizo | Hallazgo principal |
|---|---|---|
| 1 · Fórmula base y delimitadores | [completar] | [completar] |
| 2 · System prompt y resistencia | [completar] | [completar] |
| 3 · Del requisito a los casos de prueba | Cadena de tres prompts (few-shot, formato fijo, tabla con auto-verificación), repetida en una versión corregida | El error nació en el Prompt 1 (el modelo inventó una inscripción automática) y se propagó a los criterios y a los casos de prueba; un bloque `<restricciones>` lo eliminó. La tabla final aún tenía un caso que contradecía la regla del cierre |
| 4 · Parte A: cadena de pensamiento | Pregunta directa contra razonamiento paso a paso con `phi-demo` | El razonamiento paso a paso no mejoró la exactitud (el modelo se contradijo y concluyó mal), pero dejó evidencia para auditarlo |
| 4 · Parte B: contexto | [completar] | [completar] |
| 5 · Bonus | No desarrollado | No aplica |

Detalles en las carpetas `reto1` a `reto4`.

## 6. Cómo reproducirlo

```bash
ollama pull qwen3.5:4b
ollama pull phi4-mini
ollama create qwen-demo -f modelfiles/Modelfile.qwen-demo
ollama create phi-demo  -f modelfiles/Modelfile.phi-demo
ollama run qwen-demo --think=false < reto3/prompt1_fewshot.txt
```

Los `Modelfile` están en el repositorio de la clase (`fet-diplomadoIA-prompting-p2`).

## 7. Reflexión del grupo

**¿Qué técnica tuvo más efecto en nuestro caso?**
Las restricciones explícitas dentro de un bloque delimitado. En el reto 3, una sola corrección en el primer prompt eliminó la inscripción automática inventada y, con ella, todo lo que se había propagado a los criterios y a los casos de prueba. Corregir en el origen resultó más eficaz que arreglar cada salida posterior. [Completar con la técnica que más pesó en los retos 1, 2 y 4B, y con lo que el grupo decida.]

**Si mañana tuviéramos que pedirle a una herramienta de IA que programe una funcionalidad de nuestro sistema, ¿qué haríamos distinto?**
- Le daríamos las reglas de negocio dentro de un bloque delimitado, porque el modelo no las conoce.
- Pediríamos primero la historia de usuario, los criterios de aceptación y los casos de prueba, y solo después el código.
- Agregaríamos restricciones del tipo «no inventes reglas ni comportamientos» y revisaríamos la salida buscando lo que el modelo supuso por su cuenta.
- No confiaríamos en la auto-verificación ni en el razonamiento paso a paso del modelo sin comprobarlos contra el código o con pruebas, porque en nuestros experimentos el modelo se contradijo y no detectó sus propios errores.
- Guardaríamos prompts y salidas en archivos, para poder ver en qué paso apareció el primer error.

## 8. Notas

- Todas las comparaciones se hicieron con `qwen-demo` (temperatura 0, semilla 42), por lo que repetir el mismo prompt debe dar la misma salida.
- Una primera ejecución del reto 4A dio un archivo vacío porque el archivo del prompt no se había guardado; se corrigió y se repitió.
