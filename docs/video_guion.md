# Guion del video de presentacion - Grupo 7

Política Pública y Asignación de Recursos - Ciudad UVG

Duración total: 3 a 5 minutos. Debe mostrar el modelo en funcionamiento
(pantalla del notebook o de las gráficas generadas) y al menos un
integrante del grupo hablando en cámara. No se aceptan videos de solo
pantalla sin voz ni guiones leídos.

Los cuatro puntos deben presentarse en el orden indicado, con los tiempos
exactos exigidos por el enunciado.

> **IMPORTANTE — el enunciado prohíbe explícitamente los "guiones leídos".**
> El texto de abajo es un **borrador para ensayar y decir con sus propias
> palabras frente a cámara**, no un libreto para leer en voz alta. Practíquenlo
> hasta poder decirlo de memoria, en su propio tono, mirando a la cámara — no
> a un papel o pantalla. Está escrito ya con los números reales del modelo
> para que no tengan que estar buscando datos mientras ensayan, pero la forma
> de decirlo (pausas, énfasis, orden de las frases) debe volverse suya.

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

### Borrador (para ensayar, no leer)

"La decisión más difícil no fue elegir el paradigma, fue cómo medir la
urgencia de una zona cuando el modelo maneja 8 recursos con unidades que no
se pueden sumar entre sí: presupuesto en millones de quetzales, vehículos en
unidades, combustible en galones, kits de agua en kits. Si simplemente
sumábamos faltantes en unidades crudas, el presupuesto —que se mueve en
millones— iba a dominar completamente el índice de urgencia, y una zona
podía verse 'urgente' solo porque le faltaba dinero, no porque realmente
estuviera peor que las demás.

La solución que elegimos fue convertir el backlog en un índice
adimensional: no cuánto falta en unidades absolutas, sino qué proporción
relativa de la necesidad de cada recurso quedó sin cubrir, promediada entre
los 8 recursos. Eso significa que un déficit de agua y un déficit de
presupuesto pesan lo mismo si son proporcionalmente del mismo tamaño,
sin importar la escala numérica de cada uno.

Lo justificamos así: el objetivo del backlog no es contabilizar dinero, es
capturar qué tan insatisfecha está una zona en términos comparables entre
sí, para que ese índice pueda alimentar de forma justa el ciclo de
retroalimentación de arrastre y crecimiento de urgencia que tiene el
modelo."

*(En pantalla: la celda del notebook donde se calcula `relative_unmet` y la
ecuación de actualización del backlog, Sección 4.4.)*

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

### Borrador (para ensayar, no leer)

"El resultado más importante es esta tabla: la evaluación de nuestra
política de asignación —`severity_weighted`— contra las 4 métricas de
política, con 300 réplicas de Monte Carlo para las 72 horas completas.

Dos métricas pasan cómodamente: mortalidad evitable, en 6.99%, muy por
debajo del umbral de 15%; y tiempo de respuesta, en 1.57 horas, por debajo
del umbral de 6. Pero dos métricas fallan, y fallan por mucho: cobertura de
suministros llega solo a 29.31%, contra un umbral de 80%; y eficiencia
presupuestaria llega a 693.6 personas atendidas por millón de quetzales,
contra un umbral de 850.

Lo que hace esto contundente no es el promedio, es el intervalo de
confianza del 95%: para cobertura de suministros, el intervalo va de
24.8% a 33.3%. Ni en el escenario más optimista de las 300 realizaciones
nos acercamos al 80% requerido. Eso nos dice que este no es un problema de
mala suerte en una corrida particular, ni de qué política elegimos —las 6
políticas que probamos dan resultados en ese mismo rango—, es una
restricción física real: solo hay 2,800 kits de agua para 84,900 personas
en riesgo. Por eso elegimos esta tabla como el resultado más importante:
nos dice, con la incertidumbre ya incorporada, exactamente cuál es el
techo real del sistema y por qué ninguna política de asignación puede
superarlo por sí sola."

*(En pantalla: la tabla de evaluación de la Sección 3.2 del informe /
Sección 9 del notebook, y opcionalmente el gráfico de trayectorias de
cobertura, `informe/images/coverage_trajectories_baseline.png`.)*

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

### Borrador (para ensayar, no leer)

"En el intercambio recibimos los tres reportes que esperábamos: de Grupo 2,
saturación hospitalaria; de Grupo 3, balance de suministros; y de Grupo 5,
despliegue de personal. El hallazgo que más nos importó fue el de Grupo 3:
encontraron que la zona Z1 no tiene ningún centro de acopio ni ninguna
ruta de entrada en su red de distribución. No es que se le acabe el
inventario, es que nunca le llega nada.

Aquí viene lo interesante: cuando incorporamos ese dato a nuestro modelo,
verificamos —no lo asumimos, lo corrimos con la misma semilla, con y sin el
dato del intercambio— que nuestra asignación física no cambió ni una sola
unidad. ¿Por qué? Porque la política que usamos reparte recursos según daño
estructural y prioridad declarada, dos datos fijos del Excel, y nunca lee
la necesidad reportada. Entonces el intercambio no movió nuestro plan.

Lo que sí movió fue nuestro diagnóstico: la cobertura de suministros en las
primeras 24 horas cayó de 29.3% a 22.2%, una caída de 7.1 puntos
porcentuales con un intervalo de confianza que va de 9.1 a 4.9 puntos —o
sea, no cruza cero, es un cambio real. El intercambio no cambió qué
decidimos hacer, cambió qué tan bien pensábamos que lo estábamos haciendo.
Y eso en sí mismo es una limitación que documentamos: una política que no
puede reaccionar a mejor información, por más que se la den."

*(En pantalla: `informe/images/q2_pct_change.png`, o la celda de
verificación `allocation_identical` / `need_identical` del notebook,
Sección 7.)*

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

### Borrador (para ensayar, no leer)

"Ninguna de nuestras 4 métricas captura lo que llamaríamos el trauma de la
promesa incumplida: la diferencia entre que a una zona nunca se le asigne
nada, y que se le asigne algo en papel que nunca llega. Nuestras métricas
miden cobertura y mortalidad agregadas, pero no distinguen esos dos casos.

Y no es una idea abstracta, la encontramos en nuestros propios resultados:
nuestro registro de auditoría encontró que Z1 —zona crítica— recibió apenas
0.97 de la unidad mínima de equipo de rescate exigida en el primer bloque,
casi cumpliendo pero no del todo. Y Grupo 3 nos confirmó algo más grave:
Z1 no tiene ninguna ruta de suministro física, así que nuestra política le
sigue asignando recursos —porque tiene el daño más alto de las 5 zonas— pero
esos recursos nunca llegan. Eso no es un problema de escasez, es una
desconexión que ninguna métrica de cobertura agregada revela.

Si esa dimensión se incorporara formalmente —una especie de penalización
por 'asignación fantasma', recursos comprometidos que no se entregan— la
decisión que cambiaría es dejar de seguir asignando kits de agua a Z1 bajo
el supuesto de que algún día llegan, y redirigir esa fracción del
presupuesto a resolver el problema de acceso físico, aunque eso signifique
coordinar con Grupo 4 en vez de repartir más recursos del pool propio. La
zona beneficiada sería Z1 —no con más kits en papel, sino con una
intervención que sí le puede llegar."

*(En pantalla: el rostro de quien habla, cámara al frente — este punto no
necesita pantalla del notebook, es la reflexión final.)*

---

## Notas de producción

- Tiempos totales: 45 + 60 + 60 + 45 = 210 segundos = 3.5 minutos —dentro
  del rango de 3 a 5 minutos exigido, con margen para transiciones entre
  pantalla y cámara.
- Grabar cada punto por separado y cronometrar antes de unir: es más fácil
  ajustar el tono/ritmo de 45-60 segundos sueltos que de un video completo.
- Evitar mirar hacia abajo o hacia un costado de forma sostenida (señal de
  estar leyendo) — practicar cada punto en voz alta, sin texto visible,
  hasta poder decirlo mirando a la cámara.
- Las imágenes y tablas exactas que se sugieren mostrar en pantalla ya
  están generadas en `informe/images/` y en `outputs/`; no hace falta
  recrear ninguna gráfica nueva para el video.
