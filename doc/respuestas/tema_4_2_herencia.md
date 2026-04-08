## 1. La herencia y la relación "Es-un"

La **herencia** es un mecanismo fundamental de la orientación a objetos que permite definir una clase nueva a partir de una ya existente. Se establece una relación jerárquica donde la clase derivada (subclase) "es-una" instancia especializada de la clase base (superclase). Esta relación implica que cualquier operación o expectativa que se tenga sobre la superclase debe ser válida también para la subclase, permitiendo la reutilización de código y la creación de estructuras lógicas más complejas.

La **herencia de estado y comportamiento** significa que la subclase adquiere los atributos (estado) y los métodos (comportamiento) definidos en la superclase. Por otro lado, la **compatibilidad de tipos** permite que un objeto de la subclase sea tratado legalmente como un objeto de la superclase. Esto es vital en programación, ya que permite diseñar sistemas que operen con tipos genéricos (como `Soldado`) sin necesidad de conocer las particularidades de las implementaciones concretas (`Artillero` o `Zapador`).

```java
class Soldado {
    private String nombre;
    public Soldado(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Soy el soldado " + nombre); }
}

class Artillero extends Soldado {
    private int cohetes;
    public Artillero(String nombre, int cohetes) { super(nombre); this.cohetes = cohetes; }
    public int getCohetes() { return cohetes; }
}

class Zapador extends Soldado {
    private int minas;
    public Zapador(String nombre, int minas) { super(nombre); this.minas = minas; }
    public int getMinas() { return minas; }
}

// Ejemplo de compatibilidad de tipos
Soldado[] ejercito = { new Artillero("Ivan", 5), new Zapador("Marta", 10) };
for (Soldado s : ejercito) {
    s.saludar(); // Todos pueden saludar porque heredan el comportamiento
}
```

---

## 2. Ejecución de constructores y la palabra `super`

Al instanciar una subclase, se ejecutan siempre los constructores de toda la jerarquía, comenzando desde la clase raíz hasta la clase concreta. Esto garantiza que la "parte" del objeto que corresponde a la superclase esté correctamente inicializada antes de que la subclase añada o modifique su propio estado. En Java, el orden es estrictamente de arriba hacia abajo (de padre a hijos).

La palabra reservada `super` dentro de un constructor se utiliza para invocar explícitamente a un constructor de la superclase. Si la superclase no posee un constructor sin parámetros (o este no es visible), es **obligatorio** llamar a `super(...)` como la primera instrucción en el constructor de la subclase, pasando los argumentos necesarios. De no hacerlo, el compilador generará un error, ya que Java intenta insertar automáticamente una llamada a `super()` (sin parámetros) por defecto.

---

## 3. Atributos privados y memoria

Cuando se crea una instancia de una subclase, los atributos privados de la superclase **sí forman parte del objeto en memoria**. Un objeto de la subclase es una única entidad que reserva espacio para todos los atributos definidos en su jerarquía. Por tanto, el estado definido como `private` en la clase base reside físicamente dentro de cada instancia de la subclase, asegurando que el objeto esté completo.

Sin embargo, el hecho de que existan en memoria no implica que sean accesibles directamente desde el código de la subclase. El principio de **encapsulación** se mantiene: un `Zapador` no puede acceder directamente a la variable `nombre` de `Soldado` si esta es privada. Debe interactuar con ella a través de métodos públicos o protegidos de la superclase. Es importante distinguir entre la existencia física (memoria) y la visibilidad lógica (encapsulación).

---

## 4. Extensibilidad y compatibilidad de tipos

La compatibilidad de tipos fomenta la **extensibilidad** mediante el cumplimiento del principio de "abierto/cerrado": el código debe estar abierto a la extensión pero cerrado a la modificación. Al utilizar referencias de la superclase para gestionar colecciones de objetos, se pueden añadir nuevos tipos de datos al sistema sin alterar la lógica de control ya escrita. Esto reduce el riesgo de introducir errores en procesos críticos al escalar el software.

Por ejemplo, si se añade un `Medico`, este se integra perfectamente en el array de `Soldado` existente. El bucle que invoca el método `saludar()` no requiere ningún cambio, ya que el contrato de `Soldado` garantiza que cualquier nueva subclase sabrá cómo responder a ese mensaje.

```java
class Medico extends Soldado {
    public Medico(String nombre) { super(nombre); }
    // Hereda saludar() automáticamente
}

// El código de recorrido permanece idéntico:
Soldado[] ejercito = { new Artillero("Ivan", 5), new Medico("Elena") };
for (Soldado s : ejercito) { s.saludar(); } // Funciona sin tocar esta lógica
```

---

## 5. Referencias, Upcasting, Downcasting e `instanceof`

En Java, es perfectamente válido que una referencia de un supertipo apunte a un objeto de un subtipo (**upcasting**). Sin embargo, la referencia determina qué métodos son visibles: a través de una referencia de `Soldado`, solo se pueden invocar métodos definidos en la clase `Soldado`, aunque el objeto real sea un `Artillero`. El compilador solo permite llamar a lo que la referencia garantiza que existe.

El **upcasting** es automático y seguro. El **downcasting** es la conversión de una referencia de supertipo a una de subtipo y requiere un "cast" explícito, ya que es una operación arriesgada. Para evitar errores en tiempo de ejecución (`ClassCastException`), se utiliza el operador `instanceof`, que comprueba si un objeto pertenece a una clase determinada antes de realizar la conversión.

```java
for (Soldado s : ejercito) {
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // Downcasting
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```

---

## 6. Acceso protegido (`protected`)

El modificador de acceso `protected` es un nivel intermedio entre `public` y `private`. Permite que un miembro (método o atributo) sea accesible por la propia clase, por sus subclases (independientemente del paquete en el que estén) y por otras clases dentro del mismo paquete. Es la herramienta estándar para permitir que las subclases manipulen directamente partes controladas del estado del padre sin exponerlas al resto del mundo.

Se implementa utilizando la palabra clave `protected` en la declaración del miembro. En el caso del `Zapador`, si el atributo `nombre` en `Soldado` se define como protegido, el `Zapador` puede concatenar ese nombre directamente en sus propios métodos, facilitando la especialización del comportamiento sin violar totalmente la privacidad frente a clases externas.

```java
class Soldado {
    protected String nombre; // Accesible para hijos
}

class Zapador extends Soldado {
    public void ponerMina() {
        System.out.println(nombre + " está colocando una mina...");
    }
}
```

---

## 7. Clase base universal

En Java, existe una clase base universal llamada **`Object`**. Si una clase no especifica explícitamente de quién hereda mediante `extends`, Java hace que herede automáticamente de `java.lang.Object`. Esto garantiza que todos los objetos en Java compartan comportamientos básicos, como la capacidad de compararse (`equals`), convertirse a texto (`toString`) o clonarse.

No ocurre así en todos los lenguajes. En C++, por ejemplo, no existe una raíz única obligatoria para todas las clases; se pueden crear jerarquías de herencia totalmente independientes entre sí. La existencia de una clase raíz como en Java o C# facilita la creación de estructuras de datos genéricas que pueden almacenar cualquier tipo de objeto.

---

## 8. Herencia múltiple

La **herencia múltiple** es la capacidad de una clase de heredar estado y comportamiento de más de una superclase simultáneamente. Aunque teóricamente potente, introduce problemas de ambigüedad conocidos como el "problema del diamante" (cuando dos padres tienen métodos con el mismo nombre pero distinta implementación).

**Java no permite la herencia múltiple de clases**. Una clase solo puede tener un padre directo (`extends`). Esta decisión de diseño busca mantener el lenguaje simple y evitar los conflictos de ambigüedad. Para lograr objetivos similares de polimorfismo múltiple, Java utiliza **interfaces**, que permiten a una clase cumplir con múltiples contratos sin heredar implementación de estado de varios sitios.

---

## 9. Excepciones personalizadas y herencia

En Java, las excepciones son objetos que forman parte de una jerarquía cuya raíz es `Throwable`. Al crear una excepción personalizada, se suele heredar de `Exception` (controladas) o de `RuntimeException` (no controladas). Al ser objetos, pueden contener atributos adicionales para transportar información relevante sobre el error ocurrido hacia el bloque que lo captura.

A continuación, se presenta la implementación de una excepción no controlada que almacena el usuario implicado y permite la propagación de la causa original del error.

```java
class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() { return usuario; }
}
```

---

## 10. Reutilización de código: ¿Herencia o Composición?

Utilizar la herencia exclusivamente para evitar duplicar código se considera una mala práctica. La herencia es un vínculo muy fuerte y rígido; si se utiliza herencia donde no existe una relación semántica de "es-un", se acaba forzando a la subclase a cargar con comportamientos y contratos de la superclase que podrían no tener sentido para ella, complicando el mantenimiento a largo plazo.

Si el único objetivo es reutilizar una funcionalidad, la **composición** es preferible. Al componer, simplemente se incluye una instancia de la clase con el código deseado como un atributo. Esto evita exponer una jerarquía pública innecesaria y mantiene las clases centradas en su verdadera responsabilidad, siguiendo el principio de alta cohesión.

---

## 11. Favorecer la composición frente a la herencia

La máxima de "favorecer la composición" se basa en que la composición es mucho más flexible y dinámica que la herencia. Mientras que la herencia se define en tiempo de compilación (estática), la composición permite cambiar el comportamiento de un objeto en tiempo de ejecución simplemente sustituyendo la instancia del objeto compuesto por otra distinta.

Además, la composición reduce el acoplamiento. Un cambio en la superclase puede afectar a todas sus subclases de forma inesperada (efecto cascada), mientras que en la composición, la relación se limita a una interfaz o contrato específico. Esto facilita las pruebas unitarias y permite construir sistemas más modulares y resistentes al cambio.

---

## 12. La herencia rompe la encapsulación

Se afirma que la **herencia rompe la encapsulación** porque la subclase depende de los detalles de implementación de su superclase. Al heredar, la subclase se expone a los cambios internos del padre; si el padre altera el orden en que sus métodos se llaman entre sí o modifica su estado interno, la subclase podría dejar de funcionar correctamente aunque su propio código no haya cambiado.

Este fenómeno se denomina "el problema de la superclase frágil". La encapsulación busca ocultar los detalles internos, pero la herencia crea una relación tan íntima entre clases que la distinción entre lo que es privado y lo que es compartido se vuelve borrosa, obligando a los desarrolladores a conocer la implementación del padre para extenderlo con seguridad.

---

## 13. Comparativa: Herencia vs. Composición

El modelado mediante **herencia** enfatiza la identidad y la jerarquía. Un `Estudiante` se define intrínsecamente como una `Persona`. Es una solución elegante si todas las personas del sistema comparten una lógica común que nunca cambiará de forma independiente para estudiantes o trabajadores.

Por contra, el modelado mediante **composición** trata los datos personales como un componente que el objeto "tiene". Esto es más flexible: permite, por ejemplo, que un objeto cambie sus datos o que los comparta con otros módulos sin estar atado a una jerarquía rígida.

### Modelo con Herencia
```java
class Persona {
    protected String dni;
    protected String nombre;
}
class Estudiante extends Persona { /* ... */ }
class Trabajador extends Persona { /* ... */ }
```

### Modelo con Composición
```java
class DatosPersonales {
    private String dni, nombre;
    public DatosPersonales(String dni, String nombre) { ... }
}
class Estudiante {
    private DatosPersonales datos;
    public Estudiante(DatosPersonales datos) { this.datos = datos; }
}
class Trabajador {
    private DatosPersonales datos;
    public Trabajador(DatosPersonales datos) { this.datos = datos; }
}
```
