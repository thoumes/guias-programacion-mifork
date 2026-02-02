# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Las cuatro características fundamentales son la abstracción, el encapsulamiento, la herencia y el polimorfismo. La abstracción permite enfocarse en las características esenciales de un objeto, ignorando los detalles complejos de implementación. El encapsulamiento agrupa los datos y los métodos que los manipulan, protegiéndolos del acceso directo exterior.

La herencia permite que una clase derive de otra, reutilizando y extendiendo su funcionalidad. Por último, el polimorfismo permite que diferentes clases respondan de forma distinta a una misma interfaz o llamada a un método, facilitando la flexibilidad y la extensibilidad del código.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Existen numerosos lenguajes que soportan este paradigma. Entre los más destacados se encuentran Java, diseñado bajo la premisa de "escribir una vez, ejecutar en cualquier lugar", y C++, que añade capacidades de objetos sobre la base de C que ya se conoce.

Otros dos lenguajes de gran relevancia son Python, conocido por su sintaxis limpia y su tipado dinámico, y C#, desarrollado por Microsoft y muy similar en estructura a Java. Todos ellos permiten definir clases y gestionar la memoria de diversas formas.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### La programación estructurada es el paradigma basado en el uso de estructuras de control (bucles, condicionales) y funciones, evitando el uso del salto incondicional (goto). Se centra en la lógica de control y en la descomposición de un problema en tareas jerárquicas simples.

La programación modular da un paso más al dividir el programa en módulos independientes y separables. En C, esto se asemeja a separar el código en distintos archivos .c y .h. Cada módulo tiene su propia responsabilidad y se comunica con los demás a través de interfaces bien definidas, mejorando la mantenibilidad.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Un objeto se define por tres elementos clave: estado, comportamiento e identidad. El estado está representado por los valores de sus atributos (variables) en un momento dado. El comportamiento se define mediante los métodos (funciones) que el objeto puede ejecutar.

La identidad es lo que distingue a un objeto de otro, incluso si sus estados son idénticos. En memoria, dos objetos pueden tener los mismos valores en sus variables, pero ocupan posiciones distintas, lo que garantiza que sean entidades únicas dentro del sistema.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Una clase es la plantilla o plano de diseño que define cómo serán los objetos; describe qué variables y métodos tendrán. Un objeto es la entidad concreta creada a partir de esa clase, mientras que el término instancia se usa para referirse al objeto en relación con su clase (un objeto es una instancia de una clase).

Aunque la mayoría de los lenguajes de POO modernos (como Java o C++) usan clases, no todos los lenguajes orientados a objetos las requieren. Existen lenguajes basados en prototipos, como JavaScript, donde los objetos se crean clonando o extendiendo otros objetos ya existentes sin necesidad de una clase formal.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### En Java, los objetos se almacenan siempre en el Heap (montón), mientras que las referencias a ellos residen en el Stack (pila). Esto difiere de C++, donde un objeto puede crearse tanto en el Stack como en el Heap dependiendo de la sintaxis utilizada.

La recolección de basura (Garbage Collection) es un proceso automático que identifica y libera la memoria ocupada por objetos que ya no tienen ninguna referencia activa. A diferencia de C, donde se usa free(), en Java el programador no libera la memoria manualmente, lo que reduce errores como los memory leaks.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Un método es una función que pertenece a una clase y define el comportamiento de sus instancias. Los métodos operan sobre los datos almacenados en los atributos del objeto, permitiendo interactuar con su estado de forma controlada.

La sobrecarga de métodos ocurre cuando una clase tiene dos o más métodos con el mismo nombre, pero con diferentes parámetros (distinto número o tipo). Es una forma de polimorfismo estático que permite realizar acciones similares con diferentes tipos de entrada bajo un mismo identificador.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### public class Punto {
    double x; // Visibilidad por defecto
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

// Ejemplo de uso
public class Principal {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3.0;
        p.y = 4.0;
        System.out.println("Distancia: " + p.calculaDistanciaAOrigen());
    }
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### El punto de entrada en Java es el método public static void main(String[] args). La palabra clave static indica que el método o atributo pertenece a la clase en sí y no a una instancia específica, por lo que puede llamarse sin crear un objeto. No es exclusivo de main; se usa para constantes o funciones de utilidad (como en la clase Math).

Cuando static se combina con final, se crean constantes de clase. final impide que el valor de una variable sea modificado una vez asignado. Es el equivalente funcional a los #define o a const en C, pero con una estructura más robusta y tipada.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Java es un lenguaje híbrido: se compila a un lenguaje intermedio llamado byte-code (ficheros .class) mediante el comando javac. Este byte-code no es código máquina nativo, sino instrucciones para la Máquina Virtual de Java (JVM).

Para ejecutarlo se usa el comando java, que arranca la JVM y esta interpreta o traduce el byte-code a código máquina en tiempo de ejecución. Esto permite que el mismo archivo .class funcione en cualquier sistema operativo que tenga instalada una JVM compatible.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### La palabra clave new se encarga de reservar memoria en el Heap para un nuevo objeto y devolver su referencia. Un constructor es un método especial que se invoca automáticamente al usar new; su función es inicializar el estado del objeto y no tiene tipo de retorno.

public class Empleado {
    String dni, nombre, apellidos;

    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### La palabra clave this es una referencia que el objeto utiliza para referirse a sí mismo. Se usa principalmente para evitar ambigüedades cuando los nombres de los parámetros de un método coinciden con los nombres de los atributos de la clase.

En otros lenguajes puede variar: en Python se llama explícitamente self, y en C++ es un puntero llamado this. En Java, this.x = x; indica que el valor del parámetro x debe asignarse al atributo x del objeto actual.


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### En Java, todo se pasa por valor. Sin embargo, cuando se pasa un objeto, lo que se copia es la referencia (la dirección de memoria). Por tanto, si se modifican los atributos del objeto dentro del método, los cambios persisten fuera, ya que ambos apuntan al mismo objeto en el Heap.

Si se pasa un tipo primitivo como un int, se crea una copia local del valor. Cualquier modificación interna al int no afectará a la variable original, de forma idéntica a como funciona el paso por valor de una variable normal en C.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### El método toString() devuelve una representación en cadena de texto de un objeto. Existe en casi todos los lenguajes modernos (como __str__ en Python). En Java, todos los objetos lo heredan, pero es recomendable sobrescribirlo para mostrar información útil.

Java
public String toString() {
    return "Punto(x=" + x + ", y=" + y + ")";
}


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Una clase es conceptualmente similar a un struct de C, ya que ambos agrupan datos. Sin embargo, al struct le falta la capacidad de contener comportamiento (métodos) y mecanismos de ocultación de datos (visibilidad).

Para que un struct fuera una clase, debería permitir incluir funciones dentro de su definición y gestionar automáticamente una referencia al dato que se está manipulando (this). En C, los datos y las funciones están separados; en POO, están unidos indisolublemente.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Para emular una clase en C, se definen funciones que reciben un puntero a la estructura como primer argumento. Este puntero manual es el equivalente funcional de this.

C
struct Punto {
    double x, y;
};

double calculaDistanciaAOrigen(struct Punto *p) {
    return sqrt(p->x * p->x + p->y * p->y);
}
Al llamar a la función, se debe pasar explícitamente la dirección de la estructura (calculaDistanciaAOrigen(&miPunto)). En la POO, este proceso ocurre de forma implícita y transparente para el programador.
