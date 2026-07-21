# buildapp.se — landningssida

Katalogsida som länkar vidare till fyra fristående småprojekt. Domänen är inte en
produkt, den är "huset". **Live:** https://buildapp.se/
Repo: https://github.com/buildapp-se/buildapp-se.github.io (org root-repo, GH Pages,
branch `main`, root. `CNAME`-filen är GitHub-genererad från Pages custom domain-inställningen,
rör den inte manuellt.)

## Arkitektur
- Allt är `index.html`: inline CSS + en liten vanilla JS-snutt för språktoggeln. Inget
  byggsteg, inga beroenden, samma mönster som recept/sipdeck/ai.
- `favicon.svg`: självbärande, byter färg via `@media (prefers-color-scheme: dark)`
  inbyggt i SVG:n. `assets/icon-32.png` = PNG-fallback för äldre browsers,
  `assets/icon-192.png` = apple-touch-icon (iOS stödjer inte SVG-favicons).
- `assets/ai-mark.png`: självhostad kopia av orgutveckling.se:s 3×3-märke, eftersom
  `ai`-appen inte har någon egen ikon på sin egen domän (dess `<link rel="icon">` pekar
  externt på orgutveckling.se). Sipdeck/recept/tidslinjes ikoner refereras direkt via
  `https://buildapp.se/<app>/...`, ingen dubblering.

## Designkoncept
Arkitektonisk skyltning/adressplakett, inte en mjukvaru-startup. buildapp.se är
"huset", varje undersida är en "dörr" i sin egen färg nerför en betongkorridor.

**Tokens (CSS custom properties i `:root`, ljust/mörkt via `prefers-color-scheme`):**
- `--wall` / `--wall-2` / `--surface`: betong/kalksten, ljust `#DFDDD5`/`#D2CFC5`/`#EAE8E0`,
  mörkt `#18181A`/`#222224`/`#201F1F`
- `--ink` / `--ink-2`: text, ljust `#211F1A`/`#6E6A5D`, mörkt `#EDEAE2`/`#9C978A`
- Typsnitt: **Big Shoulders Display** (rubriker/wordmark, kondenserad signage-känsla),
  **IBM Plex Sans** (brödtext), **IBM Plex Mono** (url-slugs, språktoggel)
- buildapp.se-loggan (huset i headern + favicon) använder ENDAST `--ink`/`--wall`,
  aldrig en apps egen accentfärg, den ska förbli varumärkesneutral.

**Varje dörr (`.door`):**
- `--accent` sätts inline (`style="--accent:#hex"`) = appens EGEN färg, återanvänd rakt
  av, aldrig uppfunnen här.
- Ikon = appens riktiga ikon/mark (ingen ny illustration, inga skärmdumpar).
- `.knob` = liten accentprick i ikonrutans hörn, byggt på mönstret som redan fanns
  organiskt i tre av fyra appikoner (bokstav/piktogram + prick).
- Ordning: nyast repo överst (se `created_at` per repo i `buildapp-se`-orgen).

## Språk
Ingen build-time-i18n. Varje textnod som ska översättas har `data-sv`/`data-en`.
`navigator.language` gissar startspråk, manuellt val (SV/EN-knapparna i headern) sparas
i `localStorage` (`buildapp-lang`) och vinner över gissningen vid återbesök. Ingen
cookie, ingen server-side-koppling, exakt samma mönster som `ai`-appen redan använder.

## Lägga till en femte dörr
1. Hämta appens egen accentfärg + ikon-URL (helst samma domän, `https://buildapp.se/<app>/...`).
   Finns ingen ikon på samma domän: kopiera in den lokalt under `assets/` som för `ai`.
2. Kopiera ett helt `.door`-block, byt `--accent`, ikon-`src`, titel, tagline, beskrivning
   (med `data-sv`/`data-en` om appen är engelsk-först), `.door-slug`.
3. Skriv beskrivningen sanningsenlig, inga påstådda funktioner (t.ex. inlogg/spara) som
   inte faktiskt finns i appen, det har hänt fel en gång.
4. Placera enligt nyast-överst-regeln.

## Kvarstår (framtida, inte påbörjat)
Eget grafiskt paket per undersida: `og:image` för länkförhandsvisningar i
Messenger/Facebook/Slack m.m., ev. egna touch-icons per app i stället för att låna
`buildapp.se`:s.

## Deploy
`git push` till `main` (via GitHub API i den här sessionen, annars vanlig git) →
GitHub Pages bygger automatiskt (~30 s).
