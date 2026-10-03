# Análisis de Tráfico de Red: Incidente DNS e ICMP

**Descripción del Proyecto:**

 Este proyecto forma parte de mi portafolio de ciberseguridad. Consiste en el análisis de un registro de tráfico de red capturado con tcpdump para diagnosticar un incidente donde los usuarios no podían acceder a un sitio web corporativo. El objetivo es identificar los protocolos de red afectados, interpretar los paquetes y determinar la causa raíz del fallo desde la perspectiva de un analista de seguridad.


## 1. Resumen del Análisis del Registro

Tras analizar la captura y analisis del trafico de red, se identificó un fallo persistente en la resolución de nombres de dominio (DNS). Los protocolos implicados en este incidente fueron DNS e ICMP (utilizado por el servidor destino para reportar el error de entrega)

Hallazgos técnicos del tráfico de red:

 - **Flujo de origen y destino:** El navegador del cliente (192.51.100.15) envía repetidamente consultas DNS hacia el servidor de la empresa (203.0.113.2).

 - **Desglose de la consulta válida (** 35084+ A? yummyrecipesforme.com. (24 **)):**

    - 35084: ID de transacción generado por el cliente para rastrear la solicitud.
    - +: Bandera de "Recursión Deseada", solicitando al servidor que busque la IP si no la tiene en su memoria caché.
    - A?: Solicitud de un Registro A (para traducir el nombre de dominio a una dirección IPv4).
    - (24): Tamaño de la carga útil (payload) del paquete DNS en bytes.
    - yummyrecipesforme.com.: El nombre de dominio que se está resolviendo.

 - **El Error (Respuesta ICMP):** Inmediatamente después de cada solicitud, el servidor destino responde a la IP del cliente con un paquete ICMP indicando udp port 53 unreachable (puerto UDP 53 inalcanzable).

 **Interpretación:** La solicitud DNS del cliente está bien formada y llega exitosamente a la máquina destino. La máquina está encendida y conectada a la red (lo sabemos porque responde activamente con paquetes ICMP), pero carece de un servicio escuchando en el puerto UDP 53.


## 2. Reporte de Incidente y Resolución

###  Contexto y Estado del Incidente

- **Reporte inicial:** A la 1:24 p.m. (según el registro de red), múltiples clientes informaron la imposibilidad de cargar [www.yummyrecipesforme.com](https://www.yummyrecipesforme.com), recibiendo un error de "puerto de destino inalcanzable" en sus navegadores.

- **Estado actual:** El problema se ha reproducido y persiste. La captura de red confirma que el navegador no puede iniciar la conexión HTTPS hacia el servidor web debido a que el paso previo (la resolución DNS) está fallando sistemáticament



### Presunta Causa Raíz e Hipótesis de Seguridad

La causa raíz es la interrupción total del servicio DNS en el servidor 203.0.113.2. Desde una perspectiva de ciberseguridad y operaciones, esto abre dos líneas de investigación principales:

1. **Fallo operativo o de configuración:** El servicio/daemon de DNS se detuvo accidentalmente o se implementó una regla de firewall a nivel de sistema operativo que está bloqueando el tráfico entrante al puerto 53.

2. **Ataque de Denegación de Servicio (DoS):** Un actor malicioso pudo haber saturado el servidor con una avalancha masiva de solicitudes de red, provocando la caída del servicio DNS por agotamiento de recursos (CPU/RAM). Para confirmar esta hipótesis se requeriría el análisis de métricas de rendimiento y capturas de tráfico previas al incidente.

### Siguientes pasos para la remediación

1. **Escalar el incidente:** Notificar inmediatamente a los ingenieros de infraestructura sobre la inactividad del servicio en el servidor 203.0.113.2

2. **Verificación local del servicio:** El equipo responsable debe acceder al servidor para revisar el estado del servicio DNS y analizar los registros (logs) de eventos del sistema en busca del motivo exacto de la interrupción.

3. **Auditoría de Firewall:** Revisar las listas de control de acceso  locales para garantizar que el puerto UDP 53 esté habilitado para tráfico entrante.

4. **Pruebas de validación:** Tras restaurar el servicio, ejecutar pruebas de resolución locales e internacionales (ej. comandos dig o nslookup) antes de notificar la resolución definitiva del ticket.

