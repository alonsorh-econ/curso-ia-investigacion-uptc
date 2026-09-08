# Sesión 4 · Actividades
**De los resultados al producto** · martes 8 de septiembre

Todo se hace en **Google Colab**. Quien tenga VS Code o Cursor puede usarlo igual.
Los archivos están en el sitio del curso, sección *Materiales de la sesión 4*.

---

## Actividad 1 · ¿Cuál de estos resultados sobrevive?
**25 minutos · Colab · datos reales**

Estos son los resultados de verdad del análisis de contratación pública que vimos el sábado.
Se probaron **cinco variables de resultado** sobre los mismos 146 municipios.

**Suban `resultados_did.csv` a Colab** y pídanle al agente:

```
Con este archivo de resultados de una regresión, hazme un gráfico de coeficientes:
cada resultado en una fila, el punto en el coeficiente, y una línea horizontal que
vaya de coef-1.96*ee a coef+1.96*ee. Una línea vertical en cero.

Después píntame de color los que tengan p menor a 0.05/5, y de gris los demás.
Escribe el valor de p al lado de cada uno.
```

**Lo que tienen que responder, por escrito:**
1. ¿Cuántos resultados quedan de color?
2. El segundo de la lista tiene **p = 0,040**. En cualquier clase les enseñaron que eso es
   significativo. **¿Por qué queda gris?**
3. Si yo hubiera probado **solo** esa variable y no las otras cuatro, ¿el resultado sería
   distinto? ¿Cambió el dato, o cambió lo que yo hice?

> **La regla que sale de aquí.** Probar muchas cosas y reportar la que salió es la forma más
> común de engañarse sin mentir. El umbral se ajusta por cuántas cosas se probaron —
> **y hay que decir cuántas fueron.**

---

## Actividad 2 · La misma cifra, tres audiencias
**25 minutos · sin computador, o en su carpeta**

Tomen **un** resultado — el suyo, o el de arriba que sobrevivió — y escríbanlo tres veces:

| Para | Cuánto | Qué tiene que llevar |
|---|---|---|
| **El paper** | 3–4 renglones | la magnitud, la incertidumbre y **el límite**, en la misma frase |
| **Una lámina** | 1 renglón | solo lo que se entiende de un vistazo, sin p ni error estándar |
| **Un tomador de decisión** | 2 renglones | qué decisión cambia, y qué NO se puede concluir |

**Ejemplo, con el resultado que sobrevivió:**

> **Paper.** Los municipios cuyo alcalde ganó por menor margen dirigen una proporción mayor
> del valor contratado por contratación directa (β = 0,275; p = 0,003; 146 municipios).
> La magnitud equivale a unos 8 puntos porcentuales entre una elección holgada y una
> apretada. **La muestra cubre el 13 % de los municipios del país** y no es aleatoria.

> **Lámina.** Donde la elección fue apretada, se contrata más a dedo — unos 8 puntos más.

> **Decisión.** Vale la pena mirar los municipios de elección reñida al priorizar auditorías.
> **Esto no dice que haya corrupción:** la contratación directa es legal, y solo se midieron
> municipios con capacidad de reportar.

> **La trampa de este ejercicio:** casi todos escriben la versión de lámina **sin el límite**.
> Y esa es la versión que se viraliza.

---

## Actividad 3 · Ensamblar
**30 minutos · Colab + la plantilla**

Con la figura de la Actividad 1 y el párrafo de la Actividad 2:

1. Guarden la figura como `figura_1.png` en `05_salidas/` de su carpeta.
2. Creen `05_salidas/resultados.md` con esta estructura:

```
# Resultados

[la figura]

[el párrafo de paper de la Actividad 2]

## Límites
- [qué no se puede decir · 1]
- [qué no se puede decir · 2]
- [la variable que no tengo]
```

3. Pídanle al asistente:

```
Con este archivo de resultados y esta figura, escríbeme la sección de Resultados
y la de Límites de un working paper.

Reglas: no inventes ninguna cifra que no esté en el archivo. No digas "significativo"
sin decir contra qué umbral. Y en Límites, sé más duro conmigo de lo que sería un
evaluador.
```

> **Lo que se entrega hoy:** `05_salidas/resultados.md` con la figura y las dos secciones.
> **Es la casilla 04 del paper — la única sesión que produce dos secciones.**
