# Lab 3 — Preguntas de comprobación

**Nombre:** Miguel Ángel García Ogando
**Profesor:** Richard Aviles Lopez
**Reviewer:** César Pérez Montero

## 1. ¿Qué diferencia hay entre una imagen y un contenedor? Usa como ejemplo lo que hiciste en los ejercicios G2 y G4.

La imagen es la plantilla que contiene lo necesario para ejecutar un programa. El contenedor es la instancia que creamos a partir de esa imagen, con sus procesos y su espacio de trabajo.

En G2, Docker utiliza la imagen hello-world para crear un contenedor que muestra un mensaje y termina. En G4 utilizamos alpine:3.20 para crear el contenedor prueba y abrir una terminal dentro. Por tanto, una misma imagen puede servir para crear varios contenedores.

## 2. En el Ejercicio G5 el archivo nota.txt desapareció y en el G6 no. Explica por qué.

En G5 el fichero se guardó dentro del propio contenedor. Al eliminar ese contenedor también se eliminó lo que habíamos escrito dentro y el nuevo contenedor partió de la imagen original.

En G6 guardamos el fichero en un volumen llamado datos-prueba. Ese volumen existe de forma independiente al contenedor, por lo que al montar el mismo volumen en otro contenedor pudimos recuperar nota.txt. Detener un contenedor no es lo mismo que borrarlo.

## 3. ¿Qué diferencia hay entre docker ps y docker ps -a, y qué significa STATUS = Exited (0)?

docker ps muestra los contenedores que están en ejecución. Con docker ps -a se muestran también los que están detenidos.

Exited (0) significa que el proceso del contenedor ha terminado correctamente. No indica que esté funcionando en segundo plano. En hello-world es el resultado esperado porque su trabajo termina después de mostrar el mensaje.

## 4. En -p 8181:8181, ¿qué número corresponde a tu equipo y cuál al contenedor? ¿Qué pasaría con -p 80:8080 en el ejercicio de nginx?

El primer número corresponde al puerto de mi equipo y el segundo al puerto dentro del contenedor. En este caso coinciden, pero son dos extremos distintos de la conexión.

Con -p 80:8080 enviaríamos las peticiones del puerto 80 de nuestro equipo al 8080 del contenedor. En el ejercicio nginx escucha en el puerto 80, por lo que no cargaría la página con ese mapeo. El que utilizamos fue 8080:80.

## 5. ¿Por qué un contenedor de Oracle se queda en marcha y el de hello-world termina solo?

Porque el contenedor se mantiene en ejecución mientras siga activo su proceso principal. Oracle es un servicio que permanece funcionando para atender conexiones y consultas. hello-world solo tiene que imprimir un mensaje, por lo que termina su proceso y el contenedor queda detenido.

## 6. ¿Qué es el digest de una imagen y por qué lo registramos si ya sabemos que usamos :latest?

El digest es una huella que identifica el contenido exacto de la imagen. La etiqueta latest puede apuntar a otra versión cuando el fabricante publique una actualización.

Por tanto, decir que hemos utilizado latest no permite saber exactamente qué imagen descargamos. Guardando el digest dejamos constancia de la versión concreta y podemos compararla con la de otro compañero.

## 7. ¿Qué comando borraría realmente los datos de Oracle? ¿Por qué docker rm oralab-26ai no lo hace?

El comando que eliminaría el volumen de datos es `docker volume rm oralab-26ai-data`, siempre que ya no esté siendo utilizado por un contenedor. Es un comando destructivo, no una comprobación que tengamos que ejecutar.

docker rm oralab-26ai elimina el contenedor, pero los datos están guardados en el volumen con nombre oralab-26ai-data. Si creamos otro contenedor y conectamos ese mismo volumen, los datos siguen disponibles.

## 8. ¿Por qué este laboratorio se hace dentro del repositorio oracle-database-lab, con Issue, branch y Pull Request, en vez de en una carpeta aparte?

Porque la instalación también forma parte del proyecto y debe poder revisarse y reproducirse. En el repositorio guardamos los scripts, los cambios y las evidencias de que se han ejecutado.

El Issue define lo que hay que conseguir, la branch separa el trabajo de main y la PR permite que un compañero lo revise. En una carpeta aparte podríamos tener la instalación funcionando, pero perderíamos ese seguimiento del trabajo.

## 9. ¿Qué diferencia hay entre source 00-config.sh y bash 00-config.sh? ¿Por qué usamos source?

source ejecuta el contenido del fichero en la shell actual. Las variables y funciones que define quedan disponibles para los siguientes comandos.

bash ejecuta el fichero en otro proceso y sus cambios de variables no se trasladan a la shell desde la que lo llamamos. Usamos source porque necesitamos seguir utilizando constantes como EVID y la función ts en nuestra terminal.

## 10. Explica cada parte del nombre 20260915T091230Z_02-docker.script.log.

- 20260915 es la fecha: 15 de septiembre de 2026.
- T separa la fecha de la hora.
- 091230 es la hora: 09:12:30.
- Z indica que se utiliza UTC.
- 02 es el número del paso de la práctica.
- docker indica qué se está comprobando.
- .script.log identifica un registro de terminal dentro de la nomenclatura del laboratorio.

De esta forma podemos identificar a qué paso corresponde la evidencia y distinguir varias ejecuciones sin sobrescribirlas.

## 11. ¿Para qué sirve .gitattributes y qué error evita?

Sirve para establecer cómo debe tratar Git determinados archivos. En esta práctica lo utilizamos para mantener finales de línea LF en scripts, SQL y Markdown.

Esto evita que los finales de línea de Windows, CRLF, provoquen errores al ejecutar los scripts en Linux, como un bad interpreter por el carácter de retorno de carro. Es especialmente útil porque trabajamos con Windows y Ubuntu en el mismo equipo.

## 12. ¿Por qué en este Pull Request elegimos Create a merge commit en lugar de Squash and merge?

Porque queremos conservar los commits de cada parte del laboratorio. Así se puede seguir el orden del trabajo y revisar qué se hizo en cada paso.

Squash and merge agruparía todos esos commits en uno solo. Puede ser útil en otros casos, pero aquí perderíamos el detalle del proceso que el profesor pide conservar.

## 13. Describe las cuatro capas de la estrategia de contraseñas (Parte D) y qué pasaría si te saltas la primera.

La primera es añadir config/.env y backups/ a .gitignore antes de guardar secretos. La segunda es crear .env.example con los nombres de las variables y valores de ejemplo, sin contraseñas reales. La tercera es crear nuestro config/.env local y rellenarlo con los valores reales. La cuarta es cargar esas variables con `set -a; source config/.env; set +a` para que los scripts las utilicen.

Si nos saltamos la primera capa, podemos añadir el fichero de contraseñas por accidente al hacer git add y acabar publicándolo. Añadirlo después a .gitignore no elimina lo que ya haya quedado guardado en los commits.

## 14. ¿Por qué no escribimos la contraseña directamente en el comando docker run, aunque el script no se suba a Git?

Porque también puede quedar guardada en el historial de la terminal o aparecer en una captura o grabación. Que un fichero no se suba a Git no significa que la contraseña no pueda quedar expuesta por otro sitio.

Usando la variable ORACLE_PWD evitamos escribir el valor real en el comando que tecleamos. En el historial queda la referencia a la variable.

## 15. Si descubres tu contraseña en un commit ya publicado, ¿basta con borrarla en un commit nuevo? ¿Qué debes hacer?

No basta porque el commit anterior sigue formando parte del historial y se puede consultar. Hay que considerar que esa contraseña ya no es segura y cambiarla en el servicio donde se utiliza, actualizando también la configuración local.

Además hay que avisar al profesor y coordinar la limpieza del historial. No sería correcto cambiar el historial publicado por nuestra cuenta sin tener en cuenta a los compañeros que puedan estar trabajando sobre él.

## 16. ¿Por qué no usamos SPOOL ni @archivo.sql con sqlplus dentro del contenedor, y qué hicimos en su lugar?

Porque SQL*Plus está ejecutándose dentro del contenedor y las rutas que utiliza son las de ese contenedor. Un fichero SQL que está en nuestro repositorio no aparece automáticamente dentro, y un SPOOL se guardaría allí en lugar de en la carpeta de evidencias de Ubuntu.

Lo que hicimos fue enviar el SQL por la entrada estándar con docker exec -i y la redirección `< archivo.sql`. Para guardar el resultado utilizamos tee desde Ubuntu. Estas funciones sí se pueden usar dentro del contenedor si se preparan las rutas, pero en la práctica evitamos esa complicación.

## 17. ¿Qué hace WHENEVER SQLERROR EXIT SQL.SQLCODE al inicio de V000 y V001, y qué pasaría sin esa línea?

Hace que SQL*Plus se detenga cuando se produce un error SQL y devuelva un código de error. Así el script que aplica las migraciones puede detectar el fallo.

Sin esa línea podría seguir ejecutando las instrucciones siguientes y dejar la base a medias. En nuestro primer intento de V001 falló la referencia a fin_cuentas y después siguieron fallando los índices de fin_transacciones. Aun así se mostró el mensaje de migraciones aplicadas, lo que demuestra que no basta con mirar la última línea de la salida.

## 18. ¿Qué es una migración y por qué V000 y V001 no se deben editar una vez aplicadas?

Una migración es un script versionado que realiza un cambio en la estructura o configuración de la base de datos. En nuestro caso V000 crea los tablespaces y los usuarios, y V001 crea las tablas de los cinco negocios.

Una vez aplicada correctamente no se debería modificar, porque el fichero dejaría de representar lo que se ejecutó. Dos personas podrían tener una migración con el mismo nombre pero con cambios diferentes. Para cambios posteriores se crea otra migración, manteniendo el orden y el historial.

## 19. ¿Por qué en SQL Developer se usa el servicio FREEPDB1 y no FREE ni un SID?

Porque nuestros usuarios y tablas de trabajo están dentro de la base enchufable FREEPDB1. El servicio FREE corresponde al contenedor raíz, que es otro contexto de administración.

El SID identifica la instancia y no selecciona por sí mismo nuestra PDB de trabajo. Por eso configuramos Service name y escribimos FREEPDB1, para que la conexión llegue al lugar donde hemos creado los esquemas.

## 20. ¿Qué aporta SQLcl frente a SQL*Plus, y por qué un DBA debe dominar ambas?

SQLcl aporta comodidades como autocompletado, historial, formatos de salida más legibles y conexiones guardadas. También integra herramientas como Liquibase para trabajar con cambios en la base de datos.

SQL*Plus sigue siendo una herramienta habitual en servidores Oracle y sirve para ejecutar comandos y scripts con pocos requisitos. Un DBA debe conocer las dos porque puede tener SQLcl en su equipo, pero encontrarse con SQL*Plus al trabajar directamente en un servidor. En esta práctica también nos sirvió SQL*Plus para cambiar la contraseña cuando el comando fallaba en SQLcl.

## 21. ¿Por qué el curso pasa de Git Bash a Ubuntu en WSL 2? Da al menos dos problemas concretos de Git Bash que desaparecen en Ubuntu.

Porque vamos a trabajar con herramientas y scripts propios de un entorno Linux. Git Bash nos sirvió para aprender Git, pero no proporciona el mismo entorno que un servidor Linux.

Por ejemplo, Git Bash puede transformar las rutas que se pasan a Docker y provocar que una ruta pensada para el contenedor se interprete como una de Windows. También puede dar problemas con las terminales interactivas al usar docker exec -it. En Ubuntu trabajamos con rutas y terminales Linux, además de poder instalar las herramientas mediante apt.

## 22. ¿Por qué clonamos el repositorio en ~/oracle-database-lab y no trabajamos sobre la carpeta de Windows (/mnt/c/...)? ¿Y por qué recomendamos bash frente a zsh para los scripts del curso?

Porque trabajar dentro del sistema de archivos de Linux evita tener que cruzar continuamente al de Windows. Esto mejora el rendimiento de Git y de las operaciones con archivos, y reduce los problemas con permisos y finales de línea. En mi caso el repositorio está en ~/code/oracle-database-lab, que también está dentro de mi carpeta de usuario de Linux.

Usamos bash porque es la shell para la que están preparados los ejemplos y scripts del curso. zsh puede interpretar de otra forma algunas expresiones y patrones, por lo que utilizar bash facilita que los comandos se comporten igual para todos.
