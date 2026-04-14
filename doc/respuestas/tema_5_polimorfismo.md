# TEMA 5. POLIMORFISMO

## 1. Polimorfismo y Sobreescritura
**Polimorfismo:** Capacidad de los objetos de diferentes subclases de responder a una misma interfaz o llamada a un método de manera específica según su tipo real. Sirve para escribir código genérico que pueda manipular objetos de una jerarquía de herencia sin conocer su implementación exacta, facilitando la escalabilidad del software.

**Sobreescritura (*Overriding*):** Mecanismo que permite a una subclase proporcionar una implementación específica de un método que ya está definido en su clase superior. Para que sea efectiva, el método debe conservar la misma firma (nombre y parámetros), permitiendo que el comportamiento de la clase hija prevalezca sobre el de la clase padre.

## 2. Ligadura dinámica y Enlace tardío
Consiste en el proceso por el cual la resolución de qué método ejecutar se pospone hasta el tiempo de ejecución, en lugar de decidirse durante la compilación. Está intrínsecamente ligada al polimorfismo, ya que permite que el entorno de ejecución identifique el tipo real del objeto al que apunta una referencia y ejecute el código correspondiente a su clase específica.

En C++, este comportamiento debe indicarse explícitamente mediante la palabra clave `virtual` en la clase base; de lo contrario, se aplica ligadura estática. En Java, la ligadura dinámica es el comportamiento predeterminado para todos los métodos no estáticos ni finales. Python, al ser un lenguaje dinámico, utiliza enlace tardío de forma nativa para todas sus llamadas a métodos sin necesidad de declaraciones especiales.

## 3. Implementación de Polimorfismo en Java
Ejemplo práctico utilizando una jerarquía de combate donde el comportamiento varía según el tipo de unidad instanciada:

```java
class Soldado {
    public void saludar() { System.out.println("Soldado presente."); }
}

class Zapador extends Soldado {
    @Override
    public void saludar() { System.out.println("Zapador despejando camino."); }
}

class Artillero extends Soldado { } // Hereda saludo base

// Uso polimórfico
Soldado[] ejercito = { new Zapador(), new Artillero() };
for (Soldado s : ejercito) { s.saludar(); }
```

## 4. Invocación de métodos de la clase base
Se refiere a la capacidad de una subclase de ejecutar la lógica original del padre antes o después de añadir su propio comportamiento. Esto permite la reutilización de código en lugar de la sustitución total, permitiendo que la subclase complemente la funcionalidad heredada de forma incremental.

Para lograr esto en Java, se utiliza la palabra clave `super`. En el caso del `Zapador`, se invocaría como `super.saludar()`, lo que permite ejecutar primero el saludo estándar del `Soldado` y posteriormente imprimir el mensaje específico de su unidad.

## 5. Restricciones, Sobrecarga y Anotación @Override
La sobreescritura exige mantener la firma exacta del método y permite tipos de retorno covariantes (un subtipo del original). Se diferencia de la sobrecarga (*overloading*) en que esta última ocurre cuando existen varios métodos con el mismo nombre pero distintos parámetros en una misma clase, resolviéndose en tiempo de compilación y no por polimorfismo.

La anotación `@Override` actúa como una directiva para el compilador que valida si realmente se está sobreescrito un método de la superclase. Es recomendable usarla siempre para evitar errores lógicos, como escribir mal el nombre del método o errar en los parámetros, lo que crearía un método nuevo por error en lugar de sobreescribir el existente.

## 6. Polimorfismo implícito en Java
El uso de polimorfismo es constante en Java debido a que todas las clases derivan de `Object`. Al redefinir métodos estándar como `toString()` o `equals()`, se está aplicando polimorfismo, ya que muchas funciones del sistema (como la impresión por consola) llaman a estos métodos a través de referencias de tipo `Object`, ejecutando la versión específica de nuestra clase.

## 7. Clases y Métodos Abstractos
**Clase Abstracta:** Clase que no puede ser instanciada y actúa como un concepto genérico o base para una jerarquía. Se define con la palabra clave `abstract` y su propósito es obligar a que otras clases hereden de ella y completen su funcionalidad.

**Método Abstracto:** Declaración de un método sin implementación (sin cuerpo `{ }`) precedida por `abstract`. Siguiendo el ejemplo del `Soldado`, se definiría `public abstract void atacar();` en la clase base, forzando a que cada tipo de soldado implemente su propia forma de combate obligatoriamente.

## 8. Palabra clave `final`
Este modificador restringe la extensibilidad: una clase `final` no puede tener subclases y un método `final` no puede ser sobreescrito. Su relación con el polimorfismo es prohibitiva, utilizándose para garantizar que el comportamiento de ciertos componentes sea inmutable por razones de seguridad o diseño arquitectónico.

Un ejemplo clásico en la API de Java es la clase `String`, la cual es `final`. Esto impide que se herede de ella y se alteren operaciones fundamentales del manejo de texto, asegurando la consistencia de las cadenas de caracteres en todas las librerías del lenguaje.

## 9. Interfaces en Java
Son estructuras que definen un contrato de comportamiento puro sin incluir implementación ni estado (atributos). A diferencia de las clases abstractas, una clase en Java puede implementar múltiples interfaces simultáneamente, lo que permite que un objeto pertenezca a varios tipos polimórficos sin estar limitado a una única línea de herencia.

## 10. Polimorfismo con Downcasting e Instanceof
Técnica utilizada para recuperar la identidad específica de un objeto cuando se maneja a través de una referencia genérica. El operador `instanceof` verifica si el objeto pertenece a un tipo determinado, y el *downcasting* realiza la conversión de tipo para acceder a métodos que no están presentes en la clase base.

```java
abstract class Punto { public abstract double calcularDistanciaA(Punto otro); }

class Punto2D extends Punto {
    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p2 = (Punto2D) otro; // Downcasting seguro
            // Lógica de cálculo 2D...
        }
        return 0;
    }
}
```

## 11. Herencia de interfaces
Mecanismo que permite a una interfaz extender otra interfaz mediante `extends`, heredando sus requisitos. En este ámbito, Java sí permite la herencia múltiple, por lo que una interfaz puede derivar de varias simultáneamente. Por ejemplo, una interfaz `FicheroEscribible` puede extender `Fichero`, obligando a cualquier clase implementadora a proporcionar tanto los métodos de lectura como los de escritura y borrado.
