# Aprender Herramientas de Desarrollador

App estilo Duolingo para aprender herramientas de desarrollo en español: lecciones cortas, ejercicios, puntos, racha y ligas. Primero se aprende jugando, con una terminal simulada para practicar comandos de verdad.

## Qué es

Una app de aprendizaje con 9 rutas, una por herramienta. Cada ruta tiene niveles (Principiante, Intermedio y Avanzado), unidades con lecciones de unos 5 minutos en un camino que se desbloquea paso a paso. Al completar una ruta se obtiene un certificado de nivel.

La app vive como artefacto web fullstack de Muse (slug: `aprender-herramientas-de-desarrollador`). El APK para Android está en construcción (envoltorio Capacitor de la app web).

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

Acciones del servidor (verificadas): `register`, `login`, `getsession`, `getprogress`, `completelesson`, `setdisplayname`, `resetprogress`, `logout`.

## Modelo de datos (progreso por usuario)

Por cada cuenta se guarda: XP total, racha diaria, lecciones completadas (por ruta y nivel), logros desbloqueados (de 13) y nombre para certificados. No hay datos personales más allá del usuario, la contraseña y el nombre para el certificado.

## Estado actual (2026-11-19)

- Plan aprobado con las 9 rutas (Git, GitHub, Slack, Terminal, VS Code, Docker, Jira, CI/CD y Kubernetes).
- App web construida y funcionando: 9 rutas, 4 tipos de ejercicio, 13 logros, ligas semanales.
- Corrección de legibilidad (2026-11-19): al seleccionar una respuesta el texto se perdía por el color; ahora usa un color oscuro fijo sobre el fondo claro, legible en modo claro y oscuro. Gerardo lo confirmó: "Perfecto se ve bien".
- Login agregado (2026-11-19).
- APK en construcción (otro agente lo está armando en paralelo).
- Documentación en este repositorio.

## Este repositorio

Este repo contiene **solo la documentación** de la app (`docs/`). El código de la app vive en el artefacto web de Muse, no hay código local que subir, así que no hay nada que compilar desde aquí. El APK se construye por separado (ver CHANGELOG.md).

## Pendiente / por definir

- Detalle del stack interno del artefacto web (pendiente de documentar; no inventado aquí).
- Contenido de las lecciones de Docker, Jira, CI/CD y Kubernetes por nivel.
- Entrega y pruebas del APK en un teléfono real.
- Cómo se agregan o editan rutas y lecciones (pendiente de definir).
