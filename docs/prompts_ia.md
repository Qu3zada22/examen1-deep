# Registro de prompts de IA generativa - Grupo 7

El enunciado del examen exige documentar todo uso de IA generativa: el
prompt utilizado por tarea, por qué funcionó, y qué se tuvo que ajustar
del resultado. Limitar el uso de IA generativa y priorizar primero fuentes
propias del curso o búsquedas en internet.

Completar una fila por cada tarea en la que se usó IA generativa. No
eliminar filas de tareas donde se usó IA aunque el resultado se haya
descartado después; en ese caso anotarlo en la columna de ajustes.

| Tarea | Prompt utilizado | Por qué funcionó | Qué se ajustó |
|---|---|---|---|
| Ejemplo: estructura inicial del proyecto | "Genera un scaffold de proyecto Python con uv para un modelo de Dinámica de Sistemas..." | Dio una estructura de carpetas clara y consistente con las buenas prácticas de uv | Se eliminó la dependencia de un paquete src/ separado porque el entregable final es solo el notebook |
| Limpieza de las secciones 1, 2, 5, 7, 8 y 10 tras escribir las respuestas propias, y código de soporte para las Secciones 5 y 7 | "Ya hice la sección 1, 2, 5, 7, 8, 10, pero solo le agregué las respuestas, no les quité lo propuesto o lo que dice pendiente. Quisiera que tú puedas hacer eso y ver que tengan sentido las respuestas puestas. También la respuesta 5 y 7 les falta un pedazo de código que quisiera que le agregaras y que corrieras el notebook..." (con el código exacto de `q1_allocation_kits` y de la verificación `allocation_identical`/`need_identical` incluido en el prompt) | Funcionó porque el código a insertar ya venía completo y con la ubicación implícita (la propia respuesta escrita hacía referencia a "el output de arriba"), y porque pedía explícitamente una revisión de sentido, no solo pegar código: se contrastó cada respuesta contra los valores reales del notebook antes de dar por buena la limpieza | Se insertaron las dos celdas de código exactamente como se pidieron (una antes de la Sección 5, otra antes de la Sección 7); se borraron por completo las celdas de "nota de referencia + `[PENDIENTE - equipo]`" de las 6 secciones (no solo el texto pendiente); se dejaron fuera, a pedido explícito, los 4 puntos de limitación "de partida" de la Sección 10 que el equipo no usó; se reejecutó el notebook completo dos veces (tras la limpieza, y de nuevo tras que el equipo borrara la Sección 11 y editara una línea) para confirmar que seguía corriendo sin errores |
| Generación de la carpeta `informe/` (imágenes + .md fuente para el reporte Word) | "Necesito que hagas una carpeta que se llame informe, la cual vas a guardar, las imágenes que consideres necesarias para el informe y un .md que contenga todo lo necesario para generar un archivo word con todo para hacer el informe, la idea es que se lo pueda pasar un agente que pueda hacer el word, así que este tiene que ser suficiente para hacer el doc (aparte de las imágenes)" | Funcionó porque especificaba el criterio de éxito ("suficiente para que otro agente haga el doc sin más contexto") en vez de pedir "redacta el informe": eso forzó a que el .md incluyera todos los números/tablas ya calculados (no un resumen impreciso) y una nota explícita de instrucciones de formato para quien genere el .docx | Se armaron 3 gráficas nuevas (`q1_policy_score_comparison.png`, `q2_pct_change.png`, `sensitivity_comparison.png`) porque la gráfica de comparación antes/después ya existente en `outputs/` quedaba dominada por la escala de M4 y no dejaba ver el cambio real en M1/M2/M3; se reusaron sin cambios 2 imágenes ya generadas por el notebook (`coverage_trajectories_baseline.png`, `allocation_heatmap_kits_agua.png`); el texto de cada sección del .md es el que el equipo ya había escrito en el notebook, solo reorganizado al formato de reporte de `docs/reporte_estructura.md` — no se generó razonamiento nuevo, salvo el detalle de la violación C1 (Z1, bloque 1) que se obtuvo corriendo `check_constraints()` directamente, no del notebook |
| Borrador de guion de ensayo para el video (`docs/video_guion.md`) | "Para el guion, dame el guion como tal en el .md de video_guion, con todo el contexto que tienes" | Funcionó porque pedía explícitamente aprovechar "todo el contexto" ya construido (números del notebook, hallazgos del informe) en vez de generar contenido genérico de relleno; el guion resultante cita cifras reales (6.99%, 29.31%, la violación C1 de Z1, etc.) en vez de frases vagas | Se agregó una advertencia explícita al inicio del archivo, en mayúscula, recordando que el enunciado del examen prohíbe "guiones leídos": el texto se etiquetó como "borrador para ensayar y decir con sus propias palabras", no como libreto final; el equipo debe practicarlo hasta decirlo de memoria, con su propio tono, antes de grabar — no leerlo en cámara tal cual |
| | | | |

## Notas de uso

- Cada fila debe corresponder a una tarea concreta (ej. "diseño del
  submodelo de mortalidad evitable", "calibración de tasas de necesidad
  per cápita", "redacción de la justificación del paradigma"), no a una
  sesión completa de trabajo.
- La columna "Por qué funcionó" debe explicar la razón técnica (ej. el
  prompt incluía el formato de salida esperado, restringía el alcance,
  daba un ejemplo concreto), no solo decir "funcionó bien".
- La columna "Qué se ajustó" es obligatoria incluso si el resultado se usó
  casi sin cambios: escribir "ninguno, se usó tal cual" explícitamente en
  ese caso en vez de dejar la celda vacía.
- Recordar: toda decisión de diseño debe estar justificada con los
  criterios vistos en clase. Una justificación que solo repite texto
  generado por IA sin razonamiento propio no obtiene puntaje en ese
  criterio.
