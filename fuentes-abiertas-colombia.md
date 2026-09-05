# Fuentes abiertas colombianas — para bajar algo hoy, en clase

*Sesión 3 · «Datos» · CENES–UPTC · sábado 5-sep-2026, 8:00 a.m.*

Esta hoja es para quien **no trajo datos propios**: dieciocho fuentes públicas de las que se
puede descargar algo en los próximos diez minutos. No es un catálogo para leer: es una lista
para escoger **una** y bajarla. Cada ficha trae cinco campos, y el quinto es el que importa —
**la trampa que trae esa fuente**: el cambio de escala, de cobertura, de plataforma o de
definición que arruina una serie de tiempo si uno no lo sabe. Ese campo es el hilo de la sesión.

> ### La regla, antes de bajar nada
> **Abra `04_datos/FUENTES.md` y anote la descarga ANTES de hacerla.** Cuatro cosas:
> **fuente** (nombre y URL) · **fecha de descarga** · **filtro aplicado** (qué años, qué
> municipios, qué columnas) · **decisión de muestra** (a quién dejó por fuera y por qué).
> Sin eso, en tres semanas usted no puede reproducir su propia tabla — y el evaluador tampoco.
> Y lo que entra a `04_datos/crudo/` **no se toca nunca más**.

**Cómo se leen las marcas.** Todos los enlaces se comprobaron con petición HTTP real
**la noche del 4-sep-2026** (entre las 10:15 p.m. y la madrugada del 5):
✅ responde limpio · ⚠️ responde pero redirige a otra dirección, o el servidor va lento e
intermitente (se dice a cuál y qué pasó) · ✗ no responde. Los enlaces que salieron muertos
**no quedaron en la lista**: se reemplazaron por el que sí responde, y al final se dice cuáles
eran. Un enlace que responde hoy puede no responder mañana: por eso `FUENTES.md` pide la fecha.

---

## 1 · Contratación pública

### 1.1 datos.gov.co — el catálogo nacional de datos abiertos
- **Qué hay:** miles de conjuntos publicados por entidades del Estado, sobre plataforma Socrata. Es donde vive SECOP y buena parte de lo demás de esta lista.
- **Enlace:** https://www.datos.gov.co/browse — ✅ 200
- **Cómo se baja:** CSV/JSON con el botón *Exportar*, o por **API de Socrata sin llave**: `https://www.datos.gov.co/resource/<id>.csv?$limit=5000`. El `<id>` de cuatro-más-cuatro caracteres está en la URL del conjunto. Los filtros van en la misma URL (`$where`, `$select`, `$limit`); la llave de API solo sube el límite de peticiones por minuto.
- **Grano y cobertura:** depende del conjunto. El portal es la puerta, no el dato.
- **⚠️ La trampa que trae:** el mismo tema vive en **varios conjuntos casi homónimos con cortes distintos**. «SECOP I» (`x6v4-i8gf`) quedó **congelado el 19-dic-2025**; «SECOP I - Procesos de Compra Pública» (`f789-7hwg`) se actualiza a diario pero **empieza el 1-ene-2018**; «SECOP Integrado» (`rpmr-utcd`) junta SECOP I, SECOP II y Tienda Virtual, donde la unidad ya no es la misma (proceso vs. contrato). Bajar «el primero que sale en la búsqueda» le define la serie sin que usted lo decida.

### 1.2 SECOP II — Contratos Electrónicos
- **Qué hay:** todos los contratos registrados en SECOP II desde su lanzamiento; 85 columnas (entidad, municipio, modalidad, valor, fechas, proveedor).
- **Enlace:** https://www.datos.gov.co/d/jbjy-vk9h — ⚠️ 200, redirige a `.../Estad-sticas-Nacionales/SECOP-II-Contratos-Electr-nicos/jbjy-vk9h` (mismo conjunto; la forma `/d/<id>` es la estable). Contexto institucional en https://www.colombiacompra.gov.co/transparencia/conjuntos-de-datos — ⚠️ 200, redirige a `.../transparencia/datos-abiertos/conjuntos-de-datos-abiertos`. **La descarga se hace por datos.gov.co, no por el portal de Colombia Compra.**
- **Cómo se baja:** **API de Socrata sin llave** (la exportación completa son decenas de GB). Ejemplo: `https://www.datos.gov.co/resource/jbjy-vk9h.csv?$where=departamento='Boyacá'&$limit=50000`
- **Grano y cobertura:** un contrato por fila · desde 2017–2018 hasta hoy · filas actualizadas el 4-sep-2026.
- **⚠️ La trampa que trae — son dos:**
  1. **La adopción de SECOP II fue gradual y por entidad.** Un municipio que pasa de 11 contratos en 2021 a 338 en 2025, y de 18 % a 81 % de contratación directa, casi nunca cambió de conducta: **empezó a usar la plataforma**. La serie mide adopción tecnológica antes que comportamiento contractual. Antes de interpretar cualquier salto, grafique el número de entidades que reportan por año — en el ejercicio del curso pasan de **368 a 717**.
  2. **SECOP II no trae código DIVIPOLA**, solo el nombre del municipio escrito a mano por cada entidad. Emparejar por nombre produce duplicados («Tunja», «TUNJA », «Tunja - Boyacá») y pierde filas; en el CSV del curso, el **18,3 %** de los contratos no dice municipio. Qué hacer con esas filas es decisión suya, no de la máquina, y va escrita en `FUENTES.md`. Y `valor_del_contrato` trae erratas de digitación capaces de inflar una suma anual varios órdenes de magnitud: mire el máximo antes de sumar.

### 1.3 SECOP I — Procesos de Compra Pública
- **Qué hay:** procesos de compra pública del SECOP I — proceso, fase de selección y adjudicación; 79 columnas.
- **Enlace:** https://www.datos.gov.co/d/f789-7hwg — ⚠️ 200, redirige a `.../SECOP-I-Procesos-de-Compra-P-blica/f789-7hwg`
- **Cómo se baja:** API de Socrata sin llave o exportación CSV.
- **Grano y cobertura:** un proceso por fila · **desde el 1-ene-2018** (lo dice la propia ficha del conjunto) · actualizado el 4-sep-2026.
- **⚠️ La trampa que trae:** **arranca en 2018, no antes.** Quien necesite 2013–2017 tiene que ir al conjunto legado «SECOP I» (`x6v4-i8gf`), que **dejó de actualizarse el 19-dic-2025** y no comparte esquema de columnas. Pegar los dos sin homologar produce un hueco o un salto artificial en 2018. Y ojo con la unidad: aquí la fila es un **proceso**, en SECOP II es un **contrato** — un proceso puede terminar en varios contratos o en ninguno.

---

## 2 · Educación

### 2.1 Saber 11 — Resultados únicos (ICFES, vía datos.gov.co)
- **Qué hay:** **7.109.704 registros** individuales de la prueba Saber 11, con 51 columnas: puntajes, colegio, municipio del colegio y de residencia, variables socioeconómicas.
- **Enlace:** https://www.datos.gov.co/d/kgxf-xxbe — ⚠️ 200, redirige a `.../Educaci-n/Resultados-nicos-Saber-11/kgxf-xxbe`, ficha canónica del mismo conjunto. Publicado por «ICFES DATOS ABIERTOS».
- **Cómo se baja:** **API de Socrata sin llave**, en tajadas. Ejemplo probado el 4-sep-2026 (devuelve CSV con encabezado): `https://www.datos.gov.co/resource/kgxf-xxbe.csv?$where=cole_depto_ubicacion='BOYACA'&$limit=50000`
- **Grano y cobertura:** **un estudiante por fila** · períodos `20101` a `20224` (2010 a 2022).
- **⚠️ La trampa que trae — son dos, y las dos matan la serie:**
  1. **Cambio de escala en 2014-2.** El puntaje pasó de un esquema **por materias de 0 a 100** a un **global de 100 a 500**. Graficar 2010–2020 sin separar produce un «milagro educativo» que es puramente aritmético: en el reto del curso, **2013 = 47,05 y 2014 = 254,65**. Revise en qué período aparece por primera vez la columna del puntaje global antes de promediar nada.
  2. **La columna `periodo` mezcla poblaciones y cambió de codificación.** Conteo del propio conjunto, verificado el 4-sep-2026: `20101` = **41.656** estudiantes frente a `20102` = **628.374** — el primer semestre es calendario B, una población mucho menor y distinta. Y peor: **`20182`, `20192`, `20202` y `20212` no existen**; en su lugar aparecen `20194` = **1.096.524** y `20224` = **1.065.888**. Si usted agrupa por año, 2018, 2020 y 2021 le quedan con 15.000–32.000 estudiantes y 2019 y 2022 con más de un millón. No es que Colombia dejara de presentar el examen: **es la codificación del período.**
  > *Nota de coherencia con el material de clase:* los números de arriba son del conjunto completo publicado en datos.gov.co. El CSV recortado que se reparte en el aula da **5.948 (2010-1) frente a 97.444 (2010-2)** — dieciséis veces más — y **3.278 frente a 80.333** en 2021. Distinta base, mismo golpe.

### 2.2 DataIcfes — bases anonimizadas del ICFES
- **Qué hay:** bases de todas las pruebas (Saber 11, Saber Pro, Saber TyT, Saber 3-5-9), diccionarios y documentación técnica. Es la fuente primaria, más completa que el extracto de datos.gov.co.
- **Enlace:** https://www.icfes.gov.co/data-icfes/ — ⚠️ 200, redirige a https://www.icfes.gov.co/investigaciones/data-icfes/ (use la segunda).
- **Cómo se baja:** **formulario / requiere registro** (datos del solicitante y aceptación de términos) antes de habilitar la descarga. No sirve para bajar en el minuto uno de la clase; sí para el trabajo de la semana.
- **Grano y cobertura:** estudiante · por aplicación; series largas según la prueba.
- **⚠️ La trampa que trae:** hay una columna `estu_estadoinvestigacion` — **registros anulados o en investigación por presunto fraude** que siguen en la base y no deben contarse como resultados válidos. Y `estu_consecutivo` **se reasigna en cada aplicación**: no sirve para seguir a la misma persona entre Saber 11 y Saber Pro.

### 2.3 MEN — matrícula de educación superior por municipio
- **Qué hay:** matrícula de educación superior por municipio y nivel de formación (técnica profesional, tecnológica, universitaria, especialización, maestría, doctorado) y número de IES con oferta.
- **Enlace:** https://www.datos.gov.co/d/y9ga-zwzy — ⚠️ 200, redirige a `.../Educaci-n/MEN_ESTADISTICAS-MATRICULA-POR-MUNICIPIOS_ES/y9ga-zwzy`
- **Cómo se baja:** CSV directo o API de Socrata sin llave. Es chiquito: **19.618 filas**.
- **Grano y cobertura:** municipio × año × nivel · **2005 a 2021** (verificado el 4-sep-2026).
- **⚠️ La trampa que trae:** el municipio es el de la **sede de la institución**, no el de residencia del estudiante — y **la matrícula virtual se le atribuye al municipio de la IES**. Como la oferta virtual creció fuerte después de 2016, Bogotá y las capitales se inflan y los municipios pequeños parecen vaciarse: un mapa de «acceso a educación superior» hecho con esta tabla mide dónde están las universidades, no dónde estudia la gente. **Y se detiene en 2021**: si su trabajo necesita 2022–2025, esta no es la fuente.

### 2.4 Observatorio Laboral para la Educación (OLE)
- **Qué hay:** vinculación laboral y salario de enganche de los graduados de educación superior, por programa, institución y nivel.
- **Enlace:** https://ole.mineducacion.gov.co/ — ⚠️ 200, redirige a https://ole.mineducacion.gov.co/portal/
- **Cómo se baja:** consulta en el portal con exportación a Excel/CSV; sin registro para los agregados.
- **Grano y cobertura:** programa × institución × año de graduación · series desde comienzos de los 2000.
- **⚠️ La trampa que trae:** el OLE mide vinculación **cruzando contra la planilla de aportes (PILA)**. Quien trabaja como independiente, informal o se fue del país **no aparece como vinculado**: cuenta como no empleado. El «salario de enganche» está calculado solo sobre los formales — es un promedio condicionado a estar en la formalidad, no el ingreso del egresado promedio.

---

## 3 · Empresas y mercado laboral

### 3.1 DANE — ANDA / microdatos (GEIH y las demás encuestas)
- **Qué hay:** el repositorio oficial de microdatos anonimizados del DANE — **570 operaciones estadísticas** catalogadas (verificado el 4-sep-2026), entre ellas la Gran Encuesta Integrada de Hogares año por año, la Encuesta de Micronegocios (EMICRON), el Censo 2018 y la ENPH.
- **Enlace (catálogo):** https://microdatos.dane.gov.co/index.php/catalog — ✅ 200 · **GEIH 2025:** https://microdatos.dane.gov.co/index.php/catalog/853 — ✅ 200 · descarga: https://microdatos.dane.gov.co/index.php/catalog/853/get-microdata — ✅ 200 · **GEIH 2026:** https://microdatos.dane.gov.co/index.php/catalog/900 — ✅ 200
- **Cómo se baja:** **formulario corto** en «Obtener microdatos» (motivo de uso y correo); luego bajan los `.zip` con los módulos en CSV/DTA/SAV. Sin costo.
- **Grano y cobertura:** **persona y hogar** · GEIH continua desde 2007, con ficha ya abierta para 2026.
- **⚠️ La trampa que trae:** en el mismo catálogo conviven **«GEIH – 2010…2020»** y **«GEIH – Empalme – 2010…2020»** (`catalog/755` a `catalog/765`). No son duplicados: la serie de empalme es la **reponderada con las proyecciones del Censo 2018**, y sus niveles de ocupación, desempleo y pobreza **no coinciden** con los de la serie original. Armar un panel 2015–2024 tomando «el archivo del año» mete un salto metodológico en el punto donde uno cambió de colección sin darse cuenta. Además, la GEIH viene **partida en módulos** (Características generales, Ocupados, Desocupados, No ocupados…) que hay que unir por `DIRECTORIO`, `SECUENCIA_P` y `ORDEN`, y **nada se promedia sin el factor de expansión**: un promedio simple de la GEIH no es una cifra de Colombia, es una cifra de la muestra.

### 3.2 RUES — Registro Único Empresarial y Social
- **Qué hay:** la consulta pública del registro mercantil de las cámaras de comercio del país: matrículas, renovaciones, actividad CIIU, ubicación y estado.
- **Enlace:** https://www.rues.org.co/ — ✅ 200 · consulta avanzada: https://www.rues.org.co/consultas-avanzadas — ✅ 200
- **Cómo se baja:** **formulario de consulta**, no descarga masiva. Para bases completas hay que pedirlas a la cámara de comercio de la jurisdicción (en Boyacá, la Cámara de Comercio de Tunja, Duitama y Sogamoso). En clase sirve para verificar casos, no para bajar un panel.
- **Grano y cobertura:** establecimiento y comerciante · estado a la fecha de consulta, sin histórico descargable.
- **⚠️ La trampa que trae:** **«matriculado» no es «activo».** Una empresa que dejó de renovar sigue apareciendo en el registro hasta que alguien la cancela, y la cancelación puede tardar años o no ocurrir. Y **comerciante ≠ establecimiento**: una persona natural puede tener varios establecimientos, así que contar filas cuenta locales, no empresas. Cualquier serie de «empresas creadas» construida sobre matrículas mide trámites, no negocios en operación.

---

## 4 · Territorio y finanzas municipales

### 4.1 TerriData (DNP)
- **Qué hay:** el tablero territorial del DNP: cientos de indicadores demográficos, económicos, fiscales, de educación, salud y seguridad para los más de 1.100 municipios y los 32 departamentos. Es la forma más rápida de tener un **panel municipio-año** sin construirlo.
- **Enlace:** https://terridata.dnp.gov.co/index-app.html#/descargas — ✅ 200 · perfiles municipales: https://terridata.dnp.gov.co/index-app.html#/perfiles — ✅ 200 · portal: https://terridata.dnp.gov.co/ — ✅ 200
- **Cómo se baja:** **descarga directa** de Excel/CSV desde la pestaña *Descargas*, escogiendo entidad territorial y categoría, o bajando el paquete completo de una dimensión. Sin cuenta.
- **Grano y cobertura:** municipio y departamento × año, según el indicador.
- **⚠️ La trampa que trae:** **TerriData no produce datos, los recoge** — y cada indicador trae su propia fuente y su propio año de corte; el mismo indicador puede venir del DANE hasta cierto año y del ministerio del ramo después. En una misma ficha municipal puede haber un dato de 2018 al lado de uno de 2023. **La columna de fuente de la hoja de descarga no es decorativa: es la que dice si la serie se puede leer como serie.** Y para las tasas hay algo peor: **los denominadores poblacionales se recalcularon con el Censo 2018**, así que las cifras per cápita de años anteriores cambiaron hacia atrás.

### 4.2 CHIP / FUT — Contaduría General de la Nación
- **Qué hay:** ingresos, gastos, inversión y deuda reportados por cada entidad territorial en el Formulario Único Territorial y en las categorías contables del CHIP.
- **Enlace:** https://www.chip.gov.co/ — ✅ 200 (recomprobado la mañana del 5-sep-2026). ⚠️ **Entre por la raíz, no por la ruta directa de la aplicación:** `https://www.chip.gov.co/schip_rt/index.jsf` respondió ✅ 200 la noche del 4-sep pero **✗ 502 en tres intentos seguidos la mañana del 5-sep** — el servidor de la aplicación se cae a ratos. Si la raíz carga y el enlace interno no, espere y reintente; no es que la fuente haya desaparecido.
- **Cómo se baja:** **formulario de consulta** (entidad, período, categoría) con exportación a Excel/CSV. La interfaz es vieja pero funciona sin registro para lo público.
- **Grano y cobertura:** entidad territorial × período (trimestral y anual) · desde comienzos de los 2000, con calidad creciente.
- **⚠️ La trampa que trae:** **cero no es lo mismo que faltante, y aquí se confunden.** Un municipio que no reportó un trimestre sale con casillas vacías o en cero; si usted suma por año le queda un municipio «sin inversión» que en realidad no reportó. Antes de cualquier serie fiscal, cuente **cuántas entidades reportaron cada período**. Y el FUT cambió de estructura de categorías con los años: los rubros no se llaman igual en 2012 que en 2024.

### 4.3 Portal de Transparencia Económica (PTE)
- **Qué hay:** ejecución presupuestal del Presupuesto General de la Nación por entidad, rubro y proyecto de inversión.
- **Enlace:** https://www.pte.gov.co/ — ✅ 200 *(la ruta `.../WebsitePTE/` que aparece en documentos viejos da **✗ 404**: no la use)*
- **Cómo se baja:** consulta en línea con exportación; sin registro para lo público.
- **Grano y cobertura:** entidad nacional × rubro × vigencia · mensual dentro de la vigencia.
- **⚠️ La trampa que trae:** **es el presupuesto nacional, no el gasto territorial** — lo que gasta una alcaldía no está aquí, está en el CHIP. Y adentro hay tres cifras que no son la misma y que se confunden a diario: **apropiación** (lo asignado), **compromiso** (lo contratado) y **obligación/pago** (lo girado). Una serie que mezcle compromiso de un año con pago de otro no significa nada.

### 4.4 DIVIPOLA y el Marco Geoestadístico Nacional (DANE)
- **Qué hay:** la codificación oficial de departamentos y municipios (`gdxc-w37w`, con longitud y latitud) y las capas geográficas del MGN para hacer mapas.
- **Enlace (códigos):** https://www.datos.gov.co/d/gdxc-w37w — ⚠️ 200, redirige a `.../Mapas-Nacionales/DIVIPOLA-C-digos-municipios/gdxc-w37w` · **corte 30-dic-2024** · **Enlace (capas):** https://geoportal.dane.gov.co/servicios/descarga-y-metadatos/datos-geoestadisticos/ — ✅ 200 *(la ruta `.../descarga-mgn-marco-geoestadistico-nacional/` que circula en foros da **✗ 404**)*
- **Cómo se baja:** CSV directo o API de Socrata para los códigos — probado el 4-sep-2026: `https://www.datos.gov.co/resource/gdxc-w37w.csv?$limit=1200` devuelve las siete columnas; descarga directa de shapefile/geopackage en el Geoportal.
- **Grano y cobertura:** municipio · vigencia del corte.
- **⚠️ La trampa que trae:** **el código municipal es texto con cero a la izquierda.** En el archivo real, Antioquia es `"05"` y Medellín `"05001"`; si Excel o `pandas` lo leen como número se vuelven `5` y `5001`, y **el cruce con cualquier otra base falla en silencio** — no da error, simplemente no empata y usted pierde departamentos enteros. Lea siempre esas columnas como `str`. Y no cruce por nombre: hay municipios homónimos en departamentos distintos y las tildes son inconsistentes entre fuentes.

### 4.5 IDEAM — DHIME (datos hidrometeorológicos)
- **Qué hay:** series de precipitación, temperatura, caudal y nivel de las estaciones de la red nacional.
- **Enlace:** https://dhime.ideam.gov.co/atencionciudadano/ — ⚠️ 200 en la primera comprobación del 4-sep-2026 y **tiempo de espera agotado en una segunda petición esa misma noche**: el servidor es lento e intermitente. Si no carga, reintente; no está caído.
- **Cómo se baja:** consulta por estación, variable y período, con exportación a CSV/Excel; sin registro.
- **Grano y cobertura:** estación × día (algunas variables, horaria) · desde mediados del siglo XX en las estaciones más antiguas.
- **⚠️ La trampa que trae:** **las estaciones abren y cierran.** Una serie municipal de precipitación que «cae» a partir de cierto año casi siempre es una estación que dejó de reportar, no una sequía. Antes de graficar el promedio de un departamento, grafique **el número de estaciones activas por año**: si esa línea se mueve, la otra no es interpretable. Es la misma trampa de SECOP con otra ropa.

---

## 5 · Precios y macro

### 5.1 Banco de la República — Suameca (estadísticas económicas)
- **Qué hay:** el catálogo de series macro del Emisor: TRM, tasas de interés, agregados monetarios, balanza de pagos, IPC, deuda, PIB.
- **Enlace:** https://suameca.banrep.gov.co/estadisticas-economicas/#/home — ✅ 200. El antiguo https://www.banrep.gov.co/es/estadisticas **redirige aquí** (⚠️ 200 con redirección de host).
- **Cómo se baja:** buscador de series con **descarga directa** en Excel/CSV, escogiendo frecuencia y rango de fechas. Sin registro.
- **Grano y cobertura:** serie de tiempo nacional · diaria, mensual, trimestral o anual según la serie; algunas arrancan en los años cincuenta.
- **⚠️ La trampa que trae — dos capas:**
  1. **La frecuencia es una decisión de muestra.** Casi todo está en diario, mensual, trimestral y anual a la vez, y **el promedio anual de una serie diaria no es el dato de diciembre**. Cuál se escogió va escrito en `FUENTES.md`.
  2. **Las series se revisan hacia atrás y se reexpresan.** La misma consulta el mes entrante puede devolver otro número para 2023 — por eso la fecha de descarga. Y el caso que más daño hace es el PIB real: **el DANE cambió el año base de las cuentas nacionales**, así que una serie armada pegando publicaciones de vigencias distintas mezcla dos bases y produce crecimientos que no existieron. Baje la serie completa de una sola descarga, nunca por pedazos de años distintos. *(El año base exacto: `[VERIFICAR]` en la ficha metodológica de la serie antes de escribirlo en el documento.)*
- **Advertencia de acceso:** algunas rutas de `www.banrep.gov.co` responden con un **filtro antibot** (redirigen a `validate.perfdrive.com`) cuando se llaman por script, y la ruta de la serie histórica de la TRM que circula en apuntes viejos (`/es/estadisticas/serie-historica-tasa-cambio-representativa-mercado-trm`) dio **✗ 404** en la comprobación de la madrugada del 5-sep. Entre por Suameca y busque la serie ahí.

### 5.2 DANE — Índice de Precios al Consumidor (IPC)
- **Qué hay:** IPC total, por ciudad, por división de gasto y por nivel de ingreso; índices, variaciones y ponderaciones, en anexos de Excel.
- **Enlace:** https://www.dane.gov.co/index.php/estadisticas-por-tema/precios-y-costos/indice-de-precios-al-consumidor-ipc — ✅ 200
- **Cómo se baja:** **descarga directa** de los anexos `.xlsx` del boletín mensual. Sin registro.
- **Grano y cobertura:** nacional y por ciudad · mensual, con histórico largo.
- **⚠️ La trampa que trae:** **el IPC cambió de base y de canasta.** La serie vigente está en base **diciembre 2018 = 100** y su canasta se construyó con la ENPH 2016–2017, con divisiones de gasto distintas a las del esquema anterior. En la práctica: **las variaciones sí se encadenan; los índices de bases distintas, no** — hay que reescalar. Y deflactar una serie de pesos con el índice equivocado le mueve todos los resultados sin dar ningún error.

---

## 6 · Salud y social

### 6.1 SISPRO — MinSalud (cubos y bodega de datos)
- **Qué hay:** afiliación al sistema de salud (BDUA), prestación de servicios (RIPS), talento humano, oferta de prestadores (REPS) y estadísticas vitales integradas.
- **Enlace:** https://www.sispro.gov.co/Pages/Home.aspx — ⚠️ 200, pero **respondió lento y agotó el tiempo de espera en una de tres peticiones** la noche del 4-sep: el servidor es intermitente. Si no carga, reintente.
- **Cómo se baja:** los cubos públicos se consultan en línea y exportan a Excel; los de más detalle **requieren registro** de usuario.
- **Grano y cobertura:** municipio, prestador y atención · mensual y anual, desde mediados de los 2000.
- **⚠️ La trampa que trae:** **los RIPS cuentan atenciones, no personas.** Un paciente con cinco consultas produce cinco registros: una tasa de «morbilidad» construida sobre filas mide **frecuencia de uso del servicio**, no prevalencia de enfermedad. Y la cobertura depende de que el prestador reporte, así que un municipio con «poca enfermedad» puede ser un municipio con poco reporte — mire cuántos prestadores reportaron en cada uno antes de comparar. Lo mismo aplica a las estadísticas vitales que integra: el año más reciente es **preliminar y se revisa al alza** durante los dos años siguientes, y hay que decidir —y dejarlo escrito— si se cuenta por **lugar de ocurrencia** o por **lugar de residencia**.

### 6.2 Sinergia (DNP) — seguimiento a metas de política pública
- **Qué hay:** metas e indicadores del Plan Nacional de Desarrollo con su avance; útil para preguntas de evaluación de política.
- **Enlace:** https://sinergia.dnp.gov.co/ — ✅ 200
- **Cómo se baja:** consulta en el portal con exportación; sin registro para lo público.
- **Grano y cobertura:** indicador × año (y territorio, cuando el indicador lo tiene) · por cuatrienio de gobierno.
- **⚠️ La trampa que trae:** **las metas y los indicadores se redefinen con cada Plan Nacional de Desarrollo.** Un indicador con el mismo nombre en dos cuatrienios distintos puede tener otra fórmula, otra línea base y otro universo, así que **la serie no cruza los gobiernos**. Y el «avance» es reporte de la entidad ejecutora, no medición independiente: es un dato sobre lo que se reportó, no necesariamente sobre lo que pasó.

---

## Antes de bajar cualquier cosa · las tres preguntas

Se hacen **antes** de graficar, no después, y la respuesta —**aunque sea «nada»**— va en el campo
*decisión de muestra* de `FUENTES.md`, escrita con un número y no con un adjetivo:

- [ ] **¿Cambió la escala?** ¿La variable se mide hoy con la misma regla que al principio de la serie? *(Saber 11 en 2014-2 es el ejemplo del día.)*
- [ ] **¿Cambió la cobertura?** ¿Hay las mismas unidades reportando todos los años? Cuente cuántas hay por año. *(SECOP II es el ejemplo del día; el IDEAM, el mismo caso con estaciones.)*
- [ ] **¿Hay dos poblaciones adentro?** ¿La columna que estoy promediando mezcla grupos de tamaños muy distintos? Cuente las observaciones de cada celda antes de promediar. *(Los dos semestres de Saber 11.)*

---

## Tabla resumen · verificación de enlaces

*Comprobado con petición HTTP la noche del 4-sep-2026 (10:15 p.m. – madrugada del 5).*

| # | Fuente | Enlace | Estado |
|---|---|---|---|
| 1.1 | datos.gov.co (catálogo) | `datos.gov.co/browse` | ✅ 200 |
| 1.2 | SECOP II · Contratos Electrónicos | `datos.gov.co/d/jbjy-vk9h` | ⚠️ 200 · redirige a la ficha `Estad-sticas-Nacionales/...` |
| 1.3 | SECOP I · Procesos de Compra Pública | `datos.gov.co/d/f789-7hwg` | ⚠️ 200 · redirige a la ficha canónica |
| 2.1 | Saber 11 · Resultados únicos | `datos.gov.co/d/kgxf-xxbe` | ⚠️ 200 · redirige a `Educaci-n/...` · descarga CSV probada |
| 2.2 | DataIcfes | `icfes.gov.co/data-icfes/` | ⚠️ 200 · redirige a `/investigaciones/data-icfes/` |
| 2.3 | MEN · matrícula superior por municipio | `datos.gov.co/d/y9ga-zwzy` | ⚠️ 200 · redirige a la ficha canónica |
| 2.4 | Observatorio Laboral (OLE) | `ole.mineducacion.gov.co` | ⚠️ 200 · redirige a `/portal/` |
| 3.1 | DANE · ANDA / microdatos (GEIH) | `microdatos.dane.gov.co/index.php/catalog` | ✅ 200 · fichas 853, 900 y `get-microdata` también ✅ |
| 3.2 | RUES | `rues.org.co` | ✅ 200 · consulta avanzada ✅ 200 |
| 4.1 | TerriData (DNP) | `terridata.dnp.gov.co/index-app.html#/descargas` | ✅ 200 |
| 4.2 | CHIP / FUT · Contaduría | `chip.gov.co` | ⚠️ raíz ✅ 200 · la ruta `/schip_rt/index.jsf` pasó de ✅ 200 (noche 4-sep) a **✗ 502** (mañana 5-sep) |
| 4.3 | Portal de Transparencia Económica | `pte.gov.co` | ✅ 200 |
| 4.4 | DIVIPOLA + MGN | `datos.gov.co/d/gdxc-w37w` · `geoportal.dane.gov.co/.../datos-geoestadisticos/` | ⚠️ 200 (redirige) · ✅ 200 |
| 4.5 | IDEAM · DHIME | `dhime.ideam.gov.co/atencionciudadano/` | ⚠️ 200 intermitente (una petición agotó el tiempo) |
| 5.1 | Banco de la República · Suameca | `suameca.banrep.gov.co/estadisticas-economicas/#/home` | ✅ 200 · `banrep.gov.co/es/estadisticas` ⚠️ redirige aquí |
| 5.2 | DANE · IPC | `dane.gov.co/.../indice-de-precios-al-consumidor-ipc` | ✅ 200 |
| 6.1 | SISPRO · MinSalud | `sispro.gov.co/Pages/Home.aspx` | ⚠️ 200 intermitente (una petición agotó el tiempo) |
| 6.2 | Sinergia (DNP) | `sinergia.dnp.gov.co` | ✅ 200 |

**Resumen: 18 fuentes · 8 ✅ · 10 ⚠️ · 0 ✗.** Ninguna ficha quedó con un enlace muerto.

> **Recomprobación de la mañana del 5-sep-2026, antes de clase:** se volvieron a llamar los dieciocho enlaces principales. **Diecisiete siguen respondiendo.** El único que cambió es el CHIP: la ruta interna `/schip_rt/index.jsf` pasó a **502** y la raíz `chip.gov.co` sigue en 200 — la ficha 4.2 ya quedó apuntando a la raíz. SISPRO volvió a comportarse como se describe: 200 en dos intentos, tiempo agotado en el tercero.
>
> *Esto es, además, el argumento de la sesión en vivo: un enlace verificado anoche puede no servir hoy. Por eso `FUENTES.md` pide la fecha de descarga y no la fecha de la lista.*

### Enlaces que se probaron, salieron ✗ y por eso NO están en la lista
*(quedan anotados para que nadie los vuelva a poner)*

| Enlace descartado | Qué pasó | Con qué se reemplazó |
|---|---|---|
| `www.sisben.gov.co` (y `sisben.gov.co`, `portal.sisben.gov.co`) | ✗ el dominio **no resuelve** desde ningún nombre probado | se sacó el Sisbén de la lista; para focalización social quedó SISPRO |
| `geoportal.dane.gov.co/servicios/descarga-y-metadatos/descarga-mgn-marco-geoestadistico-nacional/` | ✗ **404** | `.../descarga-y-metadatos/datos-geoestadisticos/` (✅ 200) |
| `www.pte.gov.co/WebsitePTE/` | ✗ **404** | `www.pte.gov.co` (✅ 200) |
| `www.banrep.gov.co/es/estadisticas/serie-historica-tasa-cambio-representativa-mercado-trm` | ✗ **404** en la madrugada del 5-sep | Suameca (✅ 200) |
| `www.banrep.gov.co/es/estadisticas/indice-precios-consumidor-ipc` | ⚠️ redirigió a un **filtro antibot** (`validate.perfdrive.com`) | Suameca y el anexo del DANE (✅ 200) |
| `microdatos.dane.gov.co/index.php/catalog/MICRODATOS/about` | ✗ **500** | `.../index.php/catalog` (✅ 200) |

---

## Lo que quedó marcado `[VERIFICAR]`

Una sola cosa, y no bloquea la clase:

- **5.1 · Banco de la República — el año base de las cuentas nacionales del PIB.** El hecho de que hubo cambio de base está fuera de discusión y la advertencia es válida tal como está escrita; **el año exacto no se confirmó contra fuente esta noche** y no se escribió un número inventado. Si va a citarlo en un documento, ábralo en la ficha metodológica de la serie.

---

*Hoja preparada la noche del 4-sep-2026 para la sesión 3. **Nota de procedencia:** esa noche
corrieron dos rutinas nocturnas en paralelo sobre esta misma carpeta y las dos escribieron un
archivo con este nombre. Esta versión es **la fusión de las dos**: conserva las cinco fichas y la
lista de tres preguntas de la corrida `noche-sesion3-4sep` (incluida su trampa de los nombres de
municipio en SECOP II y el 18,3 % sin municipio), y las amplía a dieciocho fuentes con el formato
de cinco campos y la tabla de verificación. Nada de la otra corrida se perdió.*
