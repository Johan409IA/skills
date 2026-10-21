# Skills

Colección personal de [agent skills](https://skills.sh) para agentes de código (Zed, Claude Code, Codex, Cursor, etc.), instalables y actualizables con la CLI `skills`.

## Skills disponibles

| Skill                                                                          | Descripción                                                                                                                                                                                                                                                              |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [`design-reference-implementer`](skills/design-reference-implementer/SKILL.md) | Rediseña una interfaz o componente completo del frontend a partir de una referencia visual (captura, prototipo, imagen, logo o iconos): spec previo aprobado por el usuario, implementación con los componentes y tokens del proyecto y verificación con Playwright CLI. |
| [`project-docs`](skills/project-docs/SKILL.md)                                 | Crea y mantiene la documentación de un proyecto: `README.md`, `AGENTS.md`, `MEMORY.md`, `CHANGELOG.md`, `DESIGN.md`, `docs/architecture.md`, `docs/plans/` y docs de API (OpenAPI + Scalar).                                                                             |

## Instalación

Requiere Node.js. Reemplaza `TU-USUARIO` por tu usuario de GitHub.

```sh
# Instalar todas las skills del repositorio
npx skills add Johan409IA/skills

# Instalar solo una skill
npx skills add Johan409IA/skills --skill project-docs
```

## Gestión

```sh
# Listar las skills instaladas
npx skills list

# Buscar actualizaciones
npx skills check

# Aplicar actualizaciones
npx skills update
```

Las opciones pueden variar según la versión de la CLI. Consulta `npx skills --help`.

## Estructura del repositorio

```
.
├── LICENSE
├── README.md
└── skills/
    └── <nombre-de-la-skill>/
        ├── SKILL.md          # obligatorio: frontmatter (name, description) + instrucciones
        └── references/       # opcional: plantillas y material de apoyo
```

## Añadir una nueva skill

1. Crea la carpeta `skills/<nombre-de-la-skill>/`. El nombre debe coincidir con el campo `name` del frontmatter.
2. Crea `SKILL.md` con este encabezado:

   ```markdown
   ---
   name: nombre-de-la-skill
   description: Qué hace y cuándo debe usarse (máx. 1024 caracteres).
   ---
   ```

3. Mueve los detalles largos (plantillas, ejemplos) a `references/` y enlázalos desde `SKILL.md`.
4. Añade la skill a la tabla de [Skills disponibles](#skills-disponibles).
5. Haz commit y push. Después, `npx skills update` aplicará los cambios en cada máquina.

## Licencia

Consulta el archivo [LICENSE](LICENSE).
