1. Instalar BorgBackup
|-> sudo apt install borgbackup.
   
2. Crear un repo de copias de seguridad cifrado en /tmp llamado `repobackup`
|->  borg init encryption=contraseña /tmp/repobackup.

3. Crear una primera copia de seguridad de un directorio en el sistema (la carpeta personal del usuario).
|-> borg create /tmp/repobackup::copia1 /home/garperan.

4. Verificar que la copia de seguridad se ha creado correctamente
|-> borg list /tmp/repobackup::copia1 y borg info /tmp/repobackup::copia1.

5. Modificar archivos en el directorio original agregando y/o eliminando cosas
|-> touch archivo_ejemplo.md o mkdir carpeta_ejemplo o rm archivo_ejemplo.

6. Crear una segunda copia de seguridad del directorio original
|-> borg create /tmp/repobackup::copia2 /home/garperan.

7. Verificar que la segunda copia de seguridad se ha creado correctamente
|-> borg list /tmp/repobackup::copia2 o borg info /tmp/repobackup::copia2

8. Usar el comando `diff` para comparar las 2 copias de seguridad
|-> borg diff /tmp/repobackup::copia1::copia2

9. Restaurar la primera copia de seguridad en un directorio diferente llamado `restored`
|-> borg extract  /tmp/repobackup::copia1

> [!IMPORTANT]
> Debemos ejecutar el comando en el directorio restored ya que Borg nos extrae los archivos en el directorio
> donde nos encontramos

10. Verificar que la restauración se ha realizado correctamente
|-> borg diff /home/garperan/restored o ls/tree /home/garperan/restored


### Copias de seguridad Remotas (Modo Pull)

Averigua cómo se usaría `borgbackup` para hacer copias de seguridad en
un repositorio remoto, explicando en detalle cómo sería el procedimiento
a medida que lo realizas. Los gráficos de más abajo muestran los dos
modos de copia remota que puedes llevar a cabo, cuyos nombres dependen
de dónde se encuentra ubicado el comando que realiza las copias

Utiliza `madrox` como máquina para alojar el repositorio remoto y haz
una copia de alguna carpeta que tengas en tu máquina física. Recuerda
que para poder conectar tu máquina física a `madrox` necesitarás que la
red que comparten ambas máquinas se encuentre en modo `bridge`
Finalmente, prueba a restaurar una de esas copias en la carpeta `/tmp`
de tu máquina física

Usa `tmux` para dividir ahora la pantalla en tres paneles:

- Izquierda: para explicar lo que estás haciendo mediante el comando
  `nano -b0 borgbackup.md`, lo que te permitirá guardar tus explicaciones
  en un fichero markdown
- Derecha arriba: para la máquina de cuyos datos se quieren hacer copias
- Derecha abajo: para la máquina en la que se quieren guardar las copias

#### Push Backup / Pull Restore

```asciiart
              ┌────────────┐
              │            │
              │   madrox   │ repositorio
              │            │
              └───o──┬──▲──┘
                  │  │  │
                  │  │  │
──────────────────┴──┼──┼────────────────o─────────────── virbr0
                     │  │                │
                     │  │  backup   ┌────┴────┐
                     │  └───────────┤         │
                     │              │   M.F.  │ datos a copiar/restaurar
                     └─────────────►│         │ comando borg
                       restore      └─────────┘
```

#### Pull Backup / Push Restore

```asciiart
              ┌────────────┐
              │            │ repositorio
              │   madrox   │ comando borg
              │            │
              └───o──┬──▲──┘
                  │  │  │
                  │  │  │
──────────────────┴──┼──┼────────────────o─────────────── virbr0
                     │  │                │
                     │  │  backup   ┌────┴────┐
                     │  └───────────┤         │
                     │              │   M.F.  │ datos a copiar/restaurar
                     └─────────────►│         │
                       restore      └─────────┘
```
