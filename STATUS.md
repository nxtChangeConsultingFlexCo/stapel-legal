# STATUS Stapel Website (Repo `stapel-legal`)

## STA-7 Branch-Überblick
Stand 03.11.2026, Quelle: `git branch -r`, `git log`, `git merge-base` gegen `origin/main` (d2dde02, 29.10.2026). Nur Bestandsaufnahme: keine Bewertung offener Entscheidungspunkte (STA-2 wartet auf Karl), nichts gemergt. Plane-Karte nur, wenn sie im Commit oder Branch erkennbar ist.

| Branch | Letzter Commit | Inhalt | Merge-Stand | Abgezweigt von |
|---|---|---|---|---|
| `seo/2026-11` | a23d16a (ohne Karte erkennbar) | SEO-Paket: Beschreibung, Canonical, hreflang, JSON-LD, Ollama-Zweitmeinung (in `tools/`) | offen (4 Commits vor main) | main d2dde02 |
| `review/sta3-website` | f783861 (STA-3) | Befundliste `docs/review-sta3-website.md` (nur Bericht), enthält `seo/2026-11` | offen (5 Commits) | `seo/2026-11` a23d16a (Basis main d2dde02) |
| `fix/sta4-seo-technik` | 90c24ff (STA-4) | Statische Sitemap und robots.txt, `url` in `_config.yml`, og:locale/twitter:card, JSON-LD-`url`, Build-Check-Bericht, `docs/` von Jekyll ausgeschlossen; enthält `review/sta3-website` | offen (9 Commits) | `review/sta3-website` f783861 (Basis main d2dde02) |
| `docs/status-ueberblick` | dieser Branch (STA-7) | Dieses `STATUS.md` | offen | main d2dde02 |

Offene Branches neben main: **3** (ohne diesen). Die drei bilden eine lineare Kette (seo -> review -> fix); main ist der gemeinsame Ausgangspunkt, die Kette ließ sich in STA-4 konfliktfrei probe-mergen (reiner Fast-Forward). Der lokale Branch `fix/sta4-seo-technik` in `/work/stapel-legal` steht hinter origin (nicht angefasst).

**PO-Antworten zu STA-6 (03.11.2026):** (1) Unlesbarer Beleg im Posteingang bleibt liegen, mit Hinweis (Datenverlust vermeiden); gilt für das spätere Testpaket 1. (2) Mehrere geteilte Belege einzeln nacheinander prüfen und (3) Originalbild statt JPEG-Neukodierung: Annahmen bleiben bis zu Karls Antwort gültig, nichts umgesetzt.
