# Aprender Herramientas de Desarrollador

App estilo Duolingo para aprender herramientas de desarrollo en español: lecciones cortas, ejercicios, puntos, racha y ligas. Primero se aprende jugando, con una terminal simulada para practicar comandos de verdad.

## Qué es

Una app de aprendizaje con 11 rutas, una por herramienta. Cada ruta tiene 4 niveles (Básico, Intermedio, Avanzado y Experto) con 15 lecciones cada uno: 60 lecciones por ruta, 660 en total. Las lecciones son de unos 5 minutos en un camino que se desbloquea paso a paso. Al completar una ruta se obtiene un certificado de nivel.

La app vive como artefacto web fullstack de Muse (slug: `aprender-herramientas-de-desarrollador`). El APK para Android es una reconstrucción local aparte con el mismo contenido (`~/workspace/android/devtools-app/`: un solo `www/index.html` + `www/data/*.js`, localStorage y login local por cuenta) — el artefacto web no tiene URL pública, así que no se puede envolver en un WebView. Desde la versión 2.0 la app web y la APK comparten el mismo backend en Firebase, así que el avance se sincroniza entre las dos.

## Rutas y niveles

| Ruta | Básico | Intermedio | Avanzado | Experto |
|------|--------|------------|----------|---------|
| Git | config, .gitignore, diff, ramas, clone, tags | fetch/pull, merge vs rebase, log, blame, cherry-pick | rebase interactivo, bisect, hooks, LFS, submódulos | GitFlow/trunk-based, rescate con reflog/fsck, GitOps, monorepo |
| GitHub | repos, README, issues, PR, fork | revisión de código, plantillas, Actions, ramas protegidas | workflows YAML, secrets, API, webhooks, code scanning | Enterprise, SAML/SSO, runners, seguridad avanzada, DORA |
| Slack | canales, mensajes, hilos, búsqueda | workflows, Canvas, recordatorios, Slack Connect | API, Block Kit, OAuth, Enterprise Grid, DLP | bots a escala, gobierno, auditoría, incidentes |
| Terminal | navegar, archivos, redirección, pipes, grep | permisos, variables, ssh, procesos, crontab | sed, awk, scripting bash, tmux, rsync | scripting avanzado, paralelismo, strace, rescate del sistema |
| VS Code | paleta, atajos, extensiones, terminal | depuración, Git, tareas, Emmet, Live Share | depuración avanzada, Dev Containers, Remote-SSH | crear extensiones, LSP, rendimiento, setup empresarial |
| Docker | contenedores, imágenes, run, volúmenes, puertos | Compose, redes, multi-etapa, healthcheck | Compose avanzado, Swarm, BuildKit, seguridad | producción HA, CIS, registros empresariales, GitOps |
| Jira | proyectos, issues, tableros, sprints | JQL, Scrum/Kanban, automatización, dashboards | JQL avanzado, flujos, Advanced Roadmaps, API | administración a escala, gobierno, portafolio, DORA |
| CI/CD | pipelines, stages, jobs, YAML | tests, matrices, staging, rollbacks, secrets | blue/green, canary, IaC, SAST/DAST | GitOps, SLSA/SBOM, SLOs, platform engineering |
| Kubernetes | pods, kubectl, deployments, services | ReplicaSets, Ingress, PVC, HPA, probes | Helm, RBAC, NetworkPolicies, GitOps | producción a escala, service mesh, multi-tenancy, costos |
| SQL (MySQL/MariaDB) | tipos de datos, CREATE TABLE, SELECT, WHERE | subconsultas, UNION, CTEs, transacciones, vistas | optimización, particionado, replicación, bloqueos | diseño a escala, sharding, Galera, ProxySQL, tuning InnoDB |
| Kali Linux | instalar, apt, red, bash, whois/dig | nmap avanzado, enumeración, sqlmap, Metasploit | explotación, post-explotación, AD, kerberoasting | pentest completo, red team, evasión de EDR, reportes profesionales |

Cada lección tiene 5 ejercicios de los 4 tipos. Las lecciones de Experto son avanzadas de verdad: escenarios reales, buenas prácticas, solución de problemas y automatización.

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

Desde la versión 2.2 cada lección tiene un id estable (`<ruta>-<nivel><nn>`, por ejemplo `git-b01`, `sql-e15`) y el avance se guarda por id, no por posición. Al cargar, el avance guardado en 2.0/2.1 (claves numéricas 0-6) se migra solo a su id (0→b01, 1→b02, 2→b03, 3→i01, 4→i02, 5→a01, 6→a02), así el avance no se pierde ni se marca en la lección equivocada cuando crecen los niveles. El nivel 1 se llama "Básico" desde la 2.2 (antes "Principiante").

## Estado actual (2026-11-20)

- Plan aprobado con las 9 rutas (Git, GitHub, Slack, Terminal, VS Code, Docker, Jira, CI/CD y Kubernetes).
- App web construida y funcionando: 9 rutas, 4 tipos de ejercicio, 13 logros, ligas semanales.
- Corrección de legibilidad (2026-11-19): al seleccionar una respuesta el texto se perdía por el color; ahora usa un color oscuro fijo sobre el fondo claro, legible en modo claro y oscuro. Gerardo lo confirmó: "Perfecto se ve bien".
- Login agregado (2026-11-19).
- APK 1.0 entregada (2026-11-19): paquete `mx.gerardo.devtools`, `versionCode` 1, firmada con keystore permanente (SHA-256 `bd1d91d66efb95f14cd340d2be8a76b1fcec89dfc8e116386f0cd8ccb5fd7aa1`), md5 `417ceded2d75a74095756d9d5708f7a0`. Verificada estructuralmente; sin probar aún en un teléfono real. El avance NO se sincroniza entre la web y la APK 1.0.
- Versión 2.0 en la nube (desde 2026-11-20): código de la APK listo y verificado en modo local (15/15 pruebas: registro, lección completa de Git, +15 XP, logros, perfil, re-login). Proyecto Firebase `devtools-aprender` creado (consola https://console.firebase.google.com/project/devtools-aprender/overview), Auth por correo/contraseña activado y reglas de Firestore publicadas (por consola), configuración real pegada en `www/firebase-config.js` y APK 2.0 compilada y verificada (md5 `86c0bd944a0c6c25aad854707c354855`, se instala sobre la 1.0). La app web ya usa el mismo backend Firebase (migración terminada el 2026-11-20, auditoría aprobada: mismo login, progreso en `progress/{uid}`, suscripción en vivo, perfil "Versión 2.0 · Nube"). Pendiente: confirmar la sincronización web ↔ APK con una cuenta real en un navegador o teléfono (registrar, completar una lección, salir y volver a entrar).
- Documentación en este repositorio (pusheada a GitHub el 2026-11-20, commit `a8afd41b`, que cubre la 2.0 y la 2.1; la actualización con la 2.2 aún no se sube, se pide aparte con la aprobación de Gerardo).
- Dos rutas nuevas (2026-11-20): **SQL** (MySQL/MariaDB) y **Kali Linux**, pedidas por Gerardo el mismo día. Cada una con 7 lecciones (3 Principiante, 2 Intermedio, 2 Avanzado) y 35 ejercicios de los 4 tipos. El contenido está en la APK (`www/data/sql.js` y `www/data/kali.js`, cargados en `www/index.html`) y en la app web con los mismos IDs y textos, verificados palabra por palabra. APK 2.1 compilada y verificada el 2026-11-20 (`versionCode` 3, 3,657,538 bytes, md5 `dde17545926b252c774260f41771630d`, mismo keystore, se instala sobre la 2.0). La app web ya muestra las 11 rutas y su perfil dice "Versión 2.1 · Nube".
- Versión 2.2 (2026-11-20, mismo día): Gerardo probó la 2.1 en su teléfono y pidió dos cosas: que el nombre de cada herramienta se lea bien (estaba en negro y se perdía con el fondo) y más lecciones por nivel. Cada ruta pasa de 7 a 60 lecciones (15 por nivel, 4 niveles: Básico, Intermedio, Avanzado y Experto), 660 lecciones y 3,300 ejercicios en total. APK 2.2 compilada y verificada el mismo día (`versionCode` 4, 3,958,554 bytes, md5 `95e2d78710f07df4ec8a5515100ed850`, mismo keystore, se instala sobre la 2.1); verificación en modo local: 32/32 pruebas jsdom (11 tarjetas, 60 nodos en Git y SQL, lección Experto git-e15 completa con +15 XP y 3 estrellas, migración del avance viejo, perfil "Versión 2.2 · Local"). La app web muestra las 11 rutas × 60 lecciones y su perfil dice "Versión 2.2 · Nube".

## Versión 2.1 — dos rutas nuevas (2026-11-20)

Gerardo pide agregar SQL (MySQL/MariaDB) y herramientas de Kali Linux. Se agregan como rutas 10 y 11, al final de la lista, con la misma estructura que las demás (7 lecciones, 35 ejercicios, 4 tipos). El contenido es idéntico en la web y en la APK (mismos IDs de ruta y lección), para que el progreso sincronice por Firebase.

- **SQL** (`sql`): SELECT, WHERE/ORDER BY/LIMIT, INSERT/UPDATE/DELETE, JOINs, GROUP BY/HAVING y agregados, índices/usuarios/respaldos con mysqldump y las diferencias MySQL vs MariaDB.
- **Kali Linux** (`kali`): qué es Kali, terminal y navegación, permisos, nmap, netcat y Wireshark, Metasploit, Hydra y John the Ripper. Enfoque educativo: solo se practica en equipos y redes propias o de laboratorio, con permiso.
- **Prompt propio en la terminal simulada:** los ejercicios de terminal ahora pueden traer su propio prompt: `mysql>` en las consultas SQL y `kali@kali:~$` en Kali (antes siempre era `usuario@equipo:~$`).
- **APK 2.1:** `versionCode` 3, `versionName` 2.1, mismo `appId` y keystore permanente que la 2.0 (SHA-256 `bd1d91d66efb95f14cd340d2be8a76b1fcec89dfc8e116386f0cd8ccb5fd7aa1`, firma v2 verificada). Compilada el 2026-11-20 (3,657,538 bytes, md5 `dde17545926b252c774260f41771630d`); el `index.html` y los datos empaquetados se verificaron por md5 contra la fuente. Respaldo de la 2.0 en `~/workspace/tmp/aprender-herramientas-de-desarrollador-2.0.apk.bak` (md5 `86c0bd944a0c6c25aad854707c354855`). Verificación en modo local: 18/18 pruebas jsdom (11 tarjetas de ruta, lección 0 de SQL completa con +15 XP y 3 estrellas, perfil "Versión 2.1 · Local", re-login con XP persistente, 7 nodos en Kali) más la comprobación del prompt `mysql>`. La VM se reinició antes del build y vació `/root/.gradle`, así que hubo que re-descargar Gradle 8.14.3 con curl y re-importar la CA del proxy (121 certs).
- **App web:** las dos rutas se agregaron con el contenido exacto de la APK (verificado palabra por palabra contra `www/data/sql.js` y `www/data/kali.js`), con el mismo soporte de prompt y el perfil en "Versión 2.1 · Nube". Login, Firebase, logros, ligas y el progreso existente no se tocaron.

## Versión 2.2 — 60 lecciones por ruta y arreglo del color (2026-11-20)

Gerardo probó la APK 2.1 en su teléfono y pidió dos cambios, aplicados el mismo día en la web y en la APK:

- **Color del nombre de la ruta:** en modo oscuro el nombre se veía en negro y se perdía con el fondo. Causa: la tarjeta de ruta es un `<button>` y su color de texto lo pone el navegador (negro en páginas con `color-scheme` claro) mientras el fondo oscuro viene del media query. Arreglo: `color:var(--ink)` explícito en `.route-card` y `.route-info b`, más `<meta name="color-scheme" content="light dark">` y `color-scheme:light dark` en `:root`. El nombre ahora se lee en modo claro y oscuro.
- **Más lecciones, 4 niveles:** cada ruta pasa de 7 lecciones (3 Principiante, 2 Intermedio, 2 Avanzado) a 60 (15 Básico, 15 Intermedio, 15 Avanzado, 15 Experto). Por ruta se agregan 53 lecciones nuevas (Básico +12, Intermedio +13, Avanzado +13, Experto +15); las 7 existentes no se tocan. En total: 660 lecciones y 3,300 ejercicios. Las de Experto son avanzadas de verdad (escenarios reales, buenas prácticas, solución de problemas, automatización). El nivel 1 pasa a llamarse "Básico" (antes "Principiante").
- **IDs estables y migración del avance:** cada lección tiene id (`git-b01`, `sql-e15`...) y el avance se guarda por id. El avance de 2.0/2.1 (claves 0-6) se migra solo al cargar, así no se pierde ni se marca en otra lección. El logro "Maestro dev" ahora dice "Completa todas las rutas" (ya no son 9).
- **APK 2.2:** `versionCode` 4, `versionName` 2.2, mismo `appId` y keystore permanente que la 2.1 (SHA-256 `bd1d91d66efb95f14cd340d2be8a76b1fcec89dfc8e116386f0cd8ccb5fd7aa1`, firma v2 verificada). Compilada el 2026-11-20 (3,958,554 bytes, md5 `95e2d78710f07df4ec8a5515100ed850`); el `index.html` y los datos empaquetados (las 11 rutas, `firebase-config.js` real) se verificaron por md5 contra la fuente. Se instala sobre la 2.1; respaldo de la 2.1 en `~/workspace/tmp/aprender-herramientas-de-desarrollador-2.1.apk.bak` (md5 `dde17545926b252c774260f41771630d`). Verificación en modo local: 32/32 pruebas jsdom (11 tarjetas, 60 nodos en Git y SQL, niveles Básico/Intermedio/Avanzado/Experto, lección Experto git-e15 completa con +15 XP y 3 estrellas, migración del avance viejo con claves 0 y 5, perfil "Versión 2.2 · Local", re-login con XP persistente) y validación estructural de los datos: 17,220 comprobaciones, 660 lecciones, 3,300 ejercicios, 0 fallos. La VM se reinició antes del build y vació `/root/.gradle`: hubo que re-descargar Gradle 8.14.3 con curl y re-importar la CA del proxy (121 certs). El build tardó 4m 39s.
- **App web:** mismo contenido que la APK (copiado textual de `www/data/*.js`, mismos IDs), mismo arreglo de color, misma migración del avance y perfil "Versión 2.2 · Nube". Login, Firebase, logros, ligas y el progreso existente no se tocan salvo la migración.
- **Nota de formato heredado:** las 7 lecciones originales de 2.1 de algunas rutas (CI/CD, Terminal, Kubernetes, Jira, VS Code, Docker) usan otro orden de ejercicios y sus `complete` no tienen pregunta o tienen 4 opciones; el reproductor las maneja igual que antes y no se tocaron por decisión expresa. Las 583 lecciones nuevas siguen el formato estándar (`mc, mc, complete, terminal, order`, `complete` con 3 opciones).

## Este repositorio

Este repo contiene **solo la documentación** de la app (`docs/`). El código de la app vive en el artefacto web de Muse y el de la APK en `~/workspace/android/devtools-app/` (no hay código en este repo, así que no hay nada que compilar desde aquí). El repo lo creó Gerardo el 2026-11-19 y los 3 documentos se subieron el 2026-11-20 (commit `52a4ca3b`).

## Pendiente / por definir

- Detalle del stack interno del artefacto web (pendiente de documentar; no inventado aquí).
- Verificar la sincronización web ↔ APK con una cuenta real en un navegador o teléfono (la app web ya usa Firebase; falta el ciclo completo: registrar, completar una lección, salir y volver a entrar).
- Subir estos docs actualizados a GitHub (incluyen la 2.2; se pide aparte con la aprobación de Gerardo, no se sube solo).
- Entrega y pruebas de la APK 2.2 en un teléfono real: que los nombres de las rutas ya se lean bien (modo claro y oscuro), completar una lección de Experto y que el avance viejo siga en su lección (también siguen pendientes las pruebas de 1.0 y 2.0).
- Cómo se agregan o editan rutas y lecciones (pendiente de definir).
