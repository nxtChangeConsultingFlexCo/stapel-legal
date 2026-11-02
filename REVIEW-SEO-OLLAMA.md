<!-- Zweitmeinung aus Ollama (Alias review = kimi-k3) über /work/bin/llm, 02.11.2026, auf Basis des Diffs main..seo/2026-11.
     Nur gelesen, nichts geändert. Vom Orchestrator (Claude) stichprobenartig gegengeprüft:
     Befund 1 trifft zu (SoftwareApplication mit gleicher @id, inLanguage de bzw. en). Frage 4: og:url und og:image sind auf beiden Startseiten gesetzt.
     Alle übrigen Punkte sind Einschätzungen des Modells und bleiben zu prüfen. -->
# Zweitmeinung (Ollama) zu Branch seo/2026-11 – stapel-legal (stapel.nxtchange.app)

## Kurzurteil

Der Branch setzt Paket A4 technisch sauber um: Description unter 160 Zeichen (nachgezählt: de 149, en 143), selbstreferenzielle Canonicals, vollständig bidirektionales hreflang mit x-default und ein konsistent verketteter JSON-LD-Graph ohne Preise und ohne Store-Links. Einen mittleren Befund gibt es: die SoftwareApplication trägt auf beiden Sprachseiten dieselbe @id, aber widersprüchliche `inLanguage`-Werte (de vs. en). Dazu kommen vier niedrige Punkte (Keyword-Verlust in der Description, elliptische Formulierung, fehlendes `offers` für Rich Results, manuelle Sync-Pflicht laut README). Empfehlung: Freigabe mit Auflagen – den `inLanguage`-Widerspruch vor dem Merge korrigieren, der Rest kann nachgezogen oder bewusst akzeptiert werden.

## Befunde

| Nr | Schwere | Datei/Stelle | Befund | Empfehlung |
|---|---|---|---|---|
| 1 | mittel | `index.html` + `en/index.html`, JSON-LD → `SoftwareApplication` | Gleiche `@id` (`https://stapel.nxtchange.app/#app`), aber je Seite unterschiedliche Werte: `inLanguage` `"de"` vs. `"en"` und unterschiedliche `description`. Parser mergen das zu einer Entität mit widersprüchlichen Sprachangaben. | `inLanguage` auf beiden Seiten identisch als `["de","en"]` setzen (analog zu `WebSite`), oder je Sprache eigene `@id` vergeben (`#app-de` / `#app-en`). |
| 2 | niedrig | `index.html` + `en/index.html`, meta description | Durch die Kürzung entfallen die Passagen „Privat und Firma getrennt, Übergabe an die Kanzlei als ZIP" / „hand-over to your accountant as a ZIP" sowie „App aus Graz" / „An app from Graz, Austria". Damit gehen die differenzierenden Suchbegriffe Kanzlei/accountant, ZIP und Graz verloren. „Ohne Account, ohne Server." ist als generische Aussage der schwächere Teil, blieb aber erhalten. | Prüfen, ob der Verzicht bewusst ist; falls nicht, Text neu balancieren (z. B. Kanzlei-Nennung statt No-account-Satz), Budget < 160 halten. |
| 3 | niedrig | `index.html` + `en/index.html`, meta description | Die Kürzung erzeugt Ellipsen: de „…Preisverlauf, PDF in deinem Cloud-Ordner." (zuvor „Ablage als PDF in deinem Cloud-Ordner"), en „…price history, PDF in your cloud folder." (zuvor „filing as PDF to your cloud folder"). Liest sich telegrammatisch und steht so auch in der `SoftwareApplication.description`. | Formulierung glattziehen, ohne das Zeichenbudget zu sprengen; de ist mit 149 Zeichen nahe am Limit, also eher umverteilen als ergänzen. |
| 4 | niedrig | beide Startseiten, JSON-LD → `SoftwareApplication` | Weder `offers` noch `aggregateRating` vorhanden. Nach Google-Dokumentation ist für den Rich-Result-Typ „Software App" eines der beiden Felder erforderlich; zu erwarten ist daher ein Search-Console-Hinweis und kein Rich Result. Valide bleibt das Markup trotzdem. Angesichts „Bald im App Store" und der Vorgabe „keine Preise" nachvollziehbar. | Beim Store-Launch `offers` nachrüsten; bis dahin Verzicht dokumentieren und die Search-Console-Meldung als erwartet abhaken. |
| 5 | niedrig | `README.md`, neuer Wartungshinweis | Canonical, absolutes hreflang und das komplette JSON-LD liegen redundant in zwei Dateien vor und müssen laut README „von Hand gleich gehalten" werden. Bei jeder Textänderung sind jetzt mindestens drei Stellen pro Sprache betroffen (meta description, JSON-LD `description`, ggf. og-Texte) – Drift zwischen HTML-Meta und JSON-LD ist vorprogrammiert. | Kurzfristig: Punkt in eine PR-Checkliste. Mittelfristig: Jekyll-Include/Layout für den Kopf oder ein einfaches Abgleich-Skript. |

## Geprüft und in Ordnung

- **Description-Länge:** nachgezählt de 149, en 143 Zeichen – Commit-Angaben stimmen, Vorgabe < 160 erfüllt.
- **Canonical:** auf beiden Seiten vorhanden, selbstreferenziell, absolut, Trailing-Slash konsistent (`/` bzw. `/en/`).
- **hreflang:** vollständig bidirektional (de ↔ en), Selbstreferenz enthalten, `x-default` zeigt auf die deutsche Seite, auf beiden Seiten identischer Satz. Die Ersetzung der bisherigen relativen hrefs (`../`, `./`, `./en/`) durch absolute URLs ist eine echte Verbesserung.
- **JSON-LD-Struktur:** `@graph` mit Organization, WebSite, WebPage, SoftwareApplication sauber verkettet (`isPartOf` → `#website`, `about` → `#app`, `publisher` → `#unternehmen`); Organization-`@id` deckt sich mit dem Muster aus Paket A2.
- **Auftragsvorgaben eingehalten:** keine Preise/`offers`, keine Store-Links im JSON-LD.
- **Details korrekt:** `WebSite.inLanguage` als Array `["de","en"]`; `WebPage.name` entspricht je Seite dem `<title>`; unmaskiertes `&` im JSON-String innerhalb von `ld+json` ist zulässig (im `<title>` korrekt als `&amp;`); `image`-URL absolut und verweist auf das in der README belegte Asset `assets/press/stapel-icon-1024.png`.
- **Open Graph:** `og:title`, `og:description`, `og:type` sind im Diff-Kontext unverändert vorhanden; `og:url`, `og:image`, `og:locale` sind aus dem Diff nicht ableitbar (siehe Fragen).

## Restrisiken vor dem Merge

- Manuelle Synchronisation mehrerer Kopf-Blöcke über zwei Dateien (Befund 5): jede zukünftige Description-Änderung muss in Meta-Tag und JSON-LD beider Sprachversionen nachgezogen werden; ein Vergessen erzeugt direkt widersprüchliche Signale.
- Live-Validierung (`validator.schema.org`, Rich-Results-Test, hreflang-Gegenseitigkeit nach Crawl) ist aus dem Diff nicht ableitbar und sollte nach Deploy erfolgen.
- Die Rechtstext-Unterseiten (Impressum/Datenschutz) sind nicht Teil des Diffs; ob dort Canonical/hreflang ergänzt wurden oder bewusst fehlen, ist offen.
- Ohne `offers` dauerhaft kein „Software App"-Rich-Result – beim Store-Launch muss der Kopf ohnehin angefasst werden, was mit Befund 5 zusammenfällt.

## Fragen an den Autor

1. Warum hat die `SoftwareApplication` je Seite ein anderes `inLanguage` (de/en), obwohl die `@id` identisch ist – Absicht oder Überbleibsel? Falls Absicht: warum nicht `["de","en"]` wie beim `WebSite`?
2. Ist der Verzicht auf „Kanzlei/ZIP" und „Graz" in der Description ein bewusster Keyword-Trade-off, oder sollte stattdessen der No-account-Satz weichen?
3. Haben die Unterseiten (Impressum, Datenschutz) Canonical und hreflang, oder sind sie bewusst nicht angereichert?
4. Sind `og:url` und `og:image` auf beiden Startseiten gesetzt? Aus dem Diff ist das nicht ersichtlich, die README deutet das og:image-Asset nur an.
5. Wurde das JSON-LD vor dem Push durch einen Validator (Rich-Results-Test oder validator.schema.org) geprüft, oder steht das noch aus?
