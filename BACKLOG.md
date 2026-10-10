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
- [ ] `[P2]` `orgutveckling@buildapp.se` har ingen regel i Email Routing. Bara `kontakt@buildapp.se` vidarebefordras och catch-all är avstängd, så post dit studsar. Resend används bara för `beefcake.buildapp.se` och `familjehubben.buildapp.se`.
- [ ] `[P2]` `[ready-for-human]` DNSSEC påslaget i Cloudflare 2026-10-10 18:00, status `pending` tills DS-posten ligger hos registraren (Strato/InterNetX): `buildapp.se. 3600 IN DS 2371 13 2 AA6ACC36388B759724C8A26546D3EA866BA94A75F6E41FD51B80620EABB68916` (key tag 2371, algoritm 13, digest type 2). Utan DS är inget skyddat men inget heller trasigt. Fel DS gör hela domänen onåbar.
- [ ] `[P3]` CAA-post saknas. **Fälla:** `auth`, `beefcake`, `flaskor` och `sipdeck` är DNS only-CNAME till Firebase (`web.app`), som utfärdar egna certifikat. En CAA på apex ärvs av dem, så en för snäv post kan stoppa förnyelsen av Sipdecks inloggningsdomän. Låg vinst mot den risken.
- [ ] `[P3]` DMARC är `p=reject` utan `rua`, så inga rapporter kommer. HSTS saknar `includeSubDomains`.
- [x] (2026-10-10, `56ca548`) Google Fonts ersatt med egna filer, `security.txt` tillagd.
