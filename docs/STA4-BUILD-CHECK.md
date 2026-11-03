# STA-4: Jekyll-Build- und Pages-Vorschau-Check

Stand: 03.11.2026. Geprüft: `fix/sta4-seo-technik` (da37dd4), das die Branches `seo/2026-11` (a23d16a) und `review/sta3-website` (f783861) enthält, plus die Korrektur aus diesem Paket.

## Ergebnis
- Build mit allen drei Ständen: **ja, ohne Fehler, ohne Jekyll-Warnungen**.
- Ein technischer Fehler gefunden und behoben: `docs/` wurde veröffentlicht (siehe Befund 1).
- Merge-Kette konfliktfrei. Freigabereif aus Build-Sicht, sobald die Fragen unten beantwortet sind.

## Umgebung (Ersatz für fehlendes Ruby im Container)
Im Container gab es kein Ruby/Jekyll, und ohne Root konnte nichts installiert werden. Ersatzweise, alles im Scratchpad und nichts global:
- Portables Ruby 3.3.9 (ruby-builder, Ubuntu 22.04, glibc-kompatibel), gestartet mit `LD_LIBRARY_PATH`/`RUBYLIB`.
- Gem `github-pages` 232 (Jekyll 3.10.0, jekyll-theme-primer 0.6.0, jekyll-seo-tag 2.8.0, jekyll-sitemap 1.4.0, kramdown 2.4.0), also derselbe Stack wie GitHub Pages.
- Build in einem eigenen `git worktree` (detached). Das Arbeitsverzeichnis `/work/stapel-legal` und der Worktree `/work/wt/seo-stapel-legal` blieben unberührt. Der Worktree wurde danach entfernt.
- `LANG=C.UTF-8` ist nötig, sonst bricht Sass mit „Invalid US-ASCII character“ ab. Das ist ein Locale-Effekt der Umgebung, kein Fehler der Seite.
- Grenze der Nachbildung: Das Repo hat kein Gemfile und kein `plugins:`. Die Standard-Plugins von Pages werden daher nur über eine Zusatzkonfiguration außerhalb des Repos nachgebildet: default-layout, optional-front-matter, readme-index, titles-from-headings, relative-links, seo-tag, sitemap. Ohne default-layout bekamen die Markdown-Seiten kein Layout. Dass Pages es anwendet, zeigt die Live-Seite (SEO-Tag-Kopf auf /datenschutz/). `jekyll-github-metadata` wurde nicht nachgebildet (braucht GitHub-API), `site.github.build_revision` ist lokal leer, in Pages gefüllt. `jekyll-feed` ist live nicht aktiv (feed.xml = 404) und deshalb nicht im Test.
- Nicht prüfbar: das tatsächliche Pages-Deployment. Danach live prüfen: `/sitemap.xml`, `/robots.txt`, `/docs/…` (soll 404 sein).

## Prüfungen
| Punkt | Ergebnis |
|---|---|
| Build | 0 Fehler, 0 Warnungen. Einziger Hinweis ist eine Konsolenmeldung „install faraday-retry“ aus dem Gem github-metadata, Umgebungsrauschen ohne Auswirkung. |
| `sitemap.xml` | 9 URLs, alle mit `https://stapel.nxtchange.app/`, passend zu `url` in `_config.yml`. Jede URL löst auf eine gebaute Seite auf. Keine Duplikate. |
| Sitemap vs. jekyll-sitemap | Statische `sitemap.xml` gewinnt. Die Ausgabe ist byte-identisch zur Repo-Datei, es wird kein zweites Dokument erzeugt. Jede gebaute Seite steht in der Sitemap. |
| `robots.txt` | `Allow: /` plus `Sitemap: https://stapel.nxtchange.app/sitemap.xml`, konsistent. |
| Interne Links und Assets | 0 kaputte Links, Bilder, Icons oder Anker auf allen 9 Seiten. `og:image` zeigt auf eine vorhandene Datei. |
| Externe Links | `nxtchange.app`, `nxtchange-consulting.com`, `ris.bka.gv.at` liefern 200. |
| meta description | Vorhanden auf allen 9 Seiten (DE und EN), je genau eine. |
| og:locale | Startseiten: `de_AT` bzw. `en_US`, jeweils mit `alternate`. Markdown-Seiten (seo-tag): `de` bzw. `en`. |
| twitter:card | `summary` auf allen 9 Seiten. |
| JSON-LD | Alle Blöcke parsen. Startseiten: Organization, WebSite, WebPage, SoftwareApplication. `#app` hat `url` = `https://stapel.nxtchange.app/` auf DE und EN (ein gemeinsamer Knoten). Markdown-Seiten: WebPage mit korrekter `url`. |
| Merge-Kette | Probe-Merge lokal auf `origin/main` (d2dde02): seo -> review -> fix konfliktfrei. Die Kette ist linear (seo ⊂ review ⊂ fix), `main` ist der gemeinsame Ausgangspunkt, also ein reiner Fast-Forward. Nichts gepusht. |

## Befunde
1. **Behoben: `docs/` wurde veröffentlicht.** `docs/review-sta3-website.md` (interner Review-Bericht) landete im Ausgabeordner, mit Pages-Standardplugins sogar als `/docs/review-sta3-website.html` samt Sitemap-Lücke. Auch dieser Bericht läge sonst öffentlich. Fix: `exclude: [tools, docs, README.md]` in `_config.yml`. Danach 9 Seiten statt 11 Ausgabedateien mit Inhalt, `docs/` ist weg.
2. Hinweis: Das Repo enthält bereits hreflang (`<link rel="alternate" hreflang>` auf beiden Startseiten, `xhtml:link` in `sitemap.xml`, aus dem SEO-Paket). Ich habe nichts daran geändert. Siehe Frage 1.
3. Hinweis: Die Markdown-Seiten haben `og:locale` = `de`/`en` (seo-tag-Standard), die Startseiten `de_AT`/`en_US`. Funktioniert, ist aber uneinheitlich. Siehe Frage 2.
4. Die statische Sitemap muss bei neuen Seiten von Hand ergänzt werden (steht im README). Bei jedem `exclude`- oder Seiten-Change gegenprüfen.
5. Der lokale Branch `fix/sta4-seo-technik` im Arbeitsverzeichnis `/work/stapel-legal` steht auf da37dd4 und liegt nach dem Push hinter `origin`. Dort bei Bedarf `git pull --ff-only` (nicht angefasst).

## Offene Fragen an Karl
1. Ist hreflang in Startseiten und Sitemap so gewollt? Der Auftrag sagte „kein hreflang“, im Branch ist es aber aus dem SEO-Paket schon enthalten. Annahme: bleibt unverändert.
2. `og:locale` der Markdown-Seiten auf `de_AT`/`en_US` angleichen? Ginge über `locale:` im Front Matter, das wäre aber eine Änderung an den Rechtstext-Dateien. Annahme: bleibt, kein Handlungsbedarf.
3. `support.md` ist zweisprachig mit einer gemeinsamen Beschreibung (DE und EN in einem Satz) und `lang` = `de`. Annahme: bleibt, wie beauftragt kein neues support.md.
