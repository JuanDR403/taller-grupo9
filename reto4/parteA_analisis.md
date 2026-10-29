# Reto 4 · Parte A: cadena de pensamiento (phi-demo)

## 1. Contexto

- **Caso:** InscribeFET (inscripción a cursos libres).
- **Modelo:** `phi-demo` (basado en `phi4-mini`), ejecutado **sin** `--think=false`, como pide el reto.
- **Archivos:**
  - Versión 1: `parteA_v1_directa.txt` → `salidaA_v1.md`
  - Versión 2: `parteA_v2_paso_a_paso.txt` → `salidaA_v2.md`
- Los dos prompts son idénticos (mismas reglas de negocio dentro de `<reglas>` y la misma función dentro de `<codigo>`). **Solo cambia la última instrucción**, de modo que la comparación mide una sola variable.

## 2. Qué se hizo

- **Versión 1 (directa):** se preguntó en una línea: «¿Por qué esta función deja pasar algún caso inválido?».
- **Versión 2 (paso a paso):** se pidió evaluar la función con datos concretos (un estudiante sin deuda con 2 cursos en el periodo y un curso con 25 inscritos), indicando para cada `if` su condición, su valor y lo que devuelve, antes de concluir, y después listar las reglas que la función no cumple.

La función evaluada es:

```python
def puede_inscribir(est, curso, inscritos):
    if est.deuda_vencida:
        return False
    if len(est.cursos_periodo) > 2:
        return False
    return inscritos < 26
```

## 3. Clave de corrección

**Errores reales de la función (4):**

1. `> 2`: un estudiante que ya tiene 2 cursos pasa el filtro y puede tomar un tercero (debería bloquearse con `>= 2`).
2. `< 26`: acepta al estudiante número 26, cuando el cupo máximo es 25.
3. No revisa el cruce de horarios.
4. No revisa el cierre de inscripción 3 días antes del inicio.

**Traza correcta con los datos de la versión 2** (sin deuda, 2 cursos, 25 inscritos):

1. `if est.deuda_vencida`: es `False`, no retorna.
2. `if len(est.cursos_periodo) > 2`: es `2 > 2`, `False`, no retorna.
3. `return inscritos < 26`: es `25 < 26`, `True`, **la función devuelve `True`**.

Es decir, con esos datos la función autoriza la inscripción aunque el estudiante ya tiene 2 cursos y el curso ya está en su cupo máximo.

## 4. Resultados

| Versión | Errores reales encontrados (de 4) | Afirmaciones falsas o inventadas | ¿Trazó bien cada `if`? |
|---|---|---|---|
| 1 · directa | **1** (cruce de horarios) | **3 falsas** (dice que el cupo no se verifica, define «lleno» como más de 25 y da por bueno el `> 2`) y **2 inventadas** (`inscritos[curso]` y `curso.cruzado_with(c)`) | No aplica |
| 2 · paso a paso | **2** (cruce de horarios y cierre, como reglas ausentes), pero **ninguno** de los dos errores de lógica (`> 2` y `< 26`) | **5 falsas** (pasos 1, 2 y 3, la conclusión y «no verifica el cupo») | **No**: falló en los tres pasos y en la conclusión |

## 5. Evidencia (fragmentos de las salidas)

**Versión 1**

- «la función no verifica si el curso ya está lleno (más de 25 estudiantes inscritos)». Es falso: la función sí compara el cupo con `inscritos < 26`. El defecto real es que deja entrar al estudiante 26, y el modelo además entiende «lleno» como «más de 25», cuando con un cupo de 25 el curso ya está lleno al llegar a 25.
- En el código que propone aparecen `inscritos[curso]` y `curso.cruzado_with(c)`. Ninguno existe en el problema: en la función `inscritos` es un número y el método `cruzado_with` no está definido en ninguna parte.
- Conserva `> 2` en su versión «corregida», así que no detecta el primer error.
- Respondió más de lo preguntado: se pedía **por qué** falla y reescribió la función.

**Versión 2**

- Paso 1: dice que `est.deuda_vencida` es `False` («el estudiante no tiene deuda») y, a la vez, que «devuelve `False`, lo que significa que el estudiante no puede inscribirse». Es una contradicción: si la condición del `if` es falsa, la función no retorna ahí.
- Paso 2: con `len = 2` afirma que devuelve `False` «porque ya tiene 2 cursos». Pero `2 > 2` es falso. El modelo aplicó la regla de negocio (máximo 2 cursos) en lugar de ejecutar el código, y con eso ocultó el error del operador.
- Paso 3: «No se proporciona el número de inscritos». Es falso: el propio modelo había escrito arriba «Curso (curso): Con 25 inscritos».
- Conclusión: «La función devuelve `False` en el primer `if`... debido a la deuda vencida». Contradice su propio paso 1 (no había deuda). La respuesta correcta es `True`.
- Señaló como reglas ausentes el cruce de horarios y el cierre de inscripción, pero dijo que «no se aplican aquí», cuando la omisión es un defecto de la función aunque estos datos no la activen.

## 6. Análisis

**Qué cambió entre las dos versiones.** La versión 2 nombró una regla ausente más (el cierre de inscripción), pero cometió bastantes más errores verificables. La versión 1 se equivocó en el diagnóstico (el cupo) e inventó código; la versión 2 se equivocó al ejecutar la función paso a paso y llegó a una conclusión opuesta a la correcta.

**Si ayudó pedir el razonamiento paso a paso.** No mejoró la exactitud: el modelo falló en la traza y en la conclusión. Lo que sí hizo fue **dejar evidencia para auditarlo**. En la versión 1 las afirmaciones falsas solo se descubren comparando con el código; en la versión 2 la contradicción entre «el valor es `False`» y «devuelve `False`» se ve en el propio texto, sin ejecutar nada. Pedir el razonamiento no hace que el modelo acierte, pero permite ver dónde se equivocó.

**Dónde falló el modelo y qué dice eso.**

- Evaluó la regla de negocio en lugar del código. Por eso no detectó que el error del límite está en el operador (`>` en lugar de `>=`), un tipo de error que un razonamiento «de memoria» suele ocultar.
- Perdió un dato que estaba en el propio prompt (los 25 inscritos). Esto se relaciona con el manejo del contexto, que es el tema del reto 4: un modelo pequeño no siempre usa toda la información que se le da.
- Dedujo una conclusión que contradecía sus pasos anteriores sin darse cuenta.

## 7. Conclusión

La cadena de pensamiento no sustituye la verificación. Sirve para auditar el razonamiento del modelo, pero su traza hay que contrastarla con el código o con pruebas. En este caso, una función de tres líneas y una traza de tres pasos bastaron para que el modelo se contradijera, lo que refuerza la idea del curso de verificar todo lo que genera una IA.

## 8. Notas

- La primera ejecución de la versión 2 produjo un archivo de salida vacío porque el archivo del prompt estaba vacío (no se había guardado). Se corrigió el prompt y se volvió a ejecutar. Fue un error de preparación del grupo y no un comportamiento del modelo.
- La Parte B del reto 4 (manejo de la ventana de contexto) la documenta el integrante 3 en un archivo aparte; la explicación final de `reto4/` integra ambas partes.
- Referencia: prácticas 9 y 14B de `GUIA_PRACTICAS.md`.
