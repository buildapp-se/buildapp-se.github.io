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

- [ ] Säkerhetsheaders via en Transform Rule i Cloudflare. GitHub Pages ignorerar
  `_headers`, så det går inte att lösa i repot. Samma åtgärd som för de andra
  Pages-sajterna, se säkerhetsrepot.

## Underhåll

- [ ] Håll dörrarna i takt med projekten. En ny app betyder ett nytt `.door`-block
  med appens egen accentfärg och riktiga ikon, placerat enligt nyast-överst-regeln.

## Granskning 2026-09-16

Fynd från cockpitens granskningskolumner (Lighthouse mobil, W3C, UX-skript, headers, TLS, OWASP). Mätvärdena står under `## Audits` i CONTEXT.md.

- [x] `[P3]` (rättad 2026-09-16: `--ink-2` #6E6A5D till #56534A i ljust läge på tre sidor, AI-dörrens accent #35606F, loggans aria-label borta; a11y 100 live) Lighthouse: färgkontrast under 4,5:1 och en länk vars synliga text inte ingår i det tillgängliga namnet (a11y 96).
