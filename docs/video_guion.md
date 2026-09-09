# Guion del video de presentacion - Grupo 7

Política Pública y Asignación de Recursos - Ciudad UVG

Duración total: 3 a 5 minutos. Debe mostrar el modelo en funcionamiento
(pantalla del notebook o de las gráficas generadas) y al menos un
integrante del grupo hablando en cámara. No se aceptan videos de solo
pantalla sin voz ni guiones leídos.

Los cuatro puntos deben presentarse en el orden indicado, con los tiempos
exactos exigidos por el enunciado.

---

## Punto 1: Decisión de diseño más difícil (45 segundos)

Explicar cuál fue la decisión de diseño más difícil que tomó el grupo
(paradigma, scheduling, inicialización, regla de comportamiento) y por qué
se tomó así. No describir el modelo completo, concentrarse solo en esa
decisión.

Bullets de apoyo para preparar la respuesta:
- Candidata sugerida: cómo agregar el backlog de 8 recursos con unidades
  incompatibles (quetzales en millones vs. unidades físicas) sin que el
  presupuesto domine artificialmente el índice de urgencia, decisión de
  usar un índice de backlog adimensional (proporción relativa de faltante)
  en vez de sumar unidades crudas.
- Alternativa: la decisión de calibrar las tasas de necesidad per cápita
  (no dadas por el Excel) para que la escasez fuera severa pero no
  absoluta, de modo que las políticas de asignación pudieran diferenciarse
  entre sí.
- Justificar la decisión elegida con el razonamiento propio del grupo, no
  solo repetir lo que generó una IA.

---

## Punto 2: Resultado más importante (60 segundos)

Mostrar en pantalla el gráfico o tabla más importante de los resultados
(sugerido: la tabla de evaluación contra las 4 métricas con intervalos de
confianza del 95%, o el gráfico de trayectorias de cobertura por zona) y
explicarlo verbalmente. Debe mencionarse el intervalo de confianza y qué
implica para la incertidumbre de la conclusión.

Bullets de apoyo:
- Qué métrica(s) cumplen el umbral y cuáles no.
- Cuál es el ancho del intervalo de confianza del 95% y qué tan seguros
  están de la conclusión presentada.
- Por qué se eligió ese gráfico/tabla como "el resultado más importante".

---

## Punto 3: El intercambio presencial (60 segundos)

Explicar qué información se recibió de otro grupo (Grupo 2, 3 o 5, o los
tres), cómo se incorporó al modelo, y si eso cambió o confirmó las
conclusiones previas al intercambio.

Bullets de apoyo:
- Qué reporte(s) llegaron realmente y en qué formato.
- Qué decisión de asignación cambió más después de incorporar esa
  información.
- Cuánto cambió (usar la tabla de deltas con intervalos de confianza de la
  Sección 7 del notebook).

---

## Punto 4: Pregunta abierta (45 segundos)

Pregunta asignada al Grupo 7 (transcrita textualmente del enunciado del
examen):

> "Su modelo produce una recomendación de asignación de recursos que
> maximiza alguna combinación de las métricas definidas, pero toda métrica
> es una simplificación de lo que realmente importa en una emergencia.
> Qué dimensión del bienestar humano durante el desastre no está
> capturada en ninguna de las cuatro métricas de su archivo, y si esa
> dimensión se incorporara formalmente, qué decisión de asignación
> cambiaría y a favor de qué zona?"

Bullets de apoyo para preparar la respuesta:
- Elegir una dimensión concreta no capturada por M1-M4 (ej. bienestar
  psicológico/trauma, cohesión social y desplazamiento familiar, dignidad
  en el refugio temporal, equidad intergeneracional).
- Explicar por qué ninguna de las 4 métricas (mortalidad evitable, tiempo
  de respuesta, cobertura de suministros, eficiencia presupuestaria) captura
  esa dimensión.
- Argumentar concretamente qué decisión de asignación cambiaría si esa
  dimensión se incorporara formalmente, y a favor de qué zona (usar el
  índice de daño y la prioridad declarada de las zonas CRÍTICA, Z1 y Z5,
  como punto de partida del argumento).
- Evitar una respuesta genérica; debe conectarse con los resultados
  concretos del modelo del grupo.
