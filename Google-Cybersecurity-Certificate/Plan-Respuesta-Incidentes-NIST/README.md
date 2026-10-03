# Plan de Respuesta a Incidentes: Ataque DoS y Marco NIST CSF

**Descripción del Proyecto:**
Este proyecto documenta el análisis de un incidente de ciberseguridad en una empresa multimedia que sufrió una interrupción de red de dos horas debido a un ataque de Denegación de Servicio (DoS). El informe estructura la investigación, la contención y la mitigación a largo plazo utilizando las cinco funciones principales del Marco de Ciberseguridad del NIST (NIST CSF).

## Resumen Ejecutivo del Incidente
La red interna de la organización sufrió una caída total de servicios debido a un ataque DoS basado en una avalancha de paquetes ICMP (ICMP Flood). El equipo de gestión de incidentes logró contener la amenaza bloqueando el tráfico entrante y reiniciando los servicios esenciales. La investigación posterior reveló que la saturación fue posible por una falta de políticas de filtrado en el cortafuegos perimetral. Para prevenir futuros incidentes, se desplegaron controles de limitación de tasa, verificación antispoofing y sistemas de detección de intrusos.


## Análisis y Estrategia (NIST CSF)

### 1. Identificación (Identify)

Los activos afectados incluyeron la totalidad de los servicios críticos de la red interna, los cuales quedaron inaccesibles para el tráfico legítimo. Se determinó que la vulnerabilidad raíz fue un cortafuegos perimetral mal configurado que carecía de reglas de denegación implícita. Esto permitió a un actor malicioso externo inundar la red con paquetes ICMP, agotando el ancho de banda y los recursos de conectividad de la empresa.

### 2. Proteger (Protect)

Para salvaguardar la infraestructura y evitar la recurrencia del ataque, se implementaron controles técnicos de endurecimiento perimetral:

- Se establecieron reglas de cortafuegos para limitar la tasa (rate limiting) de paquetes ICMP entrantes.

- Se habilitó la verificación de la dirección IP de origen en el cortafuegos para bloquear intentos de suplantación de identidad (IP spoofing).


### 3. Detectar (Detect)

Para garantizar la visibilidad de la red y anticiparse a comportamientos maliciosos en tiempo real, se desplegaron las siguientes soluciones:

- Implementación de software de supervisión de red para monitorizar y alertar sobre patrones de tráfico anómalos.

- Integración de un sistema de Detección y Prevención de Intrusos (IDS/IPS) configurado específicamente para identificar y filtrar tráfico ICMP que presente firmas sospechosas.


### 4. Responder (Respond)
Durante la fase crítica del ataque, el equipo de gestión de incidentes ejecutó acciones de contención inmediatas para frenar la degradación de la red:

- Se bloqueó temporalmente todo el tráfico de paquetes ICMP entrantes.

- Se desconectaron de la red todos los servicios no críticos (offline) para aislar la amenaza y liberar recursos de procesamiento.


### 5. Recuperar (Recover)
Tras la contención del ataque, se procedió a restaurar los servicios esenciales de la red y a implementar medidas de resiliencia para garantizar la continuidad operativa:

- Se reiniciaron los servicios críticos de la red interna y se verificó su disponibilidad para el tráfico legítimo.

- Una vez estabilizada la red y confirmada la mitigación, se procedió a habilitar los servicios no críticos restantes.

- Mejora continua: Se recomienda documentar un inventario jerarquizado de activos y un Plan de Recuperación ante Desastres (DRP) formal para agilizar este proceso de triaje en futuros incidentes.
