# CHANGELOG — Aprender Herramientas de Desarrollador

Historial de la app, en orden. Fechas en hora de Ciudad de México.

## 2026-11-19 — Plan aprobado

Gerardo pide una app tipo Duolingo para aprender git, Slack, GitHub y otras herramientas de desarrollador. Se acuerda primero el plan (temas y niveles) y luego la app. El plan aprobado:

- 9 rutas completas: Git, GitHub, Slack, Terminal, VS Code, Docker, Jira, CI/CD y Kubernetes (Docker, Jira, CI/CD y Kubernetes entran como rutas desde el inicio, no como "más adelante").
- Niveles Principiante, Intermedio y Avanzado por ruta, con unidades de lecciones de ~5 minutos.
- 4 tipos de ejercicio: elegir el comando correcto, ordenar los pasos, completar el comando y terminal simulada.
- Gamificación: XP por lección, racha diaria, 3 corazones por lección, ligas semanales, logros y certificado de nivel por ruta completada.

## 2026-11-19 — App web construida

Se construye la app como artefacto web fullstack de Muse (`aprender-herramientas-de-desarrollador`): 9 rutas, 4 tipos de ejercicio, 13 logros, ligas semanales (Bronce a Diamante), progreso guardado en la app y certificado por ruta.

## 2026-11-19 — Corrección de legibilidad

Gerardo reporta: al seleccionar una opción de las respuestas cambia de color y deja de verse la respuesta. Causa: el fondo de selección era un pastel claro fijo pero el texto seguía el color del tema (casi blanco en modo oscuro). Arreglo: el texto de la opción seleccionada (y de la respuesta correcta al comprobar) usa un color oscuro fijo, legible en modo claro y oscuro; aplicado también a las fichas de completar, al selector de ruta activa y a otros textos sobre fondos claros. Gerardo confirma: "Perfecto se ve bien".

## 2026-11-19 — Login

Se agrega login a la app web: cuentas con usuario y contraseña (con nombre para los certificados), sesión persistente en el teléfono, progreso por cuenta. La primera cuenta creada hereda el avance previo a la existencia del login. Acciones del servidor: `register`, `login`, `getsession`, `getprogress`, `completelesson`, `setdisplayname`, `resetprogress`, `logout`.

## 2026-11-19 — APK 1.0

Se entrega la APK 1.0 (`versionCode` 1, `versionName` 1.0, paquete `mx.gerardo.devtools`, 3,509,606 bytes, md5 `417ceded2d75a74095756d9d5708f7a0`, firmada con keystore permanente, SHA-256 `bd1d91d66efb95f14cd340d2be8a76b1fcec89dfc8e116386f0cd8ccb5fd7aa1`). No es un envoltorio del artefacto web (no tiene URL pública): es una reconstrucción local aparte con el mismo contenido (`www/index.html` + `www/data/*.js`, localStorage, login local por cuenta). Verificada estructuralmente (badging, firma, `index.html` empaquetado); sin probar aún en un teléfono real. El avance NO se sincroniza entre la web y la APK 1.0.

## 2026-11-19 — Documentación y GitHub

Se crea la documentación en `docs/` (README.md, PREGUNTAS-DEVS.md y este CHANGELOG.md). Gerardo crea el repositorio público `gcruzhz/aprender-herramientas-desarrollador` (la Muse GitHub App no puede crear repositorios) y los 3 documentos se suben el 2026-11-20, commit `52a4ca3b`. El repo contiene solo la documentación; el código de la app vive en el artefacto web y el de la APK en `~/workspace/android/devtools-app/`.

## 2026-11-20 — Versión 2.0 en la nube (en curso)

Gerardo pregunta si la web y la APK pueden sincronizar el avance y aprueba la versión en la nube (00:23). Se decide un proyecto Firebase nuevo y separado (`devtools-aprender`, plan Spark), con Firebase Auth (usuario y contraseña, correos sintéticos `usuario@devtools.local`) y Firestore (`users`, `usernames`, `progress`; cada usuario solo ve lo suyo), con caché offline en la APK. En la APK se integra la capa de nube (`www/cloud/firebase.js` + bundle esbuild `www/cloud/firebase.bundle.js`, md5 `98c44bccc9f4495cd896ea8fb5a9f145`), login en la nube con correo de recuperación opcional, vista de "Recuperar contraseña", migración del avance local en el primer inicio de sesión en la nube y sincronización en vivo entre sesiones. El código 2.0 de la APK quedó parcheado, con el bundle reconstruido y `npx cap sync` hecho, y se verificó en modo local con 15/15 pruebas automatizadas (registro, lección completa de Git, +15 XP, 3 estrellas, logro, perfil "Versión 2.0 · Local", re-login con XP persistente). Durante la verificación se encontró y corrigió un `async` huérfano que el parche había dejado antes de la vista de recuperar contraseña (rompía toda la app al arrancar; `node --check` no lo detecta porque es válido sintácticamente).

Pendiente para cerrar la 2.0: crear el proyecto Firebase (necesita el inicio de sesión de Gerardo en la CLI de Firebase — la sesión iniciada el 2026-11-20 es `A0121`), activar Auth por correo/contraseña y Firestore, publicar las reglas, pegar la configuración real en `www/firebase-config.js`, reconstruir el bundle, `npx cap sync` y compilar la APK 2.0 (mismo `appId` y keystore, `versionCode` 2). Después, pasar la app web al mismo backend de Firebase y verificar la sincronización web ↔ APK.

## 2026-11-20 — Proyecto Firebase creado y APK 2.0 compilada

El inicio de sesión de la CLI se completó a la segunda (la sesión `A0121` expiró sin usarse; la `FA59B` entró bien, 00:52). Proyecto `devtools-aprender` creado por CLI (nombre visible "Aprender Herramientas Dev", consola https://console.firebase.google.com/project/devtools-aprender/overview), con Firestore en `nam5` y APIs de Firestore e Identity Toolkit habilitadas. Auth por correo/contraseña se activó por la consola (el PATCH por CLI fallaba por la ruta Node→proxy del sandbox) y las reglas de Firestore (38 líneas: `users`, `usernames`, `progress`) se publicaron también por consola (el `firebase deploy` por CLI fallaba igual). App web registrada por CLI (App ID `1:977997910205:web:6844e62b522ba79f9da7d4`) y configuración real pegada en `www/firebase-config.js` (md5 `d310a0ea6d56af91294775e7f6f9b78b`). El bundle NO embebe la configuración (se lee de `window.DEVTOOLS_FIREBASE_CONFIG` en runtime), así que su md5 no cambió con la config: sigue `98c44bccc9f4495cd896ea8fb5a9f145`.

APK 2.0 compilada y verificada (08:16): `versionCode` 2, `versionName` 2.0, paquete `mx.gerardo.devtools`, 3,648,739 bytes, md5 `86c0bd944a0c6c25aad854707c354855`, firmada con el mismo keystore permanente (SHA-256 `bd1d91d66efb95f14cd340d2be8a76b1fcec89dfc8e116386f0cd8ccb5fd7aa1`, firma v2 verificada), contenido empaquetado verificado por md5 (`index.html`, `firebase-config.js` real y bundle). Se instala sobre la 1.0. Respaldo de la 1.0 en `~/workspace/tmp/aprender-herramientas-de-desarrollador-1.0.apk.bak` (md5 `417ceded2d75a74095756d9d5708f7a0`). Notas del build: la VM se reinició a media mañana y vació `/root/.gradle`, así que hubo que descargar Gradle 8.14.3 con curl (el wrapper no descarga por el proxy) y agregar los `systemProp` de proxy a `android/gradle.properties`.

Pendiente: verificar la sincronización web ↔ APK con una cuenta real en un navegador o teléfono, y subir estos docs a GitHub.

## 2026-11-20 — App web pasada a Firebase (auditoría aprobada)

La app web (`aprender-herramientas-de-desarrollador`) ahora usa el mismo backend Firebase que la APK: SDK cliente Firebase 11.10.0, Auth por correo/contraseña con login por usuario (correo sintético `usuario@devtools.local`, mapeo en `usernames`, perfil en `users/{uid}`), recuperación de contraseña al correo guardado, sesión persistente, progreso en `progress/{uid}` con el formato compartido con la APK y suscripción en vivo (`onSnapshot`); el perfil muestra "Versión 2.0 · Nube" y el avance viejo de la web se migra a Firestore en el primer inicio de sesión, solo si aún no hay documento en la nube. La auditoría del cambio pasó sin errores de consola ni desbordes: pantalla de login sin cambios, campo opcional de correo de recuperación en el registro, vista de "¿Olvidaste tu contraseña?" y aviso claro "No se pudo conectar con la nube" con botón Reintentar cuando la nube no responde. Comprobaciones del backend: Identity Toolkit responde 200, Firestore accesible y las reglas se comportan como se espera (lectura pública de un `usernames` inexistente da 404 limpio; `runQuery` POST da 200). Las acciones viejas del servidor quedan solo como fuente de lectura para la migración.

Sin verificar todavía de punta a punta: crear una cuenta real, completar una lección y volver a entrar contra Firebase Auth — los POST a `identitytoolkit.googleapis.com` se quedan colgados desde el sandbox (GET al mismo host responde en 0.66 s y los POST a Firestore en 0.89 s; los POST de Auth no responden ni a 25, 40 o 150 s). Ese ciclo necesita confirmarse una vez en un navegador o teléfono real. Algunos intentos de registro de diagnóstico (`diag*@devtools.local`) pueden existir o no en el servidor; no se escribió ningún dato de progreso de prueba en Firestore.

## 2026-11-20 — Dos rutas nuevas: SQL y Kali Linux

Gerardo pide agregar SQL (MySQL/MariaDB) y herramientas de Kali Linux. Se agregan como dos rutas nuevas con la misma estructura de las demás: niveles Principiante, Intermedio y Avanzado, lecciones de ~5 minutos y los 4 tipos de ejercicio.

- **SQL** (`www/data/sql.js`): Principiante — qué es SQL y una base de datos, SELECT, filtrar con WHERE/ORDER BY/LIMIT; Intermedio — INSERT/UPDATE/DELETE, JOINs; Avanzado — GROUP BY/HAVING y funciones de agregado, índices/usuarios/respaldos. 7 lecciones, 35 ejercicios.
- **Kali Linux** (`www/data/kali.js`): Principiante — qué es Kali Linux, terminal y navegación básica, permisos de archivos y usuarios; Intermedio — nmap, netcat y Wireshark; Avanzado — Metasploit, Hydra y John the Ripper. 7 lecciones, 35 ejercicios. Con enfoque educativo: solo se practica en equipos y redes propias o de laboratorio, con permiso.
- Ambas rutas están cargadas en `www/index.html` y verificadas estructuralmente (carga sin errores, misma estructura que Git y Docker: 3+2+2 lecciones, 4 tipos de ejercicio).
- **APK 2.1 compilada y verificada el mismo día:** `versionCode` 3, `versionName` 2.1, mismo `appId` y keystore permanente (SHA-256 `bd1d91d66efb95f14cd340d2be8a76b1fcec89dfc8e116386f0cd8ccb5fd7aa1`, firma v2 verificada), 3,657,538 bytes, md5 `dde17545926b252c774260f41771630d`. El `index.html` y los datos empaquetados (`sql.js`, `kali.js`, `firebase-config.js` real) se verificaron por md5 contra la fuente. Se instala sobre la 2.0; respaldo de la 2.0 en `~/workspace/tmp/aprender-herramientas-de-desarrollador-2.0.apk.bak` (md5 `86c0bd944a0c6c25aad854707c354855`). Verificación en modo local: 18/18 pruebas jsdom (11 tarjetas de ruta, lección 0 de SQL completa con +15 XP y 3 estrellas, perfil "Versión 2.1 · Local", re-login con XP persistente, 7 nodos en Kali) más la comprobación de que el prompt `mysql>` se renderiza en la simulación de terminal. La VM se reinició antes del build y vació `/root/.gradle`: hubo que re-descargar Gradle 8.14.3 con curl (224,584,249 bytes, verificado con `unzip -t`) y re-importar la CA del proxy (121 certs). El build tardó 6m 6s.
- **App web actualizada el mismo día:** las dos rutas se agregaron con el contenido exacto de la APK (verificado palabra por palabra contra `www/data/sql.js` y `www/data/kali.js`, incluidas las respuestas alternativas de los ejercicios de terminal), al final del currículo después de Kubernetes, con el mismo soporte de prompt propio del ejercicio (`mysql>`, `kali@kali:~$`) y el perfil en "Versión 2.1 · Nube". Login, Firebase, logros, ligas y el progreso existente no se tocaron.

Subidos a GitHub el 2026-11-20, en el mismo push que cubre la 2.0 y la 2.1 (aprobado por Gerardo ese día).

## 2026-11-20 — Versión 2.2: 60 lecciones por ruta y arreglo del color del nombre

Gerardo probó la APK 2.1 en su teléfono y pidió dos cosas: que el nombre de cada herramienta se lea bien (estaba en negro y se perdía con el fondo) y más lecciones por nivel. Lo pidió en tres pasos: primero 3 por nivel, luego 5, y al final 15 por nivel. Meta final: 15 lecciones por nivel × 4 niveles = 60 lecciones por ruta, 660 en total.

- **Color del nombre:** la tarjeta de ruta es un `<button>`; su color de texto lo pone el navegador (negro en páginas con `color-scheme` claro) mientras el fondo en modo oscuro viene del media query. Arreglo: `color:var(--ink)` explícito en `.route-card` y `.route-info b`, más `<meta name="color-scheme" content="light dark">` y `color-scheme:light dark` en `:root`. No hay navegador headless en el sandbox para captura de pantalla: el arreglo se verificó en código y en las pruebas jsdom (la regla está en el CSS empaquetado), y queda pendiente verlo en el teléfono real de Gerardo en modo oscuro. El nivel 1 pasa a llamarse "Básico" (antes "Principiante").
- **60 lecciones por ruta:** cada ruta tenía 7 lecciones (3 Principiante, 2 Intermedio, 2 Avanzado) y queda en 60 (15 Básico, 15 Intermedio, 15 Avanzado, 15 Experto). Se agregan 53 lecciones nuevas por ruta (Básico +12, Intermedio +13, Avanzado +13, Experto +15), 583 en total, cada una con 5 ejercicios en orden `[mc, mc, complete, terminal, order]` (3,300 ejercicios en total). Las 7 existentes no se tocan (solo se les agrega el `id` y el renombre del nivel). Las de Experto son avanzadas de verdad: escenarios reales, buenas prácticas, solución de problemas y automatización. El contenido se escribió ruta por ruta (un agente por ruta, con validador por ruta) y se verificó al final con el validador completo: 17,220 comprobaciones, 0 fallos. Dos agentes se cayeron a medio camino por límite de salida (Docker y GitHub) y se relanzaron por tandas pequeñas; sus archivos no habían cambiado nada.
- **IDs estables y migración del avance:** cada lección tiene id (`<ruta>-b01..b15`, `-i01..i15`, `-a01..a15`, `-e01..e15`) y el avance se guarda por id en vez de por índice plano. Sin esto, al crecer los niveles el avance viejo se marcaría en la lección equivocada. Al cargar, las claves numéricas viejas (0-6) se migran solas a su id (0→b01, 1→b02, 2→b03, 3→i01, 4→i02, 5→a01, 6→a02), en local y en la nube. El logro "Maestro dev" ahora dice "Completa todas las rutas".
- **Formato heredado (deuda, no un error nuevo):** las 7 lecciones originales de 2.1 de CI/CD, Terminal, Kubernetes, Jira, VS Code y Docker usan otro orden de ejercicios y sus `complete` no tienen pregunta o tienen 4 opciones. El reproductor las maneja igual que antes y no se tocaron por decisión expresa (la tarea lo prohibía); el validador las exime de los chequeos de formato nuevo pero sigue verificando que estén intactas contra el respaldo 2.1. Las 583 nuevas siguen el formato estándar.
- **APK 2.2 compilada y verificada el mismo día:** `versionCode` 4, `versionName` 2.2, mismo `appId` y keystore permanente (SHA-256 `bd1d91d66efb95f14cd340d2be8a76b1fcec89dfc8e116386f0cd8ccb5fd7aa1`, firma v2 verificada), 3,958,554 bytes, md5 `95e2d78710f07df4ec8a5515100ed850`. El `index.html` y los datos empaquetados (las 11 rutas, `firebase-config.js` real) se verificaron por md5 contra la fuente. Se instala sobre la 2.1; respaldo de la 2.1 en `~/workspace/tmp/aprender-herramientas-de-desarrollador-2.1.apk.bak` (md5 `dde17545926b252c774260f41771630d`). Verificación en modo local: 32/32 pruebas jsdom (11 tarjetas, 60 nodos en Git y SQL, niveles Básico/Intermedio/Avanzado/Experto, lección Experto git-e15 completa con +15 XP y 3 estrellas, migración del avance viejo, perfil "Versión 2.2 · Local", re-login con XP persistente). La VM se reinició antes del build y vació `/root/.gradle`: hubo que re-descargar Gradle 8.14.3 con curl (224,584,249 bytes, verificado con `unzip -t`) y re-importar la CA del proxy (121 certs). El build tardó 4m 39s.
- **App web actualizada el mismo día:** mismo contenido que la APK (copiado textual de `www/data/*.js`, mismos IDs), mismo arreglo de color, misma migración del avance y perfil "Versión 2.2 · Nube". Login, Firebase, logros, ligas y el progreso existente no se tocan salvo la migración.

## Pendiente

- Verificación de sincronización web ↔ APK con una cuenta real en un navegador o teléfono (pendiente; la app web ya usa Firebase y la APK 2.0 también, falta el ciclo completo: registrar, completar una lección, salir y volver a entrar).
- Subidos a GitHub el 2026-11-20 (aprobado por Gerardo ese día).
- Detalle del stack interno del artefacto web.
- Pruebas de Gerardo en teléfono real (APK 2.2: nombres legibles en claro y oscuro, lección de Experto, avance viejo en su lección; también siguen pendientes las de 1.0 y 2.0).
