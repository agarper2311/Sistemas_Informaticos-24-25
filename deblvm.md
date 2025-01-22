Hola, el dia de hoy vamos a realizar la instalación de una máquina virtual 
Debian. 

Este trabajo consiste en que dicha máquina se pueda administrar el 
almacenamiento con la tecnología 'deblvm', para ello usaremos los comandos 
pertinentes para crear la máquina con los comandos de virsh.

Debemos tener en cuenta que crearemos 2 discos, uno de 6 GB y otro de 4 GB, 
comencemos con la instalación:

- Comando virt-install para crear la máquina virtual.
- Parámetro --name -> Es el nombre de la máquina a nivel de libvirt para 
diferenciarla de las demás.
- Acabamos de asignar 1 GB de RAM mediante ese parámetro.
- Ahora hemos creado 2 discos duros con el parámetro --disk size.
- Acabamos de decir que no queremos un hardware gráfico.
- Le hemos dicho donde se encuentra la ISO que vamos a usar, en mi caso la 
versión 12.8.0.
- Como no tenemos hardware gráfico, le decimos que queremos soporte para la 
consola serie y un tema oscuro.
- Le decimos que autodetecte el tipo de sistema operativo.


En este apartado escogemos la primera opción.

Aquí seleccionamos nuestra ubicación.

En este apartado configuraremos nuestro teclado (que no haya nadie 
diciendo "es que no encuentro mi teclado Logitech...", no es neurocirugía).

Como nombre de máquina a nivel de sistema le asignaremos deblvm.

Nombre de dominio lo dejamos vacío.

La contraseña de root la dejamos vacía también porque queremos que nuestro 
usuario tenga permisos de administrador.

Ponemos nuestro nombre completo y después nuestro lorzaman que también 
usaremos como contraseña.

Seleccionamos la hora de Madrid.

Aquí elegiremos la segunda opción.

Y seleccionamos el primer disco.

Separamos la partición /home del resto.

Le decimos que guarde los cambios y que configure el LVM.

Dejaremos el tamaño que viene por defecto 5,9.

Guardamos los cambios.

En este punto le decimos que no queremos analizar otro medio de instalación 
ya que vamos a descargar los paquetes desde el servidor.

En el ordenador de casa no tengo configurado un proxy, en el oredenador de 
clase si lo pondríamos para no saturar la red.

aquí solo dejaremos las 2 opciones que hay por defecto.

Seleccionamos que Sí queremos instalar el GRUB en nuestro disco principal.

Elegimos sda aunque puede aparecer como vda.

Reiniciamos para que se guarden los cambios


## Instalación completada.

### Diferencias entre la máquina física de clase y ésta máquina virtual.

Voy a copiar y pegar la información de la máquina de clase para compararla 
con esta:

jcromero@daw1-1xx:~$ sudo fdisk -l
      Disco /dev/vda: 223,57 GiB, 240057409536 bytes, 468862128 sectores
      Modelo de disco: KINGSTON SA400S3
      Unidades: sectores de 1 \* 512 = 512 bytes
      Tamaño de sector (lógico/físico): 512 bytes / 512 bytes
      Tamaño de E/S (mínimo/óptimo): 512 bytes / 512 bytes
      Tipo de etiqueta de disco: gpt
      Identificador del disco: F7D56B56-3C5A-447B-8671-8CD63E21B58B

      Disposit. Comienzo Final Sectores Tamaño Tipo
      /dev/vda1 264192 1286143 1021952 499M Entorno de recuperación de Windows
      /dev/vda2 1286144 1490943 204800 100M Sistema EFI
      /dev/vda3 1490944 239744897 238253954 113,6G Datos básicos de Microsoft
      /dev/vda4 239745024 240893951 1148928 561M Entorno de recuperación de Windows
      /dev/vda5 240896000 466860031 225964032 107,7G Sistema de ficheros de Linux
      /dev/vda6 466860032 468860927 2000896 977M Linux swap


1º Diferencia:
- Máquina física: Tiene 2 sistemas operativos instalados y no tiene la 
partición del LVM.

- Máquina virtual: Tiene 1 sistema operativo y tiene la partición de LVM 
para administrar los discos.

2º Diferencia:
- Máquina física: Contiene varias particiones las cuales detallan un poco 
para que son.

- Máquina virtual: Tiene 3 particiones y solo nos indica para qué se está 
usando de manera muy sencilla.

Ahora pasaremos a la creación de un Phisical Volume o `PV`, con el 
siguiente comando.

Nos muestra un mensaje confirmando de que se ha realizado con éxito la 
creación.

Ahora tenemos que añadir el Volumen Fisico al Grupo de Volumenes con el 
siguiente comando.

Nos vuelve a confirmar de que se ha realizado con éxito.

Ahora agregaremos 3 GB usando el siguiente comando.

Ahora redimensionaremos el `VG` para que no de fallos lo haremos con el 
siguiente comando.

Como podemos observar todo ha salido bien.

Hay que tener en cuenta de que cuando hicimos los pasos para terminar la 
instalación de la máquina, nos preguntaba donde queríamos que se instalase 
el GRUB, el cuál se guarda en la partición /boot. /boot se encarga de 
almacenar todos los archivos necesarios para el arranque del sistema, 
además permite que GRUB pueda acceder fácilmente a los archivos.

Ahora confirmaremos de que todo está correctamente con el siguente comando.

Aquí podemos ver cuanto espacio tiene ahora cada partición.

Una vez realizado estos pasos, debemos agregar 2 GB al segundo disco, con 
lo cual debemos apagar nuestra máquina virtual con el siguiente comando.

Y eso es todo.
Ahora con la ayuda de un comando agregaremos esos 2 GB necesarios para 
continuar con la práctica.

Al no poner que queremos 2GB más, tira error.

Ahora encenderemos la máquina virtual con el siguiente comando para poder 
ver que se ha realizado este procedimiento correctamente.

Ejecutamos el siguiente comando para ver los cambios.

Lógicamente para ver los cambios debemos redimensionarlo de nuevo.

Ahora agregaremos 2GB al disco principal, para ello volveremos a apagar la 
máquina.

Procedemos a añadir esos 2GB al disco principal.

Ya hemos redimensionado el disco principal.

Volvemos a encender la máquina virtual.

Ejecutamos de nuevo el siguiente comando 

Ahora verificamos que se ha hecho correctamente.

Ahora añadimos el nuevo PV al VG con el siguiente comando.

Ahora para no dejar espacio sin usar vamos a ampliar el LV con el siguiente 
comando.



