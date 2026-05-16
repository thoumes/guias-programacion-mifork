# TEMA 6. GENERICIDAD

## 1. Estructura con `void*` (C) u `Object` (Java)

### Respuesta

Para almacenar cualquier tipo de dato sin genéricos, se recurre al puntero genérico `void*` en C o a la clase raíz `Object` en Java a través del polimorfismo. Esto permite que un único array primitivo apunte a cualquier dirección de memoria o instancia de clase.

```c
// C: Array de punteros opacos
typedef struct {
    void* datos[10];
    int tamano;
} ContenedorC;

```

```java
// Java: Array de la superclase común
public class ContenedorJava {
    private Object[] datos = new Object[10];
    public void agregar(Object elem) { /* ... */ }
}

```

---

## 2. Definición de Programación Genérica

### Respuesta

La programación genérica es un paradigma orientada a diseñar estructuras de datos y algoritmos abstractos que operan sobre **parámetros de tipo**, evitando duplicar código para cada tipo de dato diferente.

El ejemplo con `void*` u `Object` no es programación genérica real, sino un sustituto basado en la pérdida de información del tipo. La verdadera genericidad retiene el tipo exacto y garantiza la seguridad en tiempo de compilación.

---

## 3. Problemas de Chequeo de Tipos con `void*` / `Object`

### Respuesta

El principal problema es la anulación del chequeo de tipos en compilación, lo que traslada el riesgo de errores al tiempo de ejecución. El compilador no puede verificar si el elemento extraído es del tipo esperado.

Esto obliga a realizar conversiones explícitas (*downcasting*) propensas a fallos (como `ClassCastException` en Java). Además, permite introducir datos heterogéneos erróneos en el contenedor sin que el compilador emita ninguna advertencia.

---

## 4. Parámetros de Tipo

### Respuesta

Los parámetros de tipo son variables o marcadores de posición (*placeholders*), representados comúnmente con letras como `<T>` o `<E>`, que sustituyen a los tipos concretos en la declaración de clases o métodos.

Actúan como un contrato: el programador define la lógica de forma abstracta y, al instanciar la estructura, especifica el tipo real. El compilador valida entonces los accesos con total rigurosidad estática.

---

## 5. Ejemplo de Lista de Strings en C++ y Java

### Respuesta

Al parametrizar las colecciones de las bibliotecas estándar con el tipo `String`, el compilador restringe los elementos del contenedor y automatiza la extracción segura sin conversiones manuales.

```cpp
// C++: Plantilla std::vector
std::vector<std::string> lista;
lista.push_back("Texto");
for (const std::string& s : lista) { std::cout << s.length(); }

```

```java
// Java: Genérico List
List<String> lista = new ArrayList<>();
lista.add("Texto");
for (String s : lista) { System.out.println(s.length()); }

```

---

## 6. Funcionamiento Interno: C++ vs Java

### Respuesta

C++ utiliza **instanciación de plantillas**: el compilador genera código binario independiente y optimizado para cada tipo detectado (duplica código en el ejecutable, máximo rendimiento).

Java emplea **borrado de tipos** (*type erasure*): elimina los genéricos tras compilar y los sustituye por su límite (normalmente `Object`). Existe un solo archivo de clase y el compilador inyecta los *casts* automáticamente para mantener la compatibilidad hacia atrás.

---

## 7. Clase Genérica `Par` y Ejemplo de Retorno

### Respuesta

Se define una clase con dos parámetros de tipo independientes para almacenar y retornar pares de datos heterogéneos con tipado seguro.

```java
public class Par<F, S> {
    private final F primero; private final S segundo;
    public Par(F f, S s) { this.primero = f; this.segundo = s; }
    public F getPrimero() { return primero; }
    public S getSegundo() { return segundo; }
}

```

```java
// Uso en función estadística: retorna Media (Double) y Desviación (Double)
public static Par<Double, Double> analizar(double[] datos) {
    double media = 12.5; double desviacion = 1.2; // (Cálculos omitidos)
    return new Par<>(media, desviacion);
}
```</E></T>

```

## 8. Métodos genéricos: `seleccionaUno`

### Respuesta

Un método genérico declara parámetros de tipo locales antes de su tipo de retorno, permitiendo que la restricción de tipo se aplique solo a esa invocación.

```java
// Opción con Object (Insegura y heterogénea)
public static Object seleccionaObject(Object o1, Object o2) { return true ? o1 : o2; }

// Opción Genérica (Segura y homogénea)
public static <T> T seleccionaGenerico(T o1, T o2) { return true ? o1 : o2; }

```

La diferencia respecto a los dos criterios planteados es:

1. **Evitar downcasting:** La versión con `Object` devuelve una referencia abstracta que exige un cast explícito (`String s = (String) seleccionaObject(...)`). La versión genérica devuelve exactamente el tipo `T` de los argumentos, eliminando conversiones.
2. **Forzar homogeneidad:** La versión con `Object` permite mezclar tipos sin relación (un `String` y un `Integer`). La versión genérica con un único parámetro `<T>` obliga en compilación a que ambos objetos pertenezcan al mismo tipo o jerarquía compatible.

---

## 9. Parámetros acotados y la clase `Punto`

### Respuesta

Es posible restringir un parámetro de tipo mediante la cláusula `extends` (parámetros acotados). Al declarar `<T Number extends>`, se obliga a que el tipo sea la clase `Number` o una de sus subclases, permitiendo invocar métodos como `doubleValue()`.

```java
// Solución 1: Sin generics (Uso directo de Number)
public class PuntoNumber {
    private Number x, y;
    public PuntoNumber(Number x, Number y) { this.x = x; this.y = y; }
    public Number getX() { return x; }
    public double calcularDistanciaA(PuntoNumber o) {
        return Math.hypot(x.doubleValue() - o.x.doubleValue(), y.doubleValue() - o.y.doubleValue());
    }
}

```

```java
// Solución 2: Con generics (Parámetro acotado)
public class PuntoGenerico<T extends Number> {
    private T x, y;
    public PuntoGenerico(T x, T y) { this.x = x; this.y = y; }
    public T getX() { return x; }
    public double calcularDistanciaA(PuntoGenerico<T> o) {
        return Math.hypot(x.doubleValue() - o.x.doubleValue(), y.doubleValue() - o.y.doubleValue());
    }
}

```

Debido al proceso de **borrado de tipos** (*type erasure*), el parámetro genérico `<T Number extends>` se elimina en el bytecode compilado y se sustituye por su límite superior. Por lo tanto, el tipo final en los atributos y firmas tras la compilación es `Number` en ambas clases.

---

## 10. Reflexión sobre el chequeo de tipos en `Punto`

### Respuesta

La solución sin genéricos (`PuntoNumber`) sí permite crear un punto con tipos heterogéneos (X como `Integer` e Y como `Double`), ya que cada atributo acepta cualquier subtipo de `Number` de forma independiente. La solución genérica lo impide, obligando a que ambas coordenadas compartan el mismo tipo exacto (`T`) en la instancia.

Respecto al método `getX()`, la solución sin genéricos devuelve siempre un tipo abstracto `Number`, forzando al cliente a realizar un cast si necesita la clase concreta. La solución con genéricos devuelve el tipo exacto con el que se parametrizó la clase (por ejemplo, devuelve `Integer` en un `PuntoGenerico<Integer>`), aportando precisión y limpieza al código.

---

## 11. Ejemplo avanzado: Interfaz autorreferencial para `Punto`

### Respuesta

Para garantizar en compilación que un punto solo pueda calcular la distancia frente a otro punto de su misma clase exacta, se utiliza una estructura genérica autorreferencial. Esto elimina la necesidad de usar `instanceof` y conversiones forzadas de tipo.

```java
// Interfaz genérica autorreferencial
public interface Punto<P extends Punto<P>> { 
    public double distanciaA(P p); 
} 

// Implementación 2D acotada a sí misma
public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 
    public Punto2D(double x, double y) { this.x = x; this.y = y; } 

    @Override 
    public double distanciaA(Punto2D p2d) { 
        return Math.hypot(this.x - p2d.x, this.y - p2d.y); 
    } 
} 

// Implementación 3D acotada a sí misma
public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z; 
    public Punto3D(double x, double y, double z) { this.x = x; this.y = y; this.z = z; } 

    @Override 
    public double distanciaA(Punto3D p3d) { 
        return Math.sqrt(Math.pow(x-p3d.x, 2) + Math.pow(y-p3d.y, 2) + Math.pow(z-p3d.z, 2)); 
    } 
}

```

---

## 12. Covarianza, Contravarianza e Invarianza

### Respuesta

Una `List<String>` **no** es subtipo de `List<Object>`, ya que las colecciones genéricas son **invariantes** para evitar corrupciones de datos. Sin embargo, un array `String[]` **sí** es subtipo de `Object[]` porque los arrays en Java se diseñaron como **covariantes**.

La covarianza en arrays permite que el compilador acepte asignar un `String[]` a una referencia `Object[]`. El problema en tiempo de ejecución ocurre si se intenta guardar un entero en esa referencia (`array[0] = 10;`); el compilador no lo detecta, pero el programa falla inmediatamente lanzando una excepción `ArrayStoreException`.

Definiciones formales en base a una relación donde $A$ es subtipo de $B$:

* **Covariante:** Conserva la relación de tipos en el contenedor ($C<A>$ es subtipo de $C<B>$).
* **Contravariante:** Invierte la relación de tipos en el contenedor ($C<B>$ es subtipo de $C<A>$).
* **Invariante:** No existe ninguna relación entre los contenedores ($C<A>$ y $C<B>$ son independientes).

---

## 13. Uso controlado de Wildcards (`?`)

### Respuesta

Un *wildcard* o comodín (`?`) representa un tipo desconocido en Java. Permite flexibilizar la invarianza de los genéricos de forma segura mediante las reglas de lectura y escritura (*PECS: Producer Extends, Consumer Super*).

`List<? extends T>` (límite superior) recupera la covarianza y sirve para **lectura exclusiva** (productores), garantizando que todo elemento extraído deriva de `T`. `List<? super T>` (límite inferior) recupera la contravarianza y sirve para **escritura exclusiva** (consumidores), garantizando que el contenedor acepta elementos de tipo `T`.

```java
import java.util.List;

public class UtilidadesWildcards {
    // (i) Método con '? extends': Solo lee información numérica para sumar
    public static double calcularSuma(List<? extends Number> lista) {
        double suma = 0.0;
        for (Number n : lista) { suma += n.doubleValue(); }
        return suma;
    }

    // (ii) Método con '? super': Solo escribe enteros en la lista recibida
    public static void agregarEnteros(List<? super Integer> lista) {
        lista.add(10);
        lista.add(20);
    }
}
```</B></A></A></B></B></A></Object></String></Integer></T></T></T>

```
