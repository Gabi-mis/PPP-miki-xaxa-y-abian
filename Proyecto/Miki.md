# Proyecto Hipervisor con QEMU/KVM

**Autor:** Miguel Ángel Garrido García  
**Fecha:** Marzo 2026

---

## 📋 Índice
1. [Introducción](#introducción)
2. [Objetivos](#objetivos)
3. [Arquitectura](#arquitectura)
4. [Instalación y configuración](#instalación-y-configuración)
5. [Problemas encontrados](#problemas-encontrados)
6. [Gestión de máquinas virtuales](#gestión-de-máquinas-virtuales)
7. [Monitorización](#monitorización)
8. [Conclusiones](#conclusiones)
9. [Tecnologías utilizadas](#tecnologías-utilizadas)

---

## 1. Introducción

Este proyecto consiste en la implementación de un **hipervisor mediante QEMU/KVM** en un servidor Ubuntu para la creación y gestión de máquinas virtuales. Se ha explorado la virtualización completa como alternativa a los contenedores, permitiendo ejecutar sistemas operativos completos de forma aislada.

---

## 2. Objetivos

- [x] Instalar y configurar QEMU/KVM en Ubuntu Server
- [x] Verificar la compatibilidad de virtualización por hardware
- [x] Crear y gestionar máquinas virtuales
- [x] Explorar métodos de visualización gráfica (VNC)
- [x] Documentar problemas y soluciones encontradas

---

## 3. Arquitectura
Servidor Ubuntu
- │
- ├── QEMU/KVM (Hipervisor)
- │ ├── Máquina Virtual 1
- │ ├── Máquina Virtual 2
- │ └── ...
- │
- ├── libvirt (Gestión de VMs)
- │
- └── Interfaces de gestión
- ├── VNC (acceso gráfico)
- └── virsh (línea de comandos)

text

---

## 4. Instalación y configuración

### 4.1 Actualización del sistema


sudo apt update && sudo apt upgrade -y
## 4.2 Instalación de QEMU y dependencias

sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
## 4.3 Verificación de KVM
bash
# Comprobar soporte de virtualización
kvm-ok
## 4.4 Añadir usuario a grupos necesarios

sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
## 5. Problemas encontrados
## 5.1 Incompatibilidad de virtualización por hardware
Problema: Al ejecutar kvm-ok se detectó que la CPU no soportaba virtualización por hardware.


INFO: /dev/kvm no existe
HINT: sudo modprobe kvm_intel
HINT: Your CPU does not support KVM extensions
Causa: El proyecto se estaba ejecutando dentro de una máquina virtual (VirtualBox), lo que impide el anidamiento de virtualización sin configuración adicional.

Solución: Habilitar el anidamiento de virtualización en VirtualBox:


# En Windows (PowerShell como administrador)
& "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" list vms
& "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" modifyvm "Qemu" --nested-hw-virt on
## 5.2 Limitaciones detectadas
QEMU por sí solo no ofrece monitorización avanzada de las máquinas virtuales

Para monitorización completa es necesario usar virt-install y libvirt

El acceso gráfico requiere configuración adicional de VNC

Las máquinas creadas con QEMU directamente no se integran con las herramientas de monitorización estándar

## 6. Gestión de máquinas virtuales
## 6.1 Creación de máquinas virtuales
Para una gestión más completa, se recomienda usar virt-install en lugar de QEMU directamente:


# Ejemplo de creación de VM con virt-install
virt-install \
  --name ubuntu-vm \
  --ram 2048 \
  --vcpus 2 \
  --disk path=/var/lib/libvirt/images/ubuntu-vm.qcow2,size=20 \
  --os-variant ubuntu22.04 \
  --network network=default \
  --graphics vnc,listen=0.0.0.0 \
  --cdrom /path/to/ubuntu.iso
## 6.2 Gestión mediante virsh

# Listar máquinas virtuales
virsh list --all

# Ver información detallada
virsh dominfo nombre_vm

# Iniciar una VM
virsh start nombre_vm

# Detener una VM
virsh shutdown nombre_vm

# Conectar por consola
virsh console nombre_vm
6.3 Visualización gráfica
QEMU permite visualizar las máquinas virtuales mediante VNC:


vnc://servidor:5900
Para configurar VNC en una VM existente:


virsh edit nombre_vm
# Añadir en la sección <devices>:
# <graphics type='vnc' port='5900' autoport='yes' listen='0.0.0.0'/>
## 7. Monitorización
Para monitorizar las máquinas virtuales se pueden utilizar:

Herramienta	Función	Comando
virt-top	Monitorización de recursos en tiempo real	virt-top
virsh	Consulta de estado	virsh dominfo vm
libvirt	Gestión y monitorización	virsh list
Limitación importante: QEMU por sí solo no guarda históricos de monitorización. Para obtener métricas históricas sería necesario:

Instalar las VMs mediante virt-install (integración con libvirt)

Configurar agentes de monitorización dentro de las VMs

Utilizar soluciones externas como Prometheus + libvirt exporter

Implementar un sistema de logging centralizado

## 8. Conclusiones
## 8.1 Logros alcanzados
Instalación completa de QEMU/KVM

Configuración de libvirt para gestión de VMs

Comprensión del anidamiento de virtualización

Identificación de limitaciones y soluciones

Documentación del proceso completo

## 8.2 Dificultades superadas
Dificultad	Solución
Falta de soporte de virtualización en VM anidada	Habilitar nested-hw-virt en VirtualBox
Monitorización limitada	Uso de virt-top y virsh
Acceso gráfico	Configuración de VNC
Gestión compleja	Uso de virt-install y libvirt

## 8.3 Comparativa con contenedores
Aspecto	QEMU/KVM	Contenedores (Docker)
Aislamiento	Sistema completo	Procesos
Consumo de recursos	Alto (GB de RAM)	Bajo (MB de RAM)
Tiempo de arranque	Minutos	Segundos
Sistemas soportados	Cualquier SO	Mismo kernel que el host
Tamaño de imágenes	GB	MB
Portabilidad	Baja	Alta
Casos de uso	Entornos heredados, múltiples SO	Microservicios, desarrollo

## 8.4 Ventajas de QEMU/KVM
Permite ejecutar cualquier sistema operativo (Windows, diferentes versiones de Linux)

Aislamiento más fuerte a nivel de kernel

Ideal para entornos de pruebas con requisitos específicos

Útil para simular infraestructuras completas

## 8.5 Próximos pasos
Completar la instalación de VMs mediante virt-install

Explorar monitorización avanzada con Prometheus

Probar diferentes sistemas operativos invitados

Implementar backup y snapshot de las VMs

## 9. Tecnologías utilizadas
Tecnología	Versión	Uso
Ubuntu Server	24.04 LTS	Sistema operativo base
QEMU/KVM	latest	Hipervisor
libvirt	latest	Gestión de máquinas virtuales
virt-manager	latest	Interfaz gráfica de gestión
virt-install	latest	Creación automatizada de VMs
virsh	latest	Línea de comandos para VMs
VNC	-	Acceso remoto a las VMs
VirtualBox	-	Entorno de pruebas (host)
📁 Estructura del proyecto
text
proyecto-qemu/
│
├── README.md                 # Documentación principal
├── scripts/
│   ├── instalar-qemu.sh      # Script de instalación
│   ├── crear-vm.sh           # Script para crear VMs
│   └── monitorizar.sh        # Script de monitorización
└── docs/
    ├── problemas.md          # Problemas y soluciones
    ├── comandos.md           # Comandos útiles
    └── monitorizacion.md     # Notas sobre monitorización
📝 Notas adicionales
Comandos útiles recopilados durante el proyecto
bash
# Verificar módulos KVM
lsmod | grep kvm

# Ver permisos de /dev/kvm
ls -l /dev/kvm

# Información de la CPU
lscpu | grep Virtualization

# Ver redes de libvirt
virsh net-list
virsh net-dumpxml default

# Ver almacenamiento
virsh pool-list
virsh vol-list default

# Snapshots
virsh snapshot-list nombre_vm
virsh snapshot-create-as nombre_vm snapshot1
virsh snapshot-revert nombre_vm snapshot1
<div align="center">
Proyecto realizado por Miguel Ángel Garrido García
Marzo 2026

</div> 
