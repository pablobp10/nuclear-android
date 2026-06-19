# Política de Seguridad

Este documento define la política de seguridad para el repositorio, los flujos de trabajo de CI/CD y las compilaciones de **Nuclear CE**.

## ⚠️ Exención de Responsabilidades (Disclaimer)

**Este software se proporciona "tal cual" (AS IS), sin ningún tipo de garantía expresa o implícita, incluyendo, pero no limitándose a, las garantías de comerciabilidad o idoneidad para un propósito particular.** 

Los mantenedores de este *fork* no asumen **ninguna responsabilidad** por la pérdida de datos, daños al dispositivo, vulnerabilidades del sistema, interrupción del servicio o cualquier otro problema técnico o legal derivado de la instalación, compilación o uso de este software.

Nuclear CE es un *port* móvil no oficial y experimental mantenido por la comunidad. Al utilizar, descargar o compilar este código, el usuario asume la totalidad de los riesgos asociados. No nos hacemos responsables de la seguridad de los *plugins* de terceros, de las dependencias subyacentes, ni del contenido transmitido a través de la aplicación.

## Ámbito del Proyecto (Upstream vs. Fork)

Es fundamental distinguir entre la aplicación original y las modificaciones introducidas en este repositorio. Nuestra capacidad de actuación se limita estrictamente a nuestras inyecciones.

* **Vulnerabilidades del Núcleo (Upstream):** Si el fallo de seguridad reside en el motor original de React, el gestor de *plugins*, la lógica de red o las dependencias base de Rust, este **no es el lugar correcto para reportarlo**. Debes dirigirte al repositorio oficial de los creadores originales.
* **Vulnerabilidades del Port Móvil:** Solo procesaremos reportes de seguridad que afecten exclusivamente a nuestra infraestructura: vulnerabilidades en los *workflows* de GitHub Actions (`android.yml`, `sync.yml`), en los scripts de pre-compilación (`patch.cjs`) o brechas introducidas por la arquitectura móvil V18.

## Cómo reportar una vulnerabilidad

Si encuentras un fallo crítico que aplique exclusivamente a las modificaciones de este repositorio:

1. **NO abras un *Issue* público.** La divulgación de fallos de seguridad en abierto pone en riesgo a los usuarios.
2. Comunícalo de forma privada a través del correo electrónico o vías de contacto indicadas en el perfil de GitHub del propietario de este repositorio.
3. Incluye los pasos de reproducción o el vector de ataque detectado en el *pipeline* o la compilación.

*Nota:* Este es un proyecto mantenido de forma voluntaria. **No operamos bajo Acuerdos de Nivel de Servicio (SLA), no podemos garantizar tiempos de respuesta rápidos y no nos comprometemos a emitir parches de corrección.** Trabajaremos en los problemas reportados según la disponibilidad de tiempo y recursos de la comunidad.
