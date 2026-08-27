---
schemaVersion: 1
status: active
currentGoal: Hålla katalogsidan buildapp.se korrekt när projekten bakom dörrarna ändras
nextAction: Ta fram delningsbilder för de fristående projekten bakom dörrarna. Katalogsidans og:image är klar.
blockers: []
reviewedAt: 2026-08-27
---

# Handoff: buildapp.se

## Läget

Live på `https://buildapp.se/`. Sidan är en katalog som länkar vidare till de
fristående småprojekten. Domänen är inte en produkt, den är huset.

Designkoncept, tokens och regler för att lägga till en dörr står i `CONTEXT.md`
(det finns ingen `PROJECT.md`, den referensen var fel).

## Recent work

**2026-08-27: länkförhandsvisningen saknades helt.**

- `index.html` hade varken `og:`- eller `twitter:`-taggar, så delade länkar
  renderades utan bild och utan titel bortom `<title>`.
- Full uppsättning tillagd, plus `assets/og-image.png` (1200x630) i sajtens egen
  stil: skyltfont, väggfärger, gatuband och de fyra dörrslugsen i sina accentfärger.
- Bilden renderas från en HTML-mall via `npx playwright screenshot
  --viewport-size=1200,630`. Mallen ligger inte i repot. Ska bilden göras om,
  återskapa mallen från tokens i `CONTEXT.md`.
- `og:image:width`, `height` och `type` är med. Utan dem hämtar skraparen bilden
  och mäter den själv, vilket är varför en förhandsvisning ibland dyker upp först
  vid andra delningen.

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

- Undersidorna har fortfarande inget eget grafiskt paket: ingen egen `og:image`,
  och de lånar `buildapp.se`:s touch-ikoner. Katalogsidan är klar sedan 2026-08-27.
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

Katalogsidans delningsbild är klar men **inte pushad**. Verifiera i produktion efter
push, och kör Facebooks Sharing Debugger så cachen töms.

Därefter `og:image` per undersida, samma metod. Det är det som syns när någon delar
en länk in i ett enskilt projekt.
