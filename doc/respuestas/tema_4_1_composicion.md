# TEMA 4.1. COMPOSICION 
## 1. Composición en C con struct
### En el lenguaje C, la composición se logra anidando estructuras de datos. En este modelo, una estructura actúa como contenedor de otras, estableciendo una relación donde el "todo" conoce a sus "partes" de forma directa y estática.

    #include <stdio.h>
    #include <math.h>

    typedef struct {
        double x, y;
    } Punto;

    typedef struct {
        Punto p1;
        Punto p2;
    } Linea;

    double calcularDistancia(Punto a, Punto b) {
        return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
    }

    double calcularLongitud(Linea l) {
        return calcularDistancia(l.p1, l.p2);
    }

## 2. Composición en Java (OOP)
### Al trasladar el ejemplo a Java, se utiliza la encapsulación para proteger el estado de los objetos. Para garantizar la inmutabilidad, los atributos se declaran como private y final, y no se proporcionan métodos "setter", asegurando que una vez creada la Linea, sus coordenadas no cambien.

    public class Punto {
    
        private final double x;
        private final double y;

        public Punto(double x, double y) {
            this.x = x;
            this.y = y;
        }

        public double distanciaA(Punto otro) {
            return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
        }
    }

    public class Linea {
        private final Punto inicio;
        private final Punto fin;

        public Linea(Punto inicio, Punto fin) {
            this.inicio = inicio;
            this.fin = fin;
        }

        public double obtenerLongitud() {
            return inicio.distanciaA(fin);
        }
    }
## 3. Multiplicidad
### La multiplicidad define cuántas instancias de una clase pueden estar asociadas con una instancia de otra clase. Es un concepto fundamental para entender la estructura de la base de datos o el modelo de objetos que se está construyendo.En el ejemplo anterior, la multiplicidad se expresa así:De Linea a Punto: 1 a 2 ($1 \rightarrow 2$). Una línea está compuesta exactamente por dos puntos.De Punto a Linea: 1 a N ($1 \rightarrow 0..*$). Un punto puede pertenecer a ninguna, a una o a múltiples líneas diferentes en el sistema.

## 4. Composición Fuerte vs. Débil
### La composición fuerte (o simplemente composición) implica una dependencia total del ciclo de vida: si el objeto contenedor desaparece, sus partes también deben ser destruidas. Las partes no tienen sentido fuera del todo.La composición débil (conocida como agregación o asociación) permite que las partes existan de forma independiente al contenedor. Si el contenedor se destruye, los objetos que lo integraban pueden seguir existiendo y ser utilizados por otros elementos del programa.

## 5. Dependencia
### Cuando una clase utiliza a otra de forma temporal (como variable local en un método o como parámetro recibido), se habla de dependencia o uso. A diferencia de la composición, la clase utilizada no forma parte del "estado" permanente del objeto; es una relación de "usa-un" en lugar de "tiene-un".

## 6. Implementación de ciclos de vida
### En la composición fuerte, la Linea crea sus propios puntos (encapsulación total). En la débil, los recibe desde fuera. Composición FUERTE: La línea es dueña de la creación
    public Linea(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }

### // Composición DÉBIL: Los puntos existen antes y después de la línea
    public Linea(Punto p1, Punto p2) {
        this.inicio = p1;
        this.fin = p2;
    }
    
## 7. Destrucción de objetos en Java
### En Java no se observa una destrucción explícita de objetos (como el free de C) debido al Garbage Collector (GC). Cuando el objeto Linea deja de ser referenciado, sus puntos internos (en el caso de composición fuerte) también pierden su única referencia y el GC libera esa memoria automáticamente.

## 8. Ejemplo: Departamento y Profesores
### Se implementa la lógica de gestión de profesores con arrays fijos y validación de invariantes mediante excepciones.
    public class Departamento {
        private String nombre;
        private Profesor[] profesores = new Profesor[50];
        private int numProfesores = 0;
        private Profesor director;

        public Departamento(String nombre, Profesor directorInicial) {
            if (directorInicial == null) throw new IllegalArgumentException("El director no puede ser nulo");
            this.nombre = nombre;
            this.director = directorInicial;
            añadirProfesor(directorInicial);
        }

        public void añadirProfesor(Profesor p) {
            if (numProfesores >= 50) throw new IllegalStateException("Array lleno");
            profesores[numProfesores++] = p;
        }

        public void eliminarProfesor(int pos) {
            if (pos < 0 || pos >= numProfesores) throw new IndexOutOfBoundsException();
            if (profesores[pos] == director) throw new IllegalStateException("No se puede eliminar al director");
        
            for (int i = pos; i < numProfesores - 1; i++) {
                profesores[i] = profesores[i + 1];
            }
            profesores[--numProfesores] = null;
        }

        public void cambiarDirector(Profesor nuevoDirector) {
            boolean existe = false;
            for (int i = 0; i < numProfesores; i++) {
                if (profesores[i] == nuevoDirector) existe = true;
            }
            if (!existe) throw new IllegalArgumentException("El director debe ser del departamento");
            this.director = nuevoDirector;
        }
    
        public int getTotal() { return numProfesores; }
        public Profesor getProfesor(int pos) { return profesores[pos]; }
    }

## 9. Uso de List
### Al usar ArrayList, se elimina la necesidad de gestionar manualmente el tamaño del array, el contador de elementos y el desplazamiento de índices al eliminar. Se gana en seguridad y legibilidad de código.El problema de devolver la lista interna (return profesores;) es que se rompe la encapsulación: cualquier código externo podría añadir o borrar profesores sin pasar por las reglas de validación del Departamento. Se resuelve devolviendo una copia de la lista o una vista inmodificable (Collections.unmodifiableList).

## 10. Composición Recursiva
### Ocurre cuando un objeto contiene una referencia a otro objeto de su mismo tipo. Es la base de estructuras de datos como las listas enlazadas o los árboles.
    public class Persona {
        private final String nombre;
        private final Persona madre; // Composición recursiva

        public Persona(String nombre, Persona madre) {
            this.nombre = nombre;
            this.madre = madre;
        }

        public static void main(String[] args) {
            Persona abuela = new Persona("Ana", null);
            Persona madre = new Persona("Maria", abuela);
            Persona nieto = new Persona("Luis", madre);
        }
    }
### Otros ejemplos clásicos incluyen la estructura de carpetas y archivos (una carpeta contiene otras carpetas) o los comentarios en un foro (un comentario puede tener hilos de respuestas que son también comentarios).

## 11. Relaciones Bidireccionales
### Una relación es bidireccional cuando ambos objetos tienen una referencia al otro. Para implementarlo entre Profesor y Departamento, la clase Profesor debería tener un atributo private Departamento depto;.El reto principal es mantener la consistencia: si un profesor cambia de departamento, el departamento antiguo debe eliminarlo de su lista y el nuevo debe añadirlo. Esto suele requerir métodos que actualicen ambas referencias simultáneamente para evitar datos contradictorios.
