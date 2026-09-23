---
schemaVersion: 1
status: active
currentGoal: Hålla katalogsidan buildapp.se korrekt när projekten bakom dörrarna ändras
nextAction: Kontrollera efter några dagar att Web Analytics i Cloudflare-dashboarden visar sidvisningar per sökväg (/sipdeck/, /grammat/ osv) och att beaconen inte blockeras av någon CSP-header som senare sätts i zonen
blockers: []
reviewedAt: 2026-09-23
---

# Handoff: buildapp.se

## 2026-09-23: Flaskors dörr
Överst (nyast), accent `#85444F`, ikon `buildapp.se/flaskor/pwa-512x512.png`, chip SV, plus `/flaskor/` i `sitemap.xml` (`27d4dc5`). Verifierat lokalt (sex dörrar, bilden laddar, sv/en, ingen sidscroll) och live. **Fälla:** `git push` hänger här, remote-URL:en bär `Elwyndaz@` och Git Credential Manager väntar på en osynlig ruta. Fungerar: `git -c credential.helper= -c credential.helper='!gh auth git-credential' push https://github.com/buildapp-se/buildapp-se.github.io.git main`. **Öppet:** i mörkt läge har taglinen i accentfärg cirka 2,5:1 mot `--wall` för Grammat och Flaskor, `--tag` finns bara för ljust läge.

## 2026-09-16: Lighthouse-kontrasten

Sekundärfärgen `--ink-2` mörkare i ljust läge (index, om, integritet), AI-dörrens
accent `#35606F` (den enda av fem under 4,5:1), loggans `aria-label` borta så att
den synliga texten är namnet. `ba39ea6` och `fdbd92e`, Pages-deployade, mätt
live: Lighthouse a11y 100.

## Läget

Live på `https://buildapp.se/`. Sidan är en katalog som länkar vidare till de
fristående småprojekten. Domänen är inte en produkt, den är huset.

Designkoncept, tokens och regler för att lägga till en dörr står i `CONTEXT.md`
(det finns ingen `PROJECT.md`, den referensen var fel).

## Recent work

**2026-09-15: Valheim Food Planner fick en dörr, och alla dörrar visar språk.**

- Valheim var olistad bara medan den byggdes. Dörren ligger sist, ikonen är en kopia av appens V-märke i `assets/valheim-mark.svg`.
- Språkchips per dörr, verifierade mot live-sajterna: Sipdeck och AI har språktoggel (SV, EN), Grammat och Tidslinjen är bara svenska, Valheim bara engelska.
- Ember-accenten `#d8a13a` klarar inte kontrast som textfärg på ljus vägg, så taglinen använder `--tag`. Kontrollerat i Playwright vid 1280 och 390 px.

**2026-09-14: Valheim Food Planner i policylistan.**

- `integritet.html` version 1.2 länkar till `/valheim/privacy.html` (sv + en). Policyn ligger i repot `buildapp-se/valheim`. Valheim har ingen dörr på startsidan än, olistad med avsikt.

**2026-08-27, kväll: Cloudflare Web Analytics påslaget för hela zonen.**

- Valet "Enable, JS snippet automatically injected" under Observe → Web
  Analytics → Manage site. Cloudflare injicerar `beacon.min.js` i all proxyad
  HTML under `buildapp.se`, alltså även `/sipdeck/`, `/grammat/`, `/ai/` och
  `/tidslinje/`. Verifierat med curl mot roten. Ingen kodändring, inget att
  underhålla. Beaconen rapporterar till `buildapp.se/cdn-cgi/rum`.
- `integritet.html` (sv + en, version 1.1) beskriver tjänsten: cookiefri, ingen
  enhetsidentifiering, berättigat intresse. Motsvarande text ligger i varje
  projekts policy i respektive repo, ändrade samma kväll.
- **Gräns:** bara sidvisningar, inga events. Adblockers stoppar beaconen, räkna
  med 20–40 % underrapportering. Ingen CSP sätts i zonen idag; införs en måste
  `static.cloudflareinsights.com` in i `script-src`.

**2026-08-27, senare: engelsk policy och en rättad dörrbeskrivning.**

- `integritet.html` finns nu på svenska och engelska, med samma språkväljare och
  samma `buildapp-lang`-nyckel som katalogsidan, så valet följer med mellan sidorna.
- **Dörrtexten för `/ai` var fel efter att den sajten ändrades.** Den lovade
  "Bocka av det du gått igenom och rösta fram det bästa", men röstningen och
  avbockningen togs bort ur `/ai` samma dag. Texten beskriver nu filtren och
  listvyn i stället. Kontrollera dörrtexterna mot projekten när de ändras, de
  ligger i ett annat repo och följer inte med automatiskt.



**2026-08-27: integritetspolicy, ingen av sajterna hade informationsplikt.**

- Ny `integritet.html` i katalogsidans gatu- och skyltdesign. Täcker Cloudflare,
  GitHub Pages, Google Fonts och `localStorage`-nyckeln `buildapp-lang` som
  språkväljaren använder. Länkar vidare till de fyra projektens egna policyer,
  eftersom de behandlar olika mycket och katalogsidan inte kan tala för dem.
- Sidfotslänk tillagd i `index.html`, tvåspråkig via `data-sv`/`data-en`.
- **Ingen cookiebanner, och det är avsiktligt.** Samtyckeskravet i lagen om
  elektronisk kommunikation triggar på lagring eller läsning på besökarens enhet.
  Språkvalet är nödvändigt för en funktion besökaren själv begär och är därför
  undantaget. GDPR:s informationsplikt gäller ändå, och den fyller policyn.
- `og.source.html` sparad enligt konventionen i syskonrepona, med
  renderingskommandot i en kommentar högst upp. Mallen låg tidigare bara i en
  temp-katalog.

**Öppet beslut:** Web Analytics är inte påslaget. Slås det på måste avsnittet
Cookies i `integritet.html` uppdateras först, eftersom det i dag påstår att sidan
inte laddar någon analystjänst.


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

**Rättelse samma dag:** det här repot påstod i både `HANDOFF.md` och `BACKLOG.md` att
undersidorna saknade `og:image`. Fel. Kontroll mot live visar att `/sipdeck`,
`/grammat`, `/ai` och `/tidslinje` alla har egen 1200x630 PNG med
`summary_large_image`. Undersidorna bor i **egna repon** som deployas under samma
domän, så när de fick sina delningsbilder uppdaterades aldrig det här repots
dokument. Påståenden om undersidor kontrolleras mot live, inte mot den här filen.

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

- Undersidorna lånar fortfarande `buildapp.se`:s touch-ikoner. Egen `og:image` har
  de däremot, det påståendet var fel, se rättelsen under Recent work.
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

Undersidornas `og:image` behöver inget arbete, de har redan egna. Enda kvarvarande
grafikpunkten är touch-ikoner per app, vilket är kosmetik.

## Granskning 2026-09-16

Cross-project audit run from elwyn-dash (session 5 in the daily note). Results written to `## Audits` in CONTEXT.md, findings appended to BACKLOG.md under `## Granskning 2026-09-16`. Headers on buildapp.se and the TLS grade are zone-level and are fixed once in Cloudflare, not here.
