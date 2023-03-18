# Análisis Forense Informático: Guía de Artefactos By S3rC4D1

![images](https://user-images.githubusercontent.com/42890499/226121909-a9a30923-356e-4673-8aaa-19f6ca5cfd0c.jpeg)


Esta es una guía realizada por mí, siguiendo pequeñas investigaciones por la red abierta o viendo material de libros, de uso y explicación de diferentes artefactos forenses en sistemas Windows, Linux, Mac o móbiles como Android/IOS aunque sea este más privativo.

# Windows

![pngegg (4)](https://user-images.githubusercontent.com/42890499/226121958-cd3645b2-179e-4957-88b5-e09273a8aa0a.png)

Papelera de Reciclaje o RecicleBin

Aquí dentro van los ficheros cuando se borran, sin embargo, en todo sistema se queda guardada una cantidad de ficheros y metadatos dentro de los misma. Normalmente esto pasa en sistemas que llevan incorporados un HDD o discos duros de antaño, o memorias USB/SD u otro tipo de sistema de almacenamiento. Sin embargo, los sistemas que hoy en día llevan un SSD es difícilmente que los ficheros que se hayan borrado de dicha carpeta de la memoria se puedan recuperar ya que llevan implementada una función llamada TRIM aunque puede llegar a depender de la propia implementación o configuración del sistema operativo en cuestión, ya que por ejemplo puede ser que Windows lo guarde un tiempo pero en una distribución Linux/Unix lo borre automáticamente dependiendo de la función.

Estructura de un fichero en general

-	Header: Cabecera del fichero,Signature, normalmente guarda lo que se llama Magic Number en Hexadecimal e indica que tipo de fichero puede o suele llegar a ser, por ejemplo la cabecera de una imagen jpg sería por defecto FF D8 FF. Algunas herramientas para visualizar estas cabeceras pueden ser editores hexadecimales como HxD, programas de análisis de imagen como Active Disk Editor, FTK Imager, Autopsy, PhotoRec en Windows o Sleuth Kit en Linux gracias a su módulo de previsualización de contenido.
-	Footer: Pie de página final del fichero, es necesaria que exista cuando el fichero no tiene un tamaño definido como tal en la estructura interna o bien la cabecera no contiene el tamaño de dicha estructura interna.
-	Cuerpo: Contiene el resto de la estructura del fichero y es útil para saber cómo poder recuperar los datos en general, ya que muchas veces si es un comprimido por ejemplo sin cifrar, los datos se pueden visualizar dentro de ese fichero ya que están internamente en el mismo.

Todo esto lo explico, porque la estructura del fichero borrado es posible que se pueda recuperar con algún método como el carving en general, o a través de la $MFT o Master File Table en caso del sistema operativo Windows y posteriormente recuperar el fichero como tal, por lo que tenemos varias formas. Eso sí siempre hay que recordar que solo se mantendrá un tiempo debido a la función TRIM o bien porque los datos sustituyan a los bloques guardados dentro de la memoria de almacenamiento del dispositivo (por ejemplo, si borras 100GB de ficheros, te quedan 300 disponibles y metes 200, alguno de esos 200GB podría sustituir bloques de almacenamiento de los 100, haciendo imposible recuperar dichos datos).

Localizaciones de la Papelera de Reciclaje en Windows

C: Puede ser cualquier letra así que podríamos sustituirlo por %SystemDrive
-	Windows 95/98/ME: C:\RECYCLED
-	Windows XP: C:\Reycler, aquí se empezó a incluir lo que se llama SID del usuario o identificador de usuario, que indica a que usuario pertenece cada directorio de la papelera, por ejemplo, un número tipo SID-192851206, a lo mejor indica que el usuario es Pepito, mientras que otro número parecido indicará lo borrado por Manolo. Dentro de dicho directorio podemos tener los ficheros borrados y un fichero llamado INFO2 que contiene los metadatos de dicho directorio, útil para ver como ha pasado en una línea de tiempo los borrados y actualización de dicho directorio. Su tamaño máximo es el 10% del disco duro o sistema de almacenamiento que tengamos.
-	Windows Vista/8/10/11: C: \$Recycle.bin, aquí tenemos el directorio que tendríamos en un análisis de Vista hacia delante, que es W11 hasta la fecha, contiene un SID como anteriormente, pero esta vez los ficheros van nombrados según una estructura inicial que empieza por $R y un id de fichero, por ejemplo $R9681A, también tenemos los metadatos pero como $I (algunos metadatos son el nombre, la ruta original donde se guardaba el fichero, el tamaño del mismo como tal y la fecha de eliminación, útil en un análisis forense.  Podemos extraerlos de alguna forma y con una herramienta como $I Parse o Rifiuti2 extraer un CSV con la información de dichos metadatos y poder ver una timeline con dichos datos.



