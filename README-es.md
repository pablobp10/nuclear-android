*Lee esto en otros idiomas: [English](README.md) | [Español](README-es.md)*

<p align="center">
  <picture>
    <img alt="Logo de Nuclear CE" src="android_ui/packages/player/public/logo-icon-on-white.png" width="200" style="border-radius: 20%;">
  </picture>
</p>
<div align="center">
# Nuclear CE (Community Edition)
### *Port Móvil No Oficial para Android*
</div>
<div align="center">
  **Nuclear CE** es un reproductor de música libre y de código abierto sin anuncios ni rastreadores, diseñado estrictamente para proporcionar compatibilidad nativa en entornos **Android (aarch64)** con una interfaz móvil táctil personalizada.<br>
  Busca cualquier canción o artista, crea listas de reproducción y empieza a escuchar tu música en cualquier lugar.
</div>
  
## ⚠️ Aviso Legal y Marcas Registradas
**ESTE PROYECTO ES 100% NO OFICIAL.** No está afiliado, respaldado ni soportado por los desarrolladores originales de Nuclear Player. 
* El nombre "Nuclear" se utiliza estrictamente bajo el **Uso Nominativo (Fair Use)** para indicar el origen del software y su motor interno. "CE" significa *Community Edition*.
* Para respetar las marcas gráficas registradas, todos los logotipos oficiales con copyright han sido sustituidos programáticamente por iconos genéricos libres de derechos durante nuestro proceso de compilación automatizada (CI/CD).

## 📱 Reestructuración Arquitectónica Móvil
Adaptar una aplicación de escritorio basada en React/Tauri a un entorno nativo de Android requiere una manipulación agresiva del DOM y parches en el backend. Esta bifurcación (*fork*) introduce la **Arquitectura Móvil V18**, un conjunto exhaustivo de inyecciones CSS y anulaciones en Rust diseñadas para eludir las restricciones de escritorio originales sin alterar el árbol de componentes principal de React.

### 1. Reestructuración Avanzada del DOM (Radix UI)
La aplicación original depende de Radix UI para la gestión de modales, lo cual tiene un comportamiento errático en pantallas móviles. Hemos implementado un paradigma estricto de tarjetas flotantes:
* **Modales Flotantes al 95%:** Los diálogos (como Ajustes) se separan forzosamente de sus contenedores nativos usando `position: absolute` y `transform: translate(-50%, -50%)`. Están limitados matemáticamente a un ancho de `95vw` y un alto de `75vh`, con bordes redondeados para emular la interfaz nativa de Android.
* **El Hack del Fondo de 2000px:** Para solucionar el fallo de renderizado de capas oscuras de Radix UI en resoluciones móviles, inyectamos una sombra masiva `box-shadow: 0 0 0 2000px rgba(0, 0, 0, 0.6)` directamente en el contenedor del diálogo. Esto garantiza un efecto de oscurecimiento perfecto en cualquier tamaño de pantalla sin depender de estados frágiles de React.

### 2. Jerarquía Estricta de Z-Index
Las pantallas móviles requieren reglas de superposición estrictas para evitar colisiones en la interfaz. El parche V18 aplica una cola Z-Index rígida y libre de conflictos:
* **`z-index: 10` (Controles Flotantes):** Los botones de QR y Tema se desvinculan de la barra superior de escritorio y se anclan en la parte inferior izquierda, flotando sobre el área de trabajo principal pero manteniéndose por debajo de cualquier modal del sistema.
* **`z-index: 40` (El Panel de Cola):** La barra lateral derecha de escritorio se transforma en un panel deslizante vertical que emerge desde abajo. Su botón se ha reubicado quirúrgicamente en la barra de reproducción (zona del altavoz) y rotado `-90deg` para indicar la expansión vertical.
* **`z-index: 50+` (Modales):** Los ajustes y superposiciones de búsqueda se mantienen en la cima de la pila visual, asegurando que ningún botón flotante perfore los fondos oscuros.

### 3. Optimización Táctil y Áreas Seguras (Notch)
* **Erradicación de la Top Bar:** El marco de la ventana de escritorio (`<header data-tauri-drag-region>`) se colapsa agresivamente a `height: 0` para recuperar espacio vertical vital.
* **Compatibilidad con Notch:** Se aplica CSS dinámico usando `env(safe-area-inset-top)` al contenedor de trabajo para evitar que la interfaz choque con el recorte de hardware de la cámara.
* **Restricciones Anti-Deformación:** Se aplican reglas profundas de anidación CSS (`flex-shrink: 0`, `white-space: nowrap`) para prohibir que el texto se rompa dentro de elementos interactivos y evitar el aplastamiento de los menús laterales en pantallas verticales estrechas.

### 4. Motor de Compilación Cruzada Backend (`patch.cjs`)
El backend original en Rust no está configurado para `aarch64-linux-android`. Nuestro *pipeline* de CI/CD inyecta un script quirúrgico (`patch.cjs`) antes de compilar para mutar agresivamente el código base:
* **Amputación de Plugins de PC:** Analiza y elimina automáticamente plugins de Tauri incompatibles (`tauri-plugin-updater`, `tauri-plugin-window-state`, `tauri-plugin-process`) del `Cargo.toml` y `lib.rs` que provocarían fallos de compilación en Android (errores de formato Exec).
* **Parches de Red y TLS:** Reescribe las dependencias de `reqwest` para forzar el uso de `rustls-tls-webpki-roots`, solucionando los problemas de validación de certificados SSL nativos de Android en arquitecturas ARM.
* **Capacidades del Sistema:** Inyecta un archivo de capacidades `android-almighty.json` para otorgar al *frontend* los permisos necesarios del sistema de archivos (`fs:*`) y gestión de ventanas dentro del *Sandbox* aislado de Android.

## 📥 Descarga
Descarga los últimos artefactos compilados para Android desde la [página de Releases](../../releases).

| Plataforma | Formatos | Arquitectura |
| :--- | :--- | :--- |
| Android | `.apk` | `aarch64` (ARM64) |

*Nota: Todos los APKs son generados, firmados y optimizados automáticamente a través de nuestro pipeline de GitHub Actions.*
## ✨ Características
- **Enfoque Móvil:** Interfaz completamente rediseñada para entornos Android.
- **Streaming Universal:** Busca música y reprodúcela desde cualquier fuente.
- **Exploración Profunda:** Navega por páginas de artistas con biografías, discografías y artistas similares.
- **Gestión de Cola:** Orden aleatorio, repetición y reordenamiento de canciones (drag-and-drop) optimizado para pantallas táctiles.
- **Sincronización de Biblioteca:** Favoritos (álbumes, artistas y pistas) y Listas de reproducción.
- **Ecosistema de Plugins:** Potente sistema de plugins con una tienda integrada para metadatos y fuentes de streaming.
- **Personalización:** Temas integrados optimizados para pantallas móviles OLED.

## 🧩 Plugins y MCP
¡Nuclear CE hereda el potente sistema de plugins del proyecto principal! Cada funcionalidad está impulsada por plugins. Los plugins pueden proporcionar fuentes de streaming, metadatos, listas de reproducción y contenido para el panel principal.
* **Tienda de Plugins:** Navega e instala plugins directamente desde la aplicación móvil.
* **Integración MCP:** El servidor del Protocolo de Contexto de Modelos (MCP) permite a agentes de IA controlar el reproductor localmente.

## 🛠️ Desarrollo y Compilación
Nuclear CE es un monorepositorio de pnpm gestionado con Turborepo. La aplicación principal está construida con Tauri (Rust + React). Este *fork* incluye parches rigurosos en el backend para permitir la compilación cruzada a `aarch64-linux-android` a través del Android NDK.
### Requisitos previos para compilar localmente
- Node.js >= 22
- pnpm >= 9
- Rust (estable) con el target `aarch64-linux-android`
- Cadenas de herramientas de Android Studio / Android NDK & SDK

### CI/CD Automatizado (GitHub Actions)
Utilizamos una arquitectura defensiva de GitHub Actions para compilar y mantener la aplicación sin intervención manual:
1. **Auto-Sincronización Defensiva con Upstream:** Un bot programado se sincroniza con el repositorio original (upstream). Utiliza una estrategia de "fail-fast squash" para heredar las nuevas características del núcleo, mientras rescata agresivamente nuestra interfaz móvil (`android_ui/`), scripts de compilación (`patch.cjs`) y avisos legales para que no sean sobrescritos.
2. **Rebranding y Limpieza Automatizada:** Los plugins de escritorio incompatibles se eliminan dinámicamente y la marca genérica "CE" se genera sobre la marcha para garantizar el cumplimiento de las marcas registradas.
3. **Compilaciones de Android Automatizadas:** El *pipeline* inyecta la interfaz móvil V18 y compila los artefactos finales `.apk` utilizando Tauri Android.
   
### Instrucciones de Compilación Manual
```bash
git clone [https://github.com/TU_USUARIO/nuclear-android.git](https://github.com/TU_USUARIO/nuclear-android.git)
cd nuclear-android
pnpm install
# 1. Aplicar parches de compilación cruzada específicos para Android
node patch.cjs
# 2. Compilar el frontend y el APK de Android
cd packages/player
pnpm run build:frontend
npx tauri android build --apk --target aarch64
