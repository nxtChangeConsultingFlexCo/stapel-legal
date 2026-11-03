# Review STA-3: Stapel-Website (Branch seo/2026-11, Commit a23d16a)

Stand 03.11.2026. Geprüft: `index.html`, `en/index.html`, `_layouts/default.html`, `_config.yml`, alle `*.md`-Seiten.
Methode: Quelltext gelesen, JSON-LD mit Python geparst, Längen gezählt, interne Links gegen Dateien, externe Links und
die Live-Seite per `curl` geprüft. Kein Ruby/Jekyll im Container, daher **kein lokaler Build**: was `{% seo %}`
(jekyll-seo-tag) auf den Markdown-Seiten erzeugt, ist nicht verifiziert. Ollama wurde nicht eingesetzt (Review von Hand).
Keine Inhalte geändert. Hinweis: Die Live-Seite (stapel.nxtchange.app) liefert noch den Stand von `main`; sie hat weder
JSON-LD noch hreflang, die Befunde gelten für den Branch.

## Ergebnis in Kürze
Hoch: 0 · Mittel: 3 · Klein: 7. Kein Befund braucht zwingend Karls Entscheidung.

## In Ordnung (geprüft)
- **JSON-LD** (`index.html:21`, `en/index.html:21`): gültiges JSON, je 4 Knoten, keine doppelten `@id` innerhalb einer
  Seite. Zwischen DE und EN identisch: Organization, WebSite (`inLanguage` ["de","en"]), SoftwareApplication `#app`
  (ohne `inLanguage`). Verschieden nur, wie gewollt: WebPage `#webpage` bzw. `/en/#webpage` mit `inLanguage`, `name`,
  `description` (stimmen mit title/meta description der jeweiligen Seite überein). Kein `offers`, kein `aggregateRating`,
  keine Preise oder Bewertungen. E-Mail und Anschrift stimmen mit dem Impressum überein.
- **hreflang/canonical Startseiten** (`index.html:17-20`, `en/index.html:17-20`): de, en, x-default (= DE) auf beiden
  Seiten identisch, gegenseitig, absolute URLs; canonical zeigt je auf die eigene Seite; `og:url` passt.
- **Längen:** title 27 Zeichen (DE und EN), description 149 (DE) und 143 (EN); beide im üblichen Rahmen.
- **Interne Links:** alle relativen Ziele der Startseiten und alle absoluten Pfade in den `*.md` existieren
  (`/imprint/`, `/impressum/`, `/support/#support-for-stapel` u. a.); Sprachumschalter `index.html:97` und
  `en/index.html:97` funktionieren. Live: `/`, `/en/`, `/support/`, Icon geben 200.
- **Externe Links:** 200 (teils nach Weiterleitung): apple.com, policies.google.com, ris.bka.gv.at, myapps.microsoft.com,
  nxtchange.app, Apple-EULA.

## Befunde

### Mittel
1. **Keine sitemap.xml und keine robots.txt** (Live: 404 für `/sitemap.xml`, `/robots.txt`; `_config.yml` hat keinen
   `plugins:`-Eintrag). Für zwei Sprachen mit hreflang wäre eine Sitemap nützlich.
   Vorschlag: `jekyll-sitemap` in `_config.yml` aktivieren oder statische `sitemap.xml` mit den hreflang-Paaren und
   `robots.txt` mit Sitemap-Verweis anlegen.
2. **Rechtstexte ohne hreflang-Paarung** (`datenschutz.md`↔`privacy.md`, `nutzungsbedingungen.md`↔`terms.md`,
   `impressum.md`↔`imprint.md`; `_layouts/default.html:7` nutzt nur `{% seo %}`, das keine Alternates erzeugt).
   `support.md` ist eine zweisprachige Seite mit `lang: de` (Front Matter fehlt), EN-Teil ab `support.md:101`.
   Vorschlag: im Layout Alternates für die drei Paare ergänzen (Front-Matter-Feld `alt:`), Support entweder so lassen
   oder in zwei Seiten teilen; auf jeden Fall `lang`-Auszeichnung für den englischen Abschnitt (`<div lang="en">`).
3. **Meta-Daten der Markdown-Seiten nicht verifiziert / vermutlich deutsch auf englischen Seiten.** `_config.yml:2`
   setzt `description: Rechtstexte zur iPhone-App Stapel` (deutsch) als Rückfall. `privacy.md`, `terms.md`,
   `support.md`, `datenschutz.md`, `nutzungsbedingungen.md` haben kein `description:` im Front Matter (nur `impressum.md:4`
   und `imprint.md:5`). Ohne eigenes Feld nimmt jekyll-seo-tag Auszug oder Site-Beschreibung; für `privacy.md`/`terms.md`
   wäre das falschsprachig oder zufällig. Vorschlag: pro Seite `description:` (≤ 160 Zeichen, passende Sprache) setzen
   und nach einem Build die erzeugten `<title>`, `canonical` und `description` prüfen.

### Klein
4. `_config.yml` hat kein `url:`/`baseurl:`; `absolute_url` im Layout (`default.html:15,19,21`) kann `http://` liefern
   (bereits im Verlauf vermerkt). Vorschlag: `url: https://stapel.nxtchange.app` setzen und canonical nach Build prüfen.
5. **Weiterleitungen in Links:** `https://www.nxtchange-consulting.com/` → Apex (`impressum.md:20`, `imprint.md:22`);
   `privacy.microsoft.com/privacystatement` → microsoft.com/de-de (`datenschutz.md:111`, `privacy.md:110`);
   `docs.github.com/site-policy/...` → `/en/...` (`datenschutz.md:177`, `privacy.md:173`). Vorschlag: Ziel-URLs direkt
   eintragen (`nxtchange-consulting.com`, GitHub mit `/en/`); bei Microsoft die sprachneutrale Adresse behalten.
6. **Nicht prüfbar:** `https://www.dsb.gv.at` (`datenschutz.md:191`, `privacy.md:186`) war aus dem Container nicht
   erreichbar (keine HTTP-Antwort, kein Beleg für defekt). Vorschlag: einmal im Browser prüfen.
7. **Login-Weiterleitungen (erwartet):** `account.live.com/consent/Manage` (`datenschutz.md:116`, `privacy.md:115`) und
   `reportaproblem.apple.com` (`support.md:75`, `support.md:160`) leiten zur Anmeldung. Kein Fehler, nur Hinweis.
8. **Sprachbezug im JSON-LD:** Der gemeinsame Knoten `#app` hat `url` auf die deutsche Startseite, auch von
   `en/index.html:21` aus. Konsistent und widerspruchsfrei, aber EN-Crawler sehen als App-URL die DE-Seite.
   Vorschlag: bewusst so lassen oder `url` weglassen und nur über `about` verknüpfen.
9. **Titel knapp** (27 Zeichen, `index.html:6`, `en/index.html:6`): ohne Kategorie-Begriff, der Suchbegriff
   „Kassenbon“/„receipt scanner“ steht nur in der Description. Reine Option, kein Fehler.
10. **Social-Meta:** In `index.html:8-13` und `en/index.html:8-13` fehlen `og:locale`/`og:locale:alternate` und
    `twitter:card`. Vorschlag: ergänzen, wenn Link-Vorschauen wichtig sind.

## Entscheidungsbedarf für Karl
Keiner zwingend. Optional: ob Sitemap/robots (1) und hreflang für Rechtstexte (2) vor dem Launch gewünscht sind, und
ob Support geteilt werden soll (2). Merge von seo/2026-11 bleibt wie bisher offen (WUM-5, Karls Freigabe).
