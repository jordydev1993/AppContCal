# AppContCal — App de conteo de calorías

PWA personal para registrar comidas, ejercicio, agua y peso, con carga asistida por IA.
Metodología: **Spec Driven Development + Vibe Engineering** → las specs viven en [`specs/`](specs/) y guían la implementación.

| Spec | Estado |
|---|---|
| [00-relevamiento.md](specs/00-relevamiento.md) | Cerrado |
| [01-requirements.md](specs/01-requirements.md) | Borrador en revisión |
| 02-design.md | Pendiente |
| 03-tasks.md | Pendiente |

## Clonar el proyecto en otra PC

1. Instalá [Git](https://git-scm.com/downloads) si no lo tenés (comprobalo con `git --version`).
2. Abrí una terminal en la carpeta donde quieras el proyecto y ejecutá:
   ```bash
   git clone https://github.com/jordydev1993/AppContCal.git
   cd AppContCal
   ```
3. Configurá tu identidad (una sola vez por PC), si no lo hiciste antes:
   ```bash
   git config --global user.name "jordydev1993"
   git config --global user.email "tu-email@ejemplo.com"
   ```
4. Para trabajar: antes de empezar, traé los últimos cambios; al terminar, subí los tuyos.
   ```bash
   git pull                   # traer cambios
   git add .
   git commit -m "mensaje"
   git push                   # subir cambios
   ```
   Para hacer `push` tenés que estar autenticado en GitHub. La forma más simple es instalar
   [GitHub CLI](https://cli.github.com/) y ejecutar `gh auth login`.

> Cuando exista el código de la app, en esta sección se van a agregar los pasos de instalación
> (Node.js, `npm install`, variables de entorno `.env.local`).
