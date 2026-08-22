# Packages
## Problema
La gestion manual en la infraestructura actual de innovaCloud Solutions presenta varias deficiencias. Cada administrador instala paquetes de forma independiente, pudiendo generar diferencias entre servidores e inconsistencias entre versiones; ademas de lo mas preocupante, y es que la instalacion manual aumenta la probabilidad de error, pudiendo generar dependencias rotas o configuraciones incorrectas.

## Propuesta
Para solucionar esta problematica, se recomeinda implementar repositorios espejos locales, ya que esto permitira centralizar las descargas y garantizar cosistencias entre las disintas versiones de los paquetes.

Aplicar esta solucion traera grandes beneficios en los servidores internos, ya que se descargaran desde el mirror local y reducira asi el trafico externo. Ademas de que todos los equipos utilizarian una misma fuente de paquetes, generando una mayor consistencia al evitar discrepancia entre versiones. Asi teniendo un mayor control, ya que el equipo de TI puede auditar y validar los paquetes antes de querer distribuirlos. Todo esto permitiria una gran optimizacion en el ancho de banda, permitiendo que en una sola descarga externa se pueda replicar internamente para cada uno de los servidores.

## Solucion 
Para solcucionar la problematica se deben de usar sistemas de paqueteria, en este caso se usaran ips ficticias para demostrar la solucion:

> sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup

tras esto debemos de apuntar al repositorio espejo, usando el comando sed podemos reemplazar las direcciones web por la IP del servidor local de la empresa en el archivo de configuracion: 

> sudo sed -i 's/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/192.168.10.50\/ubuntu\//g' /etc/apt/sources.list

Por ultimo, se actualizan las dependencias y se aplican las instalaciones:

>sudo apt update

>sudo apt upgrade -y


