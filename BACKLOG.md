# Backlog

## Grafik

- [x] `og:image` för katalogsidan, `assets/og-image.png` (2026-08-27). Renderad från
  en HTML-mall i sajtens egen stil med `npx playwright screenshot`, mallen ligger inte
  i repot.
- [ ] Eget `og:image` per undersida. Katalogsidan är klar, de fristående projekten
  bakom dörrarna har fortfarande ingen egen delningsbild.
- [ ] Eventuellt egna touch-ikoner per app, i stället för att låna `buildapp.se`:s.

## Säkerhet

- [ ] Säkerhetsheaders via en Transform Rule i Cloudflare. GitHub Pages ignorerar
  `_headers`, så det går inte att lösa i repot. Samma åtgärd som för de andra
  Pages-sajterna, se säkerhetsrepot.

## Underhåll

- [ ] Håll dörrarna i takt med projekten. En ny app betyder ett nytt `.door`-block
  med appens egen accentfärg och riktiga ikon, placerat enligt nyast-överst-regeln.
