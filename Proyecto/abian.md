# Proyecto Contenedores en la Nube (AWS)  
**Autor:** Abián García Martel  
**Fecha:** Marzo 2026  

---

## 📋 Índice  
1. Introducción  
2. Objetivos  
3. Arquitectura  
4. Instalación y configuración  
5. Problemas encontrados  
6. Gestión del contenedor  
7. Monitorización  
8. Conclusiones  
9. Tecnologías utilizadas  

---

## 1. Introducción  
Este proyecto consiste en la implementación de un contenedor en la nube utilizando **AWS EC2 + Podman**, en lugar de AWS ECS/Fargate.  
La decisión se tomó para mantener **mayor control del entorno**, evitar ampliar el plan de AWS y **reducir costes**, aunque implique un proceso ligeramente más complejo.

---

## 2. Objetivos  
- Estudiar el funcionamiento de los contenedores en la nube.  
- Comparar distintos métodos de implementación en AWS.  
- Elegir la opción más directa y económica.  
- Crear un contenedor con una página web Apache.  
- Ejecutarlo en una instancia EC2.  
- Validar que la página se sirve desde el contenedor.  

---

## 3. Arquitectura  
AWS EC2 (Instancia Ubuntu)
│
├── Podman (Motor de contenedores)
│   └── Contenedor Apache + Página Web
│
└── Acceso público por puerto 8080

---

## 4. Instalación y configuración  

### 4.1 Apertura de puertos  
Antes de abrir la instancia se habilitó el **puerto 8080** en entrada y salida.

### 4.2 Preparación de la carpeta de la página  
Se revisó la carpeta donde se encontraba la página web a desplegar.

### 4.3 Creación del Dockerfile  
Dockerfile sencillo basado en Apache, incluyendo la carpeta de la página.

### 4.4 Construcción del contenedor  
Se construyó la imagen con Podman incluyendo la página web.

### 4.5 Ejecución del contenedor  
Se ejecutó el contenedor exponiendo el puerto 8080.

### 4.6 Comprobaciones  
- Se verificó que el contenedor estaba en ejecución.  
- Se accedió mediante la IP pública de la instancia.  
- Se confirmó que la página cargaba correctamente.  

---

## 5. Problemas encontrados  

### 5.1 Validación del origen de la página  
Para demostrar que la página se servía desde el contenedor y no desde Apache del host:  
- Se ejecutaron comandos dentro del contenedor.  
- Se comprobó que el directorio de Apache del host solo tenía la página por defecto.

---

## 6. Gestión del contenedor  

### 6.1 Comandos utilizados  
- Construcción de imagen  
- Listado de contenedores  
- Ejecución  
- Inspección del contenedor  

### 6.2 Acceso web  
http://IP_PUBLICA:8080

---

## 7. Monitorización  
La monitorización se realizó manualmente mediante comandos de Podman para verificar:  
- Estado del contenedor  
- Puertos expuestos  
- Logs básicos  

---

## 8. Conclusiones  

### 8.1 Ventajas de usar contenedores en la nube  
- No es necesario gestionar servidores físicos.  
- Escalado automático según carga (si se usara ECS/Fargate).  
- Integración con balanceadores, seguridad y monitorización.  
- Despliegue más ágil y flexible.  

### 8.2 Motivo de elegir EC2 + Podman  
- Mayor control del entorno.  
- Menor coste.  
- Evitar ampliar el plan de AWS.  

---

## 9. Tecnologías utilizadas  
| Tecnología | Uso |
|-----------|------|
| AWS EC2 | Infraestructura |
| Podman | Motor de contenedores |
| Apache | Servidor web dentro del contenedor |
| Dockerfile | Construcción de la imagen |
| Ubuntu Server | Sistema base |

---

## 📝 Nota final  
**En los dos primeros días (lunes y martes)** se estudió:  
- El funcionamiento de los contenedores en la nube.  
- Las distintas formas de implementarlos en AWS.  
- Se eligió la opción más directa y permisiva en costes: **EC2 + Podman**.
