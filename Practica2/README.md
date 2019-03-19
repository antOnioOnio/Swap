# Práctica 2. Clonar la información de un sitio web
------

## Objetivos de la práctica:
Los objetivos concretos de esta segunda práctica son:

1. Aprender a copiar archivos mediante ssh
2. Clonar contenido entre máquinas
3. Configurar el ssh para acceder a máquinas remotas sin contraseña 
4. Establecer tareas en cron
------
##SSH 
Antes de meternos de lleno en ssh conviene definir que es exactamente SSH, aunque seguramente esta información a estas alturas pueda ser redundante, nunca esta de más. 
SSH es el nombre de un protocolo cuya principal función es el acceso remoto a otras máquinas (servidores) por medio de un canal seguro en el que toda la información está cifrada. Además, SSH permite copiar datos de forma segura (tanto archivos sueltos como simular sesiones FTP cifradas), gestionar claves RSA para no escribir contraseñas al conectar a los dispositivos y pasar los datos de cualquier otra aplicación por un canal seguro tunelizado mediante SSH y también puede redirigir el tráfico para poder ejecutar programas gráficos remotamente. 
####Copiar archivos mediante SSH.
Entre las opciones que ofrece SSH, en este apartado vamos a centrarnos en como transferir archivos de una máquina a otra. 
Existen varias formas y comandos para enviar archivos de una máquina a otra, scp es una alternativa que hace una función bastante similar. 
En el caso que necesitemos crear un tar.gz de un equipo y dejarlo en otro pero no disponemos de espacio en disco local, podemos usar ssh para crearlo directamente directamente en el equipo destino, esta es la gran diferencia con SCP (secure copy protocol).
El comando a ejecutar para poder crear directamente un archivo en una máquina remota sería el siguiente:

    tar czf - directorio | ssh equipodestino 'cat > ~/tar.tgz'

Básicamente le estamos diciendo al comando tar que use stdout como destino y mandamos con un pipe la salida ssh. Éste coge la sálida del tar y la escribe como fichero en nuestra máquina remota. Tal como vemos en la siguiente imagen, se ha creado un directorio en la máquina *antonio* con un un archivo de texto plano, este se envía a la máquina *antonio2* usando el comando visto anteriormente, tras introducir la contraseña se actualiza la segunda máquina, descomprimimos y ya disponemos de nuestro archivo.

![tar](./imagenes/envioTarSSH.png)

####Comando SCP

El comando SCP hace uso de SSH para hacer copias seguras y encriptadas. Este comando esta presente tanto en Linux, Mac y Windows mediante WinSCP.
Se puede utilizar scp para copiar archivos de un ordenador local a otro remoto, también se puede copiar del remoto al local y también se puede copiar entre dos remotos, mientras estas conectado a un tercer ordenador, y el tráfico no pasará por el ordenador en que estás.
A continuación se muestra un ejemplo básico del uso de este comando.

![scp](./imagenes/comandoSCP.png)

Si el lector esta interesado en un conocimiento más profundo de este comando puede encontrar mas informacion,[aqui](https://geekytheory.com/copiar-archivos-a-traves-de-ssh-con-scp) y [aqui](https://www.garron.me/es/articulos/scp.html).

####Inconvenientes

Aunque SSH es un protocolo muy seguro para la transferencia de información, el gran inconveniente que nos encontramos a la hora de usarlo es mantenimiento de servidores es que estos comandos copian toda la información aunque no haya habido cambios en estos. Esto para pequeños archivos y bases de datos pequeñas puede no ser un problema, pero a la hora de escalarlo y tratas con grandes bases de datos o archivos de un peso considerable acarrearía un problema de demora. Es por ello que en estas circunstancias se usan otras herramientas de copia incremental tales como Rsync.
