Aquí tienes un ejemplo completo y funcional de cómo crear una función para **inscribir estudiantes**.

Dado que no especificaste el lenguaje de programación, he preparado dos versiones muy comunes:
1.  **Python**: Ideal para lógica pura y scripts.
2.  **JavaScript (Node.js)**: Ideal si estás trabajando con bases de datos o APIs.

---

### Opción 1: En Python (Lógica Pura)

Esta función simula el proceso de inscripción verificando que los datos sean válidos antes de agregar al grupo.

```python
class Estudiante:
    def __init__(self, nombre, edad, curso):
        self.nombre = nombre
        self.edad = edad
        self.curso = curso

class Escuela:
    def __init__(self):
        self.estudiantes_inscritos = []

    def inscribir_estudiante(self, nombre, edad, curso):
        """
        Función para inscribir un estudiante.
        Devuelve True si se inscribió correctamente, False si hay errores.
        """
        # 1. Validaciones básicas
        if not nombre or not isinstance(nombre, str):
            print("Error: El nombre debe ser una cadena de texto no vacía.")
            return False
        
        if edad < 5 or edad > 100:
            print(f"Error: La edad {edad} no es válida para un estudiante.")
            return False
            
        if curso not in ["Primaria", "Secundaria", "Preparatoria"]:
            print(f"Error: El curso '{curso}' no existe. Opciones: Primaria, Secundaria, Preparatoria.")
            return False

        # 2. Crear objeto estudiante
        nuevo_estudiante = Estudiante(nombre, edad, curso)
        
        # 3. Agregar a la lista de inscritos
        self.estudiantes_inscritos.append(nuevo_estudiante)
        
        print(f"¡Éxito! {nombre} ha sido inscrito en {curso}.")
        return True

# --- Uso del ejemplo ---
mi_escuela = Escuela()

# Intento 1: Inscripción válida
mi_escuela.inscribir_estudiante("Ana García", 12, "Secundaria")

# Intento 2: Edad inválida (simulando error)
mi_escuela.inscribir_estudiante("Carlos Pérez", 4, "Primaria")

# Intento 3: Curso inválido
mi_escuela.inscribir_estudiante("Lucía Méndez", 15, "Ingeniería")
```

---

### Opción 2: En JavaScript (Con Base de Datos Simulada)

Esta versión incluye manejo de errores y validación de tipos, útil si estás construyendo una aplicación web o backend.

```javascript
class Escuela {
    constructor() {
        this.estudiantes = []; // Array para guardar los datos
    }

    inscribirEstudiante(nombre, edad, curso) {
        // 1. Validaciones
        if (!nombre || typeof nombre !== 'string') {
            console.error("❌ Error: El nombre es inválido.");
            return false;
        }

        if (typeof edad !== 'number' || edad < 0 || edad