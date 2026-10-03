# Evaluación de Riesgos y Endurecimiento de Red (Network Hardening)

**Descripción del Proyecto:**
Este proyecto documenta la evaluación de riesgos post-incidente para una organización de redes sociales que sufrió una vulneración masiva de datos (exposición de PII de clientes). Tras el análisis de la infraestructura, se identificaron vulnerabilidades críticas en el control de accesos y la seguridad perimetral. El objetivo de este informe es establecer un plan de endurecimiento de red (Network Hardening) proponiendo controles técnicos y administrativos para prevenir futuras brechas.

## 1. Identificación de Vulnerabilidades

Durante la auditoría de seguridad interna, se descubrieron las siguientes brechas críticas en la postura de seguridad de la organización:

1. Prácticas inseguras de los empleados (uso compartido de contraseñas).

2. Credenciales predeterminadas activas (contraseña por defecto en el administrador de la base de datos).

3. Configuración deficiente del perímetro (cortafuegos sin reglas de filtrado de tráfico entrante/saliente).

4. Ausencia de verificación de identidad robusta (falta de Autenticación Multifactor - MFA).


## 2. Estrategias de Mitigación y Endurecimiento (Hardening)

Para proteger la red de la organización y asegurar los datos de los clientes, se propone la implementación inmediata de las siguientes prácticas de endurecimiento de seguridad:

### A. Gestión de Identidad y Accesos (IAM)

- **Vulnerabilidades mitigadas:** Uso compartido de contraseñas y contraseñas por defecto.

- **Técnica de endurecimiento:** Implementar una Política de Contraseñas estricta y forzar el uso de cuentas individuales. Configurar el sistema (ej. Active Directory) para exigir el cambio inmediato de contraseñas por defecto, requerir longitud/complejidad mínima y evitar la reutilización de claves antiguas.

- **Por qué es eficaz:** Al prohibir cuentas compartidas, se establece la trazabilidad y responsabilidad individual de cada empleado (accountability). Técnicamente, el sistema rechaza credenciales débiles, cerrando el vector de ataque principal por fuerza bruta o diccionario.

- **Frecuencia de implementación:** La aplicación técnica de la política debe ser continua. Las auditorías de cumplimiento deben realizarse de forma semestral.

### B. Reglas de Cortafuegos Perimetral (Firewall ACLs)

- **Vulnerabilidades mitigadas:** Ausencia de filtrado de tráfico en la red.

- **Técnica de endurecimiento:** Configurar Listas de Control de Acceso (ACL) en el cortafuegos basándose en el principio de Denegación Implícita (Implicit Deny). Se debe bloquear todo el tráfico por defecto y permitir únicamente el tráfico estrictamente necesario (puertos y protocolos específicos) para las operaciones comerciales.

- **Por qué es eficaz:** Reduce drásticamente la superficie de ataque. Al bloquear puertos innecesarios, se evita que los atacantes puedan explotar servicios vulnerables o aplicaciones heredadas que no están en uso, además de prevenir la exfiltración de datos hacia IPs maliciosas.

- **Frecuencia de implementación:** La configuración de las reglas de cortafuegos debe ser revisada y actualizada periódicamente, idealmente cada trimestre, para asegurar que reflejen los cambios en la arquitectura de red y en los riesgos emergentes.


### C. Autenticación Multifactor (MFA)

- **Vulnerabilidad mitigada:** Falta de MFA y riesgo de robo de credenciales.

- **Técnica de endurecimiento:** Desplegar MFA de forma obligatoria para todos los accesos a la red corporativa, VPNs y bases de datos críticas.

- **Por qué es eficaz:** Añade una capa de seguridad basada en "algo que el usuario tiene" (un token de hardware o aplicación móvil) sumado a "algo que sabe" (la contraseña). Es altamente eficaz porque, si un atacante vulnera o roba la contraseña mediante phishing o fuerza bruta, la solicitud de acceso será bloqueada al no poseer el segundo factor

- **Frecuencia de implementación:** Debe aplicarse a cada intento de inicio de sesión de forma continua e ininterrumpida. Se recomienda realizar auditorías de cumplimiento trimestrales para asegurar que todos los accesos críticos estén protegidos por MFA.





