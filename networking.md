# Limitaciones del modo NAT en un entorno de desaerrollo colaborativo para las maquinas virtuales de la empresa.

Esta configuracion por defecto tiene diferentes limitaciones, entre agnas se muestran las siguientes:

- **Aislamiento:** Esto hace referencia a que tanto la maquina anfitriona como el resto del equipo no puede acceder o tener comunicacion con la maquina virtual. Al mismo tiempo la comunicacion entre las diferentes maquinas virtuales es nula, lo que impide probar entornos de cliente-servidor, arquitectura de microservicios, etc.
- **Ip ficticia:** Al obtener una ip ficticia, no se puede conectar a la red de la empresa, ya que no la reconoce.
- **Trabajo extra:** Hay un "truco" que permite acceder a la maquina virtual, pero esto representa perdida de tiempo ya que se debe de hacer una configuracion manual llamada Port Forwarding para cada puerto, lo que puede causar diversos errores.

## Propuesta:

Hay ciertas caracteristicas de cada modo, que las hacen diferentes entre sí, como por ejemplo:

- **NAT:** este modo lo que hace es utilizar la ip del equipo anfitrion para salir a internet, pero como mencionamos antes, otra maquina virtual no puede acceder desde fuera a ella.
- **Red Interna:** en este modo como su nombre lo menciona lo que hace es crear una red privada, que permite que se comuniquen las maquinas que se encuentran desde un mismo anfitrion, no puede acceder a internet y tampoco se puede conectar con las maquinas de la red de la empresa.
- **Modo Puente:** este modo conecta directamente la maquina virtual a la red fisica, obtenienco una ip propia que le permite comunicarse con internet y con las demas maquinas que se encuentran en la red de la empresa.

### ¿Cual seria el modo idela para la empresa InnovaCloud Solutions?

En base a la dificultad detallada, que indica que el problema central es la comunicacion entre las maquinas virtuales, se sugiere el uso de la configuaracion de un Adaptador Puente, que solventará dicho problema, de manera que las diversas maquinas virtuales que forman parte de la red de la empresa se podran comunicar entre ellas.

---

## Documentacion Paso a Paso: Configuracion de IP Estática con Netplan

### Paso 1: Configurar VirtualBox en Modo Adaptador Puente

1. Apagar la maquina virtual.
2. Ir a **Configuración > Red > Adaptador 1**.
3. Seleccionar **Adaptador Puente** en el campo _Conectado a_.
4. Seleccionar la tarjeta de red física activa de la máquina anfitriona (Ethernet o Wi-Fi) y guardar los cambios.

---

### Paso 2: Identificar la interfaz de red en Linux

Iniciar la maquina virtual y ejecutar el siguiente comando para obtener el nombre exacto de la interfaz de red (por ejemplo: `enp0s3`):

```bash
ip link show
```

---

### Paso 3: Editar el archivo de configuracion de Netplan

Localizar y editar el archivo de red en `/etc/netplan/` (comúnmente llamado `50-cloud-init.yaml` o `01-netcfg.yaml`):

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

---

### Paso 4: Estructura del archivo YAML

Reemplazar o agregar la siguiente configuracion en el archivo YAML.

> **Nota:** Respetar la indentacion con espacios (no usar tabulaciones):

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 192.168.1.150/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
          - 1.1.1.1
          - 8.8.8.8
```

---

### Paso 5: Permisos, aplicacion y verificacion

1. **Ajustar permisos del archivo por seguridad:**

   ```bash
   sudo chmod 600 /etc/netplan/50-cloud-init.yaml
   ```

2. **Probar la sintaxis del archivo YAML:**

   ```bash
   sudo netplan try
   ```

3. **Aplicar los cambios de red:**

   ```bash
   sudo netplan apply
   ```

4. **Validar la asignacion de la IP estatica:**

   ```bash
   ip addr show enp0s3
   ```

5. **Verificar conectividad hacia la red y externa:**
   ```bash
   ping -c 4 192.168.1.1
   ping -c 4 google.com
   ```
