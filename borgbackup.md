# Por Ángel García Pérez 1ºDAW

En la práctica de hoy vamos a trabajar con Borgbackup para realizar copias 
de seguridad.

Empecemos:

1º Instalar borg en nuestra máquina.

2º Crear un repositorio cifrado en la carpeta /tmp llamado repobackup.

- Como podemos ver se ha creado la carpeta para las copias de seguridad.

3º Crear una primera copia de seguridad del directorio personal.

4º Revisamos que la copia se ha realizado correctamente.

- He presionado antes la tecla de enter sin darme cuenta, disculpen las 
molestias.

- Como vemos se ha realizado la primera copia la cual he llamado copia1.

5º Modificar archivos en el directorio original.

- Una vez modificado el directorio original pasamos al siguiente paso.

6º Crear una segunda copia de seguridad.

7º Verificamos que la segunda copia se ha realizado correctamente.

- Ahí tenemos ambas copias realizadas.

8º Ahora compararemos las 2 copias de seguridad.

- Como podemos ver la primera copia no tiene datos y la segunda si.

9º Restaurar la primera copia de seguridad en un directorio restored, yo 
lo haré en la carpeta personal.

- Nos movemos a la carpeta restored ya que borg extrae los archivos en la 
ruta donde nos encontremos.

- Como podemos ver se ha restaurado la carpeta que guardamos inicialmente

10º Verificamos la restauración

- Perdón pero no recuerdo como es el comando para verificar la 
restauración.


### Copias de seguridad remotas (Modo Pull)

1º Crearemos el repositorio remoto en madrox
- Se me olvidó decir que hay que instalar borg también en esta máquina, lo 
haré en bookworm debido a problemas técnicos.

- Este repositorio lo he creado sin contraseña para ser más rápido.

- Tenemos que saber cual es la ip de nuestra máquina virtual para seguir 
con la actividad.

2º Realizar la copia de seguridad desde la máquina física.

- Ahora sí, había escrito mal el comando.

3º Verificar la copia de seguridad en la máquina virtual

4º Ahora restauramos la copia desde la máquina virtual a la máquina física


Y eso es todo
