# Verificación y Diagnóstico de Red

## Contexto de la problemática:

Cuando los servicios de InnovaCloud Solutions falla o la red se comporta de forma extraña, la empresa no tiene un procedimiento establecido que indique por dónde empezar a investigar si los servicios estan comprometidos o si solamente hay que levantarlos. Cada técnico revisa lo que su experiencia personal le dicta, en el orden que le parece, y eso genera consecuencias directas para el negocio. Por ejemplo un puerto puede estar abierto cuando no debería estarlo, lo que es una puerta de entrada segura para un atacante, también estos diagnósticos pueden tardar mas de lo esperado para solucionarlo.
Podemos concluir que el problema no es como tal la falta de herramientas, ya que existen varias listas de comandos para brindar diagnostico, el problema radica en la falta de procedimientos estandarizados.

## Propuesta a la problemática:

Proponemos un procedimiento de verificación en 4 pasos, pensado para responder siempre la misma pregunta de fondo: ¿en qué punto exacto está fallando la red? Porque la conectividad no es una sola cosa sino es una cadena de preguntas que podemos hacernos, ¿el equipo tiene dirección IP? ¿sabe a dónde enviar el tráfico? ¿el servicio de destino responde? ¿el nombre de dominio se traduce correctamente?, y cada eslabón se revisa con una herramienta distinta.

1. Confirmar que el servidor tiene identidad en la red
   Antes de sospechar de nada externo, se confirma que la propia máquina tiene su configuración de red en orden.
   ip a
   El comando muestra las direcciones IP asignadas a cada interfaz de red del servidor. Si esto no está bien, nada de lo que sigue tiene sentido revisarlo todavía.
2. Confirmar que el servidor sabe a dónde enviar el tráfico
   ip route
   EL comando muestra la tabla de enrutamiento: por dónde debe salir el tráfico que va hacia fuera de la red local. Un problema muy común es la ausencia de una ruta por defecto ya que el servidor "sabe hablar" con su red local, pero no sabe cómo llegar a Internet ni a otras redes de la empresa.
3. Confirmar alcanzabilidad real
   ping -c 4 <ip_destino>
   El comando envia paquetes de prueba y confirma si el destino responde. Aclaración importante para InnovaCloud: que un ping falle no siempre significa que el servicio esté caído ya que muchos firewalls bloquean este tipo de tráfico de prueba por seguridad, aun cuando el servicio funciona perfectamente. Por eso este paso se usa para confirmar que se llega a la máquina, no para confirmar que el servicio en sí está funcionando; para eso están los pasos siguientes.
   Si el destino se identifica por nombre en lugar de IP, se agrega una verificación de resolución de nombres:
   cat /etc/resolv.conf
   resolvectl status
   este comando confirma qué servidor de DNS está usando la máquina y si está respondiendo. Un fallo aquí podria mostar un mensaje del tipo "no se encuentra el dominio" aunque la red esté funcionando perfectamente es un tipo de falla distinto y muy fácil de confundir con un problema de conectividad si no se sabe dónde mirar.
4. Confirmar qué servicios están realmente activos y expuestos
   systemctl status <nombre_del_servicio>
   ss -tuln
   systemctl confirma si el servicio está corriendo en la máquina. ss muestra qué puertos están abiertos y escuchando conexiones es la forma de auditar, desde adentro del propio servidor, qué está expuesto sin generar tráfico hacia afuera.
   Complementariamente, para tener la perspectiva de "cómo ve un tercero al servidor desde afuera":
   nmap <ip_del_servidor>

## Por qué esto resuelve el problema del cliente

Con este procedimiento, cualquier persona conocedora al tema sin importar su nivel de experiencia previa podría seguir los mismos 4 pasos en el mismo orden y llega al mismo diagnóstico. Esto reduce el tiempo de resolución de incidentes y agrega una revisión rutinaria de puertos expuestos que hoy no existe, cerrando una brecha de seguridad silenciosa.

