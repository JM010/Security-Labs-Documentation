# Auditoría Interna de TI y Evaluación de Cumplimiento - Botium Toys

## Descripción del Proyecto:
Este proyecto forma parte de mi portafolio de ciberseguridad y consiste en una auditoría interna simulada para una empresa ficticia (Botium Toys). A partir de un reporte inicial de evaluación de riesgos, mi tarea fue analizar la postura de seguridad de la empresa frente al marco NIST CSF y normativas de cumplimiento clave como PCI DSS, GDPR y SOC.

## objetivos del Proyecto:
Identificar vulnerabilidades en los controles actuales, evaluar el cumplimiento normativo y proporcionar recomendaciones accionables para mitigar riesgos.

# 1. Evaluación de Controles (Controls Assessment)

A continuación, se detalla el estado actual de los controles de seguridad en Botium Toys y la justificación de su estado:

| Control| ¿Implementado? | Justificación / Observaciones |
| ------------ | ------------ | ------------ |
| Mínimo privilegio | ❌ No | Actualmente todos los empleados tienen acceso a los datos de los clientes. Es necesario limitar los privilegios para reducir el riesgo de una filtración de datos.|
| Planes de recuperación ante desastres (DRP)| ❌ No | No hay planes de recuperación ante desastres. Deberían implementarse para asegurar la continuidad del negocio.|
| Políticas de contraseñas | ❌ No | Los requisitos actuales son mínimos y no cumplen con la complejidad mínima estándar. Esto facilita el acceso de atacantes a través de ataques de fuerza bruta o diccionario a los dispositivos finales o redes.|
| Separación de funciones | ❌ No | La empresa no aplica este control preventivo. Todos los empleados pueden acceder a los datos internos (incluyendo tarjetas de crédito y PII). Sin dividir las tareas críticas, un solo empleado o atacante podría ejecutar una cadena de acciones maliciosas de principio a fin.|
| Cortafuegos (Firewall) | ✅ Sí | Existe un Firewall que bloquea el tráfico basándose en un conjunto de criterios establecidos adecuadamente.|
| Sistema de detección de intrusiones (IDS) | ❌ No | No hay un sistema de detección de intrusiones implementado. Debería instalarse para monitorear y alertar sobre actividades sospechosas en la red.|
| Copias de seguridad (Backups) | ❌ No | No se realizan copias de seguridad regulares. Se necesita implementar una política de backups robusta para poder restaurar los sistemas y mantener la continuidad del negocio en caso de un incidente grave.|
| Software antivirus | ✅ Sí |Hay un software antivirus instalado y es monitoreado regularmente por el departamento de TI.|
| Mantenimiento para sistemas heredados | ❌ No | Aunque los activos mencionan sistemas heredados y se indica que se supervisan, no existe una política clara que establezca los responsables ni los procedimientos de mantenimiento e intervención.|
| Cifrado (Encryption) | ❌ No | El cifrado no está implementado; es fundamental para asegurar la confidencialidad de los datos sensibles de los clientes.|
| Gestión de contraseñas (Password Manager) | ❌ No | Adoptar esta herramienta es prioritario. Operativamente, simplificará la administración y optimizará el flujo de trabajo (menos tickets a TI). En seguridad, es un mecanismo preventivo para forzar estándares de complejidad y mitigar la fatiga de contraseñas.|
| Cerraduras (Físicas) | ✅ Sí | Las oficinas, el local y el almacén disponen de cerrojos adecuados.|
| Vigilancia CCTV | ✅ Sí | Hay cámaras de videovigilancia instaladas y en funcionamiento.|
| Prevención de incendios | ✅ Sí | Los mecanismos de alarma y mitigación de incendios están operativos.|


---

## 2. Evaluación de Cumplimiento (Compliance Checklist)

### **Payment Card Industry Data Security Standard (PCI DSS)**

| Mejor Práctica | ¿Cumple? | Justificación / Observaciones |
| ------------ | ------------ | ------------ |
| Solo usuarios autorizados acceden a tarjetas de crédito| ❌ No | Actualmente todos los empleados tienen acceso a los datos de los clientes.|
|Datos almacenados/procesados en entorno seguro | ❌ No | Los datos de tarjetas de crédito carecen de cifrado y el personal entero posee acceso a dicha información.|
| Implementar cifrado para transacciones | ❌ No | El cifrado no está implementado y es necesario para la confidencialidad.|
| Políticas de gestión segura de contraseñas | ❌ No | Las políticas de contraseñas son mínimas y no cumplen con los estándares de complejidad.|


### General Data Protection Regulation (GDPR)

| Mejor Práctica | ¿Cumple? | Justificación / Observaciones |
| ------------ | ------------ | ------------ |
| Privacidad/seguridad de datos de clientes de la UE| ❌ No | Al no estar implementado el cifrado, la confidencialidad de los datos sensibles (PII/SPII) está en riesgo.|
| Plan de notificación en 72h por brecha de datos | ✅ Sí | Existe un plan para notificar a los clientes de la U.E. dentro de las 72 horas si hay una filtración.|
| Datos clasificados e inventariados | ❌ No | Los activos han sido inventariados o registrados, pero no clasificados.|
| Políticas de privacidad implementadas | ✅ Sí | Se han definido e implementado políticas y esquemas de privacidad para el equipo de TI y el personal |


### System and Organizations Controls (SOC tipo 1, SOC tipo 2)

| Mejor Práctica | ¿Cumple? | Justificación / Observaciones |
| ------------ | ------------ | ------------ |
| Políticas de acceso de usuarios establecidas | ❌ No | No se implementan controles de separación de funciones ni de mínimo privilegio. |
| Datos sensibles (PII/SPII) son confidenciales | ❌ No | La confidencialidad de los datos sensibles no está garantizada debido a la falta de cifrado y controles de acceso.|
| Integridad de los datos validada | ✅ Sí  | Se asegura que la información permanezca consistente, precisa, íntegra y validada.|
| Datos disponibles solo para personal autorizado | ❌ No | Todos los empleados tienen acceso a los datos de los clientes, lo que representa un riesgo de filtración.|


## 3. Recomendaciones Ejecutivas (Resumen para el Gerente de TI)

Basado en la evaluación de riesgos y la auditoría de cumplimiento, se recomienda a la gerencia de TI de Botium Toys implementar urgentemente las siguientes medidas para mitigar vulnerabilidades críticas y alinearse con los marcos regulatorios:

1. **Gestión de Accesos (Mínimo Privilegio y Separación de Funciones):** Es imperativo revocar el acceso global a las bases de datos. Se deben implementar políticas estrictas para garantizar que solo el personal autorizado tenga acceso a la información de tarjetas de crédito y PII/SPII, mitigando el riesgo de amenazas internas o robo de credenciales.

2. **Protección de Datos (Cifrado):** Implementar protocolos de cifrado robustos para todos los datos financieros y confidenciales, tanto en reposo (almacenamiento local) como en tránsito, para cumplir inmediatamente con los estándares de PCI DSS y GDPR.

3. **Visibilidad de Red (IDS)** Desplegar un Sistema de Detección de Intrusos (IDS) para monitorear el tráfico anómalo. Actualmente la empresa tiene un punto ciego interno.

4. **Políticas de Contraseñas y Gestión de Credenciales:** Establecer políticas de contraseñas más estrictas y adoptar un gestor de contraseñas corporativo para asegurar la complejidad y rotación periódica de credenciales, reduciendo el riesgo de ataques de fuerza bruta o diccionario.

5. **Planes de Recuperación ante Desastres (DRP) y Backups:** Desarrollar e implementar un plan de recuperación ante desastres y una política de copias de seguridad regulares para garantizar la continuidad del negocio en caso de incidentes graves.

6. **Auditoría y Mantenimiento de Sistemas Heredados:** Establecer procedimientos claros para la supervisión y mantenimiento de sistemas heredados, asegurando que estén actualizados y protegidos contra vulnerabilidades conocidas.
