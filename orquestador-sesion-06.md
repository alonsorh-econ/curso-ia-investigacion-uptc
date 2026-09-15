# Instalar el orquestador del sistema
**Sesión 6 · El engranaje** · Curso de IA aplicada a la investigación · CENES–UPTC

Un orquestador es un asistente que **lee todas sus fuentes a la vez** —su carpeta del sistema, su correo, su calendario y sus reuniones— y le dice qué toca su investigación, qué plazos vienen y qué hacer esta semana. Hay dos niveles: el primero se instala en quince minutos con Claude gratuito; el segundo es la versión por API, la misma arquitectura del sistema del profesor en pequeño.

---

## Nivel 1 · El orquestador con conectores (gratis, 15 minutos)

**Lo que necesita:** una cuenta personal de Google, Claude en plan gratuito y, si graba reuniones, Granola (el plan Basic sirve).

### Paso 1 · La carpeta del sistema en Drive
Cree en su Google Drive la carpeta `[Nombre] System` con las seis casillas. Las casillas van como **documentos de Google**, para que Claude las pueda leer.

### Paso 2 · Conecte Google
En claude.ai: **Personalizar → Conectores** (o el botón **+** del chat). Active **Google Drive, Gmail y Google Calendar** y autorice con su cuenta personal. Estos conectores están incluidos en el plan gratuito, y por defecto Claude pide aprobación antes de enviar un correo o modificar su Drive.

> Si su cuenta es de la universidad y no deja conectar, use su cuenta personal de Google.

### Paso 3 · Conecte Granola por MCP
En **claude.ai/customize/connectors** → **Agregar conector personalizado** → pegue esta dirección y autorice en la ventana de Granola. No hay llaves que copiar.

```
https://mcp.granola.ai/mcp
```

> El plan gratuito de Claude permite **un** conector personalizado: úselo para Granola. Con el plan Basic de Granola, el orquestador ve las notas de los últimos 30 días.

### Paso 4 · Instale el orquestador
En su carpeta `00_contexto`, cree un documento de Google llamado **Instrucciones del orquestador** y pegue adentro el prompt de abajo.

### Paso 5 · Úselo
Cada vez que se siente a trabajar, abra un chat nuevo y escriba:

```
Busca en mi Drive el documento «Instrucciones del orquestador» y ejecútalo.
```

---

## El prompt del orquestador

```
Eres el orquestador de mi sistema de investigación. Trabajas con estas fuentes:

1. Mi carpeta «[NOMBRE] System» en Google Drive: las seis casillas
2. Mi Gmail
3. Mi Google Calendar
4. Mis reuniones de Granola
5. Si existe, el documento más reciente llamado «Bandeja del sistema» en mi Drive

Cada vez que te lo pida, haz esto en orden:

PASO 1 · Ponte al día. Lee mi pregunta de investigación en 01_ideas y lo que
cambió desde la última vez en mi BITÁCORA. Si hay una Bandeja del sistema con
más de tres días, avísame primero.

PASO 2 · Cruza las fuentes. Revisa los correos de los últimos 7 días, los eventos
de los próximos 14 días y las reuniones de Granola de la última semana. Para cada
cosa, dime si toca mi investigación y qué casilla alimenta. Lo que no la toque,
ignóralo.

PASO 3 · Detecta. Plazos que vienen, compromisos que dije en reuniones, datos o
respuestas que estoy esperando de alguien, y contradicciones entre lo que dicen
las fuentes y lo que está en mis casillas.

PASO 4 · Propón. Máximo tres tareas para esta semana, en orden, cada una con la
casilla que alimenta.

PASO 5 · Prepara, no ejecutes. Si una tarea exige escribirle a alguien, redacta
el borrador en Gmail y NO lo envíes. Si exige un evento, muéstramelo antes de
crearlo. Al final, guarda en 05_salidas un documento «Orquestador [fecha]» con
el resumen de hoy.

Reglas:
- No inventes nada que no esté en las fuentes. Si algo no está claro, escribe [VERIFICAR].
- Cita de dónde sale cada cosa: correo, evento, reunión o casilla.
- Nunca envíes un correo, nunca borres nada y nunca me pidas una contraseña.

Responde con este formato:
## Lo que cambió
## Lo que toca mi investigación (tabla: fuente · qué dice · casilla)
## Plazos y compromisos
## Las tres tareas de la semana
## Borradores preparados
```

---

## Nivel 2 · La versión por API (Google Cloud Console + Colab)

Esta es la arquitectura del sistema del profesor en pequeño: **un recolector** que trae los datos por API y los deja en un documento, y **un orquestador** que lo lee. Sirve para entender cómo funcionan las API y para automatizar lo que los conectores no hacen.

### Paso 1 · El proyecto
1. Entre a **console.cloud.google.com** con su cuenta personal → **Seleccionar proyecto → Proyecto nuevo** → nombre: `Sistema de investigación` → **Crear**.
2. **APIs y servicios → Biblioteca** → busque y **habilite**, una por una: **Gmail API**, **Google Calendar API** y **Google Drive API**.

### Paso 2 · La pantalla de consentimiento
3. **Google Auth Platform** (antes «pantalla de consentimiento de OAuth») → **Comenzar**.
4. **Branding:** el nombre de la app y su correo de soporte.
5. **Audience:** tipo **Externo**. En **Usuarios de prueba**, agregue **su propio correo**.
6. **Data access:** agregue `gmail.readonly`, `calendar.readonly` y `drive.file`.

### Paso 3 · Las credenciales
7. **Clients → Crear cliente** → tipo **App de escritorio** → **Crear** → **Descargar JSON**. Renómbrelo `credentials.json`.

> ⚠️ Mientras la app esté en modo **Prueba**, la autorización **vence a los 7 días**: vuelva a correr la celda A cada semana. Nunca pegue `credentials.json` ni una llave en un chat.

### Paso 4 · El recolector en Colab
Abra **colab.research.google.com → Nuevo cuaderno** y cree dos celdas de código.

**Celda A · autorizar** (una vez por sesión de Colab). Al final, Google lo lleva a una página `http://localhost` que **no carga**: es normal. Copie esa dirección completa y péguela en el recuadro de la celda.

```python
# ═══ CELDA A · Autorizar (una vez por sesión de Colab) ═══
import json, os
from urllib.parse import urlparse, parse_qs
from google.colab import files
from google_auth_oauthlib.flow import Flow

os.environ["OAUTHLIB_RELAX_TOKEN_SCOPE"] = "1"
PERMISOS = [
    "https://www.googleapis.com/auth/gmail.readonly",     # leer correo, nunca enviar
    "https://www.googleapis.com/auth/calendar.readonly",  # leer calendario
    "https://www.googleapis.com/auth/drive.file",         # solo los archivos que crea este cuaderno
]

print("Suba el archivo credentials.json que descargó de Google Cloud Console:")
subido = files.upload()
config = json.loads(next(iter(subido.values())).decode("utf-8"))

flujo = Flow.from_client_config(config, scopes=PERMISOS, redirect_uri="http://localhost")
enlace, _ = flujo.authorization_url(prompt="consent", access_type="offline")
print("\n1) Abra este enlace y autorice con su cuenta personal de Google:\n")
print(enlace)
print("\n2) Si sale «Google no verificó esta app»: Configuración avanzada → Ir a la app → Continuar.")
print("3) Al final el navegador muestra una página que NO carga. Es normal.")
respuesta = input("4) Copie la dirección completa de esa página (empieza por http://localhost) y péguela aquí: ").strip()
codigo = parse_qs(urlparse(respuesta).query).get("code", [None])[0]
if not codigo:
    raise SystemExit("No encontré el código en esa dirección. Copie la dirección COMPLETA de la barra del navegador.")
flujo.fetch_token(code=codigo)
CREDENCIALES = flujo.credentials
print("\n✅ Autorizado. Ahora corra la celda B.")
```

**Celda B · recoger y escribir la Bandeja** (cuantas veces quiera)

```python
# ═══ CELDA B · Recoger y escribir la Bandeja (se puede correr cuantas veces quiera) ═══
import io, json, time, urllib.request, urllib.parse
from datetime import datetime, timedelta, timezone
from getpass import getpass
from googleapiclient.discovery import build
from googleapiclient.http import MediaIoBaseUpload

DIAS_CORREO, DIAS_AGENDA, DIAS_GRANOLA = 7, 14, 7
BUSQUEDA_CORREO = f"newer_than:{DIAS_CORREO}d -category:promotions -category:social"

def recoger_correos(creds, maximo=25):
    gmail = build("gmail", "v1", credentials=creds, cache_discovery=False)
    lista = gmail.users().messages().list(userId="me", q=BUSQUEDA_CORREO, maxResults=maximo).execute()
    salida = []
    for m in lista.get("messages", []):
        d = gmail.users().messages().get(userId="me", id=m["id"], format="metadata",
                                         metadataHeaders=["From", "Subject", "Date"]).execute()
        cab = {h["name"]: h["value"] for h in d.get("payload", {}).get("headers", [])}
        salida.append({"de": cab.get("From", "?"), "asunto": cab.get("Subject", "(sin asunto)"),
                       "fecha": cab.get("Date", ""), "extracto": d.get("snippet", "")})
    return salida

def recoger_eventos(creds, maximo=30):
    cal = build("calendar", "v3", credentials=creds, cache_discovery=False)
    ahora = datetime.now(timezone.utc)
    r = cal.events().list(calendarId="primary", timeMin=ahora.isoformat(),
                          timeMax=(ahora + timedelta(days=DIAS_AGENDA)).isoformat(),
                          singleEvents=True, orderBy="startTime", maxResults=maximo).execute()
    return [{"inicio": (e.get("start") or {}).get("dateTime") or (e.get("start") or {}).get("date", ""),
             "titulo": e.get("summary", "(sin título)"), "lugar": e.get("location", "")}
            for e in r.get("items", [])]

def recoger_granola(llave, maximo=10):
    if not llave:
        return []
    def pedir(ruta, params=None):
        url = "https://public-api.granola.ai/v1" + ruta + ("?" + urllib.parse.urlencode(params) if params else "")
        req = urllib.request.Request(url, headers={"Authorization": f"Bearer {llave}", "Accept": "application/json"})
        with urllib.request.urlopen(req, timeout=45) as r:
            return json.loads(r.read().decode("utf-8"))
    limite = datetime.now(timezone.utc) - timedelta(days=DIAS_GRANOLA)
    salida = []
    for n in pedir("/notes", {"limit": maximo}).get("notes", []):
        creada = n.get("created_at") or ""
        try:
            if datetime.fromisoformat(creada.replace("Z", "+00:00")) < limite:
                continue
        except ValueError:
            pass
        nota = pedir(f"/notes/{n['id']}")
        resumen = (nota.get("summary_markdown") or nota.get("summary_text") or "").strip()
        salida.append({"titulo": nota.get("title") or "(sin título)", "fecha": creada[:10], "resumen": resumen[:2000]})
        time.sleep(0.25)
    return salida

def armar_bandeja(correos, eventos, reuniones, hoy):
    p = [f"BANDEJA DEL SISTEMA — {hoy}",
         "Generada por el recolector de Colab. El orquestador la lee junto con las seis casillas.", ""]
    p += [f"1. CORREOS · últimos {DIAS_CORREO} días ({len(correos)})", ""]
    p += [f"- {c['fecha'][:16]} · {c['de']} · {c['asunto']}\n  {c['extracto']}" for c in correos] or ["(sin correos)"]
    p += ["", f"2. CALENDARIO · próximos {DIAS_AGENDA} días ({len(eventos)})", ""]
    p += [f"- {e['inicio'][:16].replace('T', ' ')} · {e['titulo']}" + (f" · {e['lugar']}" if e["lugar"] else "")
          for e in eventos] or ["(sin eventos)"]
    p += ["", f"3. REUNIONES DE GRANOLA · últimos {DIAS_GRANOLA} días ({len(reuniones)})", ""]
    for r in reuniones:
        p += [f"## {r['titulo']} · {r['fecha']}", r["resumen"] or "(sin resumen)", ""]
    if not reuniones:
        p += ["(sin reuniones: no hay llave de Granola o no hubo reuniones)"]
    return "\n".join(p)

def guardar_en_drive(creds, texto, hoy):
    drive = build("drive", "v3", credentials=creds, cache_discovery=False)
    media = MediaIoBaseUpload(io.BytesIO(texto.encode("utf-8")), mimetype="text/plain", resumable=False)
    doc = drive.files().create(body={"name": f"Bandeja del sistema {hoy}",
                                     "mimeType": "application/vnd.google-apps.document"},
                               media_body=media, fields="id, webViewLink").execute()
    return doc["webViewLink"]

llave_granola = getpass("Llave de Granola (grn_…). Si no tiene plan Business, deje vacío y presione Enter: ").strip()
hoy = datetime.now().strftime("%Y-%m-%d")
correos = recoger_correos(CREDENCIALES)
eventos = recoger_eventos(CREDENCIALES)
reuniones = recoger_granola(llave_granola)
texto = armar_bandeja(correos, eventos, reuniones, hoy)
enlace = guardar_en_drive(CREDENCIALES, texto, hoy)
print(f"✅ {len(correos)} correos · {len(eventos)} eventos · {len(reuniones)} reuniones de Granola")
print("Bandeja guardada en su Drive:", enlace)
```

Al correr la celda B queda en su Drive un documento **«Bandeja del sistema [fecha]»**. El orquestador del nivel 1 ya lo busca: no hay que cambiarle nada.

> **Granola por API** necesita plan Business (llave `grn_…`). Si no lo tiene, deje la llave vacía: Granola ya entra por el MCP del nivel 1.

### ¿Qué IA le ayuda a modificarlo?
- **Gemini en Colab** (gratis): escribe *y corre* el código dentro del mismo cuaderno. Pídale los cambios en español.
- **Claude gratuito:** escribe el código, pero no lo corre. Su papel es el de orquestador.
- **Claude Pro con Claude Code** (unos USD 20 al mes): lo corre en su computador y lee y escribe la carpeta del sistema. Es lo más parecido al sistema del profesor.

---

## Las tres reglas
1. **Nunca una contraseña ni una llave en el chat.** Los conectores entran por la pantalla de Google o de Granola.
2. **Nada sale de su correo sin que usted lo lea.** El orquestador redacta; usted envía.
3. **Dele solo los permisos que necesita.** El recolector solo lee correo y calendario, y solo escribe los documentos que él mismo crea.
