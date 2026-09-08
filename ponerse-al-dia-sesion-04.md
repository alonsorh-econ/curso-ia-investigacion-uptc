# Ponerse al día · el esqueleto completo del sistema
**Sesión 4** · Curso de IA aplicada a la investigación · CENES–UPTC

Si llegó tarde al curso, si se le quedó el sistema a medias, o si todavía tiene solo una
corazonada y ninguna carpeta: **este prompt lo pone donde debería estar hoy.**

Se corre **una sola vez**. Entra su pregunta o su intuición, y sale el sistema armado, un
diagnóstico honesto de qué le falta, y **un encargo por cada casilla** para que después, con
tiempo, le suelte cada parte a un asistente por separado.

---

## Antes de pegar nada · dos minutos

1. **Cree una carpeta** para su investigación, donde usted quiera. Póngale el nombre de su tema,
   sin espacios: `informalidad_tunja`, `desercion_uptc`, lo que sea.
2. **Abra su asistente ahí.** Si usa Claude Code o Cursor, ábralos en esa carpeta. Si usa
   ChatGPT o Gemini en el navegador, no importa: le va a devolver los archivos escritos y usted
   los guarda a mano.
3. **Escriba su punto de partida** en el hueco del prompt. Puede ser una sola frase mal hecha.
   *Es mejor una frase honesta que un párrafo bonito.*

---

## EL PROMPT

Copie todo lo que sigue, reemplace lo que está entre corchetes, y péguelo.

```
Vas a montarme el esqueleto de mi sistema de investigación y después
me vas a decir, sin adornos, qué me falta.

═══ MI PUNTO DE PARTIDA ═══

Mi pregunta o mi intuición es:
[ESCRIBA AQUÍ SU PREGUNTA O CORAZONADA. Una o dos frases, como le salga.
 Ejemplo: "creo que los negocios del centro de Tunja no se formalizan
 porque el trámite les cuesta más de lo que ganan en un mes"]

Lo que ya sé del terreno y que alguien de afuera no sabría:
[DOS O TRES HECHOS CONCRETOS. No "conozco la región": cosas como
 "esa base la maneja la Secretaría de Hacienda y no la entrega",
 "en ese barrio la mayoría son puestos móviles, no locales".
 Si no tiene ninguno, escriba: NO TENGO NINGUNO TODAVÍA]

Lo que ya tengo escrito o hecho:
[PEGUE AQUÍ lo que tenga, aunque esté incompleto y feo. Si no tiene
 nada, escriba: NO TENGO NADA MÁS]

═══ LO QUE TIENES QUE HACER ═══

1. CREA ESTA ESTRUCTURA, completa, con todos los archivos:

   CONTEXTO.md          quién soy, qué sé del terreno, en qué ando
   BITACORA.md          qué hice cada día, en una línea
   00_contexto/dominio.md
   01_ideas/hipotesis.md
   02_decision/interrogatorio.md
   02_decision/decisiones.md
   03_literatura/ramas.md
   03_literatura/tabla_evidencia.md
   04_datos/FUENTES.md
   04_datos/datos.md
   04_datos/crudo/          (vacía)
   04_datos/limpio/         (vacía)
   05_salidas/resultados.md

2. LLENA CADA ARCHIVO con lo que se pueda deducir de mi punto de partida.

   REGLA DURA: lo que no puedas saber por mí, NO lo inventes. Escribe
   [FALTA: qué es exactamente lo que hace falta y quién lo puede
   conseguir]. Prefiero diez [FALTA] honestos que un archivo lleno de
   relleno que suena bien.

   En 01_ideas/hipotesis.md la hipótesis tiene que decir QUÉ SE MIDE,
   EN QUIÉN y CONTRA QUÉ SE COMPARA. Si mi punto de partida no alcanza
   para eso, escríbela igual y marca con [FALTA] lo que no alcanza.

   En 02_decision/interrogatorio.md ponme las cinco preguntas más
   incómodas que le haría un jurado a mi pregunta.

   En 03_literatura/ramas.md ponme las tres o cuatro RAMAS de literatura
   donde caería esto — el tipo de trabajo, no los papers. NO inventes
   autores, títulos, años ni DOI. Si no estás seguro de una cita, no la
   pongas: escribe [FALTA: buscar].

   En 04_datos/FUENTES.md ponme qué fuentes de datos existirían para
   esto en Colombia, cuál es pública y cuál hay que pedir, y a quién.

   En 05_salidas/resultados.md deja la estructura vacía con sus dos
   secciones: Resultados y Límites.

3. DIAGNOSTÍCAME, en una tabla de tres columnas:
   PIEZA · CÓMO ESTÁ · QUÉ LE FALTA
   Solo tres estados: completa, incompleta, no existe.

4. ESCRIBE UN ARCHIVO ENCARGOS.md con SEIS encargos, uno por casilla.
   Cada encargo tiene que poder pegarse solo en una conversación nueva,
   sin que yo tenga que explicar nada. O sea que cada uno lleva: qué
   archivo trabaja, qué tiene que producir, y cuál es la regla que no
   puede romper.

5. TERMINA diciéndome DOS cosas, y solo dos:
   - LO MÁS FLOJO: cuál pieza está peor y por qué. Una sola.
   - LA SIGUIENTE TAREA: una sola, concreta, de veinte minutos.

═══ REGLAS ═══

- No inventes cifras, autores, títulos, años, DOI ni fuentes. Nunca.
  Si no lo sabes, [FALTA].
- No me elogies y no me digas que voy bien si no voy bien.
- Si mi pregunta todavía no dice qué se mide y en quién, esa es
  LO MÁS FLOJO y esa es LA SIGUIENTE TAREA. No busques otra cosa.
- Español. Sin viñetas de relleno. Corto.
```

---

## Qué debe salir · y cómo saber si salió bien

**Salió bien si:**

- La tabla de diagnóstico tiene **varias filas en «incompleta»**. Es lo normal y es lo correcto.
  Un diagnóstico donde todo aparece completo, con un punto de partida de dos frases, es un
  diagnóstico que le está mintiendo.
- Los archivos están llenos de **`[FALTA: ...]`**. Cada uno es una tarea suya, no un defecto.
- `03_literatura/ramas.md` **no trae autores inventados**. Si le aparecieron cinco citas con año
  y DOI de una pregunta que usted escribió en dos frases, **búsquelas antes de creerles**:
  la mitad no existe.

**Salió mal si:** le devolvió un documento bonito sin un solo `[FALTA]`. Vuelva a correrlo
agregando al final: *«otra vez, y esta vez marca con [FALTA] todo lo que no puedas saber por mí».*

---

## Después · una casilla a la vez

Con `ENCARGOS.md` en la mano ya no tiene que hacer todo de una. **Abre una conversación nueva
por casilla, pega el encargo, y trabaja solo esa.** Es más lento de leer y mucho más rápido de
hacer, porque cada conversación tiene una sola cosa en la cabeza.

El orden que sirve:

| Orden | Casilla | Por qué va ahí |
|---|---|---|
| 1.º | `CONTEXTO.md` y `00_contexto/` | sin esto, todo lo demás sale genérico |
| 2.º | `01_ideas/` | la hipótesis tiene que decir qué se mide y en quién |
| 3.º | `02_decision/` | descartar antes de invertir tiempo |
| 4.º | `03_literatura/` | y **verificar cada DOI**, uno por uno |
| 5.º | `04_datos/` | pedir lo que hay que pedir, que se demora |
| 6.º | `05_salidas/` | esta es la de la sesión 4 |

Y al terminar cada casilla, una línea en `BITACORA.md` con la fecha. En dos meses esa bitácora
va a ser lo único que le explique por qué tomó las decisiones que tomó.

---

## Si prefiere empezar con la carpeta ya hecha

En el sitio del curso está **`plantilla-sistema.zip`**: la estructura completa, vacía, con un
`LEEME.md` que dice qué va en cada carpeta. La descomprime, le cambia el nombre, y corre el
prompt de arriba diciéndole *«la estructura ya existe, solo llénala y diagnostícame»*.

---

> **Lo que hay que entender de este ejercicio.**
> El sistema no es una carpeta bonita: es la diferencia entre poder retomar su investigación un
> martes cualquiera a las 9 de la noche, y tener que acordarse de todo desde cero. Lo que hoy
> parece burocracia, en la sesión 6 es lo que permite armar el paper en una tarde.
