1. Encapsulación y ocultación de información
La encapsulación es el mecanismo que agrupa datos y comportamientos (métodos) en una sola unidad o contenedor, es decir, la clase. Por otro lado, la ocultación de información es la capacidad de restringir el acceso a ciertos componentes de ese objeto, de modo que los detalles internos de su implementación no sean visibles ni modificables directamente desde el exterior.

Entre las ventajas de la ocultación se encuentran la facilidad de mantenimiento, ya que se puede cambiar la implementación interna sin afectar a quienes usan la clase; la protección contra errores, al evitar que se asignen valores inválidos a los datos; y la reducción de la complejidad, puesto que el usuario solo necesita conocer qué hace el objeto y no cómo lo hace por dentro.

2. Interfaz pública
La interfaz pública de una clase es el conjunto de métodos y constantes que se ponen a disposición de otros objetos para interactuar con ella. En términos prácticos, es el "contrato" que la clase firma con el exterior, especificando qué servicios ofrece. En Java, se define principalmente mediante los miembros marcados con el modificador public.

La relación con la ocultación es directa: la interfaz pública actúa como un filtro o frontera. Mientras que la ocultación mantiene los detalles escabrosos y los datos sensibles en el ámbito privado, la interfaz pública expone solo lo estrictamente necesario para que el objeto sea funcional y útil para los demás.

3. Diseño de la interfaz pública
Se debe diseñar la interfaz pública con extremo cuidado porque, una vez que otros programadores o módulos empiezan a usarla, cualquier cambio en ella (como cambiar el nombre de un método o sus parámetros) romperá todo el código que dependa de esa clase. Es mucho más difícil de cambiar que la implementación interna, la cual puede alterarse libremente siempre que el resultado final sea el mismo.

Una buena interfaz pública debe ser mínima y completa: debe ofrecer todo lo necesario para usar la clase, pero nada más. Si se exponen demasiados detalles, se pierde flexibilidad futura, ya que se está "atado" a mantener elementos que quizás más adelante se prefiera ocultar o modificar.

4. Invariantes de clase
Las invariantes de clase son condiciones o reglas que deben cumplirse siempre para que un objeto se considere en un "estado válido". Por ejemplo, en una clase Fecha, una invariante sería que el mes siempre esté entre 1 y 12. Si los atributos fueran públicos (como en un struct de C convencional), cualquier parte del programa podría romper estas reglas accidentalmente.

La ocultación de información ayuda a preservar estas invariantes al obligar a que cualquier modificación de los datos pase por métodos controlados (como los constructores o setters). Dentro de estos métodos, se puede validar la información antes de actualizar el estado interno, garantizando que el objeto nunca sea inconsistente.

5. Ejemplo de clase Punto con ocultación
En el siguiente ejemplo, los atributos son privados y la interacción se realiza mediante métodos públicos.
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
La interfaz pública de esta clase está compuesta por el constructor Punto(double, double) y el método calcularDistanciaAOrigen(). El modificador private impide el acceso desde fuera de la clase, mientras que public permite el acceso desde cualquier otra parte del programa.

6. Aplicación de public y private
En Java, los modificadores public y private se pueden aplicar a los miembros de la clase, que incluyen atributos (variables de instancia o de clase), métodos y constructores. También se pueden aplicar a las clases de primer nivel, aunque con ciertas reglas: una clase solo puede ser public o tener visibilidad de paquete (sin modificador).

No es posible aplicar estos modificadores a las variables locales (las que se declaran dentro de un método), ya que su ámbito está limitado intrínsecamente a la ejecución de dicho bloque de código y no forman parte de la estructura de visibilidad de la clase.

7. Otros tipos de visibilidad
Además de public y private, Java ofrece el nivel de acceso por defecto (o package-private), que se aplica cuando no se escribe ningún modificador y permite el acceso solo a clases dentro del mismo paquete. También existe protected, que permite el acceso a clases del mismo paquete y a subclases (herencia), incluso si están en paquetes distintos.

En otros lenguajes como C++, existen conceptos similares, aunque la forma de declararlos varía (se suelen agrupar por bloques public: o private:). Otros lenguajes como Python no tienen una restricción real de acceso, sino que emplean convenciones (como el uso de guiones bajos _) para indicar que un miembro debería tratarse como privado.

8. Privacidad entre instancias
Los miembros privados de un objeto están ocultos para (a) otras clases, pero no para otras instancias de la misma clase. En Java, un objeto de la clase Punto puede acceder a los atributos privados de otro objeto de la clase Punto. Esto es fundamental para operaciones que involucran a dos entidades del mismo tipo.
public double calcularDistanciaAPunto(Punto otro) {
    // Es legal acceder a 'otro.x' aunque sea privado porque estamos dentro de la clase Punto
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
Esta característica permite que los objetos de la misma clase colaboren de manera eficiente sin necesidad de exponer sus datos al resto del mundo.
9. Métodos Getter y Setter
Los getters y setters (también llamados métodos de acceso y de modificación) son métodos públicos diseñados para leer o actualizar el valor de un atributo privado. El getter suele tener el formato getNombreAtributo() y devuelve el valor, mientras que el setter suele ser setNombreAtributo(valor) y lo asigna.

El uso de estos métodos permite añadir lógica adicional, como validaciones o transformaciones, sin cambiar la forma en que el mundo exterior ve al objeto. También permiten crear atributos de "solo lectura" si se proporciona un getter pero no un setter.

10. La "seguridad" en la ocultación
Cuando se habla de seguridad en este contexto, no se refiere a la protección contra ataques cibernéticos o hacking externo, sino a la integridad técnica del programa. Se trata de "seguridad frente al programador", evitando que un uso incorrecto de los objetos corrompa el estado de la aplicación.

Se busca prevenir que un módulo A modifique datos críticos del módulo B de forma imprevista, lo que causaría errores difíciles de depurar (efectos secundarios). Es, por tanto, una medida de robustez estructural y fiabilidad del software.

11. Miembro de instancia vs. de clase
Un miembro de instancia es una variable o método que pertenece a cada objeto individual; cada instancia tiene su propia copia de los datos. Un miembro de clase (marcado con static) es único para toda la clase; todos los objetos comparten el mismo valor y la misma posición de memoria para ese miembro.

Los miembros de clase también pueden y deben ser ocultados. Por ejemplo, se puede tener un contador private static int totalObjetos que solo sea modificable internamente por el constructor de la clase, impidiendo que el exterior altere la cuenta global de forma arbitraria.

12. Constructores privados
Tener constructores privados es una técnica común y útil en POO. Se utiliza principalmente cuando se quiere impedir que el exterior cree instancias de la clase libremente. Esto ocurre en clases que solo contienen métodos de utilidad estáticos (como Math en Java) o en patrones de diseño específicos.

También se emplean constructores privados para obligar a los usuarios a utilizar métodos factoría, los cuales pueden decidir si devuelven un objeto nuevo, uno reciclado o incluso una subclase, proporcionando un control total sobre el proceso de instanciación.

13. Miembros de clase en Java
En Java se utiliza la palabra clave static para indicar que un miembro pertenece a la clase.
public class Punto {
    private double x, y;
    private static double xMax = 0;
    private static double yMax = 0;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > xMax) xMax = x;
        if (y > yMax) yMax = y;
    }
}
En este ejemplo, xMax y yMax son compartidos por todos los puntos. Si se crean mil instancias de Punto, solo existirá una variable xMax en memoria que mantendrá el valor más alto registrado hasta el momento.

14. Método factoría
Un método factoría es un método estático que encapsula la creación de un objeto.
public static Punto crearPuntoRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
Se utiliza static porque el método debe poder llamarse antes de que exista el objeto Punto que se va a crear. Se invoca como Punto.crearPuntoRedondeado(2.8, 4.1).

15. Cambio de implementación interna
Gracias a la encapsulación, se puede cambiar el almacenamiento interno sin que el usuario de la clase lo note.
public class Punto {
    private double[] coords = new double[2]; // Cambio interno

    public Punto(double x, double y) {
        this.coords[0] = x;
        this.coords[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }
    // El resto del programa no necesita saber que 'x' e 'y' ya no existen como variables independientes
}

16. Atributos públicos vs. privados
Aunque un atributo tenga getter y setter, es preferible mantenerlo privado. Declararlo público elimina la posibilidad de añadir lógica de validación futura o de cambiar la representación interna (como se vio en la pregunta anterior) sin romper el código ajeno.

La convención habitual es que todos los atributos sean privados. Esto protege las invariantes de clase, ya que garantiza que ninguna modificación de los datos ocurra sin la supervisión de los métodos de la clase, manteniendo la coherencia del objeto en todo momento.

17. Clases inmutables
Una clase es inmutable si su estado no puede cambiar después de haber sido creada. Un método modificador es aquel que altera el estado del objeto. No siempre es un setter (por ejemplo, un método desplazar(dx, dy) es modificador), pero en una clase inmutable, tales métodos no existen o devuelven un nuevo objeto con el cambio aplicado en lugar de modificar el original.

Las ventajas de la inmutabilidad incluyen la seguridad en entornos multihilo (varias partes del programa pueden leer el objeto sin riesgo de que cambie) y la simplicidad, ya que se sabe que el objeto siempre mantendrá el estado con el que fue construido.

18. Uso de Setters por convención
No es recomendable incluir setters siempre por defecto. Si una clase puede ser inmutable, suele ser mejor diseño que lo sea. Añadir setters innecesarios aumenta la superficie de contacto de la clase y permite que los objetos sean modificados en lugares imprevistos, lo que complica el razonamiento sobre la lógica del programa.

Se deben incluir setters solo cuando el dominio del problema realmente requiera que el objeto cambie de estado a lo largo de su vida. En muchos casos, es preferible crear un objeto nuevo con los nuevos valores.

19. La clase String en Java
La clase String en Java es inmutable. Cuando se concatenan dos cadenas (por ejemplo, con +), no se modifica ninguna de las originales, sino que se crea un objeto String completamente nuevo en memoria que contiene el resultado.

Si se van a realizar muchas concatenaciones en un bucle, el uso de String es ineficiente porque se generan miles de objetos intermedios desechables. En esos casos, se debe utilizar la clase StringBuilder (o StringBuffer), que está diseñada específicamente para construir cadenas de forma mutable y eficiente antes de convertirlas al String final.

20. Comparación de objetos e identidad
En POO, la comparación mediante == suele comprobar la identidad (si son el mismo objeto en memoria, es decir, la misma dirección de puntero). Para comparar el contenido (si dos objetos distintos tienen los mismos valores), se utiliza el método equals().

Por defecto, equals() en Java se comporta igual que ==, pero muchas clases (como String o Integer) lo sobrescriben para comparar el valor real. Para comparar dos cadenas en Java, siempre se debe usar cadena1.equals(cadena2) y nunca ==, ya que esto último podría fallar incluso si el texto es idéntico.

21. Clases Wrapper (Envoltorios)
Las clases wrapper son clases que "envuelven" un tipo de dato primitivo (como int, double) para tratarlo como un objeto. En Java existen Integer, Double, Boolean, etc. Esto es necesario porque muchas estructuras de datos en Java solo aceptan objetos y no tipos primitivos.

Java realiza este proceso automáticamente mediante el autoboxing y unboxing. La ventaja es que permiten usar nulos (null) y proporcionan métodos de utilidad (como convertir texto a número). No todos los lenguajes los necesitan; en Smalltalk o Ruby, "todo es un objeto" desde el principio, por lo que no existen tipos primitivos separados.

22. Tipos de datos enumerados
Un enumerado (enum) es un tipo de dato que define un conjunto fijo de constantes con nombre. En Java, los enumerados son mucho más potentes que en C; son clases especiales que pueden tener atributos, métodos y constructores.

En términos de encapsulación, los enum de Java son excelentes porque garantizan que solo existan las instancias definidas en la propia clase, impidiendo que el usuario cree valores inválidos. Además, permiten asociar lógica directamente a las constantes, evitando largas estructuras switch repartidas por el código.

23. Ejemplo de Enumerado Mes en Java
24. public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private final int dias;
    private final int ordinal;

    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() { return dias; }
    public int getOrdinal() { return ordinal; }

    public boolean esDePrimavera(boolean norte) {
        if (norte) return this == MARZO || this == ABRIL || this == MAYO;
        return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
    }
    
    // El resto de métodos (verano, otoño, invierno) seguirían una lógica similar
}
