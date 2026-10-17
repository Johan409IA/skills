# AGENTS.md

Este repositorio almacena skills personales, instalables con `npx skills add`. Cada skill vive en `skills/<nombre>/SKILL.md`, con `references/` opcional. El nombre de la carpeta debe coincidir con `name` del frontmatter.

## Revisión de skills (skill-judge)

Cuando el usuario pida revisar, auditar, evaluar o mejorar una skill, o diga "¿está bien esta skill?", evalúala con los criterios de abajo. Lee el `SKILL.md` completo y todos los archivos de `references/` antes de puntuar.

### Principio base

Una skill no es un tutorial, es conocimiento experto que el modelo no tiene.
**Skill buena = conocimiento experto − lo que el modelo ya sabe.**

Clasifica cada sección:

- **[E] Experto**: el modelo no lo sabe (decisiones no obvias, trade-offs, casos límite, anti-patrones). Es el valor de la skill.
- **[A] Activación**: el modelo lo sabe, pero un recordatorio breve ayuda. Aceptable si es corto.
- **[R] Redundante**: el modelo seguro lo sabe ("qué es X", tutoriales, buenas prácticas genéricas). Debe borrarse.

Una skill sana tiene más de 70% [E], menos de 20% [A] y menos de 10% [R].

### Dimensiones (120 puntos)

| Dimensión                      | Máx. | Qué se evalúa                                                                                                                                                                   |
| ------------------------------ | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| D1 Conocimiento nuevo          | 20   | Cada párrafo aporta algo que el modelo no sabe. Máximo 5 si hay tutoriales o explicaciones de conceptos básicos.                                                                |
| D2 Mentalidad y procedimientos | 15   | Enseña cómo pensar ("Antes de X, pregúntate…") y procedimientos de dominio que el modelo desconoce. Los pasos genéricos no puntúan.                                             |
| D3 Anti-patrones               | 15   | Lista de NEVER específica y con el porqué. Las advertencias vagas ("ten cuidado") puntúan 4-7 como mucho.                                                                       |
| D4 Cumplimiento de la spec     | 15   | Frontmatter válido, `name` en minúsculas con guiones y ≤64 caracteres. `description` con QUÉ hace, CUÁNDO usarla y palabras clave.                                              |
| D5 Divulgación progresiva      | 15   | `SKILL.md` <500 líneas (ideal <300). Lo pesado va en `references/`, con disparadores de carga dentro del flujo ("OBLIGATORIO leer X antes de…") y "NO cargar Y" cuando aplique. |
| D6 Libertad calibrada          | 15   | Tareas creativas → libertad alta. Operaciones frágiles → pasos exactos. Pregunta: si el agente se equivoca, ¿qué consecuencia hay?                                              |
| D7 Patrón reconocible          | 10   | Sigue un patrón coherente: Mindset (~50 líneas), Navigation (~30), Philosophy (~150), Process (~200) o Tool (~300).                                                             |
| D8 Usabilidad práctica         | 15   | Árboles de decisión, ejemplos que funcionan, manejo de errores, alternativas y casos límite.                                                                                    |

### Reglas de evaluación (NEVER)

- NUNCA des nota alta porque se vea profesional o esté bien formateada.
- NUNCA te impresione la longitud: 43 líneas pueden superar a 500.
- NUNCA perdones explicar lo básico "porque da contexto". Cada párrafo redundante resta puntos.
- NUNCA ignores la `description`. El agente solo ve la descripción antes de cargar la skill, así que una mala descripción hace que no se use nunca.
- NUNCA aceptes el "cuándo usar" solo en el cuerpo. Debe estar en la `description`.
- NUNCA des por buenos los pasos genéricos. Distingue procedimientos de dominio de pasos obvios.
- NUNCA des por buenos los árboles de decisión sin probarlos mentalmente.
- Cada puntuación necesita evidencia: cita las líneas concretas.

### Errores frecuentes a detectar

1. **Tutorial**: explica lo que el modelo ya sabe. Solución: borrarlo y dejar solo decisiones, trade-offs y anti-patrones.
2. **Volcado**: `SKILL.md` de 800+ líneas. Solución: dejar solo el enrutado y los árboles de decisión, y mover el detalle a `references/`.
3. **Referencias huérfanas**: existen pero nada indica cuándo cargarlas. Solución: disparadores en el flujo.
4. **Procedimiento tipo checklist**: "Paso 1, 2, 3". Solución: convertirlo en principios de decisión.
5. **Advertencia vaga**: "evita errores". Solución: NEVER específico con motivo.
6. **Skill invisible**: buen contenido pero descripción pobre. Solución: QUÉ + CUÁNDO + palabras clave.
7. **Ubicación errónea**: "cuándo usar" en el cuerpo. Solución: moverlo a la `description`.
8. **Sobreingeniería**: README, CHANGELOG o guías dentro de la carpeta de la skill. Solución: borrarlos, la skill solo incluye lo que el agente necesita. Las plantillas de `references/` que el agente usa para generar archivos sí son válidas.
9. **Libertad desajustada**: scripts rígidos para tareas creativas, o vaguedad en operaciones frágiles.

### Proceso

1. **Barrido de conocimiento**: marca cada sección como [E], [A] o [R] y calcula la proporción.
2. **Estructura**: valida el frontmatter, cuenta las líneas de `SKILL.md`, lista los archivos de `references/` con su tamaño, identifica el patrón y revisa los disparadores de carga.
3. **Puntuación**: asigna cada dimensión con su evidencia y una justificación de una línea. Si la nota es inferior al máximo, anota la mejora concreta.
4. **Nota final**: suma las 8 dimensiones.

| Nota | Porcentaje      | Significado                               |
| ---- | --------------- | ----------------------------------------- |
| A    | ≥90% (108+)     | Excelente, lista para producción          |
| B    | 80-89% (96-107) | Buena, mejoras menores                    |
| C    | 70-79% (84-95)  | Aceptable, hay camino claro de mejora     |
| D    | 60-69% (72-83)  | Por debajo del promedio, problemas serios |
| F    | <60% (<72)      | Rediseño de fondo                         |

### Formato del informe

Responde en español con esta estructura:

```markdown
# Evaluación de skill: <nombre>

## Resumen

- **Puntuación**: X/120 (X%) · **Nota**: A-F · **Patrón**: <patrón>
- **Proporción E:A:R**: X:Y:Z
- **Veredicto**: una frase

## Puntuación por dimensión

| Dimensión | Puntos | Máx. | Notas (con evidencia) |

## Problemas críticos

Lo que hay que corregir sí o sí.

## Top 3 mejoras

Ordenadas por impacto, con indicaciones concretas.

## Análisis detallado

Solo para dimensiones por debajo del 80%: qué falta, ejemplo citado y sugerencia.
```

### Pregunta final

¿Un experto del dominio diría "esto es conocimiento que me costó años aprender"? Si la respuesta es no, la skill solo comprime lo que el modelo ya sabe.

No modifiques la skill durante la revisión salvo que el usuario lo pida. Primero entrega el informe.
