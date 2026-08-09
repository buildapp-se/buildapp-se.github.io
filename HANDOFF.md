---
schemaVersion: 1
status: active
currentGoal: Hålla katalogsidan buildapp.se korrekt när projekten bakom dörrarna ändras
nextAction: Ta fram ett eget grafiskt paket per undersida, i första hand og:image för länkförhandsvisningar i Messenger, Facebook och Slack
blockers: []
reviewedAt: 2026-08-09
---

# Handoff: buildapp.se

## Läget

Live på `https://buildapp.se/`. Sidan är en katalog som länkar vidare till de
fristående småprojekten. Domänen är inte en produkt, den är huset.

Designkoncept, tokens och regler för att lägga till en dörr står i `PROJECT.md`.

## Recent work

- Katalogsidan byggd med arkitektonisk skyltning som koncept: varje undersida är en
  dörr i sin egen färg nerför en betongkorridor.
- Egen favicon som byter färg via `prefers-color-scheme` inbyggt i SVG:n, med
  PNG-fallback för äldre webbläsare och apple-touch-icon för iOS.
- Självhostad kopia av AI-appens märke under `assets/`, eftersom den appen inte har
  någon egen ikon på sin egen domän.
- Språktoggel med `data-sv` och `data-en`, val sparas i localStorage.

## Verification

- Sidan svarar 200 på `https://buildapp.se/`.
- Ikonerna för sipdeck, grammat och tidslinje refereras direkt från deras egna
  sökvägar under `buildapp.se`, alltså ingen dubblering.

## Unresolved details

- Inget eget grafiskt paket per undersida. Det saknas `og:image` för
  länkförhandsvisningar, och undersidorna lånar `buildapp.se`:s touch-ikoner.
- Säkerhetsheaders saknas, eftersom GitHub Pages ignorerar `_headers`. Löses med en
  Transform Rule i Cloudflare tillsammans med de andra Pages-sajterna. Se
  säkerhetsrepot.

## Regler att följa när en dörr läggs till

1. Hämta appens **egen** accentfärg och riktiga ikon. Uppfinn aldrig en ny här.
2. Kopiera ett helt `.door`-block och byt `--accent`, ikon, titel, tagline,
   beskrivning och slug.
3. Skriv beskrivningen sanningsenlig. Påstå inga funktioner som inte finns, det har
   blivit fel en gång.
4. Ordningen är nyast repo överst.

⚠️ `CNAME`-filen är genererad av GitHub från Pages-inställningen för custom domain.
Rör den aldrig manuellt.

## Resume here

Börja med `og:image` per undersida. Det är det som syns när någon delar en länk, och
det är den enda kvarvarande punkten som påverkar hur projekten uppfattas utåt.
