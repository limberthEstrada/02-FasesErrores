Funciones que mi compilador presenta para limitar el inicio y fin de las fases de traduccion:
[Preprocesamiento]
-E: Solo ejecuta la fase de preprocesamiento y muestra el resultado en la salida estandar
-P: Similar a -E, pero no genera lineas de código vacias y combina lineas de multiples directivas #define
-C: Conserva los comentarios durante el preprocesamiento

[Compilacion]
-S: Ejecuta el preprocesamiento y la compilación, generando el código ensamblador

[Ensamblado]
-c: Ejecuta el preprocesamiento, compilación y el ensamblado, generando el código objeto

as: Ensambla directamente el archivo de ensamblador a código objeto, recuerda que (-c) realiza más pasos

Paso 1 - [Preprocesador]

a) 
    Puedo ver a simple vista que a la función main le falta cerrar una llave y tiene una función no definida.

b) 
    Para preprocesar y no compilar y además generar hello2.c.
    De acuerdo al contenido de hello2.i, se puede ver que el pequeño código que estaba inicialmente se expandió, mejor dicho se expandieron las directivas include y otras directivas del preprocesador. Se puede obsevar el contenido que corresponde al archivo stdio.h y otros archivos de encabezado que son incluidos indirectamente por ese archivo.
    Utilicé el siguiente comando: gcc -E hello2.c -o hello2.i

c)  
    Esta variante es muy similar solo que esta vez tiene una definición a la función printf

d)
    Para empezar la primera linea se trata de una declaración de una función. Se trata de una función que devuelve un entero y toma al menos un párametro. Sobre el primer parametro se trata de un puntero a un caracter constante llamado 's'. El restrict es una palabra clave que le indica al compilador que el puntero 's' es el único puntero que se utilizará para acceder al objeto al que apunta. El '...' indica que la función puede tener un número indeterminado de argumentos después del primer argumento, que se utilizan para proporcionar los valores a imprimir según el formato especificado en el primer argumento.

e)
    hello3.i es el resultado de la primera fase de la compilación, conocida como la fase de preprocesamiento. El preprocesador realiza las siguientes tareas:
    elimina los comentarios del código fuente, incluye los archivos de encabezado utilizando las directivas #include, expande las macros definidas por el usuario o por el preprocesador, realiza sustitución de texto condicional basado en directivas como #if, #ifdef, #ifndef, #else, #endif. Además en el archivo hello3.i se puede ver que el preprocesador generó marcadores de linea, dichos marcadores representar: el nombre del archivo fuente, el nombre de archivo de encabezado predefinido y la linea de comando utilizada para invocar al preprocesador. El archivo hello3.i se usa como entrada para la siguiente fase de compilación, es decir la compilación en si.
    Utilicé este comando: gcc -E hello3.c -o hello3.i 

Paso 2 - [Compilación]

a)
    Al compilar el resultado y no ensamblar me arrojó un error en la función main, dicho error se trata de que no existe una declaración para la función invocada 'prontf', en el mensaje de error se menciona que pudo ser un error de tipeo y que la función sea 'printf' y no 'prontf'. Y debido a este error no se pudo generar el archivo ensamblador.
    Utilicé este comando: gcc -S hello3.c -o hello3.s

b)
    Ya fueron corregidos los errores y se pudó generar el archivo hello4.s. Y en su contenido se puede ver código ensamblador. Que al ensamblarlo se podrá generar un archivo ejecutable.
    Utilice este comando: gcc -S hello4.c -o hello4.s

c) 
    El objetivo del código es imprimir en la salida estandar la cadena "La respuesta es 42" utilizando la función printf. El valor 42 se asigna a una variable "i" y luego se formatea junto con la cadena "La respuesta es %d \n"

d)
    Quedo ensamblado, pero no es posible abrir el archivo objeto porque en su contenido hay texto binario
    Utilicé este código: as -o hello4.o hello4.s

Paso 3 - [Vinculación]

a) 
    Utilicé este comando: gcc -o hello4.exe hello4.o

b)
    Corregido, el error estaba en que al ejecutar hello4.exe no imprimia el valor de la variable 'i', devolvia cualquier valor. La razón era porque nunca pasamos como argumento la variable 'i' en la función printf
    Utilice este comando: gcc -o hello5.exe hello5.c

c)
    No hay nada para analizar, la respuesta es la esperada.

Paso 4 - [Corrección de bug]

a)
    Lo volví a hacer todo en hello6.c y funciona perfecto

Paso 5 - [Remoción de prototipo]

a)
    Listo, se puede ver que acá no hay una declaración para printf

b)  
    i) Arroja warning: implicit declaration of function 'printf'; incompatible implicit declaration of built-in function 'printf'. Significa que el programa se compilará y ejecutará correctamente, pero el compilador esta advirtiendo sobre un posible problema
    ii) Es una declaración que especifica el nombre de la función, los tipos de los parámetros y el tipo de retorno. Se puede generar un prototipo incluyendo el header file o escribiendo el prototipo manualmente. Permite al compilador verificar que las llamadas a la función sean correctas.
    iii) La declaración implicita de una función es la caracteristica del lenguaje C que ocurre cuando se llama a una función sin haberla declarado previamente. Es cuando se usa una función sin proveer previamente su prototipo. En este caso, el compilador asume un prototipo basado en la llamada a la función. Puede llevar a problemas si la suposición era incorrecta.
    iv) La especificación C indica que se debe proveer una declaración o una definición de una función antes de usarla. De no hacerlo, el comportamiento será indefinido. Muchos compiladores permiten el uso de funciones sin una declaración previa.
    v) La mayoria de las implementaciones de C permiten llamadas a funciones sin prototipo, generando un prototipo implicito, pero emiten una advertencia para alertar a un programador de un posible error.
    vi) Se trata de una función que esta integrada directamente en el compilador. El compilador conoce su prototipo sin necesidad de incluir un encabezado. Para este caso 'printf' no es una función de este tipo, sino es una función de biblioteca
    vii) GCC se comporta de esta manera para cumplir con la especificación del lenguaje C y al mismo tiempo proporcionar una advertencia al programador. También se emiten warnings para alentar al programador a mejorar el código. NO va directamente contra la especificación. El código de hello7.c funciona porque GCC permite llamadas a printf sin prototipo como una extensión, generando/asumiendo un prototipo implicito. La razón por la que GCC permite llamadas a funciones sin declaración previa es para mantener compatibilidad con código antiguo de C. La especificación de C dice que llamar a una función sin declaración previa resulta en un comportamiento indefinido. "Indefinido" significa que la especificación no define qué debe pasar en este caso - podría funcionar, podría fallar, podría hacer cualquier cosa. GCC no viola la especificación, sino que provee una extensión documentada.

    Paso 6 - Compilación separada: Contratos y Módulos
    
    a)
        Listo, ya generados
    
    b)
        Utilice el comando: gcc -c studio1.c hello8.c
        Utilice el comando: gcc studio1.o hello8.o -o programa

    c)
        Si eliminamos argumentos o agregamos argumentos a la función prontf, nos arrojará un mensaje de error porque en la declaración de la función se indica que se espera 2 argumentos, ya no es tan flexible como la función 'printf'. Por ejemplo, si se elimina el argumento 'i' de la llamada "prontf("La respuesta es %d\n", i);", entonces la función 'prontf' no recibirá el valor que debe imprimir. Como prontf a su vez llama a printf con los mismos argumentos, al no tener el valor del especificador de formato %d, printf intentará obtener ese valor de la pila, lo que resultará en un comportamiento indefinido.
        Si agregamos argumentos estos simplemente serán ignorados por prontf y printf, ya que ninguna de las 2 funciones los esta utilizando. Todo esto anterior la verdad depende del compilador porque en un escenario el compilador puede emitir una advertencia sobre los argumentos extra, pero aún asi dejar que el programa compile y ejecute; o también que al pasar más argumentos de los esperados, el comportamiento del programa es indefinido. Por lo que podria ocurrir errores de segmentación, resultados incorrectos o cualquier comportamiento impredecible.
        
        [Mejor respuesta] O también, ya sea eliminando o agregando argumentos, se produciría errores de compilación. Esto se debe a qué no hay un contrato establecido entre las unidades de traducción hello8.c y stdio1.c sobre la interfaz de la función prontf. El compilador de C asumirá que prontf toma cualquier número de argumentos de cualquier tipo (porque no tiene información sobre su interfaz real) y permitirá que el código compile, y eso producirá un error porque la definición real de prontf no coincide con la asumida. Ya sea que agreguemos o eliminimos, siempre fallará en el paso de enlace porque la llamada no coincide con la definición.

    d)
        iv) 
        - Verificación de tipos en tiempo de compilación: Se puede verificar que los tipos de los argumentos pasados a 'prontf' coincidan con los tipos declarados en su prototipo. Esto ayuda a detectar errores en tiempo de compilación
        - Documentación de interfaz: Facilita la comprensión y el uso correcto de la función por parte de los clientes, al tener acceso a la declaración de la función y los tipos de datos esperados.
        - Facilita el mantenimiento: Al tener un contrato separado, es decir, tener la interfaz por un lado y la implementación por otro lado, se logra una separación clara, esto puede permitir al proveedor modificar la implementación de prontf sin afectar a los clientes, siempre y cuando se mantenga el contrato
        - Reutilización y modularidad: Al tenerlo separado en un archivo encabezado, puede ser incluido y utilizado en multiples archivos fuente.
        - Prevención de declaraciones implicitas: Para evitar comportamientos inesperados
        - Ocultamiento de información: Al tenerlo todo en un archivo encabezado, el cliente solo necesita saber cómo llamar a las funciones, no como funcionan internamente.

Credito extra:
    Las bibliotecas son un conjunto de funciones y rutinas que pueden ser utilizadas por programas de software para realizar tareas especificas. 
    
    Son distribuibles junto con el software que las utiliza. Pueden ser distribuidas como archivos compilados (binarios) o como archivos fuente que deben ser compilados antes de su uso. Permiten compartir código entre diferentes programas y entre diferentes desarrolladores.

    En general son portables, especialmente si están escritas en un lenguaje de programación de alto nivel y no dependen de caracteristicas especificas de una plataforma o SO. Pero si dependen de eso, puede limitar su portabilidad.

    Sus ventajas son: 
    - Reutilización de código: se pueden utilizar las funciones que estan en las bibliotecas, lo que ahorra tiempo y esfuerzo
    - Modularidad: Al dividir el código en bibliotecas, se facilita la organización y mantenimiento del código
    - Abstracción: Las bibliotecas proporcionan una capa de abstracción lo que facilita el uso para los programadores, ya que se oculta la complejidad interna de las funciones.

    Sus desventajas son:
    - Dependencia: Los programas que utilizan a esas bibliotecas estan vinculadas a las mismas, por ende cualquier cambio en las bibliotecas puede afectar a multiples programas
    - Tamaño del archivo ejecutable: Si se utilizan muchas bibliotecas, el tamaño del archivo ejecutable puede aumentar.
    - Posibles problemas de compatibilidad: Las bibliotecas pueden tener diferentes versiones entre si, lo que puede causar problemas de compatibilidad en programas que las utilizan.