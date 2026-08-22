# Resolución de Caso de Estudio: Optimización de la Infraestructura de Red para "InnovaCloud Solutions"

En esta actividad presentaremos nuestras soluciones a los problemas presentados en el documento sobre InnovaCloud Solutions.

## Equipo Malas Ideas, Integrantes:

- Rodrigo Alexis Mejía Rivas MR230247
- Leonardo Enrique Flores Coto FC230433
- André Emanuel Preza Deraz PD230540
- Valeria Liseth Paredes Lara PL230400

## Enlace del video:
https://youtu.be/iwDIb-v1XuE

## Resumen General del Plan de Red:

Para resolver las limitaciones del modo NAT—como el aislamiento de las máquinas virtuales, la asignación de IPs no reconocidas por la empresa y la complejidad del reenvío de puertos (Port Forwarding)—se propone migrar la infraestructura virtual de desarrollo de InnovaCloud Solutions al modo Adaptador Puente (Bridged Adapter). A diferencia del modo NAT o de Red Interna, la configuración en puente permite que cada máquina virtual obtenga su propia identidad dentro de la red física corporativa, habilitando la comunicación bidireccional directa entre desarrolladores, servicios y otras VMs con acceso continuo a Internet. Para garantizar la estabilidad de los entornos de trabajo y evitar cambios dinámicos de dirección, el plan de acción contempla la asignación manual de una IP IPv4 estática, la configuración de la puerta de enlace predeterminada y la definición de servidores DNS mediante Netplan en el sistema operativo Linux Ubuntu Server, seguido de la correspondiente validación de conectividad.

## Resumen general del Plan de Paquetes:

La gestión manual de paquetes en InnovaCloud Solutions genera inconsistencias entre servidores y aumenta el riesgo de errores por dependencias rotas o configuraciones incorrectas. Para resolverlo, se propone implementar repositorios espejo locales que centralicen las descargas y aseguren versiones uniformes en todos los equipos. Esta solución optimiza el ancho de banda, reduce el tráfico externo y brinda mayor control al permitir que el equipo de TI audite y valide los paquetes antes de distribuirlos, garantizando así eficiencia y estabilidad en la infraestructura.

## Resumen general del Plan de Almacenamiento:
Para resolver la vulnerabilidad del servidor principal ante los fallos de disco, se implementará un arreglo RAID 5 que garantice la redundancia de los datos. Esta solución mitigará el riesgo de pérdida de información a largo plazo utilizando al menos tres discos físicos, ofreciendo a InnovaCloud Solutions el equilibrio óptimo entre una alta protección de su información crítica y el aprovechamiento eficiente del espacio total de almacenamiento.

## Resumen general del Plan de Verificación y Diagnóstico de Red:
Para solucionar las demoras operativas y las vulnerabilidades de seguridad generadas por la falta de estandarización al resolver incidentes, se diseñó un procedimiento de diagnóstico estructurado en cuatro fases. Este nuevo protocolo estandarizado permite al equipo técnico auditar de manera secuencial la identidad del servidor en la red, las rutas de tráfico, la alcanzabilidad de los destinos y el estado de los puertos expuestos, logrando así reducir los tiempos de respuesta y cerrar brechas de seguridad de forma proactiva.

