# AGENTS.md

Contexto del proyecto para los agentes de IA. Léalo antes de cualquier cambio.

## Proyecto

Plataforma web lúdica de cursos para niños. Curso: Ingles. Mecánica: Niveles.
Roles: docente y niño. El niño entra con código de clase y avatar, sin correo.

## Stack

- Python 3.11+, FastAPI, Jinja2, SQLModel, pytest, Ruff
- GitHub Actions para CI y Render para despliegue

## Comandos

- Instalar: `pip install -r requirements-dev.txt`
- Ejecutar: `uvicorn app.main:app --reload`
- Pruebas: `pytest -v`
- Estilo: `ruff check .`

## Estructura

- `app/rutas/`: reciben la petición y llaman servicios; sin regla de negocio
- `app/servicios/`: regla de negocio con pruebas propias
- `app/modelos/`: tablas SQLModel
- `app/templates/` y `app/static/`: interfaz y PWA
- `tests/`: pruebas del proyecto

## Convenciones

- Nombres en español, cambios pequeños y prueba antes del código.
- Al iniciar cada sesión, leer STATE.md.

## Qué no hacer

- No escribir claves ni contraseñas en el código: usar variables de entorno.
- No instalar dependencias sin verificar que existen en pypi.org.
- No modificar ni borrar pruebas para que pasen.
- No usar datos personales reales.
- No hacer commit ni push: los hace el desarrollador.

## Lecciones aprendidas

- Cada vez que el agente se equivoque, escribir aquí la regla.