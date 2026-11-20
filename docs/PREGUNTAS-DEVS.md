# Preguntas de desarrolladores — Aprender Herramientas de Desarrollador

Documento para que un desarrollador entienda la app rápido: qué es, cómo está hecha a grandes rasgos y qué se le puede preguntar. Solo contiene lo verificado; lo que aún no se sabe está marcado como pendiente.

## ¿Qué es la app?

Una app estilo Duolingo, en español, para aprender herramientas de desarrollo jugando: Git, GitHub, Slack, Terminal, VS Code, Docker, Jira, CI/CD y Kubernetes. Lecciones de unos 5 minutos, 4 tipos de ejercicio (elegir el comando, ordenar pasos, completar el comando y terminal simulada), 3 corazones por lección, XP, racha diaria, ligas semanales (Bronce a Diamante), 13 logros y certificado al completar cada ruta.

## ¿Dónde vive la app?

Como artefacto web fullstack de Muse (slug: `aprender-herramientas-de-desarrollador`). El código no está en este repositorio: este repo solo tiene documentación (`docs/`). El APK de Android está en construcción como envoltorio Capacitor de la app web.

## ¿En qué lenguaje / con qué stack está hecha?

Pendiente de documentar con detalle. Lo que sí está verificado: es una app web con servidor (acciones `register`, `login`, `getsession`, `getprogress`, `completelesson`, `setdisplayname`, `resetprogress`, `logout`) y guarda el progreso por cuenta de usuario. No se afirma aquí un lenguaje o framework concreto hasta confirmarlo.

## ¿Cómo funciona el login?

Cuentas con usuario y contraseña (más un nombre para los certificados). La sesión es persistente: queda guardada en el teléfono. El progreso es por cuenta. La primera cuenta creada hereda el avance hecho antes de que existiera el login (XP, lecciones y logros previos no se pierden).

## ¿Cómo se guarda el progreso?

Por usuario: XP total, racha diaria, lecciones completadas por ruta y nivel, logros desbloqueados (de 13) y nombre para certificados. Las lecciones se desbloquean paso a paso en un camino por ruta.

## ¿Qué rutas y niveles hay?

9 rutas: Git, GitHub, Slack, Terminal, VS Code, Docker, Jira, CI/CD y Kubernetes. Cada una con niveles Principiante, Intermedio y Avanzado. El contenido por nivel está detallado para Git, GitHub, Slack, Terminal y VS Code; el de Docker, Jira, CI/CD y Kubernetes está pendiente de definir (ver `docs/README.md`).

## ¿Hay APK?

Está en construcción (envoltorio Capacitor de la app web). Pendiente de entrega y de pruebas en un teléfono real. La app web necesita internet porque el login y el progreso viven en el servidor.

## ¿Qué se le puede preguntar a un dev que revise la app?

- Cómo agregar una ruta o una lección nueva (contenido y niveles).
- Cómo funciona la terminal simulada (qué comandos valida y cómo).
- Cómo se calculan XP, racha y ligas, y dónde vive esa lógica (servidor).
- Cómo se hace el despliegue del APK y qué permisos necesita (internet).
- Cómo se respalda o se migra el progreso de los usuarios.

## Enlaces

- App web: artefacto `aprender-herramientas-de-desarrollador` en Muse.
- Repo (documentación): https://github.com/gcruzhz/aprender-herramientas-desarrollador (pendiente de creación por Gerardo; ver nota en el README del repo).
