1. Instalar BorgBackup
   |-> sudo apt install borgbackup
   |-> borg --version  # Verificar instalación

2. Crear un repositorio cifrado en `/tmp` llamado `repobackup`
   |-> borg init --encryption=repokey /tmp/repobackup
   |-> Introducir contraseña segura

3. Crear una primera copia de seguridad del directorio personal
   |-> borg create --progress /tmp/repobackup::copia1 /home/garperan

4. Verificar la copia de seguridad
   |-> borg list /tmp/repobackup
   |-> borg info /tmp/repobackup::copia1

5. Modificar archivos en el directorio original
   |-> touch /home/garperan/archivo_ejemplo.md
   |-> mkdir /home/garperan/carpeta_ejemplo
   |-> rm /home/garperan/archivo_ejemplo.md

6. Crear una segunda copia de seguridad
   |-> borg create --progress /tmp/repobackup::copia2 /home/garperan

7. Verificar la segunda copia de seguridad
   |-> borg list /tmp/repobackup
   |-> borg info /tmp/repobackup::copia2

8. Comparar las dos copias de seguridad
   |-> borg diff /tmp/repobackup::copia1 copia2

9. Restaurar la primera copia de seguridad en un directorio `restored`
   |-> mkdir -p /home/garperan/restored
   |-> cd /home/garperan/restored
   |-> borg extract /tmp/repobackup::copia1

10. Verificar la restauración
    |-> diff -r /home/garperan /home/garperan/restored

### Copias de seguridad remotas (Modo Pull)
1. Crear el repositorio remoto en `madrox`
   |-> borg init --encryption=repokey /home/garperan/borgrepo

2. Realizar la copia de seguridad desde la máquina física
   |-> borg create --progress --remote-path=borg1 <usuario>@<IP-de-madrox>:/home/garperan/borgrepo::copia1 /home/garperan

3. Verificar las copias de seguridad en `madrox`
   |-> borg list /home/garperan/borgrepo

4. Restaurar la copia desde `madrox` a la máquina física
   |-> mkdir -p /tmp/restored
   |-> borg extract --remote-path=borg1 <usuario>@<IP-de-madrox>:/home/garperan/borgrepo::copia1 /tmp/restored

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
borg1 <usuario>@<IP-de-madrox>:/home/garperan/borgrepo::copia1 /home/garperan/borgrepo

3. Verificamos las copias de seguridad en madrox
|-> borg list /home/garperan/borgrepo

4. Restauramos la copia desde madrox a la máquina física
|-> borg extract --remote-path borg1 <usuario>@<IP-de-madrox>:/home/<usuario>/borgrepo::backup1  /tmp/restored


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
