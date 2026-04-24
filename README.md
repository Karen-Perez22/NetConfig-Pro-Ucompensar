🌐 UCOMPENSAR NetConfig Pro
Plataforma Web de Planificación y Configuración de Redes SD-WAN
Esta herramienta transforma el diseño manual de redes empresariales en un flujo de trabajo visual e interactivo. Centraliza el direccionamiento IP, la gestión de VLANs, la generación de CLI y la documentación técnica en una sola interfaz, sirviendo como Fuente Única de Verdad (Source of Truth) para proyectos de red SD-WAN multi-sede y multi-fabricante.
---
🏗️ Arquitectura de la Solución
La plataforma opera bajo un modelo de seis módulos integrados:
Dashboard en Tiempo Real: Panel de control con estado de sedes, alertas activas, distribución QoS y progreso del proyecto.
IP Planning: Motor de direccionamiento que genera automáticamente subredes por sede y VLAN, con calculadora CIDR integrada.
Topología SD-WAN: Diagrama interactivo Dual Hub & Spoke con visualización de nodos, enlaces y estado operativo de cada dispositivo.
Configuración de Dispositivos: Generador de CLI multi-fabricante. Soporta 12 vendors con plantillas listas para implementar.
Seguridad: Gestión de políticas de firewall, ACLs de acceso y checklist de endurecimiento de dispositivos.
Reportes y Documentación: Exportación de reportes técnicos y ejecutivos en formato `.txt`, `.conf` y `.json`.
---
✅ Requisitos Previos
Navegador moderno con soporte para ES6+ (Chrome, Firefox, Edge, Safari).
No requiere instalación de dependencias ni servidor backend.
Acceso opcional a red para implementar las configuraciones CLI generadas en dispositivos reales.
---
📁 Estructura del Proyecto
```
ucompensar-netconfig-pro/
│
├── index.html              # Aplicación completa (single-file SPA)
├── README.md               # Este documento
│
└── exports/                # Archivos generados por la herramienta
    ├── ip-plan-\*.json          # Plan de direccionamiento exportado
    ├── proyecto-\*.json         # Estado completo del proyecto
    ├── reporte-\*.txt           # Reporte técnico completo
    ├── \*-config.conf           # Configuración CLI por dispositivo
    └── acls-ucompensar.conf    # Políticas de seguridad exportadas
```
---
🔌 Fabricantes Soportados (CLI Multi-Vendor)
Fabricante	Modelo de Referencia	Lenguaje CLI
Cisco	ASR 1000	IOS-XE
Fortinet	FortiGate 60F	FortiOS
Juniper	MX480	JunOS
Arista	DCS-7050SX3	EOS
Palo Alto	PA-5220	PAN-OS
Huawei	CloudEngine 12804	VRP
MikroTik	CCR2116	RouterOS
HP / Aruba	Aruba 8400	AOS-CX
Ubiquiti	UniFi Dream Machine	EdgeOS
Dell EMC	PowerSwitch Z9332F	Dell OS10
TP-Link	Omada ER8411	Omada OS
Extreme	ExtremeXOS	EXOS
---
🚀 Flujo de Trabajo (Workflow)
Para asegurar consistencia y trazabilidad, la herramienta implementa un ciclo de vida estructurado:
Planificar: Definir sedes, usuarios y octetos de red en el módulo de IP Planning. El plan de VLANs y subredes se genera automáticamente.
Diseñar: Validar la topología SD-WAN Dual Hub & Spoke en el módulo de Topología, verificando conectividad y ancho de banda por enlace.
Configurar: Seleccionar el tipo de dispositivo y fabricante en el módulo de Dispositivos. La CLI se genera en tiempo real conforme se editan los campos.
Asegurar: Revisar y exportar políticas de firewall y ACLs desde el módulo de Seguridad.
Exportar: Descargar la configuración como `.conf`, el proyecto como `.json` o el reporte completo como `.txt`.
Verificar: Implementar los comandos en los dispositivos reales y confirmar el estado operativo desde el Dashboard.
---
🌍 Infraestructura de Red Modelada
El proyecto modela una red SD-WAN empresarial con la siguiente configuración base:
Componente	Detalle
Sedes	4 (2 Hubs + 2 Ramas)
Usuarios totales	2.280
Dispositivos	20 (4R · 8SW · 4AP · 2FW · 2SRV)
VLANs	5 (Datos, Voz, CCTV, Gestión, Invitados)
Capacidad WAN	1 Gbps (Hub-a-Hub) · 100–500 Mbps (Hub-Rama)
Esquema IP	`10.{sede}.{vlan}.0/24` por sede y segmento
Protocolo routing	OSPF (AS 65000, Area 0)
Plan de VLANs
VLAN ID	Nombre	DSCP	Prioridad
20	Datos	BE (0)	Normal
30	Voz	EF (46)	Crítica
40	CCTV	AF41 (34)	Alta
50	Gestión	CS1 (8)	Media
60	Invitados	BE (0)	Normal
Políticas QoS
Clase	Tráfico	DSCP	Ancho de Banda
EF	Voz VoIP	46	15%
AF41	CCTV	34	20%
AF31	VDI	26	25%
BE	Datos	0	40%
---
💾 Persistencia de Datos
El proyecto puede guardarse y restaurarse en cualquier momento:
Guardar en sesión: Botón 💾 en la barra superior. Persiste en `sessionStorage` del navegador.
Exportar como JSON: Botón `↓ Exportar`. Genera `proyecto-ucompensar-YYYY-MM-DD.json` con el estado completo.
Importar: Botón 📂. Carga un proyecto `.json` previamente exportado, restaurando todas las sedes, VLANs y preferencias.
---
🛡️ Políticas de Seguridad Implementadas
```
001 — Permitir Gestión     | 10.1.50.0/24 → Any   | Puertos 22, 443  | ALLOW
002 — Bloquear Telnet      | Any → Any             | Puerto 23        | DENY
003 — VoIP Prioritario     | 10.1.30.0/24 → Any   | 5060, RTP        | ALLOW
004 — Invitados Aislados   | 10.1.60.0/24 → RFC1918| Any             | DENY
005 — Bloquear SMB         | Any → Any             | Puerto 445       | DENY
```
---
📊 Estado del Proyecto
Módulo	Estado	Avance
IP Planning	✅ Completado	100%
Topología	✅ Completado	100%
Dispositivos	⚙️ En Progreso	60%
Seguridad	⚙️ En Progreso	75%
Documentación	⏳ Pendiente	10%
---
👥 Créditos
Desarrollado como proyecto académico para el curso de Diseño de redes banda ancha en la Corporación Universitaria Compensar — UCOMPENSAR.
> \*\*NetConfig Pro\*\* · UCOMPENSAR · 2026
