1. Encapsulación y ocultación de informaciónLa encapsulación busca agrupar en una misma unidad (la clase) los datos y las funciones que operan sobre ellos. Por su parte, la ocultación de información persigue proteger el estado interno del objeto, impidiendo que el código exterior acceda directamente a los datos sensibles, obligando a interactuar con ellos solo a través de mecanismos controlados.Entre las ventajas de la ocultación destacan:Mantenibilidad: Se puede cambiar la estructura interna de los datos sin afectar a quienes usan la clase.Seguridad y Robustez: Se evita que el usuario del objeto ponga datos en un estado inconsistente o erróneo.Reducción de la complejidad: El programador que usa la clase solo necesita saber qué hace el objeto, no cómo está implementado por dentro.2. Interfaz públicaLa interfaz pública es el conjunto de métodos y constantes que una clase expone al exterior. En términos de C, sería comparable a las firmas de funciones que se incluyen en un archivo .h, las cuales definen qué servicios ofrece el módulo a otros archivos.La relación con la ocultación es directa: la interfaz pública actúa como un "contrato" o frontera. Mientras que la ocultación mantiene los detalles escabrosos o variables internas bajo llave, la interfaz pública ofrece los "botones" necesarios para que el mundo exterior pueda pedirle al objeto que realice acciones de forma segura.3. Diseño de la interfaz públicaSe debe diseñar con extremo cuidado porque, una vez que otros programadores empiezan a usar una clase, la interfaz pública se convierte en un compromiso difícil de romper. Si se elimina o cambia un método público, se romperá todo el código que dependía de él, lo que obligaría a realizar refactorizaciones costosas en todo el sistema.A diferencia del código interno (privado), que se puede reescribir por completo mientras se mantenga el comportamiento esperado, la interfaz pública es rígida. Cambiarla no es fácil ni recomendable; por ello, se aplica el principio de exponer lo mínimo indispensable.4. Invariantes de claseUna invariante de clase es una condición o regla que siempre debe cumplirse para que un objeto se considere válido. Por ejemplo, en una clase Fecha, una invariante sería que el mes siempre esté en el rango $[1, 12]$.La ocultación de información ayuda a preservar estas invariantes al prohibir el acceso directo a los atributos. Si los atributos fueran públicos, cualquier código externo podría asignar un "13" al mes. Al ser privados, el objeto puede validar los datos antes de aceptarlos a través de sus métodos, garantizando que nunca entre en un estado inválido.5. Clase Punto y modificadoresJavapublic class Punto {
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
La interfaz pública de esta clase está compuesta por el constructor Punto(double, double) y el método calcularDistanciaAOrigen(). Los atributos x e y no forman parte de ella porque son inaccesibles desde fuera.public significa que el miembro es accesible desde cualquier otra clase del proyecto. private, en cambio, restringe el acceso exclusivamente a los métodos que pertenecen a la propia clase Punto. Es el mecanismo fundamental para lograr la ocultación.6. Aplicación de modificadores en JavaEn Java, los modificadores public y private se pueden aplicar a los miembros de la clase (atributos y métodos) y a los constructores. También se aplican a las clases en sí mismas (aunque una clase de nivel superior no puede ser privada, solo pública o con visibilidad de paquete).Es importante notar que no se pueden aplicar a las variables locales dentro de un método. En Java, el ámbito de una variable local está definido por las llaves {} del método y no requiere modificadores de visibilidad.7. Otros tipos de visibilidadAdemás de pública y privada, existen otros niveles. En Java, existe la visibilidad por defecto o de paquete (cuando no se pone nada), que permite el acceso a cualquier clase dentro del mismo directorio/paquete. También existe protected, que permite el acceso a subclases (herencia).Otros lenguajes como C++ tienen conceptos similares, pero con matices distintos en la forma en que se heredan los permisos. Algunos lenguajes modernos incluso tienen visibilidad a nivel de "módulo" o "ensamblado", permitiendo un control todavía más granular sobre quién puede ver qué.8. Privacidad entre instanciasLa respuesta correcta es la (a). Los miembros privados están ocultos para otras clases, pero no para otras instancias de la misma clase. Un objeto de la clase Punto puede acceder a los atributos privados de otro objeto Punto.Javapublic double calcularDistanciaAPunto(Punto otro) {
    // Es legal acceder a otro.x aunque x sea privado
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
Esto es posible porque la visibilidad en Java es a nivel de clase, no a nivel de objeto individual. Como el código está escrito dentro de la clase Punto, tiene permiso para manipular cualquier atributo definido en ella, sin importar a qué instancia pertenezca.9. Getters y SettersLos getters y setters (o métodos de acceso y modificación) son métodos públicos diseñados para leer o actualizar el valor de un atributo privado, respectivamente. Se utilizan para mantener el control sobre los datos.Un "getter" simplemente devuelve el valor, permitiendo la lectura pero no la escritura. Un "setter" permite recibir un valor nuevo, lo cual brinda la oportunidad de validarlo antes de asignarlo (por ejemplo, comprobar que una edad no sea negativa) o realizar acciones adicionales cuando un dato cambia.10. Seguridad en el programaCuando se habla de "seguridad" en este contexto, no se refiere a la protección contra ciberataques o hackers externos. Se refiere a la integridad programática y a evitar errores accidentales del propio equipo de desarrollo.Es "seguridad" en el sentido de que el código es menos propenso a fallos catastróficos causados por efectos secundarios no deseados. Se evita que una parte del programa corrompa los datos de otra parte por descuido, facilitando la depuración y la estabilidad del software.11. Miembro de instancia vs. de claseUn miembro de instancia (como x o y en Punto) existe de forma única para cada objeto creado; cada punto tiene sus propias coordenadas. Un miembro de clase (marcado con static) es compartido por todos los objetos de esa clase; solo existe una copia en memoria para todos.Los miembros de clase también se pueden (y deben) ocultar si son sensibles. Un atributo static privado solo podrá ser consultado o modificado por métodos de esa clase, lo que permite llevar contadores globales o configuraciones compartidas de forma controlada.12. Constructores privadosSí, tiene mucho sentido en varios escenarios. Un constructor privado impide que se creen instancias de la clase usando el operador new desde el exterior. Esto es útil en clases que solo contienen métodos de utilidad estáticos (como la clase Math de Java) o para implementar patrones de diseño como el Singleton.También se usan cuando se desea obligar al usuario a crear objetos a través de métodos factoría, permitiendo un control total sobre cómo y cuándo se generan nuevos objetos.13. Miembros de clase en JavaEn Java se utiliza la palabra clave static.Javapublic class Punto {
    private double x, y;
    private static double xMax = Double.NEGATIVE_INFINITY;
    private static double yMax = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > xMax) xMax = x;
        if (y > yMax) yMax = y;
    }
}
En este ejemplo, xMax e yMax pertenecen a la clase, no a cada punto individual. Cada vez que se crea un punto, se actualizan estos valores globales.14. Método factoríaJavapublic static Punto crearPuntoRedondeado(double x, double y) {
    double xRedondeado = Math.round(x);
    double yRedondeado = Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}
Se ha utilizado static porque el método debe poder ejecutarse sin que exista previamente un objeto de la clase Punto. Su función es, precisamente, fabricar uno.15. Cambio de implementación internaJavapublic class Punto {
    private double[] coords = new double[2];

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }
}
Aunque la estructura interna ha cambiado (ahora hay un array en lugar de dos variables sueltas), el código que usa Punto no notará ninguna diferencia. Esto demuestra el poder de la encapsulación.16. Atributos públicos vs. Getters/SettersCasi nunca es mejor declararlo público. La convención habitual es que los atributos sean siempre privados. Incluso si hoy solo se necesita un simple acceso de lectura/escritura, mañana se podría querer añadir una validación o un log de cambios. Si el atributo es privado, se puede añadir esa lógica en el "setter" sin romper el resto del programa.Esto tiene todo que ver con las invariantes de clase: el "setter" es el guardián que asegura que el nuevo valor no rompa las reglas de integridad del objeto.17. InmutabilidadUna clase es inmutable si su estado (sus datos) no puede cambiar una vez que el objeto ha sido creado. Un método modificador es aquel que cambia el estado interno del objeto; no siempre es un "setter" tradicional (ej. un método acelerar() en una clase Coche).Las clases inmutables tienen grandes ventajas: son intrínsecamente seguras en entornos de ejecución paralela (hilos), son más fáciles de entender y pueden ser compartidas o cacheables sin riesgo de que alguien las altere por error.18. Recomendación de SettersNo es recomendable incluirlos por defecto. De hecho, la tendencia moderna es preferir objetos inmutables o con la mínima capacidad de cambio posible. Solo se deben añadir "setters" cuando sea estrictamente necesario que el objeto cambie de estado durante su vida útil. Añadir "setters" a todo a ciegas rompe a menudo la encapsulación y expone la estructura interna innecesariamente.19. Clase String en JavaLa clase String en Java es inmutable. Al concatenar dos cadenas (por ejemplo, con +), no se modifica la cadena original, sino que se crea un objeto String completamente nuevo en memoria que contiene la unión de ambas.Si se van a realizar muchas concatenaciones en un bucle, esto es muy ineficiente. En su lugar, se debe utilizar la clase StringBuilder, que está diseñada específicamente para construir cadenas de forma mutable y eficiente antes de convertirlas al resultado final.20. Comparación de objetosEn POO se distingue entre identidad (¿son el mismo objeto en memoria?) y contenido (¿tienen los mismos datos?). Por defecto, el operador == en Java compara la identidad (direcciones de memoria).El método equals en Java sirve para comparar el contenido. Por defecto, equals hace lo mismo que ==, por lo que se debe sobrescribir en nuestras clases para definir qué significa que dos objetos sean "iguales". Para comparar dos cadenas String, siempre se debe usar s1.equals(s2) y nunca ==.21. Clases WrapperLas clases wrapper (envoltorios) son clases que representan tipos primitivos (como int o double) como si fueran objetos (Integer, Double). En Java, esto ocurre de forma automática mediante un proceso llamado autoboxing.Su ventaja principal es que permiten usar tipos primitivos en estructuras que solo aceptan objetos (como las colecciones). No todos los lenguajes los necesitan; lenguajes como Python o Smalltalk tratan todo como un objeto desde el principio, eliminando la distinción entre primitivos y objetos.22. Tipos enumerados (Enum)Un tipo enumerado es un tipo de dato que define un conjunto fijo de valores constantes (por ejemplo, los días de la semana). En Java, a diferencia de C, un enum es una clase especial muy potente.En términos de encapsulación, los enum de Java permiten añadir atributos, constructores y métodos. Esto significa que cada valor del enumerado puede guardar información asociada y ofrecer comportamiento, manteniendo los datos protegidos y agrupados bajo un tipo de dato estrictamente tipado.23. Ejemplo de Enum MesJavapublic enum Mes {
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
        return norte ? (this == MARZO || this == ABRIL || this == MAYO) :
                       (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
    }
    // (Los demás métodos seguirían una lógica similar)
}
Este diseño encapsula toda la lógica del calendario dentro del propio tipo, evitando que el resto del programa tenga que usar estructuras switch o if repetitivas para consultar propiedades de los meses.
