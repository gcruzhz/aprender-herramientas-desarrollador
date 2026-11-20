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

## 2026-11-19 — Documentación y GitHub (en curso)

Se crea la documentación en `docs/` (README.md, PREGUNTAS-DEVS.md y este CHANGELOG.md). El repositorio de GitHub `gcruzhz/aprender-herramientas-desarrollador` aún no existe: lo crea Gerardo (la Muse GitHub App no puede crear repositorios). El repo contendrá solo la documentación; el código de la app vive en el artefacto web.

## Pendiente

- APK de Android (en construcción por otro agente, en paralelo).
- Contenido detallado de las lecciones de Docker, Jira, CI/CD y Kubernetes por nivel.
- Detalle del stack interno del artefacto web.
- Pruebas de Gerardo en teléfono real (APK y login con su cuenta).
