# Informe de Incidente de Ciberseguridad: Análisis de Ataque de Red (SYN Flood)

**Descripción del Proyecto:**
Este proyecto documenta la respuesta a un incidente de seguridad que afectó la disponibilidad del servidor web de una agencia de viajes. Utilizando registros de tráfico de red (TCP/HTTP) capturados mediante herramientas como Wireshark, el objetivo fue analizar el tráfico anómalo, clasificar el tipo de ataque en curso y proponer medidas de mitigación para proteger la infraestructura corporativa.


## 1. Resumen del Incidente y Tipo de Ataque

**Naturaleza del Ataque:**
El servidor web fue víctima de un ataque de Denegación de Servicio (DoS) a nivel de red, específicamente una inundación SYN (SYN flood).

**Características y Síntomas:**
Este ataque explota el proceso de negociación de tres pasos (three-way handshake) del protocolo TCP. El actor malicioso envía una cantidad masiva de paquetes de solicitud de sincronización ([SYN]). El servidor web reserva recursos del sistema para cada solicitud entrante, pero el atacante nunca completa la conexión. Como resultado de esta inundación, los recursos del servidor se agotan y se vuelve incapaz de responder a las solicitudes de tráfico legítimo. Al provenir el ataque de una única dirección IP, se clasifica como DoS y no como DDoS (Distribuido).


## 2. Análisis del Tráfico de Red (Registro Wireshark)

El análisis del registro TCP revela el comportamiento exacto del ataque frente a las operaciones normales:

- **Identificación de Actores:**
    - **Servidor Web (Víctima):** `192.0.2.1`
    - **Trafico legítimo:** `198.51.100.0/24`
    - **Atacante:** `203.0.113.0`
    - **Protocolo:** TCP

**Patrones de Tráfico Observados:**

1. **Inicio del ataque:** El registro muestra a la IP `203.0.113.0` enviando repetidas y anormales solicitudes `[SYN] `al puerto 443 del servidor web (tráfico cifrado).

2. **Degradación del servicio:** Inicialmente, el servidor web logra responder al tráfico legítimo (ej. IP `198.51.100.14` logra establecer la conexión y enviar comandos HTTP `GET`).

3. **Colapso (Timeouts):** A medida que el atacante envía varias solicitudes `[SYN]` sin completar el handshake, las conexiones de los usuarios legítimos comienzan a fallar. El registro muestra  errores `HTTP/1.1 504 Gateway Time-out` (El servidor tardó demasiado en responder) y paquetes `[RST, ACK]` (conexiones descartadas y reiniciadas).

4. **Caída total:** A partir de la línea 125 del registro, el servidor web deja de responder por completo al tráfico; el analizador solo registra la entrada masiva de paquetes del ataque.

## 3. Impacto en la organización

El ataque tuvo un impacto directo en las operaciones comerciales de la agencia de viajes:

- **Interrupción Operativa:** Los empleados perdieron acceso a la página web de ventas de la empresa, lo que paralizó su capacidad para buscar paquetes vacacionales y atender a los clientes.

- **Agotamiento de Recursos:** El servidor web quedó completamente inoperativo bajo la carga de tráfico malicioso, requiriendo su desconexión manual para estabilizar el sistema.

## 4. Recomendaciones y Siguientes Pasos

Aunque como medida de emergencia se desconectó el servidor y se bloqueó temporalmente la IP maliciosa (`203.0.113.0`) en el firewall corporativo, esta solución es insuficiente a largo plazo debido a que los atacantes pueden falsificar (spoofing) fácilmente otras direcciones IP.

Se recomienda a la gerencia implementar las siguientes estrategias preventivas:

1. **Protección contra SYN Floods (SYN Cookies):** Configurar el sistema operativo o el balanceador de carga para utilizar SYN cookies, permitiendo al servidor verificar la legitimidad de las conexiones antes de reservar recursos del sistema.

2. **Limitación de Conexiones:** Configurar reglas en el firewall o en el Sistema de Prevención de Intrusiones (IPS) para limitar la cantidad de solicitudes TCP SYN incompletas que se pueden recibir de una misma IP en un margen de tiempo

3. **Monitoreo y Alertas:** Implementar sistemas de monitoreo de tráfico en tiempo real que puedan detectar patrones anómalos y generar alertas automáticas para el equipo de seguridad.

4. **Redundancia y Escalabilidad:** Considerar la implementación de servidores redundantes y balanceadores de carga para distribuir el tráfico y mantener la disponibilidad del servicio durante ataques.


## 5. Anexo: Evidencia del Tráfico de Red (Registro TCP/HTTP)
```text
No. | Tiempo    | Origen          | Destino     | Protocolo | Información
47  | 3.144521  | 198.51.100.23   | 192.0.2.1   | TCP       | 42584->443 [SYN] Seq=0 Win=5792 Len=120...
48  | 3.195755  | 192.0.2.1       | 198.51.100.23 | TCP       | 443->42584 [SYN, ACK] Seq=0 Win-5792 Len=120...
...
52  | 3.390692  | 203.0.113.0     | 192.0.2.1   | TCP       | 54770->443 [SYN] Seq=0 Win=5792 Len=0...
53  | 3.441926  | 192.0.2.1       | 203.0.113.0 | TCP       | 443->54770 [SYN, ACK] Seq=0 Win-5792 Len=120...
...
125 | 21.136783 | 203.0.113.0     | 192.0.2.1   | TCP       | 54770->443 [SYN] Seq=0 Win=5792 Len=0...
126 | 21.459796 | 203.0.113.0     | 192.0.2.1   | TCP       | 54770->443 [SYN] Seq=0 Win=5792 Len=0...```



