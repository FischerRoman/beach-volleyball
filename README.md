# Beach Volejbal Liberecko – Přehled turnajů

Statický jednostránkový web zobrazující přehled beachvolejbalových (a příbuzných beach) turnajů na Liberecku v dané sezóně. Slouží jako komunitní rozcestník – návštěvník na první pohled vidí, který víkend se co hraje a kde se přihlásit.

## Účel a cílová skupina

- Rekreační a amatérští hráči beachvolejbalu v okolí Liberce
- Organizátoři, kteří chtějí svůj turnaj přidat do přehledu
- Mobilní uživatelé (primární use-case: rychlá konzultace na telefonu)

## Technický stack

- **Čistý HTML/CSS/JS** – žádné závislosti, žádný build systém
- Jeden soubor `index.html` – nasaditelný kdekoliv (GitHub Pages, Netlify, vlastní hosting)
- Google Fonts (Bebas Neue + DM Sans) načítány z CDN

## Struktura dat

Všechna data jsou definována přímo v JS v souboru `index.html`:

```js
// Organizátoři – objekt s id, názvem, CSS třídou a HTML šablonou detailu
const organizers = { beachKos: {...}, vrtule: {...}, strudl: {...} };

// Turnaje – pole objektů
const tournaments = [
  { date: 'YYYY-MM-DD', name: 'Název turnaje', type: 'mixy|zeny|muzi', org: 'klíč organizátora' },
  ...
];
```

Víkendy (So–Ne) se generují automaticky pro zadané měsíce – není třeba je ručně vypisovat.

## Aktuální turnaje (sezona 2026)

| Datum | Název | Kategorie | Organizátor |
|-------|-------|-----------|-------------|
| 30.5. | Memoriál Standy Kose | mixy | Beach KOS |
| 20.6. | Plážový volejbal | mixy | Beach KOS |
| 28.6. | Plážový volejbal | ženy | Beach KOS |
| 4.7.  | Plážový volejbal | mixy | Beach KOS |
| 1.8.  | Plážový volejbal | ženy | Beach KOS |
| 15.8. | Štrůdl Cup | mixy | Štrůdl Cup |
| 29.8. | Plážový volejbal | mixy | Beach KOS |
| 29.8. | Vrtule beach cup | mixy | Vrtule Frýdlant |

## Jak přidat nový turnaj

1. Přidej záznam do pole `tournaments` v sekci `// ─── DATA ───`.
2. Pokud jde o nového organizátora, přidej ho do objektu `organizers` se stejnou strukturou jako stávající záznamy.
3. Žádný build ani restart není potřeba – stačí uložit a obnovit stránku.

> ⚠️ **Důležité:** Datum musí odpovídat sobotě daného víkendu. Turnaje konající se v neděli nebo jiný den zadávej na datum předchozí soboty, aby se správně spárovaly s víkendem.

## Jak přidat novou kategorii (filtr)

1. Přidej tlačítko do `.filter-bar` s atributem `data-filter="nová-kategorie"` a voláním `setFilter(...)`.
2. Přidej odpovídající CSS třídu pro badge (`.badge-nová-kategorie`) a pro aktivní stav filtru (`.filter-btn.active.nová-kategorie`).
3. Přidej hodnotu do `type` u relevantních turnajů.

## Plánovaná rozšíření (backlog)

- [ ] Oddělení dat do externího `tournaments.json` – umožní editaci bez zásahu do HTML
- [ ] Jednoduchý admin formulář pro přidání turnaje (generuje JSON nebo PR)
- [ ] Podpora více míst konání (mapa / filtr podle lokality)
- [ ] Přidání odkazu na přihlášení / kapacita turnaje
- [ ] Export do Google Kalendáře (.ics)
- [ ] Tmavý režim
- [ ] Automatická aktualizace roku (sezona)

## Lokální vývoj

Server: **dotnet-serve** (nástroj pro .NET)

```bash
dotnet serve -p 5000 --open-browser
```

Stránka bude dostupná na http://localhost:5000.

> Pokud `dotnet-serve` není nainstalován: `dotnet tool install -g dotnet-serve`

## Konvence

- Datum vždy ve formátu `YYYY-MM-DD` (ISO 8601)
- Datum musí být sobota – web páruje turnaje podle sobot
- Typ kategorie vždy malými písmeny: `mixy`, `zeny`, `muzi`
- Klíč organizátora v camelCase: `beachKos`, `vrtule`, `strudl`
- HTML info panel organizátora je čistý HTML string – lze použít `<a>`, `<strong>` apod.
