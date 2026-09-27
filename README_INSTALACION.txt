LIVESPEECH + MATOMO CLOUD — INSTALACIÓN
========================================

OBJETIVO
--------
La web principal ya tiene Matomo Cloud instalado mediante window._paq.
LiveSpeech funciona dentro de un iframe y el vídeo de YouTube está dentro de ese iframe.
Por eso Matomo no puede detectar de forma fiable las interacciones internas.

Esta versión hace lo siguiente:

1. Client/script.js detecta las acciones reales del reproductor y de LiveSpeech.
2. Las envía a la página principal mediante window.parent.postMessage().
3. embed.js recibe esos mensajes y los manda a window._paq de Matomo.
4. Si Media Analytics está disponible, también alimenta un MediaTracker personalizado llamado "livespeech".

ARCHIVOS QUE HAY QUE SUSTITUIR
------------------------------

1. Sustituir el embed.js actual por el embed.js de esta carpeta.
2. Sustituir Client/script.js actual por Client/script.js de esta carpeta.
3. NO cambiar config.json.
4. NO cambiar index.html.
5. NO cambiar el snippet actual de Matomo Cloud de la página principal.

RECOMENDACIÓN
-------------
Antes de sustituirlos, guardar una copia de seguridad:

embed.js -> embed.backup.js
Client/script.js -> Client/script.backup.js

EVENTOS QUE APARECERÁN EN MATOMO
--------------------------------

Categoría: LiveSpeech Video
- Impression
- Play
- Pause
- Finish
- Milestone 25%
- Milestone 50%
- Milestone 75%
- Milestone 90%

Categoría: LiveSpeech Accessibility
- Language Change
- Subtitles On
- Subtitles Off
- TTS On
- TTS Off

Categoría: LiveSpeech Controls
- Mute
- Unmute
- Volume Change
- Fullscreen Enter
- Fullscreen Exit

MEDIA ANALYTICS
---------------
Si el plugin Media Analytics está disponible en el matomo.js de vuestra cuenta Cloud,
el código crea un MediaTracker personalizado denominado "livespeech" y le comunica:

- impresión
- reproducción
- pausa
- finalización
- progreso/posición
- duración
- buffering
- fullscreen
- dimensiones del reproductor

El progreso interno se actualiza cada 5 segundos, pero NO se crea un evento normal de
Matomo cada 5 segundos. Así no se llena el informe de Eventos con miles de registros.

PRUEBA RÁPIDA
-------------

1. Abrir una página real que contenga LiveSpeech.
2. Abrir Chrome DevTools (F12).
3. Ir a Network/Red.
4. Filtrar por: matomo.php
5. Recargar la página.
6. Reproducir el vídeo.
7. Pausar.
8. Cambiar de idioma.
9. Desactivar/activar subtítulos.
10. Activar fullscreen.

Deben aparecer nuevas peticiones a matomo.php.

En Matomo, comprobar los Eventos con estas categorías:
- LiveSpeech Video
- LiveSpeech Accessibility
- LiveSpeech Controls

NOTA SOBRE LA CACHÉ
-------------------
Si la web usa caché/CDN, vaciar la caché después de subir los archivos. También conviene
hacer una recarga forzada en el navegador (Ctrl+F5 / Cmd+Shift+R).

PERSONALIZAR EL NOMBRE DEL VÍDEO
--------------------------------
Por defecto Matomo usará algo como:
session_1 | G5b1mC5-GEA

Opcionalmente podéis poner un nombre más legible en el contenedor LiveSpeech:

data-matomo-title="Sesión 1 - Apertura"

Ejemplo:
<div class="livespeech-embed"
     data-session="session_1"
     data-matomo-title="Sesión 1 - Apertura"></div>

