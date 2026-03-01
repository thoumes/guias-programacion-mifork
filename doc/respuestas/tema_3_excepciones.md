# 1. Control de errores en C
## En el lenguaje C, al carecer de un mecanismo de excepciones, el control de errores debe realizarse mediante el flujo normal del programa. La función raiz no puede detener la ejecución por sí sola, por lo que debe comunicarse con el llamador a través de valores de retorno o parámetros externos.

La primera opción consiste en utilizar un valor de retorno especial que indique un estado de error. Dado que una raíz cuadrada real nunca es negativa, se puede devolver un valor centinela como -1.0 o la constante NAN (Not a Number) de la librería math.h. El programa que llama a la función tiene la responsabilidad de verificar si el resultado es ese valor especial antes de continuar con el cálculo.

La segunda opción es utilizar un parámetro por referencia para devolver el resultado y reservar el valor de retorno de la función para un código de estado (0 para éxito, 1 para error). Este diseño es muy común en las bibliotecas de sistemas operativos, ya que separa claramente el dato obtenido de la señalización del éxito o fracaso de la operación.

2. ¿Qué es una "excepción"?
Una excepción es un evento que ocurre durante la ejecución de un programa y que interrumpe el flujo normal de las instrucciones. Se trata de una señal de que algo inesperado ha sucedido (como una división por cero, un archivo inexistente o un índice fuera de rango) y que el código actual no sabe o no debe resolver en ese punto exacto.

El objetivo del programador al implementar funciones es separar la lógica de negocio (el "camino feliz") de la gestión de errores. Al lanzar una excepción, se delega la responsabilidad de solucionar el problema a un nivel superior de la pila de llamadas que tenga el contexto necesario para decidir qué hacer (por ejemplo, mostrar un mensaje al usuario o reintentar la operación). Al llamar a funciones, el objetivo es garantizar la robustez del programa, capturando posibles fallos para evitar que la aplicación aborte de forma abrupta.

3. Ejemplo de raíz en Java
En Java, el control se realiza mediante objetos y bloques estructurales. A continuación, se muestra la clase Calculadora donde el método lanza una excepción de tipo IllegalArgumentException si el parámetro es inválido, y el método main gestiona dicha situación.

4. Conceptos de lanzamiento, control y propagación
Lanzar (throw) es el acto de crear un objeto de excepción y entregarlo al sistema de ejecución de Java para indicar un error. Controlar o capturar (catch) consiste en interceptar esa excepción mediante un bloque específico de código diseñado para manejarla y permitir que el programa continúe su ejecución.

La propagación es el proceso por el cual una excepción "viaja" hacia atrás a través de la pila de llamadas (stack) si no es capturada en el método actual. Cuando un método lanza una excepción y no tiene un bloque catch, el método finaliza inmediatamente y la excepción se transfiere al método que lo invocó. Este proceso se repite sucesivamente hasta encontrar un manejador o hasta llegar al método main.

En la pila de llamadas, las funciones por las que se propaga la excepción se cierran abruptamente; se liberan sus variables locales pero no se ejecutan las líneas de código posteriores al punto donde se originó o recibió el error. Las funciones que no la controlan no se reanudan; mueren definitivamente en esa ejecución. En el ejemplo de la raíz, si main no tuviera el try-catch, el programa terminaría mostrando el error en la consola y nunca llegaría a ejecutar ninguna línea posterior a la llamada de calc.raiz().

5. Ventajas de la propagación natural frente a C
La principal ventaja es la limpieza del código. En C, cada vez que se llama a una función que puede fallar, se debe escribir un if para comprobar el resultado. Si existe una cadena de 10 llamadas de funciones, las 10 deben incluir lógica de comprobación de errores. En Java, la propagación automática permite ignorar el error en los niveles intermedios y gestionarlo únicamente en el lugar donde realmente se puede hacer algo al respecto.

Además, se evita el riesgo de ignorar errores por omisión. En C, si un programador olvida comprobar un valor de retorno, el programa continúa con datos erróneos, lo que puede causar fallos catastróficos más adelante difíciles de depurar. En Java, si una excepción no se controla, el programa se detiene de forma segura y ruidosa, informando exactamente qué pasó y dónde, evitando que el error pase desapercibido.

6. Excepciones como objetos y encapsulación
En los lenguajes orientados a objetos como Java, las excepciones son objetos que pertenecen a una jerarquía de clases (descendientes de Throwable). Esto permite aprovechar todos los beneficios de la orientación a objetos, como la herencia y el polimorfismo, para categorizar los errores de forma estructurada.

En términos de encapsulación, esto es una gran ventaja porque la excepción puede contener no solo un mensaje de texto, sino también otros atributos útiles (códigos de error internos, el estado del objeto en el momento del fallo, etc.) sin exponer detalles internos de la implementación de la función. Sí, es posible y recomendable crear excepciones personalizadas extendiendo clases existentes (como Exception o RuntimeException), lo que permite que el sistema de tipos ayude a identificar problemas específicos de nuestro dominio de negocio.

7. Información esencial en el objeto excepción
A diferencia de C, donde a menudo solo se recibe un número o un puntero nulo, un objeto excepción en Java transporta información contextual crítica. Lo más relevante es el StackTrace (traza de la pila), que es una lista de todos los métodos y números de línea por los que ha pasado la ejecución antes de fallar. Esto permite localizar el origen exacto del error en segundos.

Además, el objeto lleva consigo un mensaje descriptivo (el campo message) que explica la causa humana del error y, opcionalmente, una causa raíz (otra excepción que provocó la actual). Esta estructura encapsulada permite que el manejador tome decisiones informadas basadas en el tipo de objeto recibido, permitiendo incluso recuperar datos que estaban siendo procesados en el momento del incidente.

8. El bloque try-catch y múltiples catch
Es perfectamente posible y común tener más de un bloque catch asociado a un mismo bloque try. Esto se utiliza para reaccionar de forma diferente ante distintos tipos de errores que pueden surgir en un mismo fragmento de código (por ejemplo, un error de red frente a un error de formato de datos).

Sin embargo, en cada ejecución del bloque try, solo se ejecuta un bloque catch como máximo: el primero que coincida con el tipo de la excepción lanzada o con una de sus superclases. El orden es fundamental; siempre se deben colocar los catch de las excepciones más específicas arriba y los de las clases más generales (como Exception) abajo, ya que si se pusiera la general primero, capturaría todo y los bloques específicos nunca se alcanzarían.

9. Garantía de ejecución con finally
Para garantizar que ciertos recursos se liberen (cerrar archivos, conexiones a bases de datos o liberar memoria) independientemente de si ocurre un error o no, Java proporciona el bloque finally. Este código se ejecuta siempre, tanto si el try termina con éxito como si salta al catch o si la excepción se propaga hacia arriba.

Ejemplo con catch:

Ejemplo sin catch (la excepción se propaga, pero el recurso se cierra antes):

10. Detalles del bloque finally
El bloque finally sí puede ir sin catch, siempre que acompañe a un bloque try. Su propósito en este caso es asegurar la limpieza de recursos permitiendo que la excepción siga su curso hacia el método llamador. Es una estructura de "limpieza obligatoria" más que de "gestión de error".

Se ejecuta siempre, incluso si hay una instrucción return dentro del bloque try o del catch. Java garantiza que, antes de que el método devuelva el control efectivamente al llamador, el bloque finally se procese. La única excepción a esta regla es si el programa termina abruptamente mediante System.exit() o un fallo crítico del sistema (como un corte de energía o un error interno de la JVM).

11. Excepciones controladas vs no controladas
Las excepciones controladas (checked) son aquellas que el compilador obliga a gestionar (con try-catch) o a declarar (con throws). Derivan de Exception. Las no controladas (unchecked) son aquellas que derivan de RuntimeException y el compilador no obliga a tratarlas explícitamente, ya que suelen deberse a errores de programación.

Ejemplos de controladas: FileNotFoundException, SQLException.
Ejemplos de no controladas: NullPointerException, ArrayIndexOutOfBoundsException.

Situaciones para excepciones controladas (errores recuperables o externos):

Fallo en la conexión de una red externa.

Un archivo que el usuario debe proporcionar no existe.

Formato de datos incorrecto en una importación.

Tiempo de espera agotado en una base de datos.

Situaciones para excepciones no controladas (errores de lógica o programación):

Acceder a un objeto que es null.

División por cero.

Pasar un argumento inválido a un método (error del programador).

Índice fuera de los límites de un array.

12. El uso de throws
La palabra clave throws se utiliza en la firma de un método para indicar que dicho método puede lanzar una o varias excepciones que no ha capturado. Es una forma de avisar al llamador de los peligros potenciales de usar esa función, trasladando la responsabilidad de la gestión del error un nivel más arriba en la pila.

Es la alternativa a capturar la excepción porque, a veces, un método de bajo nivel no sabe cómo resolver un problema. Por ejemplo, si una función que lee un archivo no encuentra el fichero, no sabe si debe pedir otro nombre al usuario o cerrar el programa; por tanto, declara que "lanza" el error (throws) para que el método de la interfaz de usuario, que sí tiene contacto con el humano, lo capture y decida.

13. Ejemplo de firma con throws y finally
En este diseño, el método procesarArchivo no captura la posible excepción de archivo no encontrado, delegándola, pero asegura que el flujo de cierre se intente.

14. throws con RuntimeException
Es técnicamente posible poner excepciones no controladas como RuntimeException en la cláusula throws, pero es innecesario y poco habitual. El compilador no lo requiere y no aporta información obligatoria al llamador.

El método llamador no estaría obligado a poner un try-catch. El único sentido que tendría es documental: informar a otros programadores de que esa excepción es probable que ocurra bajo ciertas condiciones, aunque no sea obligatorio capturarla. Es una cortesía de diseño más que una restricción técnica.

15. Recomendaciones de uso y lenguajes
Se recomienda usar excepciones controladas para condiciones de error que son razonablemente previsibles y de las que el programa podría recuperarse. Se usan no controladas para errores que indican un mal uso de la API o defectos de lógica donde la recuperación no suele ser posible y lo mejor es que el hilo de ejecución falle.

No todos los lenguajes tienen ambos tipos. Java es casi único en su uso intensivo de excepciones controladas. Lenguajes modernos como C#, Python, C++ o Kotlin solo tienen excepciones no controladas (todas son tratadas como RuntimeException en términos de Java). La tendencia actual en el diseño de lenguajes es la de evitar las excepciones controladas por considerarlas demasiado verbosas.

16. Lanzar excepciones dentro del catch
Es muy común lanzar excepciones dentro de un bloque catch. Se puede relanzar la misma excepción simplemente escribiendo throw e;. Esto tiene sentido cuando queremos que el error se propague hacia arriba, pero antes necesitamos realizar una acción local, como registrar el fallo en un log o liberar un recurso parcial.

También se puede lanzar una excepción diferente. Esto se hace para realizar una "traducción de excepciones": capturamos un error técnico de bajo nivel (como un error de SQL) y lanzamos uno que tenga sentido para el negocio (como ErrorAlRegistrarUsuarioException).

17. Excepción como "causa" (Encadenamiento)
El encadenamiento de excepciones consiste en capturar una excepción y envolverla dentro de una nueva. La excepción original se guarda en el atributo cause de la nueva. Esto permite mantener el contexto de alto nivel sin perder el detalle técnico original que provocó el fallo.

Cuando una excepción sale por pantalla, si tiene una causa, Java la muestra mediante el prefijo "Caused by:" seguido de la traza de la excepción original. Es perfectamente visible en la consola y es fundamental para que el programador pueda rastrear el error desde la capa superior hasta la línea exacta de la librería de bajo nivel donde ocurrió el problema real.
