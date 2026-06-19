# Política de Seguridad

La seguridad de **Nuclear CE** es una prioridad absoluta, especialmente dado que nuestro *pipeline* de CI/CD manipula código fuente, realiza inyecciones de parches (patch.cjs) y gestiona la compilación cruzada hacia binarios `.apk`.

## Reporte de Vulnerabilidades

Si has descubierto una vulnerabilidad de seguridad en nuestro motor de compilación, en los parches de inyección de código o en la implementación de la arquitectura móvil V18, te agradecemos que nos la comuniques de forma privada.

Por favor, **NO abras un Issue público** para reportar fallos de seguridad críticos.

### Cómo reportar una vulnerabilidad
Para informar de un problema de seguridad:
1. **Envía un mensaje privado** a través de nuestros canales de contacto oficiales mencionados en el README.
2. **Proporciona detalles técnicos:** Incluye los pasos exactos para reproducir el fallo, las versiones de los componentes afectados y, si es posible, una propuesta de solución.
3. **Privacidad:** Nos comprometemos a mantener tu reporte confidencial y a no divulgar los detalles hasta que una solución haya sido implementada y validada en una nueva versión.

## Ámbito de Seguridad
Esta política cubre exclusivamente:
* **Integridad del Pipeline:** Cualquier debilidad en las GitHub Actions que permita la inyección de código malicioso en los APKs compilados.
* **Vulnerabilidades de Dependencias:** Fallos críticos en las librerías Rust (`cargo`) o módulos Node (`pnpm`) que resulten en un compromiso de la ejecución local.
* **Seguridad de la UI/UX:** Vulnerabilidades introducidas mediante nuestras inyecciones CSS o la manipulación del DOM que pongan en riesgo los datos del usuario.

## Nuestra Promesa
* Reconoceremos la recepción de tu reporte en un plazo máximo de 48 horas.
* Trabajaremos con urgencia para auditar el código, aplicar el parche correspondiente y emitir una versión corregida.
* Agradeceremos públicamente tu contribución a la seguridad del ecosistema Nuclear CE una vez que la vulnerabilidad haya sido resuelta.

---
*Gracias por ayudarnos a mantener Nuclear CE seguro, robusto y confiable.*