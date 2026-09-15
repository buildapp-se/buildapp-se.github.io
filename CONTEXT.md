# Project context

## Product intent

Katalogsida som länkar vidare till de fristående småprojekten. Domänen är inte en
produkt, den är huset, och varje undersida är en dörr.

## Architecture

- Allt ligger i `index.html`: inline CSS plus en liten vanilla JS-snutt för
  språktoggeln. Inget byggsteg, inga beroenden, samma mönster som grammat, sipdeck
  och ai.
- Org root-repo för `buildapp-se`, GitHub Pages från branch `main`, rot.
- `favicon.svg` är självbärande och byter färg via `prefers-color-scheme` inbyggt i
  SVG:n. `assets/icon-32.png` är fallback för äldre webbläsare och `icon-192.png`
  är apple-touch-icon, eftersom iOS inte stödjer SVG-favicons.
- `assets/ai-mark.png` och `assets/valheim-mark.svg` är självhostade kopior, eftersom
  AI-appen och Valheim saknar egen ikonfil på sin domän (Valheims ikon finns bara
  som data-URI i dess HTML). Övriga appars ikoner refereras direkt via `buildapp.se`.

## Constraints

- ⚠️ `CNAME` är GitHub-genererad från Pages-inställningen för custom domain. Rör den
  aldrig manuellt.
- Beskrivningen av en app måste vara sanningsenlig. Påstå aldrig funktioner som
  inloggning eller sparande om appen inte har dem; det har blivit fel en gång.
- GitHub Pages ignorerar `_headers`, så säkerhetsheaders måste sättas i Cloudflare.

## Important decisions

- Designkonceptet är arkitektonisk skyltning och adressplakett, inte en
  mjukvarustartup.
- Varje dörr använder appens **egen** accentfärg och **riktiga** ikon, aldrig en ny
  illustration och aldrig en skärmdump.
- Loggan och faviconen använder endast `--ink` och `--wall`, aldrig en apps
  accentfärg, så att varumärket förblir neutralt.
- Ingen build-time-i18n. Varje översatt textnod har `data-sv` och `data-en`.
- Ordningen på dörrarna är nyast repo överst, med undantag för Valheim Food Planner som ligger sist (Patriks val 2026-09-15).
- Varje dörr visar vilka språk appen finns på som chips under sluggen (`.door-langs`, SV och/eller EN). Chipsen följer appen, inte katalogsidans språkval, så kontrollera dem när en app får eller tappar ett språk.

## Designtokens

CSS custom properties i `:root`, ljust och mörkt via `prefers-color-scheme`.

- `--wall`, `--wall-2`, `--surface`: betong och kalksten. Ljust `#DFDDD5`,
  `#D2CFC5`, `#EAE8E0`. Mörkt `#18181A`, `#222224`, `#201F1F`.
- `--ink`, `--ink-2`: text. Ljust `#211F1A` och `#6E6A5D`. Mörkt `#EDEAE2` och `#9C978A`.
- Typsnitt: **Big Shoulders Display** för rubriker och wordmark, kondenserad
  skyltkänsla. **IBM Plex Sans** för brödtext. **IBM Plex Mono** för url-slugs och
  språktoggel.

Varje `.door` sätter `--accent` inline med appens egen färg. En ljus accent som inte når 4,5:1 mot `--wall` sätter även `--tag` (accenten blandad med `--ink`) för taglinen, som Valheim. `.knob` är en liten
accentprick i ikonrutans hörn, byggd på mönstret som redan fanns organiskt i tre av
fyra appikoner.

## Environments and operations

`git push` till `main`, GitHub Pages bygger automatiskt på ungefär trettio sekunder.
Ingen backend, inga hemligheter.

## Audits

Read by the cockpit Audits tab. One `- Label: YYYY-MM-DD, result` per check; conventions in elwyn-dash `docs/security.md`.

- Headers: 2026-08-04, fail, 0 of 6 on buildapp.se (GitHub Pages)
- Search Console: 2026-09-15, warn, only / indexed, no sitemap submitted, subpages unknown to Google
