# TEMA 7. ASPECTOS FUNCIONALES

## 1. Punteros a funciones en C

### Respuesta

Un puntero a función es una variable que almacena la dirección de memoria del código ejecutable de una función en lugar de almacenar datos. Permite parametrizar comportamientos, pasando funciones como argumentos a otras funciones o creando tablas de salto dinámicas.

```c
#include <stdio.h>
#include <ctype.h>

void aMayusculas(char* cadena) {
    for (int i = 0; cadena[i]; i++) {
        cadena[i] = toupper(cadena[i]);
    }
}

int main() {
    char texto[] = "hola mundo";
    // Declaración del puntero a función
    void (*pf)(char*) = aMayusculas;
    // Invocación a través del puntero
    pf(texto);
    printf("%s\n", texto);
    return 0;
}

```

---

## 2. Concepto de función lambda y ejemplos

### Respuesta

Una función lambda es una función anónima, es decir, una definición de función que no está vinculada a un identificador o nombre propio. Se utiliza para delimitar bloques de código que se ejecutan de manera inmediata o que se pasan directamente como argumentos a operaciones de orden superior, reduciendo la necesidad de declarar métodos formales innecesarios.

```javascript
// JavaScript: Función lambda asignada a variable
const aMayusculas = (cadena) => cadena.toUpperCase();
console.log(aMayusculas("hola"));

```

```java
// Java: Función lambda empleando la interfaz predefinida Function
import java.util.function.Function;

Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();
System.out.println(aMayusculas.apply("hola"));

```

---

## 3. Paradigma funcional, multi-paradigma y "ciudadanos de primera clase"

### Respuesta

El paradigma funcional es un modelo de desarrollo basado en la evaluación de funciones matemáticas, evitando el cambio de estado y la mutabilidad de los datos. A lenguajes como Java 8 se les denomina multi-paradigma porque logran unificar sus capacidades nativas orientadas a objetos con la expresividad y abstracción de la programación funcional dentro de una misma plataforma.

La expresión "ciudadanos de primera clase" indica que las funciones reciben el mismo tratamiento que cualquier otra entidad del lenguaje (como variables u objetos). Esto significa que las funciones pueden ser asignadas a variables locales, almacenadas dentro de estructuras de datos, pasadas como argumentos a otros métodos y devueltas como resultado de la ejecución de una función.

---

## 4. Sintaxis básica de una función lambda en Java

### Respuesta

La sintaxis fundamental de una función lambda en Java consta de tres componentes diferenciados: la lista de parámetros de entrada, el operador de flecha (`->`) y el cuerpo de la función. Se estructura como `(parámetros) -> { cuerpo }`.

Existen reglas de simplificación sintáctica aplicables según el escenario: si hay un único parámetro se pueden omitir los paréntesis; si el cuerpo consta de una única expresión, se prescinde de las llaves y de la palabra clave `return`. Si no hay parámetros se utilizan paréntesis vacíos `() -> ...`.

---

## 5. Pasar funciones como parámetros

### Respuesta

Para pasar una función como parámetro, el lenguaje receptor debe soportar tipos funcionales en la firma de sus métodos. En JavaScript esto se produce de forma nativa por su tipado dinámico, mientras que en Java se requiere especificar una interfaz que describa la firma esperada de la lambda.

```javascript
// JavaScript: Paso de función transformadora
const transformar = (cadena, funcionTransformadora) => funcionTransformadora(cadena);
console.log(transformar("hola", (s) => s.toUpperCase()));

```

```java
// Java: Uso de Function en la firma del método
import java.util.function.Function;

public class Ejemplo {
    public static String transformar(String cadena, Function<String, String> transformadora) {
        return transformadora.apply(cadena);
    }
}

```

---

## 6. Invocación directa con una lambda anónima (Inversión de cadena)

### Respuesta

Es factible omitir el paso intermedio de asignar la lambda a una variable local, insertando la definición algorítmica directamente en la sección de argumentos de la llamada al método.

```javascript
// JavaScript: Invocación directa invirtiendo la cadena
console.log(transformar("texto", s => s.split('').reverse().join('')));

```

```java
// Java: Invocación directa implementando la inversión de caracteres
String resultado = transformar("texto", s -> new StringBuilder(s).reverse().toString());
System.out.println(resultado);

```

---

## 7. Concepto de Cierre o "Closure"

### Respuesta

Un cierre o *closure* es una función que recuerda y accede al entorno léxico en el que fue creada, incluso cuando se ejecuta fuera de dicho entorno. Esto le permite capturar y retener el estado de variables locales externas que se encontraban en el ámbito circundante al momento de su definición.

En Java, para preservar la integridad de la memoria, se impone la restricción de que toda variable local capturada por una lambda debe ser explícitamente `final` o efectivamente final (no modificada tras su asignación).

```java
// Java: Captura de variable externa en una lambda
String sufijo = "!!!"; // Variable local en el entorno léxico
Function<String, String> concatenar = s -> s + sufijo; 

// La lambda accede a 'sufijo' cuando es invocada a través de transformar
System.out.println(transformar("Alerta", concatenar)); // Imprime: Alerta!!!

```

---

## 8. Diferencia entre función lambda y punteros a funciones en C

### Respuesta

La diferencia radica en que un puntero a función en C contiene únicamente la dirección física de un bloque de código estático y global, careciendo por completo de contexto. No puede retener variables locales del entorno donde fue referenciado; toda la información que procesa debe ser enviada explícitamente mediante parámetros.

Una función lambda en lenguajes modernos combina el código ejecutable con la capacidad de empaquetar y arrastrar variables locales de su entorno de origen (*closures*). Esto dota a la lambda de un estado contextual propio asociado al momento de su declaración, comportándose de manera similar a un objeto ligero.

---

## 9. Devolución de funciones (Funciones de descuento)

### Respuesta

Las funciones de orden superior son capaces de generar y retornar nuevas funciones adaptadas a partir de parámetros de configuración iniciales. La función devuelta almacena estos parámetros gracias a una *closure*.

```java
import java.util.function.Function;

public class GeneradorDescuentos {
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // 'porcentaje' queda capturado en la closure de la lambda devuelta
        return cantidad -> cantidad - (cantidad * (porcentaje / 100.0));
    }

    public static void main(String[] args) {
        Function<Double, Double> descuentoDiez = crearDescuento(10.0);
        Function<Double, Double> descuentoVeinte = crearDescuento(20.0);

        System.out.println(descuentoDiez.apply(100.0));  // 90.0
        System.out.println(descuentoVeinte.apply(100.0)); // 80.0
    }
}

```

La *closure* opera reteniendo el valor específico de `porcentaje` dentro del contexto cerrado de cada lambda generada. Aunque el método `crearDescuento` termine su ejecución y salga de la pila, la función devuelta mantiene acceso permanente a ese valor en memoria.

---

## 10. Interfaz funcional en Java

### Respuesta

Una interfaz funcional en Java es una interfaz convencional que declara exactamente **un único método abstracto**. Actúa como el soporte estructural para el sistema de tipos de las expresiones lambda bajo el borrado de tipos, dictando la firma exacta que debe cumplir el bloque anónimo.

Los requisitos obligatorios son poseer un solo método pendiente de implementación. Puede contener de forma opcional múltiples métodos por defecto (*default*) o métodos estáticos con cuerpo, ya que estos no rompen la condición de unicidad del contrato abstracto. Suele decorarse con la anotación `@FunctionalInterface` para forzar al compilador a validar dicha restricción.

---

## 11. Interfaz funcional personalizada: `Transformador`

### Respuesta

La creación manual de una interfaz funcional requiere definir un tipo estructurado que describa explícitamente los parámetros y retornos requeridos por la lógica de negocio.

```java
@FunctionalInterface
public interface Transformador {
    // Único método abstracto de la interfaz
    String transformarCadena(String entrada);
}

class UsoTransformador {
    public static void main(String[] args) {
        // Asignación de una expresión lambda a la interfaz propia
        Transformador t = s -> s.trim().toLowerCase();
        System.out.println(t.transformarCadena("  TEXTO  "));
    }
}

```

---

## 12. Interfaz funcional genérica

### Respuesta

Combinar interfaces funcionales con parámetros de tipo genéricos potencia la reutilización del componente, abstrayendo los tipos de datos de entrada y salida para cualquier escenario de conversión.

```java
@FunctionalInterface
public interface TransformadorGenerico<T, R> {
    R aplicar(T entrada);
}

class EjemploUso {
    public static void main(String[] args) {
        // Transformador que convierte un Double en un Integer redondeado
        TransformadorGenerico<Double, Integer> redondear = d -> (int) Math.round(d);
        Integer resultado = redondear.aplicar(5.75); // Retorna 6
    }
}

```

---

## 13. Interfaces funcionales predefinidas en Java

### Respuesta

Java provee en el paquete `java.util.function` un conjunto estándar de interfaces funcionales genéricas para cubrir las firmas y patrones de diseño estadísticamente más recurrentes en el desarrollo.

| Interfaz Funcional | Firma del Método | Descripción | Ejemplos de uso |
| --- | --- | --- | --- |
| `Function<T, R>` | `R apply(T t)` | Recibe un tipo T y retorna un tipo R | Conversión, mapeo de datos |
| `Predicate<T>` | `boolean test(T t)` | Recibe un tipo T y retorna un booleano | Filtros, validaciones |
| `Consumer<T>` | `void accept(T t)` | Recibe un tipo T y no retorna nada | Impresión, consumo de datos |
| `Supplier<T>` | `T get()` | No recibe nada y provee un tipo T | Factorías, generación perezosa |
| `BiFunction<T, R U,>` | `R apply(T t, U u)` | Recibe tipos T y U, retorna tipo R | Combinación de dos elementos |

---

## 14. Uso expresivo de funcional: `List.forEach`

### Respuesta

El método `forEach` integrado en las colecciones de Java 8 sustituye al bucle iterativo tradicional, aceptando una función lambda encargada de procesar individualmente cada componente de la estructura.

```java
import java.util.Arrays;
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-5, 3, 0, 12, -8, 7);

        // Uso de forEach con una lambda de tipo Consumer
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("Número positivo detectado: " + n);
            }
        });
    }
}

```

---

## 15. Genericidad y flexibilización en firmas: PECS

### Respuesta

El método `forEach` utiliza `Consumer<? super T>` en lugar de `Consumer<T>` para habilitar la flexibilidad en la herencia. Si se procesa una lista de enteros (`List<Integer>`), un consumidor capaz de procesar números en general (`Consumer<Number>`) es semánticamente apto para la tarea. Bloquear la firma a `Consumer<Integer>` impediría reutilizar lógica genérica de las superclases.

El acrónimo **PECS** (*Producer Extends, Consumer Super*) establece que las estructuras que producen o leen información usan `? extends`, mientras que las que consumen o escriben usan `? super`.

Aplicado a la firma del método `transformar` desarrollado anteriormente, la flexibilidad total se alcanza estructurando los límites genéricos del mapeador para albergar transformaciones compatibles con la jerarquía:

```java
// Aplicación de PECS: T actúa como productor y R como consumidor en la función
public static <T, R> R transformarPECS(T entrada, Function<? super T, ? extends R> transformadora) {
    return transformadora.apply(entrada);
}

```

---

## 16. Referencias a métodos

### Respuesta

Una referencia a método es una sintaxis compacta que permite apuntar directamente a un método existente por su nombre en lugar de escribir una expresión lambda explícita que lo invoque.

```javascript
// JavaScript: Referencia al método de una instancia concreta
class Persona {
    constructor(nombre) { this.nombre = nombre; }
    saludar() { return `Hola, soy ${this.nombre}`; }
}

const p = new Persona("Ana");
// Se guarda la referencia vinculando el contexto léxico correcto
const referenciaSaludar = p.saludar.bind(p);
console.log(referenciaSaludar());

```

```java
// Java: Referencia al método utilizando el operador '::'
import java.util. some. interface. o. Supplier; // Supongamos interfaz compatible

public class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    public String saludar() { return "Hola, soy " + nombre; }
}

// En el método principal:
Persona p = new Persona("Ana");
java.util.function.Supplier<String> ref = p::saludar; // Referencia de instancia
System.out.println(ref.get());

```

---

## 17. Tipos de referencias a métodos en Java

### Respuesta

Java clasifica las referencias a métodos en cuatro variantes bien diferenciadas según la naturaleza del destino invocado a través del operador `::`.

```java
import java.util.function.*;

public class EjemploReferencias {
    public static void main(String[] args) {
        // 1. Referencia a método estático
        Function<String, Integer> refEstatico = Integer::parseInt;

        // 2. Referencia a constructor
        Supplier<StringBuilder> refConstructor = StringBuilder::new;

        // 3. Referencia a método de instancia de una instancia concreta
        String texto = "origen";
        Predicate<String> refInstanciaConcreta = texto::equals;

        // 4. Referencia a método de instancia sobre una instancia arbitraria/cualquiera
        // El primer argumento implícito de la firma pasa a ser el objeto destino
        Function<String, String> refInstanciaArbitraria = String::toUpperCase;
    }
}

```

---

## 18. Ordenación de colecciones mediante Lambdas y `Comparator`

### Respuesta

La ordenación estructurada de elementos heterogéneos se puede realizar mediante funciones de comparación que resuelven los criterios primarios y secundarios de forma secuencial.

```java
import java.util.*;

public class OrdenarPersonas {
    static class Persona {
        String nombre; int edad;
        Persona(String n, int e) { this.nombre = n; this.edad = e; }
        public int getEdad() { return edad; }
        public String getNombre() { return nombre; }
    }

    public static void main(String[] args) {
        List<Persona> lista = Arrays.asList(
            new Persona("Carlos", 25), new Persona("Ana", 25), new Persona("Beatriz", 30)
        );

        // Versión 1: Función de comparación manual escrita explícitamente
        Collections.sort(lista, (p1, p2) -> {
            int compEdad = Integer.compare(p1.getEdad(), p2.getEdad());
            if (compEdad != 0) return compEdad;
            return p1.getNombre().compareTo(p2.getNombre());
        });

        // Versión 2: Composición encadenada empleando utilidades de Comparator
        lista.sort(Comparator.comparingInt(Persona::getEdad)
                             .thenComparing(Persona::getNombre));
    }
}
```</Integer></Number></Integer></T></T,></T></T></T></T,>

```
