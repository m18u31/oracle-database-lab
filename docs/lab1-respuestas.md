**1.¿Cuál es la diferencia entre Working Directory, Staging Area y Local Repository? Da un ejemplo de un archivo pasando por las tres.**

Working directory es el espacio en disco donde se está trabajando. Staging area es el espao de preparación los cambios preparados para commit están en staging area. 
Local Repository es un repositorio que que se ubica en una máquina de forma local y donde se guarda el historial de commits. Cuando creamos un fichero, este se crea en el disco "Working Directory". Después de
crear/editar ese fichero se envía al área de preparación mediante el comando 'git add'. Finalmente se puede decir que el fichero ha entrado a formar parte del repositorio
cuando se ejecuta un 'commit'


**2.Si modificas un archivo pero no haces git add, ¿aparece ese cambio en tu próximo commit? Explica por qué.**

No, no aparece en el próximo commit. Esa modificación permanece en el disco y no pasa al área de preparación hasta que no se ejecute 'git add' sobre ese fichero.
Sólo los ficheros que están en el área de Staging pueden ser incluidos en el próximo commit.

**3.¿Por qué git status no mostraba las carpetas vacías que creaste en la Parte C? ¿Qué truco usamos para solucionarlo?**

Porque git no versiona carpetas, sólo archivos. el truco que utilizamos guen añadir un placeholder dentro de cada una de las carpetas de las cules queríamos tener 
seguimiento. En nuestro caso añadimos un fichero llamado .gitkeep.

**4.Explica con tus palabras qué es HEAD.**

HEAD es el puntero que apunta al último commit de la rama activa (la rama en la que estamos). Digamos que HEAD nos muestra la última versión actualizada de nuestos datos identificando el commit actual.


**5.¿Qué diferencia hay entre crear una branch con git switch -c y crear una carpeta nueva con mkdir? ¿Cómo lo comprobamos en la Parte G?**

git switch -c crea una rama en el sistema de versiones, no crea carpetas nuevas en el disco ni duplica datos sinó un puntero dentro de GIT. mkdir escribe en el disco 
creando una nueva carpeta. Esa carpeta es invisible para git y solo existe en el Working Directory. Lo comprobamos con el comando 'ls -la'. Vimos que el hecho de crear una nueva rama, no crea ningún fichero ni carpeta.

**6.Durante el conflicto de la Parte H, ¿qué representaba el contenido entre <<<<<<< HEAD y =======? ¿Y entre ======= y >>>>>>>?**

El contenido entre <<<<<<< HEAD y ======= es el contenido vigente del último commit. El contenido entre ======= y >>>>>>> es aquel contenido que ha entrado en conflicto
a la hora de hacer un merge. Este conflicto se produce porque hay dos versiones (versión de la rama actual VS versión de la rama que se quiere incorporar) que nacen de un antepasado común y es el usuario el que tiene que resolver el conflicto. 

**7.¿Por qué NO se debe hacer git commit --amend sobre un commit que ya se subió con git push?**

Porque un commit que ya se subió con git push, queda publicado y puede estar siendo objeto de trabajo por otro colaborador. Si mientras otros desarrolladores están
trabajando sobre una versión publicada, el autor hace un git --amend, potencialmente puede echar a perder el trabajo que otros colaboradores hayan realizado sobre 
ese commit

**8.Si borras por accidente la carpeta .git de tu proyecto, ¿qué se pierde exactamente? ¿Se pierde también el código fuente que está en el disco?**

Se pierde la base de datos de GIT. El código fuente que está en el disco no se pierde, solo se pierde el control de versiones, histórico, commits etc.

**9.Explica con tus propias palabras la diferencia entre Git y GitHub, sin usar la palabra "nube".**

GIT es un software para control de versiones en un repositorio. GitHub es un agregador de repositorios en remoto. Principalmente añade la funcionalidad de conectar
a internet un repositorio para que se pueda trabajar sobre él de forma remota y colaborativa


**10.¿Por qué no se debe subir un archivo .env con contraseñas reales a un repositorio, aunque el repositorio sea privado?**

Es una mala práctica en general almacenar contraseñas en texto plano aunque el repositorio sea privado. Una situación de riesgo puede ser que el repositorio sea privado
hoy pero no en el futuro. También el hecho de que se privado no signifca que no haya colaboradores trabajando en el repositorio y con acceso a credenciales. Es un riesgo
grave ya que se pierde la custodia de las credenciales.


**11.Un compañero te dice: "hice push y ahora GitHub me rechaza el segundo push con 'non-fast-forward'". ¿Qué ha ocurrido probablemente y qué comando ejecutarías primero?**

Probablemente haya suciedido que la versión en Github haya cambiado desde que mi compañero hizo el último push. Tiene que descargarse a su repositorio local el último commit y los cambios remotos almacenados en GitHub con el comando 'git pull'



**12.¿Qué tipo de Conventional Commit (feat, fix, docs, test…) usarías para: añadir un índice de rendimiento a una tabla, corregir una restricción mal definida, y actualizar el README?**

Añadir un índice de rendimiento a una tabla - perf
Corregir una restricción mal definida - fix
Actualizar el README - docs


