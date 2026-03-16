Planificación del trabajo (Lunes → Jueves)
 Lunes — Análisis de opciones y elección del método
Objetivos del día:

Evaluar las distintas formas de trabajar con contenedores en AWS.

Determinar qué método se ajusta mejor al proyecto y al plan de estudios.

Tareas realizadas:

Revisión de las alternativas disponibles en AWS:

AWS ECS (Elastic Container Service).

AWS Fargate (contenedores sin servidor).

Instancia EC2 con Podman/Docker.

Análisis de ventajas e inconvenientes de cada opción:

ECS/Fargate simplifican la gestión, pero requieren ampliar el plan de AWS y generan más coste.

EC2 + Podman ofrece mayor control, menor coste y encaja mejor con los requisitos del módulo.

Decisión final:

Se opta por instancia EC2 + Podman para mantener el proyecto dentro del presupuesto y tener control total del entorno.

Configuración inicial:

Apertura del puerto 8080 en reglas de entrada y salida del Security Group.

Preparación del entorno para comenzar el despliegue.

 Martes — Preparación del proyecto web y creación del Dockerfile
Objetivos del día:

Organizar los archivos de la página web.

Crear el Dockerfile necesario para el contenedor.

Tareas realizadas:

Revisión de la carpeta del proyecto web dentro de la instancia.

Creación del Dockerfile basado en Apache HTTPD:

dockerfile
FROM docker.io/library/httpd:2.4
COPY . /usr/local/apache2/htdocs/
Verificación de que todos los archivos HTML, CSS, imágenes y scripts están correctamente ubicados.

 Miércoles — Construcción y ejecución del contenedor
Objetivos del día:

Construir la imagen.

Crear y ejecutar el contenedor.

Verificar su funcionamiento.

Tareas realizadas:

Construcción de la imagen con Podman:

Código
podman build -t proyecto5 .
Comprobación de que la imagen se creó correctamente.

Ejecución del contenedor:

Código
podman start proyecto_5_container
Verificación del estado del contenedor:

Código
podman ps
Comprobación del puerto 8080 escuchando:

Código
ss -tulpn | grep 8080
 Jueves — Pruebas finales y demostración
Objetivos del día:

Validar que el contenedor sirve la web correctamente.

Demostrar que la página proviene del contenedor y no del Apache del sistema.

Tareas realizadas:

Acceso a la web mediante la IP pública de la instancia:

Código
http://<IP_PUBLICA>:8080
Visualización correcta de la página.

Prueba de que el contenedor es quien sirve la web:

Comprobación del proceso escuchando en el puerto 8080.

Revisión del directorio /var/www/html para confirmar que Apache local solo tiene la página por defecto.

Conclusión del día:

El despliegue funciona correctamente.

Se demuestra que el contenedor es el responsable del servicio web.

Se recopilan capturas y datos para el análisis comparativo final.
