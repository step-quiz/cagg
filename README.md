# Gestor d'imatges — #MathsToday

Eina per catalogar i triar les imatges recollides del compte de Bluesky
[@catrionaagg.bsky.social](https://bsky.app/profile/catrionaagg.bsky.social)
(hashtag `#MathsToday`): **317 imatges** repartides en 156 de les 187 publicacions
descarregades.

Igual que a la resta de projectes del departament, és **HTML + CSS + JavaScript
pur (vanilla)**, sense framework, sense `npm` i **sense pas de compilació**: un
sol fitxer `.html` autocontingut.

> No confondre amb `index.html` (el visor cronològic de la publicació original,
> només lectura). `gestiona-imatges.html` és l'eina de **triatge**: hi pots
> cercar, filtrar, etiquetar, comentar i seleccionar imatges per treure'n un
> subconjunt reutilitzable.

## Què fa

- **Graella de les 317 imatges**, cadascuna amb el text de la publicació d'origen,
  la data, els "m'agrada" i la descripció (`alt`) que en va escriure l'autora.
- **Cerca lliure** (sense accents) que busca alhora en el text de la publicació i
  en la descripció de la imatge.
- **Filtres combinables**: per estat (Pendent / Guardada / Descartada) i per
  etiqueta; ordenació per data o per "m'agrada".
- **Fitxa de detall** (a pantalla completa, amb navegació anterior/següent i
  dreceres de teclat ← → Esc) on es pot:
  - canviar l'estat de la imatge,
  - afegir-hi o treure-hi etiquetes lliures (no hi ha un vocabulari tancat: la
    llista de filtres es genera sola a mesura que s'etiqueta),
  - escriure-hi una nota,
  - marcar-la com a inclosa a la selecció,
  - obrir la publicació original a Bluesky.
- **Accions massives**: seleccionar diverses imatges des de la graella (casella a
  cada targeta) i aplicar-los una etiqueta o un estat de cop, o buidar la
  selecció.
- **Desat automàtic** al navegador (`localStorage`): els canvis no es perden en
  recarregar la pàgina.
- **Exportació**:
  - *Baixa catàleg* — descarrega un `.json` amb les 317 imatges i tot el que
    se'ls hagi afegit (etiquetes, nota, estat, selecció). Serveix també per fer
    còpia de seguretat o per portar la feina a un altre ordinador.
  - *Baixa .zip* (des de la barra d'accions massives) — descarrega només les
    imatges seleccionades, amb els seus fitxers originals dins `media/` i un
    `seleccio.json` amb les metadades corresponents.
- **Importa catàleg** — recarrega un `.json` exportat prèviament per continuar
  on ho vas deixar (útil si es neteja l'emmagatzematge del navegador).

Cap d'aquestes accions modifica els fitxers originals: `publicacions.json` i les
imatges de `media/` es tracten sempre com a **només lectura**; tot el que s'hi
afegeix viu en una capa a part.

## Fitxers

| Fitxer / carpeta | Contingut |
|---|---|
| `gestiona-imatges.html` | L'eina de gestió descrita en aquest document. |
| `index.html` | Visor cronològic original de la publicació (només lectura, no el toca aquesta eina). |
| `publicacions.json` | Font de dades: les 187 publicacions descarregades, amb el text, les estadístiques i la referència a les imatges de cada una. |
| `media/` | Els 317 `.jpg` originals, anomenats pel seu identificador de contingut (CID) de Bluesky. |
| `lib/jszip.min.js` | Llibreria `jszip` servida en local, només per generar la descàrrega en `.zip` de la selecció. |

## Com s'executa

Com que l'eina llegeix `publicacions.json` amb `fetch()`, **cal servir la carpeta
per HTTP(S)**; no funciona obrint el fitxer directament amb `file://`.

En local, des d'un terminal dins la carpeta:

```bash
python3 -m http.server 8000
```

i després obrir `http://localhost:8000/gestiona-imatges.html`.

Publicat a GitHub Pages / Cloudflare Pages / qualsevol allotjament estàtic
funciona igual, sense cap configuració addicional.

## Model de dades

Cada imatge es guarda, dins `localStorage`, amb aquesta forma:

```json
{
  "bafkrei....jpg": {
    "tags": ["algebra", "y10"],
    "note": "",
    "status": "pending",
    "selected": false
  }
}
```

- `status` és `pending`, `kept` o `discarded`.
- `tags` és una llista lliure (sense vocabulari predefinit).
- El `.json` que es descarrega amb *Baixa catàleg* combina aquesta capa amb les
  dades originals de cada imatge (text, data, "m'agrada", enllaç a l'original),
  de manera que és autosuficient: es pot obrir sense necessitat de tornar a
  `publicacions.json`.

## Límits coneguts

- No hi ha edició d'imatges (retallar, rotar, etc.), ni crides a cap servei
  d'IA: és només una eina de cerca, etiquetatge i selecció.
- El desat és local al navegador: si es neteja l'emmagatzematge del navegador
  sense haver fet *Baixa catàleg* abans, es perd la feina d'etiquetatge.
- No hi ha sincronització entre dispositius ni usuaris; és una eina d'un sol
  usuari, pensada per treballar-hi localment.
