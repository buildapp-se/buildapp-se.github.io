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

- [ ] `[P2]` Kvarglömd SRV-post: `_autodiscover._tcp.buildapp.se` pekar på `autoconfigure.strato.de:443`. Mailen går via Cloudflare Email Routing. Ta bort om ingen Strato-brevlåda används.
- [ ] `[P2]` DNSSEC är av (ingen DS, ingen DNSKEY). Slå på i Cloudflare och lägg DS-posten hos registraren (InterNetX).
- [ ] `[P3]` CAA-post saknas. Certifikaten utfärdas i dag av Google Trust Services via Cloudflare.
- [ ] `[P3]` DMARC är `p=reject` utan `rua`, så inga rapporter kommer. HSTS saknar `includeSubDomains`.
- [x] (2026-10-10, `56ca548`) Google Fonts ersatt med egna filer, `security.txt` tillagd.
