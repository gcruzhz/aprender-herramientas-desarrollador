# Aprender Herramientas de Desarrollador

App estilo Duolingo para aprender herramientas de desarrollo en español: lecciones cortas, ejercicios, puntos, racha y ligas. Primero se aprende jugando, con una terminal simulada para practicar comandos de verdad.

## Qué es

Una app de aprendizaje con 11 rutas, una por herramienta. Cada ruta tiene niveles (Principiante, Intermedio y Avanzado), unidades con lecciones de unos 5 minutos en un camino que se desbloquea paso a paso. Al completar una ruta se obtiene un certificado de nivel.

La app vive como artefacto web fullstack de Muse (slug: `aprender-herramientas-de-desarrollador`). El APK para Android es una reconstrucción local aparte con el mismo contenido (`~/workspace/android/devtools-app/`: un solo `www/index.html` + `www/data/*.js`, localStorage y login local por cuenta) — el artefacto web no tiene URL pública, así que no se puede envolver en un WebView. Desde la versión 2.0 la app web y la APK comparten el mismo backend en Firebase, así que el avance se sincroniza entre las dos.

## Rutas y niveles

| Ruta | Principiante | Intermedio | Avanzado |
|------|--------------|------------|----------|
| Git | init, status, add, commit, log | ramas, merge, remoto, push/pull | conflictos, stash, rebase, reset |
| GitHub | repos, README, issues | pull requests, fork, code review | Actions, Projects, Pages |
| Slack | canales, mensajes, hilos, reacciones | menciones, búsquedas, apps | huddles, canvas, flujos de trabajo |
| Terminal | navegar y crear archivos | permisos, buscar con grep, pipes | (por definir) |
| VS Code | atajos, extensiones, terminal integrada | depuración, multi-cursor | (por definir) |
| Docker | (contenido por definir) | (contenido por definir) | (contenido por definir) |
| Jira | (contenido por definir) | (contenido por definir) | (contenido por definir) |
| CI/CD | (contenido por definir) | (contenido por definir) | (contenido por definir) |
| Kubernetes | (contenido por definir) | (contenido por definir) | (contenido por definir) |
| SQL (MySQL/MariaDB) | qué es SQL, SELECT, WHERE/ORDER BY/LIMIT | INSERT/UPDATE/DELETE, JOINs | GROUP BY/HAVING/agregados, índices/usuarios/respaldos |
| Kali Linux | qué es Kali, terminal y navegación, permisos | nmap, netcat y Wireshark | Metasploit, Hydra y John the Ripper |

El contenido detallado por nivel de Docker, Jira, CI/CD y Kubernetes quedó pendiente de definir cuando se aprobó el plan (2026-11-19): esas rutas existen como rutas completas, no como "más adelante", pero sus lecciones aún no están detalladas.

## Ejercicios

Cada lección mezcla estos 4 tipos:

1. **Elegir el comando correcto** — opción múltiple.
2. **Ordenar los pasos** — poner en orden los pasos de una tarea.
3. **Completar el comando** — rellenar la parte que falta.
4. **Terminal simulada** — escribir el comando real en una terminal de práctica.

Cada lección tiene 3 corazones: fallar 3 veces la reinicia. Tras cada respuesta hay una explicación.

## Gamificación

- **XP** por lección terminada.
- **Racha diaria**: días seguidos con al menos una lección.
- **Ligas semanales por XP**: Bronce a Diamante.
- **13 logros** (por ejemplo, primera lección y lección sin errores).
- **Certificado de nivel** al completar cada ruta, con el nombre configurado en Progreso.

## Login y progreso

- Cuentas con **usuario y contraseña** (más el nombre que sale en los certificados).
- La sesión queda guardada en el teléfono: no hay que entrar cada vez.
- El progreso es **por cuenta**: XP, racha, lecciones, logros y certificados son de cada usuario.
- La primera cuenta que se crea **hereda el avance hecho antes del login** (XP, lecciones y logros no se pierden).

Acciones del servidor de la app web 1.0 (verificadas): `register`, `login`, `getsession`, `getprogress`, `completelesson`, `setdisplayname`, `resetprogress`, `logout`.

## Versión 2.0 — nube (Firebase, en curso desde 2026-11-20)

Gerardo aprobó el 2026-11-20 que la app web y la APK compartan el mismo backend, para que lo hecho en una aparezca en la otra. Modelo: la app Checklist de Soporte en la nube.

- **Proyecto Firebase nuevo y separado:** `devtools-aprender` (plan Spark, gratis). No se reutiliza el proyecto de Checklist (`checklist-soporte`) ni se toca la app de gastos.
- **Autenticación:** Firebase Auth con correo y contraseña. El login sigue siendo usuario y contraseña: el usuario se resuelve a un correo sintético `usuario@devtools.local` (igual que Checklist). Al registrarse se puede dejar un correo real de recuperación (opcional) para el enlace de "Olvidé mi contraseña"; sin él, una contraseña olvidada no se puede recuperar y hay que crear otra cuenta.
- **Datos en Firestore:** colecciones `users/{uid}` (usuario, nombre, correo real), `usernames/{usuario}` (para resolver el login) y `progress/{uid}` (XP, racha, lecciones, logros). Reglas: cada usuario solo ve y escribe lo suyo.
- **Offline:** la APK usa caché persistente de Firestore: funciona sin internet y sincroniza al volver la conexión (igual que Checklist). Mientras no haya configuración real de Firebase, la APK sigue en modo local (cuentas y avance solo en el teléfono).
- **Migración de cuentas 1.0:** Firebase Auth no puede reutilizar las contraseñas guardadas en la app web 1.0 ni en la APK 1.0 (solo existen hashes, no las contraseñas). Cada usuario vuelve a registrarse en la 2.0 con el mismo usuario y contraseña; en el primer inicio de sesión en la nube, el avance local existente se sube a Firestore si aún no hay avance en la nube (si ya hay avance en la nube, manda el de la nube).
- **APK 2.0:** `versionCode` 2, mismo `appId` (`mx.gerardo.devtools`) y mismo keystore permanente que la 1.0, para que se instale encima sin perder nada. La capa de nube es `www/cloud/firebase.js`, compilada con esbuild a `www/cloud/firebase.bundle.js` (si cambia `firebase.js` hay que reconstruir el bundle y correr `npx cap sync` antes de compilar, como en Checklist). La 2.0 se compiló el 2026-11-20 (3,648,739 bytes, md5 `86c0bd944a0c6c25aad854707c354855`, firma v2 verificada con el mismo SHA-256) con la configuración real del proyecto `devtools-aprender` ya pegada en `www/firebase-config.js`.

## Modelo de datos (progreso por usuario)

Por cada cuenta se guarda: XP total, racha diaria, lecciones completadas (por ruta y nivel), logros desbloqueados (de 13) y nombre para certificados. No hay datos personales más allá del usuario, la contraseña y el nombre para el certificado.

## Estado actual (2026-11-20)

- Plan aprobado con las 9 rutas (Git, GitHub, Slack, Terminal, VS Code, Docker, Jira, CI/CD y Kubernetes).
- App web construida y funcionando: 9 rutas, 4 tipos de ejercicio, 13 logros, ligas semanales.
- Corrección de legibilidad (2026-11-19): al seleccionar una respuesta el texto se perdía por el color; ahora usa un color oscuro fijo sobre el fondo claro, legible en modo claro y oscuro. Gerardo lo confirmó: "Perfecto se ve bien".
- Login agregado (2026-11-19).
- APK 1.0 entregada (2026-11-19): paquete `mx.gerardo.devtools`, `versionCode` 1, firmada con keystore permanente (SHA-256 `bd1d91d66efb95f14cd340d2be8a76b1fcec89dfc8e116386f0cd8ccb5fd7aa1`), md5 `417ceded2d75a74095756d9d5708f7a0`. Verificada estructuralmente; sin probar aún en un teléfono real. El avance NO se sincroniza entre la web y la APK 1.0.
- Versión 2.0 en la nube (desde 2026-11-20): código de la APK listo y verificado en modo local (15/15 pruebas: registro, lección completa de Git, +15 XP, logros, perfil, re-login). Proyecto Firebase `devtools-aprender` creado (consola https://console.firebase.google.com/project/devtools-aprender/overview), Auth por correo/contraseña activado y reglas de Firestore publicadas (por consola), configuración real pegada en `www/firebase-config.js` y APK 2.0 compilada y verificada (md5 `86c0bd944a0c6c25aad854707c354855`, se instala sobre la 1.0). La app web ya usa el mismo backend Firebase (migración terminada el 2026-11-20, auditoría aprobada: mismo login, progreso en `progress/{uid}`, suscripción en vivo, perfil "Versión 2.0 · Nube"). Pendiente: confirmar la sincronización web ↔ APK con una cuenta real en un navegador o teléfono (registrar, completar una lección, salir y volver a entrar).
- Documentación en este repositorio (pusheada a GitHub el 2026-11-20, commit `52a4ca3b`; la actualización con la 2.0 y las rutas nuevas aún no se sube).
- Dos rutas nuevas (2026-11-20): **SQL** (MySQL/MariaDB) y **Kali Linux**, pedidas por Gerardo el mismo día. Cada una con 7 lecciones (3 Principiante, 2 Intermedio, 2 Avanzado) y 35 ejercicios de los 4 tipos. El contenido está en la APK (`www/data/sql.js` y `www/data/kali.js`, cargados en `www/index.html`) y en la app web con los mismos IDs y textos, verificados palabra por palabra. APK 2.1 compilada y verificada el 2026-11-20 (`versionCode` 3, 3,657,538 bytes, md5 `dde17545926b252c774260f41771630d`, mismo keystore, se instala sobre la 2.0). La app web ya muestra las 11 rutas y su perfil dice "Versión 2.1 · Nube".

## Versión 2.1 — dos rutas nuevas (2026-11-20)

Gerardo pide agregar SQL (MySQL/MariaDB) y herramientas de Kali Linux. Se agregan como rutas 10 y 11, al final de la lista, con la misma estructura que las demás (7 lecciones, 35 ejercicios, 4 tipos). El contenido es idéntico en la web y en la APK (mismos IDs de ruta y lección), para que el progreso sincronice por Firebase.

- **SQL** (`sql`): SELECT, WHERE/ORDER BY/LIMIT, INSERT/UPDATE/DELETE, JOINs, GROUP BY/HAVING y agregados, índices/usuarios/respaldos con mysqldump y las diferencias MySQL vs MariaDB.
- **Kali Linux** (`kali`): qué es Kali, terminal y navegación, permisos, nmap, netcat y Wireshark, Metasploit, Hydra y John the Ripper. Enfoque educativo: solo se practica en equipos y redes propias o de laboratorio, con permiso.
- **Prompt propio en la terminal simulada:** los ejercicios de terminal ahora pueden traer su propio prompt: `mysql>` en las consultas SQL y `kali@kali:~$` en Kali (antes siempre era `usuario@equipo:~$`).
- **APK 2.1:** `versionCode` 3, `versionName` 2.1, mismo `appId` y keystore permanente que la 2.0 (SHA-256 `bd1d91d66efb95f14cd340d2be8a76b1fcec89dfc8e116386f0cd8ccb5fd7aa1`, firma v2 verificada). Compilada el 2026-11-20 (3,657,538 bytes, md5 `dde17545926b252c774260f41771630d`); el `index.html` y los datos empaquetados se verificaron por md5 contra la fuente. Respaldo de la 2.0 en `~/workspace/tmp/aprender-herramientas-de-desarrollador-2.0.apk.bak` (md5 `86c0bd944a0c6c25aad854707c354855`). Verificación en modo local: 18/18 pruebas jsdom (11 tarjetas de ruta, lección 0 de SQL completa con +15 XP y 3 estrellas, perfil "Versión 2.1 · Local", re-login con XP persistente, 7 nodos en Kali) más la comprobación del prompt `mysql>`. La VM se reinició antes del build y vació `/root/.gradle`, así que hubo que re-descargar Gradle 8.14.3 con curl y re-importar la CA del proxy (121 certs).
- **App web:** las dos rutas se agregaron con el contenido exacto de la APK (verificado palabra por palabra contra `www/data/sql.js` y `www/data/kali.js`), con el mismo soporte de prompt y el perfil en "Versión 2.1 · Nube". Login, Firebase, logros, ligas y el progreso existente no se tocaron.

## Este repositorio

Este repo contiene **solo la documentación** de la app (`docs/`). El código de la app vive en el artefacto web de Muse y el de la APK en `~/workspace/android/devtools-app/` (no hay código en este repo, así que no hay nada que compilar desde aquí). El repo lo creó Gerardo el 2026-11-19 y los 3 documentos se subieron el 2026-11-20 (commit `52a4ca3b`).

## Pendiente / por definir

- Detalle del stack interno del artefacto web (pendiente de documentar; no inventado aquí).
- Contenido de las lecciones de Docker, Jira, CI/CD y Kubernetes por nivel.
- Verificar la sincronización web ↔ APK con una cuenta real en un navegador o teléfono (la app web ya usa Firebase; falta el ciclo completo: registrar, completar una lección, salir y volver a entrar).
- Subir estos docs actualizados a GitHub (incluyen la 2.0, la 2.1 y las rutas SQL y Kali Linux; la aprobación del push, pedida el 2026-11-20 12:18 UTC, sigue pendiente).
- Entrega y pruebas de la APK 2.1 en un teléfono real (también siguen pendientes las de 1.0 y 2.0).
- Cómo se agregan o editan rutas y lecciones (pendiente de definir).
