🌐 UCOMPENSAR NetConfig Pro: Plataforma Web de Planificación y Configuración de Redes SD-WAN

Esta herramienta transforma el diseño manual de redes empresariales en un flujo de trabajo de **Infraestructura como Código (IaC)** visual e interactivo. Centraliza el direccionamiento IP, la gestión de VLANs, la generación de CLI y la documentación técnica en una sola interfaz, sirviendo como **Fuente Única de Verdad (Source of Truth)** para proyectos de red SD-WAN multi-sede y multi-fabricante de la institución.

---

🏗️ Arquitectura de la Solución
La plataforma opera bajo un modelo de seis módulos integrados:
* **Dashboard en Tiempo Real:** Panel de control con estado de sedes, alertas activas, distribución QoS y progreso del proyecto.
* **IP Planning:** Motor de direccionamiento que genera automáticamente subredes por sede y VLAN, con calculadora CIDR integrada.
* **Topología SD-WAN:** Diagrama interactivo Dual Hub & Spoke con visualización de nodos, enlaces y estado operativo de cada dispositivo.
* **Configuración de Dispositivos:** Generador de CLI multi-fabricante. Soporta 12 vendors con plantillas listas para implementar.
* **Seguridad:** Gestión de políticas de firewall, ACLs de acceso y checklist de endurecimiento de dispositivos.
* **Reportes y Documentación:** Exportación de reportes técnicos y ejecutivos en formato `.txt`, `.conf` y `.json`.

---

 ✅ Requisitos Previos
* Navegador moderno con soporte para ES6+ (Chrome, Firefox, Edge, Safari).
* No requiere instalación de dependencias ni servidor backend.
* Acceso opcional a red para implementar las configuraciones CLI generadas en dispositivos reales.

---

 📁 Estructura del Proyecto
text
ucompensar-netconfig-pro/
│
├── UCOMPENSAR_NetConfig_Pro.html  # Aplicación completa (single-file SPA)
├── README.md                      # Este documento (Normalización y Documentación)
│
└── exports/                       # Archivos generados por la herramienta
    ├── ip-plan-*.json             # Plan de direccionamiento exportado
    ├── proyecto-*.json            # Estado completo del proyecto
    ├── reporte-*.txt              # Reporte técnico completo
    ├── *-config.conf              # Configuración CLI por dispositivo
    └── acls-ucompensar.conf       # Políticas de seguridad exportadas
    
🔌 Fabricantes Soportados (CLI Multi-Vendor)FabricanteModelo de ReferenciaLenguaje CLICiscoASR 1000IOS-XEFortinetFortiGate 60FFortiOSHuaweiCloudEngine 12804VRPMikroTikCCR2116RouterOSY otros 8 fabricantes del mercado de Banda Ancha integrados.
🚀 Flujo de Trabajo
Para asegurar consistencia y trazabilidad, la herramienta implementa un ciclo de vida estructurado:Planificar: Definir sedes, usuarios y octetos de red en el módulo de IP Planning. El plan de VLANs y subredes se genera automáticamente.Diseñar: Validar la topología SD-WAN Dual Hub & Spoke en el módulo de Topología, verificando conectividad y ancho de banda por enlace.Configurar: Seleccionar el tipo de dispositivo y fabricante en el módulo de Dispositivos. La CLI se genera en tiempo real conforme se editan los campos.Asegurar: Revisar y exportar políticas de firewall y ACLs desde el módulo de Seguridad.Exportar: Descargar la configuración como .conf, el proyecto como .json o el reporte completo como .txt.Verificar: Implementar los comandos en los dispositivos reales y confirmar el estado operativo desde el Dashboard.
🌍 Infraestructura de Red Lógica Modelada (Caso de Estudio UCOMPENSAR)De acuerdo con las especificaciones del diseño de arquitectura del proyecto de Banda Ancha, la topología implementa un modelo SD-WAN Dual Hub & Spoke distribuido en 4 sedes de la siguiente manera:Hub Primario (Sede 1 - S1): Concentrador de servicios centrales, Core L3 redundante, clúster de virtualización productivo, SBC SIP primario y servicios de red centralizados (NTP/DNS/DHCP/AAA).Hub Secundario / DR (Sede 3 - S3): Centro de Recuperación ante Desastres (DR), clúster de virtualización secundario (Activo/Activo o Activo/Standby) y SBC de respaldo para asegurar la continuidad del negocio.Ramas / Spokes (Sedes S2 y S4): Sucursales remotas equipadas con SD-WAN Edge perimetral conectadas mediante doble ISP en modo Active/Active.
📊 Plan Jerárquico de Direccionamiento IP (IPv4 / IPv6 ULA)Para mantener los criterios de organización y normalización, se despliega una segmentación basada en bloques /16 por sede y asignación fija por tipo de VLAN:Bloques Globales: S1 (10.1.0.0/16), S2 (10.2.0.0/16), S3 (10.3.0.0/16), S4 (10.4.0.0/16).Underlay de Transporte WAN: 172.16.0.0/24.Loopbacks de Enrutamiento: 10.254.0.0/24.Esquema de Direccionamiento IPv6 ULA: fd00::/48 (un prefijo /64 dedicado por cada VLAN).VLAN IDNombre de SegmentoAsignación IPv4 TípicaPrefijo IPv6 (ULA)Clase de Servicio (QoS / DSCP)Prioridad20Datos10.{Sede}.20.0/24fd00:{Sede}:20::/64BE (DSCP 0)Normal30Voz (VoIP)10.{Sede}.30.0/24fd00:{Sede}:30::/64EF (DSCP 46)Crítica40CCTV IP10.{Sede}.40.0/24fd00:{Sede}:40::/64AF41 (DSCP 34)Alta50Gestión (OOB)10.{Sede}.50.0/24fd00:{Sede}:50::/64CS1 (DSCP 8)Media60Invitados10.{Sede}.60.0/24fd00:{Sede}:60::/64BE (DSCP 0)Normal
📈 Políticas y Modelado de QoS (Cálculo de Ancho de Banda)Para validar que el tráfico agregado en los enlaces dedicados no supere el límite operativo establecido de 800 Mbps por Hub, se aplica la siguiente reserva por clases:EF (Voz): Reserva del 5% (G.711 en LAN, G.729 en WAN). Admite más de 300 llamadas concurrentes consumiendo < 15 Mbps de la WAN.AF41 (CCTV): Reserva del 10%. Para evitar la saturación de los enlaces de banda ancha, la grabación de video se realiza de forma 100% local en NVRs por sede, permitiendo la visualización centralizada desde el VMS únicamente en flujos secundarios (substreams de ~1 Mbps) limitado a un máximo del 10% de las cámaras en simultáneo.AF31 (VDI / Apps Críticas): Reserva del 30% (Cálculo base: 30% usuarios activos × 250 kbps × 1.3 de margen de holgura).BE (Datos generales): 55% del ancho de banda asignado por defecto.
🛡️ Políticas de Seguridad de Firewall (ACLs)Plaintext001 — Permitir Gestión     | 10.1.50.0/24 → Any           | Puertos 22, 443  | ALLOW
002 — Bloquear Telnet      | Any → Any                    | Puerto 23        | DENY
003 — VoIP Prioritario     | 10.1.30.0/24 → Any           | 5060, RTP        | ALLOW
004 — Invitados Aislados   | 10.1.60.0/24 → RFC1918       | Any              | DENY
005 — Bloquear SMB         | Any → Any                    | Puerto 445       | DENY
Aislamiento total de la VRF de CCTV; las cámaras IP tienen restringido cualquier tipo de tráfico o salida directa hacia el internet público.
👁️ Evidencias de Funcionalidad y Pruebas Reales de ValidaciónDe acuerdo con las pautas de entrega en Teams, el correcto funcionamiento de la infraestructura automatizada por la plataforma se valida mediante la ejecución programada del siguiente plan de pruebas:Pruebas de SLA (Path Steering): Simulación automática mediante BFD (Bidirectional Forwarding Detection) sobre las métricas de latencia, jitter y pérdida de paquetes. Si un operador de banda ancha degrada su calidad, el Edge SD-WAN conmuta los flujos críticos en tiempo real.Conmutación ISP (Active/Active): Caídas planificadas de interfaces WAN primarias en las sedes remotas (S2 y S4) para demostrar balanceo de carga dinámico basado en flujos (ECMP per-flow) sin pérdida de sesiones.Failover de Hub Central (S1 → S3): Simulación de caída catastrófica del Hub Primario (Sede 1). Los terminales VoIP de las sucursales redirigen su registro hacia el SBC secundario de la Sede 3 de forma transparente en menos de 30 segundos, manteniendo la telefonía operativa y restaurando las instancias críticas en el clúster de virtualización de contingencia (DR).Verificación de Marcación DSCP de Extremo a Extremo: Auditorías de tráfico capturado (packet capture) que comprueban que los paquetes originados desde la aplicación interactiva mantienen la prioridad EF (Voz) o AF41 (Video) a través de los switches de acceso, núcleos de distribución y túneles IPsec de la WAN.
🤖 Registro de Prompts de Inteligencia Artificial (Buenas Prácticas)Siguiendo las normas de transparencia del curso de Redes de Banda Ancha, se documentan los prompts estructurales utilizados con asistentes de IA para el desarrollo del proyecto:Prompt para el Motor de Redes (JavaScript/HTML): "Actúa como un experto en ingeniería de redes y desarrollo frontend. Diseña una SPA web para UCOMPENSAR que calcule direccionamiento jerárquico IPv4 en bloques /16 y direcciones IPv6 ULA fd00::/48, separando subredes fijas para VLANs de Datos (20), Voz (30), CCTV (40) y Gestión (50), y que genere plantillas de comandos CLI legibles para enrutadores y switches."Prompt para la Normalización y QoS: "Ayúdame a estructurar un modelo matemático para calcular el consumo de tráfico WAN en sedes con tecnología SD-WAN, limitando la visualización central de cámaras de CCTV a flujos substream de 1 Mbps para no superar un umbral de 800 Mbps en los hubs centrales
