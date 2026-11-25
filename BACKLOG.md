# Backlog

## Grafik

- [x] `og:image` för katalogsidan, `assets/og-image.png` (2026-08-27). Renderad från
  en HTML-mall i sajtens egen stil med `npx playwright screenshot`, mallen ligger inte
  i repot.
- [x] Eget `og:image` per undersida. **Punkten var redan gjord när den skrevs.**
  Kontrollerat live 2026-08-27: `/sipdeck`, `/grammat`, `/ai` och `/tidslinje`
  serverar alla en egen 1200x630 PNG med `summary_large_image`. Undersidorna bor i
  egna repon som deployas under samma domän, så deras fixar syntes aldrig här.
  Kontrollera undersidor mot live, aldrig mot det här repots dokument.
- [ ] Eventuellt egna touch-ikoner per app, i stället för att låna `buildapp.se`:s.

## Säkerhet

- [x] (gjord 2026-09-16, mätt live igen 2026-10-10) Säkerhetsheaders via en Transform Rule i Cloudflare. GitHub Pages ignorerar
  `_headers`, så det går inte att lösa i repot. Samma åtgärd som för de andra
  Pages-sajterna, se säkerhetsrepot.

## Underhåll

- [ ] Håll dörrarna i takt med projekten. En ny app betyder ett nytt `.door`-block
  med appens egen accentfärg och riktiga ikon, placerat enligt nyast-överst-regeln.

## Granskning 2026-09-16

Fynd från cockpitens granskningskolumner (Lighthouse mobil, W3C, UX-skript, headers, TLS, OWASP). Mätvärdena står under `## Audits` i CONTEXT.md.

- [x] `[P3]` (rättad 2026-09-16: `--ink-2` #6E6A5D till #56534A i ljust läge på tre sidor, AI-dörrens accent #35606F, loggans aria-label borta; a11y 100 live) Lighthouse: färgkontrast under 4,5:1 och en länk vars synliga text inte ingår i det tillgängliga namnet (a11y 96).

## Granskning 2026-10-06

Fynd från den automatiska sviten (aifabriken `tools/audit-suite.ts`: headers, npm audit, secrets, Actions, markup, axe). Mätvärdena står som `(automated)`-rader under `## Audits` i CONTEXT.md.

- [x] `[P3]` (rättad 2026-10-06, `73b8de2` på grenen `batch/2026-10-06`, mergad till `main` och live 2026-10-07) WCAG: axe hittar 0 fel men kan inte avgöra kontrasten på 5 element. Kontrollen gjord i Chrome på alla tre sidorna, ljust och mörkt, vila och hovring: 49 av 408 textmätningar låg under 4,5:1, alla 408 klarar gränsen efter rättningen. Detaljer i `HANDOFF.md`.

## Granskning 2026-10-10

Fynd från en extern Scantide-skanning, kontrollerade mot live-DNS. Alla tre är zoninställningar i Cloudflare och rör varje app på buildapp.se.

- [x] (2026-10-10 18:00) Fyra kvarglömda Strato-poster från 2026-07-21 raderade ur zonen: SRV `_autodiscover._tcp` (`100 443 autoconfigure.strato.de`, prio 0), CNAME `autoconfig` mot `autoconfigure.strato.de` (proxied), MX `*.buildapp.se` mot `smtp.rzone.de` (prio 5), TXT `_domainkey` med `o=~; t=y; r=dkim@rzone.de`. Värdena står här så att de går att återskapa.
- [x] (2026-10-11) Sajten anger `kontakt@buildapp.se` i stället för `kontakt@orgutveckling.se` (`integritet.html` version 1.4 på svenska och engelska, `security.txt`). Email Routing vidarebefordrar adressen till Patriks verifierade privata adress, catch-all är av. Resend används bara för `beefcake.buildapp.se` och `familjehubben.buildapp.se`.
- [x] (2026-10-10) `[wontfix]` DNSSEC: beslut att hoppa över. Stratos kundpanel saknar DS-fält för domäner med externa namnservrar, så det kräver supportärende eller flytt av domänen. Vinsten är liten med HTTPS och HSTS på plats, och en felaktig eller kvarglömd DS gör hela domänen onåbar. Påslaget och avstängt igen i Cloudflare samma dag, ingen DS lades någonsin hos registraren.
- [ ] `[P3]` CAA-post saknas. **Fälla:** `auth`, `beefcake`, `flaskor` och `sipdeck` är DNS only-CNAME till Firebase (`web.app`), som utfärdar egna certifikat. En CAA på apex ärvs av dem, så en för snäv post kan stoppa förnyelsen av Sipdecks inloggningsdomän. Låg vinst mot den risken.
- [x] (2026-10-11) HSTS har `includeSubDomains` (zoninställning i Cloudflare, `max-age=15552000`, ingen preload). Mätt live på `buildapp.se`, `hemmaplan` och `cockpit`. Firebase-värdarna (`sipdeck`, `auth`, `beefcake`, `flaskor`) skickar egen HSTS på ett år. Följd: ingen underdomän på zonen kan köras utan HTTPS.
- [x] (2026-10-11) `[wontfix]` DMARC `rua`: bortvald av Patrik. DMARC är redan `p=reject`, `rua` ger bara XML-rapporter i inkorgen. Ta upp igen om någon förfalskar mejl från domänen.
- [x] (2026-10-10, `56ca548`) Google Fonts ersatt med egna filer, `security.txt` tillagd.

## Granskning 2026-11-24

UX-audit (ux-checks 6 av 6 pass, skärmbildsgranskning 390 och 1440 px, ljust och mörkt) och en dörr-i-takt-kontroll mot live.

- [x] `[P3]` (rättad 2026-11-24, `ecf3d27`) Sidfoten på `index.html` vid 390 px svenska och 320 px engelska: mittpunkten stod ensam sist på raden och "Om oss" ensam på nästa. Under 30rem står "Fortsättning följer." på egen rad och länkarna på nästa.
- [x] `[P3]` (rättad 2026-11-24, `e825a03`) `integritet.html` "Projekten bakom dörrarna" saknade Dinodecks och ./terminals policyer, båda live 200. Länkade på svenska och engelska.
- [ ] `[P2]` `integritet.html` säger att varje projekt bakom en dörr har en egen policy, men Flaskor, Beefcake och Bulk har ingen (ingen fil i repona, inga träffar live). Flaskor och Beefcake har konton. Policyn skrivs i respektive app-repo, sedan läggs länken här.
- [ ] `[P3]` Undertiteln skiljer sig: "webbappar, spel och guider" på `index.html`, "en samling småprojekt" på `om.html` och `integritet.html` (Similarity). Smakfråga, Patrik väljer.

## Granskning 2026-11-25

Fart, tillgänglighet och metadata, mätt live med headless Chromium: 390 px med 4x CPU-strypning och Slow 4G, plus 1440 px. Siffrorna står under `## Audits` i CONTEXT.md.

- [x] `[P3]` (rättad 2026-11-25, `26bc551`) `assets/fonts.css` var den enda renderblockerande begäran på alla tre sidorna. Reglerna står nu inline med preload av två typsnitt: renderblockerande 1 till 0, CLS på index vid 1440 px 0,0105 till 0. LCP oförändrad inom brus (index 602 till 588 ms, median).
- [x] `[P3]` (rättad 2026-11-25, `2c29d92`) `assets/ai-mark.png` var 512 px och 22 710 B för en 72 px ikon; nu 216 px palett-PNG, 4 254 B, `?v=4`. De fyra översta dörrikonerna (ovanför vecket vid 390 px) laddas inte längre lat. Index överför 274 till 253 KB.
- [x] Metadata kontrollerad, inget att rätta: unik titel och beskrivning, `lang="sv"`, canonical, `og:image` 1200x630 och favicon på alla tre sidorna; ingen PWA, alltså inget manifest. axe-core 4.14.0: 0 fel på alla tre sidorna i båda bredderna.
- [ ] `[P3]` Dörrikonerna från apparna är 512 px PNG men visas i 52 till 72 px: Dinodeck 47 KB, Grammat 22 KB, Sipdeck 17 KB, Beefcake 15 KB. Sex app-PNG:er väger sammanlagt 124 KB (uppmätt med curl). En mindre variant görs i respektive app-repo: regeln är att länka appens riktiga ikon, så filen skapas där, inte här.
- [x] `[P3]` (rättad 2026-11-25, `5b92166`) CLS 0,018 på index vid 390 px: undertiteln i sidhuvudet stod på en rad i reservtypsnittet (Consolas på Windows) och på två i IBM Plex Mono, så `main` flyttades 16,8 px när typsnittet kom. `--font-mono` har nu `'Courier New'` före `monospace` (samma teckenbredd som Plex Mono) på alla tre sidorna. Live med typsnitten fördröjda 1,5 s: 390 px 0,0185 till 0,0006 (svenska) och 0,0002 (engelska).
- [ ] `[P3]` CLS 0,0086 på index vid 1440 px med typsnitten fördröjda 1,5 s: dörrtexterna radbryts om när IBM Plex Sans ersätter reservtypsnittet. Under gränsen 0,1; kräver ett reservtypsnitt med `size-adjust`.

## Scenarier 2026-11-25

Headless Chromium med bfcache påslaget, mot lokal server och live. Skriptet ligger utanför repot (`%TEMP%/aifabriken-lanes/p3-buildapp-se.github.io/scenarios.mjs`), 10 av 10 pass live efter rättningen, 5 fel före.

- [x] `[P2]` (rättad 2026-11-25, `38270fe`) Bakåtknappen: byter man till engelska på `om.html` och går tillbaka visade katalogsidan svenska, eftersom webbläsaren återställer den ur bfcache utan att köra skriptet igen. Språket läses nu om vid `pageshow` med `persisted`.
- [x] `[P3]` (rättad 2026-11-25, `38270fe`) Två flikar: språkbyte i en flik slog inte igenom i den andra förrän omladdning. Nu via `storage`-händelsen.
- [x] `[P3]` (rättad 2026-11-25, `38270fe`) Ett sparat värde i `buildapp-lang` som inte är `sv` eller `en` gav ingen aktiv knapp, och på `om.html` och `integritet.html` dolde det all text. Ingen känd kod skriver ett sådant värde (ingen app i `C:\dev` använder nyckeln); nu faller sidan tillbaka på webbläsarens språk.
- [x] Utan fel: blockerad lagring plus ikoner som inte laddar (knappen fungerar, inga JS-fel), breddbyte 1440, 320, 768 och 390 px utan sidledsscroll.

## Webbläsarpass 2026-11-25

Headless WebKit (Playwright, iPhone 13-profil) och Firefox 1440 px jämförda med Chromium, live, ljust och mörkt. Detaljer i `HANDOFF.md`.

- [x] Inga skillnader som drabbar en besökare: layout, bilder, språkknapp, språk över navigering, tillbaka och flikar, `color-mix` lika i alla tre motorerna. Rubriktypsnittet ritas tunt i Playwrights WebKit på Windows även i en minimal testsida med `font-variation-settings`, alltså buildens rendering och ingen sidbugg. Riktig iPhone inte testad.
