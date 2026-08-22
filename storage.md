## Problematica 1: Almacenamiento

### Problema:

Actualmente, la empresa InnovaCloud Solutions presenta múltiples fallos de disco en el servidor principal que causan pérdida de datos por falta de redundancia, lo que hace que la integridad y el acceso de la información esté en riesgo.

### Solución:

Para mitigar este riesgo, nuestra firma consultora recomienda implementar un arreglo RAID 5.

Si la empresa cuenta con un solo disco, deberán adquirirse por lo menos dos discos más para unirlos y formar el RAID, el cual permitirá segmentar la información y prevenir pérdida de los datos a largo plazo por medio de un proceso de redundancia de datos.

Como consultor se le recomienda utilizar RAID 5 porque es la opción más equilibrada entre protección de los datos y pérdida de espacio en el almacenamiento total ya que esta solo toma el espacio de un disco para el proceso de recuperación de datos.

### Implementación

Para el despliegue en los servidores Linux del cliente, el equipo de infraestructura deberá tener disponible tres discos físicos: `/dev/sdb`, `/dev/sdc`, `/dev/sdd` y ejecutar la configuración utilizando mdadm.

```bash
# Creamos el RAID 5:

sudo mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd
```

Ahora, después de creado el RAID 5 hay que montarlo, porque sino no podemos acceder a él ni poder crear o poner archivos en él.

Creamos la carpeta con permisos:

```bash
sudo mkdir /media/raid5
sudo chmod 777 -R /media/raid5
```

Montamos el disco, si esto no se configura por defecto deberá hacerse cada vez que se enciende el servidor:

```bash
sudo mount -t ext4 /dev/md0 /media/raid5/
```
