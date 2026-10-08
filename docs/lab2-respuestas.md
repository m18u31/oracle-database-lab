# Lab 2 — Preguntas de comprobación

**Nombre:** Miguel Ángel García Ogando
**Profesor:** Richard Aviles Lopez
**Reviewer:** César Pérez Montero

---

## 1. ¿Por qué un Issue sin criterios de aceptación es un problema, aunque la descripción general parezca clara?

Un Issue sin criterios de aceptación no es funcional en tanto que no se puede desprender de él una serie de criterios concretos mediante los cuales se pueda dar por solucionado el issue. Si no hay una guía que indique al revisor qué es lo que tiene que revisar y como comprobarlo de forma concreta, no tendrá las herramientas necesarias para revisar correctamente el Issue.


## 2. Explica la diferencia entre "Refs #N" y "Closes #N" en un mensaje de commit o en la descripción de un Pull Request.

- **`Refs #N`** indica una referencia de una PR a un Issue concreto. Esa referencia se utiliza luego en los commit relacionados con ese Issue y sirve para automatizar acciones y cambios de estado. Agrupa diferentes acciones en torno a un Issue
- **`Closes #N`** También es una referencia pero en este caso sirve para el cierre del Issue. Una vez se aprueba un PR y se ejecuta merge, se cerrará automáticamente el Issue si la etiqueta Closes #N existe en el PR

## 3. ¿Qué ocurre exactamente si intentas hacer git push directamente sobre una branch main protegida? ¿Es un error tuyo o un fallo del sistema?

Al estar protegida el push no se realiza y salta la protección en forma de error. En nuestro caso recibimos error: GH006: Protected branch update failed for refs/heads/main. De todas formas hay que tener cuidado con la configuración de la protección. en mi caso olvidé marcar "Do not allow bypassing the above settings" y realicé un push accidental en la rama main porque al no estar marcada esa opción, el propietario del repositorio puede saltarsela. Tuvimos que resolver el conflicto generado y tuvo que ser aprobado por mi compañero. 

Por tanto no es un fallo del sistema. El sistema se comporta exactamente como se le pide.

## 4. Un compañero te dice: "he aprobado el PR sin mirar los archivos, total ya me fío". ¿Qué riesgo tiene esa forma de revisar?

Es una mala práctica. El sentido de que existan las PR es precisamente para que los commits sean revisado por otros compañeros y así reducir errores. Si no se ejecuta correctamente pierde todo el sentido.

## 5. Si el reviewer pide un cambio y tú ya habías hecho push de tu branch, ¿tienes que abrir un Pull Request nuevo? Explica qué ocurre técnicamente con el PR existente cuando haces un nuevo commit.

No. Los commits se van acumulando en el mismo PR. Un Pull Request no guarda una lista fija de commits: compara una branch (head) con otra (base). Si hago nuevos commits en la misma branch y los subo con `git push`, el PR los incorpora automáticamente y se actualizan sus pestañas *Commits* y *Files changed*.


## 6. Describe con tus palabras la diferencia entre Merge commit, Squash and merge y Rebase and merge. ¿Cuál usarías para una branch con commits "wip", "fix", "fix2", "ok ya"?

La diferencia está principalmente en cómo se incorporan los commits de una branch a otra y cómo queda el historial.

- **Merge commit** mantiene todos los commits originales y crea uno nuevo que une ambas ramas.
- **Squash and merge** agrupa todos los commits de la branch en uno solo, dejando un historial más limpio.
- **Rebase and merge** incorpora los commits individualmente sobre la rama de destino, manteniendo un historial lineal.

En el caso del ejemplo utilizaría Squash and merge porque los commits tienen nombres poco descriptivos y realmente no aportan información útil por separado. De esta forma se agrupan en un único commit con un mensaje que explique correctamente los cambios realizados.

## 7. ¿Por qué borrar una branch después del merge no elimina el trabajo realizado en ella?

Porque al hacer el merge los cambios de esa branch ya se han incorporado a la rama de destino, normalmente main. Por tanto, aunque eliminemos la branch original, el trabajo realizado sigue formando parte del proyecto y de su historial.

Lo que estamos eliminando es la referencia a esa rama, no los cambios que ya se han integrado.

## 8. ¿Qué información debería contener siempre la descripción de un Pull Request, como mínimo?

Una PR debería explicar qué cambios se han realizado, por qué se han hecho y cómo puede el reviewer comprobar que funcionan correctamente.

También debería incluir una referencia al Issue que se pretende resolver, si existe, y los criterios que deben cumplirse para considerar que el trabajo está terminado.

El objetivo es que el reviewer tenga toda la información necesaria para revisar la PR sin tener que preguntar continuamente al autor qué ha hecho o cómo debe comprobarlo.

## 9. Un reviewer escribe solo "esto está mal" como comentario. ¿Qué le falta a ese comentario para ser útil? Reescríbelo tú con un ejemplo inventado.

Le falta explicar qué es exactamente lo que está mal, por qué supone un problema y qué debería cambiarse para solucionarlo.

Un comentario así no sirve de mucho porque obliga al autor a interpretar qué es lo que quiere decir el reviewer.

Por ejemplo, en lugar de escribir "esto está mal", podría comentar: "La función no comprueba si el usuario ha introducido un valor vacío. Deberías añadir una validación antes de guardar los datos para evitar que se registren usuarios sin nombre".

De esta forma el autor sabe dónde está el problema y qué tiene que hacer para corregirlo.

## 10. ¿Qué diferencia hay entre que main esté protegida y que simplemente el equipo "se ponga de acuerdo" en no hacer push directo?

La diferencia es que en el primer caso es el propio sistema el que impide que se realicen cambios directamente en main, mientras que en el segundo dependemos de que todos los compañeros respeten el acuerdo.

La protección evita errores humanos y obliga a seguir el procedimiento establecido. Como comprobamos durante el laboratorio, una configuración incorrecta puede permitir que alguien se salte las restricciones y haga un push accidental.

Por tanto, es mucho más seguro establecer las restricciones técnicamente que confiar únicamente en que nadie se equivoque.

## 11. Explica con un ejemplo propio la diferencia entre un comentario issue: (blocking) y uno nitpick: (if-minor) en formato Conventional Comments.

La diferencia está en la importancia del problema que señala el reviewer y si es necesario resolverlo antes de aprobar la PR.

- **`issue: (blocking)`** indica un problema que debe solucionarse antes de hacer el merge. Por ejemplo: "La función permite registrar usuarios sin contraseña. Es necesario añadir una validación antes de aprobar la PR".
- **`nitpick: (if-minor)`** indica una observación menor que no impide aprobar la PR. Por ejemplo: "Sería mejor cambiar el nombre de esta variable por otro más descriptivo para facilitar la lectura del código".

En el primer caso es necesario corregir el problema porque puede afectar al funcionamiento del programa. En el segundo se trata de una sugerencia de mejora que no tiene por qué bloquear la aprobación.

## 12. Si tu próximo commit es feat!: cambia la firma de la función principal de la API, ¿qué tipo de versión SemVer se dispara y por qué?

Se dispara una actualización **MAJOR** de SemVer porque el símbolo `!` indica que estamos introduciendo un cambio que rompe la compatibilidad con versiones anteriores.

En este caso, al cambiar la firma de la función principal de la API, los programas que utilizaban la versión anterior podrían dejar de funcionar si no adaptan sus llamadas a la nueva función.

Por tanto, si estábamos en la versión 1.2.3, pasaríamos a la 2.0.0, indicando que se han realizado cambios importantes que pueden requerir modificaciones por parte de los usuarios de la API.

## 13. ¿Por qué abrir un Draft PR desde el primer commit estructural puede ahorrar tiempo al equipo, aunque parezca más lento para el autor individual?

Porque permite que el resto del equipo pueda ver desde el principio el trabajo que estamos realizando, aunque todavía no esté terminado.

De esta forma los compañeros pueden detectar errores, hacer sugerencias o advertirnos de posibles conflictos antes de que hayamos avanzado demasiado.

Aunque pueda parecer que se pierde tiempo compartiendo un trabajo que todavía está incompleto, en realidad estamos evitando tener que rehacer partes importantes cuando ya hemos terminado.

Además, el Draft PR deja claro que el trabajo está en desarrollo y todavía no está preparado para ser aprobado y fusionado con main.

