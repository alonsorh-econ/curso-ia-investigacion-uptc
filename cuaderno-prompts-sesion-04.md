# Cuaderno de prompts · la casilla 04
**Sesión 4 · Resultados** · Curso de IA aplicada a la investigación · CENES–UPTC

Dieciséis prompts para la etapa de resultados. No son para copiar y ya: cada uno lleva
**qué revisar de la respuesta**, porque el que acepta o rechaza es usted.

> **Los tres pedazos que tiene todo prompt que sirve.**
> **Contexto** (qué hay y dónde) · **tarea** (qué quiero) · **restricción** (qué no puede hacer).
> El tercero es el que casi nadie escribe y el que hace la diferencia: *«no inventes ninguna
> cifra que no esté en el archivo»* convierte un asistente que adivina en uno que se limita
> a lo que hay.

> **Estos prompts suponen la carpeta de la sesión 2:**
> `00_contexto/ · 01_ideas/ · 02_decision/ · 03_literatura/ · 04_datos/ · 05_salidas/`
> Si no la tiene, el prompt 1 la arregla. Si su asistente no lee carpetas (ChatGPT o Gemini
> en el navegador), suba los archivos a la conversación y reemplace «en esta carpeta» por
> «que te adjunto».

---

## Bloque 1 · Ampliar el sistema

### 01 · Abrir la casilla de salidas
*Una sola vez · al empezar la sesión 4*

```
Revisa la estructura de mi carpeta de proyecto y créame lo que
falte para la etapa de resultados: una carpeta 05_salidas/ y
adentro un archivo resultados.md vacío.

Después escríbeme en el LEEME de la carpeta qué va en 05_salidas
y qué no: ahí va solo lo que se entrega (figuras finales, tablas
y texto), no los archivos intermedios.
```

**Qué revisa:** que no le haya movido nada de lo que ya tenía. Si le propone reorganizar toda
la carpeta, dígale que no: la estructura ya está decidida.

### 02 · Dejar por escrito lo que se descartó
*Cada vez que mata una idea o una variable*

```
Agrega a 02_decision/ una entrada con la fecha de hoy:
qué probé, qué resultado dio, y por qué lo descarté.

Escríbelo en dos o tres renglones, en pasado, sin adornos.
Si no descarté nada sino que cambié de criterio, dilo así.
```

**Qué revisa:** que la razón del descarte sea la suya y no una que él inventó para que suene
mejor. Este archivo es el que le va a salvar la tesis cuando le pregunten «¿y por qué no probó…?».

---

## Bloque 2 · Producir el resultado

### 03 · Qué tengo, antes de tocar nada
*Primer contacto con los datos limpios*

```
Mira el archivo de datos que está en 04_datos/ y dime qué tengo:
cuántas filas, cuántas columnas, qué tipo es cada una, cuántos
faltantes por columna y el rango de las numéricas.

No hagas ningún análisis todavía. Solo descríbeme el archivo.
```

**Qué revisa:** el número de filas contra lo que usted esperaba. Si no cuadra, ahí hay un
filtro que se le coló — y esa es la falla más cara de todas.

### 04 · La primera comparación, sin modelo
*Antes de correr cualquier regresión*

```
Divide la muestra en los dos grupos que quiero comparar
(dime tú cuál es la variable que los separa) y muéstrame,
para mi variable de resultado: media, desviación, n de cada
grupo y la diferencia entre las dos medias.

Todavía sin regresión. Quiero ver el dato crudo primero.
```

**Qué revisa:** si la diferencia cruda ya apunta en la dirección contraria a la que después
le da el modelo, algo pasa. No siga hasta entender qué.

### 05 · El modelo y la figura, en el mismo paso
*Cuando el descriptivo ya cuadra*

```
Corre la regresión de [resultado] contra [variable principal],
con controles de [lista] y efectos fijos de [unidad y tiempo].
Errores estándar agrupados por [unidad].

Guárdame en 05_salidas/ dos cosas: la tabla de coeficientes en
un CSV, y una figura del coeficiente principal con su intervalo
al 95 %. La figura se llama figura_1.png.

El código queda guardado, no solo el resultado.
```

**Qué revisa:** que el n de la regresión sea el mismo del descriptivo. Si bajó, el modelo
botó filas por faltantes y nadie se lo dijo.

---

## Bloque 3 · Intentar matarlo
*El bloque que separa un resultado de una impresión*

### 06 · Cuántas cosas probé de verdad
*Siempre. Antes de creerse cualquier resultado*

```
Cuenta cuántas variables de resultado distintas he probado en
esta sesión, incluyendo las que no reporté.

Con ese número, dime cuál es el umbral de significancia que me
corresponde si aplico Bonferroni (0.05 dividido entre cuántas
pruebas), y cuáles de mis resultados lo pasan y cuáles no.

No me consueles: dime cuáles se caen.
```

**Qué revisa:** que haya contado **todas**, incluidas las que probó y abandonó. Si usted no se
las dice, no las sabe. Este prompt solo sirve si usted es honesto con él.

### 07 · Cambiarle la muestra a propósito
*El día que le dé un resultado que le gusta*

```
Vuelve a correr el mismo modelo tres veces, cambiando solo
la muestra: (a) quitando el decil de arriba de la variable de
resultado, (b) quitando el año más raro, (c) quedándote solo
con las unidades que aparecen en todos los periodos.

Ponme los cuatro coeficientes en una sola tabla, con su p,
para que los pueda comparar de un vistazo.
```

**Qué revisa:** si el efecto se cae al quitar el decil de arriba, lo que tenía era un puñado
de casos extremos, no un patrón.

### 08 · La misma cosa, medida distinto
*Cuando el resultado depende de cómo definió la variable*

```
Mi variable de resultado se puede medir de otra forma
[describa la alternativa: en niveles vs. en logaritmo, en
valor vs. en conteo, en tasa vs. en absoluto].

Corre el modelo con las dos versiones y muéstrame si el signo
y la magnitud se repiten. Si no se repiten, dime cuál de las
dos mediciones es más defendible y por qué.
```

**Qué revisa:** que el signo se repita importa más que el tamaño. Un efecto que cambia de
signo según cómo se mida no es un efecto.

### 09 · Que prediga el pasado — la prueba que mata
*Si su diseño usa un instrumento, un corte temporal o un tratamiento*

```
Corre mi mismo modelo pero con un resultado ANTERIOR al
tratamiento — algo que ocurrió antes y que el tratamiento
no pudo haber cambiado.

Si mi variable principal predice ese resultado del pasado,
dímelo con todas las letras: quiere decir que no está
capturando el efecto del tratamiento sino algo que ya
estaba ahí antes.
```

**Qué revisa:** este es el prompt que tumbó un paper en clase. Si le sale significativo, el
resultado no se salva con más controles: se retira.

### 10 · El abogado del diablo
*Antes de mostrarle el resultado a alguien*

```
Actúa como el evaluador más duro que me podría tocar.

Léete mi resultado y mi diseño, y escríbeme las cinco
objeciones más fuertes que me harías — en orden, de la que
más daño hace hacia abajo.

Para cada una dime si la puedo responder con los datos que
tengo, o si tendría que conseguir otra cosa.
```

**Qué revisa:** las que dice que no puede responder con lo que tiene: esas son, literalmente,
su sección de Límites. Cópielas.

---

## Bloque 4 · Escribirlo

### 11 · La misma cifra, tres audiencias
*Actividad 2 de la sesión · y después, siempre*

```
Toma este resultado y escríbemelo tres veces:

1. Para el paper: 3 o 4 renglones, con la magnitud, la
   incertidumbre y el límite en la misma frase.
2. Para una lámina: un renglón, sin p ni error estándar,
   solo lo que se entiende de un vistazo.
3. Para quien toma la decisión: dos renglones, con qué
   decisión cambia y qué NO se puede concluir.

En las tres tiene que estar el límite. Sobre todo en la de
lámina, que es la que se comparte sola.
```

**Qué revisa:** que la versión de lámina traiga el límite. Casi nunca lo trae en el primer
intento — ni cuando la escribe un humano.

### 12 · Resultados y Límites, de una vez
*El entregable de la casilla 04*

```
Con el archivo de resultados que está en 05_salidas/ y la
figura_1.png, escríbeme la sección de Resultados y la de
Límites de un working paper.

Reglas: no inventes ninguna cifra que no esté en el archivo.
No digas "significativo" sin decir contra qué umbral. Nombra
la figura por su archivo, no la describas.

Y en Límites, sé más duro conmigo de lo que sería un evaluador.
```

**Por qué la última línea:** si usted no se busca los huecos, se los busca el evaluador — y
para entonces ya no los puede arreglar.

**Qué revisa:** cifra por cifra contra el archivo. Es donde más se inventa. Y que los límites
sean los suyos: si escribió alguno que usted no reconoce, bórrelo o averigüe si tiene razón.

### 13 · La tabla que va en el paper
*Cuando la tabla de coeficientes ya está*

```
Convierte esta tabla de coeficientes en una tabla de paper:
columnas por especificación, errores estándar en paréntesis
debajo de cada coeficiente, y una nota al pie que diga qué
efectos fijos lleva cada columna y cómo están agrupados los
errores.

Dámela en LaTeX, y guárdala en 05_salidas/tabla_1.tex.

Los asteriscos van contra el umbral corregido, no contra 0.05.
```

**Qué revisa:** la nota al pie. Es lo que más se copia mal entre versiones y lo primero que
mira un referee.

---

## Bloque 5 · Empaquetarlo

### 14 · La presentación, desde su propia plantilla
*Cuando ya tiene resultado y le piden mostrarlo*

```
Con los resultados de 05_salidas/ y la plantilla que está en
[ruta de su plantilla], ármame una presentación de seis
láminas: pregunta, datos, diseño, resultado, límites, qué sigue.

Las cifras salen del archivo, no de tu memoria. La figura entra
por referencia al archivo, no pegada. Una idea por lámina.

La de límites no es opcional: va aunque quede fea.
```

**Qué revisa:** que las figuras entren por referencia. Si las pega, la próxima vez que cambie
el dato la presentación queda mintiendo y usted no se entera.

### 15 · Que la figura se pueda volver a hacer
*Antes de cerrar el día*

```
Guárdame el script que produce figura_1.png en 05_salidas/,
de forma que si cambio el archivo de datos y lo vuelvo a
correr, la figura se regenere sola.

Ponle arriba un comentario con la fecha y de qué archivo de
datos sale.
```

**Qué revisa:** bórrele la figura y vuelva a correr el script. Si no la reproduce, no lo tiene
resuelto — lo tiene aplazado.

---

## Bloque 6 · Dejar el sistema listo para mañana

### 16 · El cierre del día
*Últimos cinco minutos de cada sesión de trabajo*

```
Escríbeme al final de 05_salidas/resultados.md un bloque
con la fecha de hoy y tres cosas:

- qué quedó produciendo resultado
- qué probé y no sirvió (una línea cada cosa)
- cuál es el siguiente paso concreto

Escríbelo para mí de la semana entrante, que no se va a
acordar de nada de esto.
```

**Qué revisa:** que el siguiente paso sea una acción, no un tema. «Revisar la literatura» no
es un paso; «buscar si alguien midió esto con datos de panel» sí.

---

## De un vistazo

| Si está… | Use el |
|---|---|
| Empezando la etapa de resultados | **01 · 02** |
| Produciendo el primer número | **03 · 04 · 05** |
| Con un resultado que le gusta demasiado | **06 · 07 · 08 · 09** |
| A punto de mostrárselo a alguien | **10 · 11** |
| Escribiendo el documento | **12 · 13** |
| Armando la presentación | **14 · 15** |
| Cerrando el día | **16** |

---

> **La regla que sostiene el cuaderno entero.**
> Una figura sin su límite escrito al lado no está terminada. Y un resultado en el que no
> intentó matarlo no es un resultado: es una primera impresión.
