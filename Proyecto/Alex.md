# Proyecto On-Premise con Contenedores Docker

**Autor:** Alexander Santana Santana  
**Fecha:** Marzo 2026

---

## 📋 Índice
1. [Introducción](#introducción)
2. [Objetivos](#objetivos)
3. [Arquitectura](#arquitectura)
4. [Implementación](#implementación)
5. [Red y mDNS](#red-y-mdns)
6. [Monitorización](#monitorización)
7. [Pruebas realizadas](#pruebas-realizadas)
8. [Conclusiones](#conclusiones)
9. [Tecnologías utilizadas](#tecnologías-utilizadas)

---

## 1. Introducción

Este proyecto consiste en el despliegue de una aplicación web contenerizada en un entorno **on-premise** utilizando Docker. Se ha implementado un sistema completo que incluye la aplicación, resolución de nombres por mDNS y monitorización con herramientas profesionales.

---

## 2. Objetivos

- [x] Instalar y configurar Docker en un servidor Ubuntu
- [x] Crear una aplicación web personalizada con Flask
- [x] Contenerizar la aplicación mediante Dockerfile propio
- [x] Configurar mDNS para acceso por nombre fijo
- [x] Implementar monitorización con cAdvisor, Prometheus y Grafana

---

## 3. Arquitectura
- Servidor Ubuntu 24.04
- │
- ├── Docker Engine
- │ └── Contenedor: Aplicación Flask
- │ └── Puerto expuesto: 8080
- │
- ├── Avahi (mDNS)
- │ └── Publica: alexander-santana.local:8080
- │
- └── Stack de monitorización
- ├── cAdvisor (puerto 8081)
- ├── Prometheus (puerto 9090)
- └── Grafana (puerto 3000)

---


---

## 4. Implementación

### 4.1 Preparación del servidor
- Instalación y actualización de Ubuntu Server 24.04
- Configuración del hostname como `alexander-server`
- Instalación de herramientas básicas (git, curl, net-tools)

### 4.2 Desarrollo de la aplicación
- Aplicación web desarrollada en Python con Flask
- Interfaz personalizada que muestra información del sistema y del contenedor
- Endpoints implementados:
  - `/` → Página principal con datos del contenedor
  - `/estado` → API JSON con estado del sistema
  - `/demo/mdns` → Página explicativa de mDNS

### 4.3 Contenerización
- Creación de imagen Docker personalizada mediante Dockerfile
- Exposición del puerto 5000 interno mapeado al 8080 del host
- Configuración de reinicio automático del contenedor

---

## 5. Red y mDNS

Para solucionar el problema de IP variable por DHCP, se implementó **mDNS mediante Avahi**:

- Instalación y configuración del servicio Avahi
- Publicación del servicio web en el puerto 8080
- Configuración del nombre `alexander-santana.local`

**Resultado:** Acceso permanente al proyecto mediante `http://alexander-santana.local:8080`, independientemente de la IP asignada.

---

## 6. Monitorización

Se implementó un stack completo de monitorización:

| Herramienta | Función | Puerto |
|-------------|---------|--------|
| **cAdvisor** | Recolección de métricas de contenedores | 8081 |
| **Prometheus** | Almacenamiento y consulta de métricas | 9090 |
| **Grafana** | Visualización con dashboards profesionales | 3000 |

**Funcionalidades:**
- Visualización de consumo de CPU y memoria en tiempo real
- Monitorización de tráfico de red por contenedor
- Históricos de rendimiento
- Detección de posibles problemas de recursos

---

## 7. Pruebas realizadas

| Prueba | Resultado |
|--------|-----------|
| Contenedor en ejecución | ✅ Funcionando |
| Acceso web por IP | ✅ Correcto |
| Acceso por nombre mDNS | ✅ Correcto |
| Métricas en cAdvisor | ✅ Visibles |
| Targets en Prometheus | ✅ UP |
| Dashboards en Grafana | ✅ Importados correctamente |

---

## 8. Conclusiones

### 8.1 Logros alcanzados
- Entorno on-premise completamente funcional
- Aplicación contenerizada y accesible por nombre fijo
- Monitorización profesional integrada
- Documentación completa del proceso

### 8.2 Dificultades superadas
- **IP variable:** Solucionado con mDNS/Avahi
- **Errores de ruta:** Corregidos implementando los endpoints correctamente
- **Conexión entre servicios:** Ajustada la configuración de red en Prometheus

### 8.3 Ventajas del enfoque on-premise
- Control total sobre la infraestructura
- Sin dependencia de conexión a internet
- Sin costes recurrentes (solo hardware)
- Ideal para aprendizaje y desarrollo local
- Portabilidad: lo desarrollado aquí puede llevarse a la nube sin cambios

---

## 9. Tecnologías utilizadas

| Tecnología | Versión | Uso |
|------------|---------|-----|
| Ubuntu Server | 24.04 LTS | Sistema operativo base |
| Docker | 24.x | Motor de contenedores |
| Python | 3.9 | Lenguaje de programación |
| Flask | 2.3.2 | Framework web |
| Avahi | 0.8 | Servicio mDNS |
| cAdvisor | latest | Recolección de métricas |
| Prometheus | latest | Base de datos de métricas |
| Grafana | latest | Visualización de datos |

---

<div align="center">

**Proyecto realizado por Alexander Santana Santana**  
Marzo 2026

</div>
