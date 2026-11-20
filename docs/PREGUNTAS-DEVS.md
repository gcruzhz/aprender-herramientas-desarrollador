# Preguntas de desarrolladores — Aprender Herramientas de Desarrollador

Documento para que un desarrollador entienda la app rápido: qué es, cómo está hecha a grandes rasgos y qué se le puede preguntar. Solo contiene lo verificado; lo que aún no se sabe está marcado como pendiente.

## ¿Qué es la app?

Una app estilo Duolingo, en español, para aprender herramientas de desarrollo jugando: Git, GitHub, Slack, Terminal, VS Code, Docker, Jira, CI/CD, Kubernetes, SQL (MySQL/MariaDB) y Kali Linux. Lecciones de unos 5 minutos, 4 tipos de ejercicio (elegir el comando, ordenar pasos, completar el comando y terminal simulada), 3 corazones por lección, XP, racha diaria, ligas semanales (Bronce a Diamante), 13 logros y certificado al completar cada ruta.

## ¿Dónde vive la app?

Como artefacto web fullstack de Muse (slug: `aprender-herramientas-de-desarrollador`). El código no está en este repositorio: este repo solo tiene documentación (`docs/`). El APK de Android es una reconstrucción local aparte con el mismo contenido (`~/workspace/android/devtools-app/`: un solo `www/index.html` + `www/data/*.js`, localStorage y login local por cuenta) — el artefacto web no tiene URL pública, así que no se puede envolver en un WebView. Desde la versión 2.0 la app web y la APK comparten el mismo backend en Firebase (migración de la web terminada el 2026-11-20, auditoría aprobada; falta confirmar el ciclo completo con una cuenta real en un navegador o teléfono).

## ¿En qué lenguaje / con qué stack está hecha?

Pendiente de documentar con detalle. Lo que sí está verificado: es una app web con servidor (acciones `register`, `login`, `getsession`, `getprogress`, `completelesson`, `setdisplayname`, `resetprogress`, `logout`) y guarda el progreso por cuenta de usuario. No se afirma aquí un lenguaje o framework concreto hasta confirmarlo.

Desde la versión 2.0 (en curso, 2026-11-20) el backend es Firebase, compartido por la app web y la APK: proyecto nuevo y separado `devtools-aprender` (plan Spark), Firebase Auth con correo y contraseña (login por usuario con correos sintéticos `usuario@devtools.local`) y Firestore con colecciones `users`, `usernames` y `progress` (reglas: cada usuario solo ve lo suyo). La APK es JavaScript de una sola página (sin framework) con Capacitor 8 y usa caché persistente de Firestore para funcionar offline; la capa de nube es `www/cloud/firebase.js`, compilada con esbuild a `www/cloud/firebase.bundle.js`. El modelo es el mismo que el de la app Checklist de Soporte en la nube (proyecto `checklist-soporte`), pero con proyecto Firebase separado.

## ¿Cómo funciona el login?

Cuentas con usuario y contraseña (más un nombre para los certificados). La sesión es persistente: queda guardada en el teléfono. El progreso es por cuenta. La primera cuenta creada hereda el avance hecho antes de que existiera el login (XP, lecciones y logros previos no se pierden).

En la versión 2.0 el login es con Firebase Auth: el usuario se resuelve a un correo sintético `usuario@devtools.local`. Al registrarse se puede dejar un correo real de recuperación (opcional) para el enlace de "Olvidé mi contraseña"; sin él, una contraseña olvidada no se puede recuperar y hay que crear otra cuenta. Las cuentas 1.0 no migran automáticamente: cada usuario vuelve a registrarse en la 2.0 con el mismo usuario y contraseña, y en el primer inicio de sesión en la nube su avance local existente se sube a Firestore si aún no hay avance en la nube.

## ¿Cómo se guarda el progreso?

Por usuario: XP total, racha diaria, lecciones completadas por ruta y nivel, logros desbloqueados (de 13) y nombre para certificados. Las lecciones se desbloquean paso a paso en un camino por ruta.

En la versión 1.0 el progreso vive en la base del artefacto web (app web) o en localStorage (APK), sin sincronizar entre sí. En la versión 2.0 vive en Firestore (`progress/{uid}`), compartido por la app web y la APK; la APK guarda además una copia local y sincroniza en vivo con `onSnapshot` al volver la conexión.

## ¿Qué rutas y niveles hay?

11 rutas: Git, GitHub, Slack, Terminal, VS Code, Docker, Jira, CI/CD, Kubernetes, SQL (MySQL/MariaDB) y Kali Linux. Cada una con niveles Principiante, Intermedio y Avanzado. El contenido por nivel está detallado para Git, GitHub, Slack, Terminal, VS Code, SQL y Kali Linux; el de Docker, Jira, CI/CD y Kubernetes está pendiente de definir (ver `docs/README.md`).

## ¿Hay APK?

Sí. La APK 1.0 se entregó el 2026-11-19 (paquete `mx.gerardo.devtools`, `versionCode` 1, firmada con keystore permanente, md5 `417ceded2d75a74095756d9d5708f7a0`). Es una reconstrucción local aparte, no un envoltorio del artefacto web. La APK 2.0 se compiló el 2026-11-20 (mismo `appId` y keystore, `versionCode` 2, 3,648,739 bytes, md5 `86c0bd944a0c6c25aad854707c354855`, firma v2 verificada; se instala encima de la 1.0) y ya lleva la nube: proyecto `devtools-aprender`, configuración real y sincronización en vivo. La APK 2.1 se compiló el mismo 2026-11-20 (mismo `appId` y keystore, `versionCode` 3, 3,657,538 bytes, md5 `dde17545926b252c774260f41771630d`, firma v2 verificada; se instala encima de la 2.0) y agrega las rutas SQL (MySQL/MariaDB) y Kali Linux, 7 lecciones cada una, con el mismo contenido que la app web. Pendiente de pruebas en un teléfono real. La app web 1.0 necesita internet porque el login y el progreso viven en el servidor; la APK 2.0 funciona offline y sincroniza al volver la conexión.

## ¿Qué se le puede preguntar a un dev que revise la app?

- Cómo agregar una ruta o una lección nueva (contenido y niveles).
- Cómo funciona la terminal simulada (qué comandos valida y cómo).
- Cómo se calculan XP, racha y ligas, y dónde vive esa lógica (servidor).
- Cómo se hace el despliegue del APK y qué permisos necesita (internet).
- Cómo se respalda o se migra el progreso de los usuarios.

## Enlaces

- App web: artefacto `aprender-herramientas-de-desarrollador` en Muse.
- Repo (documentación): https://github.com/gcruzhz/aprender-herramientas-desarrollador (creado por Gerardo el 2026-11-19; docs subidos el 2026-11-20, commit `52a4ca3b`).
- Proyecto Firebase 2.0: `devtools-aprender` (creado el 2026-11-20; la consola es https://console.firebase.google.com/project/devtools-aprender). Auth por correo/contraseña activado y reglas de Firestore publicadas; app web registrada (App ID `1:977997910205:web:6844e62b522ba79f9da7d4`).
