🎙️ VozLectura
Lector de Textos con Voz — Portable Web App
Documentación Técnica Completa + Guía de Despliegue en GitHub Pages
Desarrollado por ConstruyoTuProyecto.com
Versión 1.0  ·  2025

Tabla de Contenidos
1.  Descripción General..............................................  
2.  Características y Funcionalidades..............................................  
3.  Arquitectura del Proyecto..............................................  
4.  Tecnologías Utilizadas.............................................. 
5.  Estructura de Archivos..............................................  
6.  Funciones JavaScript — Referencia Completa.............................................. 
7.  Guía de Estilos CSS.............................................. 
8.  Compatibilidad con Navegadores..............................................  
9.  Despliegue en GitHub Pages — Paso a Paso..............................................  
10.  Configuración de Dominio Personalizado (Opcional)..............................................  
11.  Uso de la Aplicación.............................................. 
12.  Preguntas Frecuentes (FAQ).............................................. 
13.  Créditos y Licencia..............................................  
    
1. Descripción General
VozLectura es una herramienta web portable de conversión de texto a voz (TTS) diseñada para funcionar como un archivo HTML autocontenido. No requiere instalación, servidores backend, ni conexión a internet (salvo para cargar las fuentes de Google). Funciona directamente al abrir el archivo en un navegador moderno.
El proyecto aprovecha la Web Speech API nativa de los navegadores para sintetizar voz, ofreciendo una interfaz moderna con soporte de múltiples voces en español e inglés, controles de velocidad, tono y volumen en tiempo real, navegación por saltos de ±10 segundos, y un sistema de reproducción por frases (chunks) que permite actualizaciones dinámicas de parámetros sin reiniciar la lectura.


🎯 Caso de uso principal: Lectura de documentos, artículos, guiones o cualquier texto en español e inglés, con control total sobre la voz y velocidad. Ideal para estudiar, revisar contenido o accesibilidad.

2. Características y Funcionalidades
2.1 Reproducción de Voz
•	Voces en Español: Detecta automáticamente todas las voces es-* instaladas en el sistema (es-ES, es-MX, es-AR, etc.)
•	Voces en Inglés: Sección separada con voces en-* priorizando géneros (masculino/femenino), máximo 6 voces
•	Detección de género: Clasifica voces como ♀ Femenino, ♂ Masculino o ◈ Neutro basado en el nombre de la voz
•	Sistema de chunks: El texto se divide en frases/oraciones. Cada frase es una utterance independiente, permitiendo parámetros dinámicos

2.2 Controles Dinámicos en Tiempo Real
•	Velocidad (Rate): 0.5× a 2.0× con slider continuo
•	Tono (Pitch): 0.5 a 2.0 con slider continuo
•	Volumen: 0% a 100% con slider continuo
•	Presets rápidos: 🐢 0.75×, ▶ 1.0×, ⚡ 1.25×, 🚀 1.5×
•	Cambio de voz en medio de la lectura — se aplica en la siguiente frase


⚡ Todos los cambios de parámetros se aplican al chunk siguiente sin reiniciar desde el inicio. La Web Speech API no permite modificar utterances en curso, por lo que el sistema cancela el chunk actual y relanza el siguiente con los nuevos valores.

2.3 Navegación y Playback
•	⏪ Retroceder 10 segundos (también con tecla ←)
•	⏩ Adelantar 10 segundos (también con tecla →)
•	⏸ Pausar / ▶ Reanudar sin perder la posición
•	⏹ Detener y resetear posición
•	✂️ Leer solo la selección del textarea
•	Ctrl + Enter: atajo de teclado para Play/Pausa

2.4 Interfaz y UX
•	Header sticky con efecto glassmorphism (backdrop-filter: blur)
•	Visualizador de ondas animado durante la reproducción
•	Barra de progreso por frases con contador Frase N / Total
•	Estado de reproducción en tiempo real en la barra de status
•	Editor de texto con contador de caracteres
•	Carga de texto de ejemplo
•	Diseño responsive para móvil y desktop
•	Historial de textos recientes (localStorage)
•	Footer con link a ConstruyoTuProyecto.com
•	Botón flotante de WhatsApp (walink.co/)

3. Arquitectura del Proyecto
VozLectura es un proyecto de archivo único (Single File Application). Todo el HTML, CSS y JavaScript reside en un solo archivo lector-voz.html. No tiene dependencias externas de JavaScript — solo carga dos fuentes tipográficas de Google Fonts.

Capa	Implementación
Presentación (HTML)	Estructura semántica con secciones: header, controles, transport, historial, footer
Estilos (CSS)	CSS Variables + Flexbox/Grid. Sin frameworks. Tema oscuro con acentos gradiente
Lógica (JS)	Vanilla JavaScript ES6+. Web Speech API (SpeechSynthesisUtterance). Sin librerías
Almacenamiento	localStorage para historial de textos recientes (máx 8 entradas)
Fuentes	Google Fonts CDN: Syne (títulos) + DM Sans (cuerpo)

3.1 Flujo de Reproducción (Sistema de Chunks)
1.	El texto se divide en oraciones usando el regex: /[^.!?\n]+[.!?\n]*/g
2.	Cada oración se convierte en una SpeechSynthesisUtterance con los parámetros actuales
3.	Al terminar cada chunk, onend dispara el siguiente chunk automáticamente
4.	Si el usuario cambia parámetros: se cancela el chunk actual, el siguiente se crea con los nuevos valores
5.	skipSeconds() calcula cuántos chunks equivalen a N segundos según la velocidad actual (~3s/chunk base)
6.	Pausa/Reanuda usa synth.pause() y synth.resume() nativos

4. Tecnologías Utilizadas
Tecnología	Versión / Tipo	Propósito
HTML5	Estándar W3C	Estructura de la aplicación
CSS3	Variables + Grid + Flex	Estilos, animaciones, responsive
JavaScript ES6+	Vanilla (sin frameworks)	Toda la lógica de la app
Web Speech API	Browser Native API	Síntesis de voz (TTS)
Google Fonts	CDN External	Tipografías Syne + DM Sans
localStorage	Browser Storage API	Persistencia del historial
GitHub Pages	Hosting Estático	Despliegue y distribución

5. Estructura de Archivos del Repositorio
Para GitHub Pages, el repositorio mínimo contiene:

vozlectura/
├── index.html          ← Aplicación principal (lector-voz.html renombrado)
├── README.md           ← Descripción del proyecto para GitHub
├── LICENSE             ← Licencia del proyecto (MIT recomendado)
└── .github/
    └── workflows/
        └── deploy.yml  ← (Opcional) GitHub Actions para auto-deploy


💡 Importante: Renombra lector-voz.html a index.html para que GitHub Pages lo sirva automáticamente en la raíz del sitio (https://usuario.github.io/repositorio/).

6. Funciones JavaScript — Referencia Completa
6.1 Gestión de Voces
Función	Descripción
loadVoices()	Carga todas las voces del sistema. Separa español (es-*) e inglés (en-*). Llama a renderVoices() para cada grupo. Se ejecuta en onvoiceschanged y con setTimeout(200ms, 800ms) para asegurar carga asíncrona.
renderVoices(list, grid, noMsg, badge, autoSelect)	Renderiza tarjetas de voz en el grid. Si autoSelect=true, selecciona la primera voz automáticamente. Muestra el badge con el conteo. Detecta género con guessGender().
guessGender(name)	Analiza el nombre de la voz contra listas de nombres femeninos y masculinos en español e inglés. Retorna "female", "male" o "neutral".
selectVoice(voice, card)	Actualiza selectedVoice. Remueve clase "active" de todas las tarjetas y la agrega a la seleccionada. Si isSpeaking, llama applyParamsNow().

6.2 Sistema de Chunks y Reproducción
Función	Descripción
splitChunks(text)	Divide el texto en oraciones usando el regex /[^.!?\n]+[.!?\n]*/g. Filtra strings vacíos. Retorna array de strings.
buildUtterance(text)	Crea una SpeechSynthesisUtterance con los valores actuales de los sliders (rate, pitch, volume) y la voz seleccionada. Asigna el lang de la voz o "es-ES" por defecto.
speakChunk(index)	Lanza el chunk en el índice dado. Registra onstart (actualiza UI), onend (avanza al siguiente chunk), onerror (manejo de errores interrumpidos). Cuando index >= chunks.length llama onAllDone().
onAllDone()	Callback de fin de lectura. Resetea isSpeaking, isPaused. Actualiza UI a estado "Completado" y resetea a "En espera" después de 2.5s.
applyParamsNow()	Aplica cambios de parámetros mientras se reproduce. Pone isSpeaking=false, cancela el chunk actual, restaura isSpeaking=true, y lanza el SIGUIENTE chunk (chunkIndex+1) con setTimeout(60ms).

6.3 Controles de Playback
Función	Descripción
togglePlay()	Gestiona los 3 estados: si isPaused → pauseResume(), si isSpeaking → pauseResume(), sino → startSpeech(). Corrige el bug de Play después de Pausa.
startSpeech(textOverride?)	Inicia reproducción desde el principio. Cancela síntesis previa, resetea chunkIndex=0, divide el texto en chunks, actualiza UI y llama speakChunk(0). Acepta texto override para "Leer Selección".
pauseResume()	Alterna entre pausa y reanudación usando synth.pause() y synth.resume(). Actualiza isPaused, textos de botones y animación de onda.
stopSpeech()	Detiene completamente. Llama synth.cancel(), resetea isSpeaking, isPaused, actualiza UI a estado inicial.
readSelection()	Obtiene el texto seleccionado del textarea con selectionStart/selectionEnd y llama startSpeech() con ese texto.
skipSeconds(secs)	Calcula chunks a saltar: secs / (SECS_PER_CHUNK_BASE / rate). Clampea entre 0 y chunks.length-1. Si está reproduciendo, cancela y lanza speakChunk(targetIndex). Si está pausado/detenido, solo mueve chunkIndex.

6.4 Helpers de UI
Función	Descripción
setStatus(msg, active)	Actualiza el texto de la barra de estado. Si active=true, activa el indicador de punto pulsante y el borde iluminado del status bar.
animateWave(active)	Activa o desactiva la animación CSS del visualizador de ondas toggleando la clase "animating" en cada .wave-bar.
updateProgressUI()	Calcula el porcentaje de progreso (chunkIndex / chunks.length * 100) y actualiza la barra de progreso y el label "Frase N / Total".
updateCharCount()	Lee el length del textarea y actualiza el badge de caracteres con toLocaleString().
onSliderChange(labelFn)	Wrapper para sliders: ejecuta labelFn() para actualizar el display, luego si isSpeaking && !isPaused llama applyParamsNow().
setSpeed(val)	Setter programático del slider de velocidad. Actualiza el valor y llama updateSpeedDisplay().

6.5 Historial
Función	Descripción
saveHistory(text)	Trunca el texto a 60 chars + "...", lo agrega al inicio del array history (dedup por contenido), limita a 8 entradas. Guarda en localStorage["vozHistory"]. Llama renderHistory().
renderHistory()	Si history.length > 0, muestra la sección historial y renderiza chips con title= para tooltip del texto completo.

7. Guía de Estilos CSS
7.1 Variables CSS (Design Tokens)
:root {
  --bg:       #0A0A0F   /* Fondo principal oscuro */
  --surface:  #111118   /* Cards y paneles */
  --surface2: #17171F   /* Toolbars y sub-paneles */
  --surface3: #1E1E28   /* Hover states */
  --border:   rgba(255,255,255,0.07)  /* Bordes sutiles */
  --accent:   #7C6AF7   /* Morado principal */
  --accent2:  #F76AC8   /* Rosa secundario */
  --accent3:  #6AF7C8   /* Verde-turquesa terciario */
  --text:     #E8E8F0   /* Texto principal */
  --muted:    #6A6A80   /* Texto secundario */
  --glow:     rgba(124,106,247,0.3)  /* Sombra del acento */
}

7.2 Componentes Principales
Clase / Selector	Descripción
header (sticky)	position:sticky, backdrop-filter:blur(16px), fondo semitransparente rgba(10,10,15,0.85). Clase .scrolled agrega box-shadow al hacer scroll.
.status-bar	Barra de estado con indicador pulsante. Clase .speaking activa el borde morado y la sombra.
.wave-bar	Barras del visualizador de ondas. Clase .animating activa la keyframe wave-anim con delays escalonados (0s a 0.25s).
.voice-card	Tarjetas de voz. Clase .active muestra borde morado y fondo semitransparente con glow.
.voice-gender-tag	Badge de género. 3 variantes: .female (rosa), .male (azul claro), .neutral (turquesa).
.btn-play	Botón circular principal con gradiente accent→accent2. box-shadow con glow. Escala 1.05 en hover.
.btn-skip	Botones ±10s circulares. Clase .flash activa borde morado y glow temporalmente al hacer click.
.wa-fab	Botón flotante de WhatsApp. position:fixed bottom-right. Verde #25D366 con sombra verde. Animación cubic-bezier en hover.
.fade-in	Animación de entrada (opacity 0→1, translateY 10px→0). Delays escalonados por nth-child para efecto cascada.

8. Compatibilidad con Navegadores
Característica	Chrome 80+	Edge 80+	Firefox 90+	Safari 14+
Web Speech API (TTS)	✅ Completo	✅ Completo	⚠️ Parcial	✅ Completo
onboundary (eventos)	✅ Sí	✅ Sí	❌ No	⚠️ Parcial
backdrop-filter (blur)	✅ Sí	✅ Sí	✅ Sí	✅ Sí
CSS Variables	✅ Sí	✅ Sí	✅ Sí	✅ Sí
localStorage	✅ Sí	✅ Sí	✅ Sí	✅ Sí


⚠️ Firefox: La Web Speech API tiene soporte limitado y las voces disponibles varían por sistema operativo. Se recomienda usar Chrome o Edge para la mejor experiencia.

9. Despliegue en GitHub Pages — Paso a Paso
Esta guía asume que tienes una cuenta en GitHub. No se requiere experiencia previa con Git.

Paso 1: Crear una cuenta en GitHub (si no tienes)
7.	Ve a 
8.	Haz clic en "Sign up" y completa el registro con tu email
9.	Verifica tu email y completa el onboarding

Paso 2: Instalar Git en tu computadora
Windows: Descarga Git desde https://git-scm.com/download/win. Instala con las opciones por defecto.
Mac: Abre Terminal y ejecuta: xcode-select --install — Git viene incluido con Xcode Command Line Tools.
Linux: Ejecuta en terminal: sudo apt install git (Ubuntu/Debian) o sudo dnf install git (Fedora).


Verifica que Git está instalado correctamente abriendo una terminal y ejecutando: git --version  — debe mostrar algo como "git version 2.x.x"

Paso 3: Crear el repositorio en GitHub
10.	Inicia sesión en GitHub y haz clic en el botón "+" (esquina superior derecha)
11.	Selecciona "New repository"
12.	Repository name: escribe vozlectura (o el nombre que prefieras)
13.	Visibility: selecciona 
14.	Marca la casilla "Add a README file"
15.	Haz clic en "Create repository"

Paso 4: Preparar los archivos localmente
Abre una terminal (o Git Bash en Windows) y ejecuta los siguientes comandos:

# 1. Crear una carpeta para el proyecto
mkdir vozlectura
cd vozlectura
 
# 2. Inicializar Git
git init
 
# 3. Copiar el archivo HTML al directorio
# (Copia manualmente lector-voz.html a esta carpeta y renómbralo a index.html)
# En Windows puedes hacer esto desde el Explorador de Archivos
 
# 4. Verificar que el archivo está ahí
ls    # Mac/Linux
dir   # Windows

Paso 5: Crear el README.md
Crea un archivo llamado README.md en la carpeta con el siguiente contenido:

# 🎙️ VozLectura
 
Herramienta portable de texto a voz en español e inglés.
Desarrollada con Web Speech API nativa del navegador.
 
## 🚀 Demo en vivo
[Abrir VozLectura](https://TU-USUARIO.github.io/vozlectura/)
 
## ✨ Características
- Voces en Español e Inglés (masculino/femenino)
- Velocidad, tono y volumen ajustables en tiempo real
- Saltar ±10 segundos con botones o teclado (← →)
- Pausa/Reanuda sin perder la posición
- Diseño moderno responsive
- Sin instalación — archivo HTML portable
 
## 🛠️ Tecnologías
HTML5 · CSS3 · JavaScript ES6+ · Web Speech API
 
## 👨‍💻 Desarrollado por
[ConstruyoTuProyecto.com](https://contruyotuproyecto.github.io/construyotupoyecto/)

Paso 6: Conectar con GitHub y subir los archivos
En la terminal, ejecuta:

# 1. Configurar tu identidad en Git (solo la primera vez)
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
 
# 2. Agregar los archivos al staging
git add .
 
# 3. Crear el primer commit
git commit -m "🎙️ Primer deploy de VozLectura"
 
# 4. Renombrar la rama principal a "main"
git branch -M main
 
# 5. Conectar con tu repositorio de GitHub
# (Reemplaza TU-USUARIO con tu nombre de usuario de GitHub)
git remote add origin https://github.com/TU-USUARIO/vozlectura.git
 
# 6. Subir el código
git push -u origin main


💡 La primera vez que haces git push, GitHub te pedirá autenticación. En la ventana que aparece, inicia sesión con tu cuenta de GitHub. Si usas Windows con Git Bash, el Credential Manager guardará tus credenciales.

Paso 7: Activar GitHub Pages
16.	Ve a tu repositorio en GitHub (github.com/TU-USUARIO/vozlectura)
17.	Haz clic en la pestaña "Settings" (configuración)
18.	En el menú lateral izquierdo, busca y haz clic en "Pages"
19.	En "Source", selecciona "Deploy from a branch"
20.	En "Branch", selecciona "main" y la carpeta "/ (root)"
21.	Haz clic en "Save"
22.	Espera 1-3 minutos y recarga la página
23.	Aparecerá el mensaje: "Your site is live at https://TU-USUARIO.github.io/vozlectura/"

Paso 8: Verificar el despliegue
Abre la URL en tu navegador: https://TU-USUARIO.github.io/vozlectura/
Si ves la aplicación VozLectura, ¡el despliegue fue exitoso! 🎉


🔄 Para actualizar la aplicación en el futuro: edita el index.html localmente, luego ejecuta git add . → git commit -m "Actualización" → git push. Los cambios se reflejarán en 1-2 minutos.

Paso 9 (Opcional): GitHub Actions para deploys automáticos
Crea el archivo .github/workflows/deploy.yml para automatizar validaciones:

name: Deploy VozLectura to GitHub Pages
 
on:
  push:
    branches: [ "main" ]
 
permissions:
  contents: read
  pages: write
  id-token: write
 
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: "."
      - name: Deploy to GitHub Pages
        uses: actions/deploy-pages@v2

10. Configuración de Dominio Personalizado (Opcional)
Si tienes un dominio propio (ej. vozlectura.com), puedes configurarlo para que apunte a tu GitHub Pages.

10.1 Crear archivo CNAME en el repositorio
Crea un archivo llamado CNAME (sin extensión) en la raíz del repositorio con tu dominio:
vozlectura.com

10.2 Configurar DNS en tu proveedor de dominio
Agrega los siguientes registros A en el panel de DNS de tu registrador:
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
O un registro CNAME si usas subdominio (ej. www.vozlectura.com):
www  CNAME  TU-USUARIO.github.io


⏱️ Los cambios de DNS pueden tardar hasta 48 horas en propagarse. GitHub activará HTTPS automáticamente con Let's Encrypt una vez que detecte el dominio.

11. Uso de la Aplicación
11.1 Flujo Básico
24.	Abre la aplicación en Chrome o Edge
25.	Selecciona una voz del panel "🇪🇸 Español" o "🇬🇧 English"
26.	Escribe o pega el texto en el área de texto
27.	Ajusta la velocidad, tono y volumen si lo deseas
28.	Presiona el botón ▶ (Play) o usa Ctrl+Enter
29.	La lectura comienza frase por frase con la barra de progreso activa

11.2 Atajos de Teclado
Atajo	Acción
Ctrl + Enter	Play / Pausa (desde cualquier parte de la página)
← (flecha izquierda)	Retroceder 10 segundos (solo si el foco no está en el textarea)
→ (flecha derecha)	Adelantar 10 segundos (solo si el foco no está en el textarea)

11.3 Leer Solo una Selección
30.	Selecciona con el mouse la porción de texto que deseas escuchar
31.	Haz clic en el botón "✂️ Leer Selección"
32.	Solo se leerá el fragmento seleccionado

11.4 Cambiar Parámetros Durante la Lectura
Puedes mover cualquier slider (velocidad, tono, volumen) o seleccionar una voz diferente mientras el texto se está leyendo. El cambio se aplica a partir de la siguiente frase, sin interrupciones bruscas.

12. Preguntas Frecuentes (FAQ)
¿Por qué no aparecen voces en español?
Las voces provienen del sistema operativo. En Windows, ve a Configuración → Hora e idioma → Idioma y región → Agrega el idioma "Español" e instala el paquete de voz. En Mac, ve a Ajustes del Sistema → Accesibilidad → Contenido hablado.

¿Funciona sin internet?
Sí, con una excepción: las fuentes tipográficas (Syne y DM Sans) se cargan desde Google Fonts. Si no hay internet, el navegador usará las fuentes del sistema y la app funcionará igual. Las voces son completamente locales.

¿Por qué Chrome/Edge son los recomendados?
Tienen la implementación más completa de Web Speech API. Firefox tiene soporte parcial y puede omitir eventos de boundary. Safari funciona bien en Mac pero las voces disponibles varían.

¿Los saltos de ±10 segundos son exactos?
Son aproximados. El sistema calcula que cada frase dura ~3 segundos a velocidad 1×. Esta estimación se ajusta con la velocidad actual. Para textos con frases muy cortas o muy largas, el salto puede ser ligeramente diferente.

¿Se guarda el texto que escribo?
Solo se guarda un resumen truncado de los últimos 8 textos en el historial (localStorage). El texto completo NO se guarda permanentemente y se pierde al cerrar la pestaña.

¿Cómo agrego más voces en inglés?
Instala paquetes de idioma inglés en tu sistema operativo. En Windows: Configuración → Idioma → Agrega "English (United States)" con paquete de voz. Voces comunes incluidas: Microsoft David (♂), Microsoft Zira (♀) en Windows; Samantha (♀), Alex (♂) en Mac.

13. Créditos y Licencia
13.1 Desarrollador
Proyecto	VozLectura — Lector de Textos Portable
Desarrollado por	ConstruyoTuProyecto.com / JP Developer
Contacto	https://contruyotuproyecto.github.io/construyotupoyecto/#contacto
WhatsApp	https://walink.co/
País	Costa Rica 🇨🇷

13.2 Tecnologías de Terceros
Tecnología	Proveedor	Licencia / Uso
Web Speech API	W3C / Browsers	API estándar open web, libre de uso
Fuente: Syne	Google Fonts	SIL Open Font License
Fuente: DM Sans	Google Fonts	SIL Open Font License

13.3 Licencia
Este proyecto se distribuye bajo la licencia MIT License. Puedes usar, copiar, modificar y distribuir el código libremente, incluyendo para uso comercial, siempre que incluyas el aviso de copyright original.

MIT License
 
Copyright (c) 2025 ConstruyoTuProyecto.com
 
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:
 
The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
 
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.

🎙️ VozLectura  ·  Documentación v1.0  ·  ConstruyoTuProyecto.com

