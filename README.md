# stapel-legal

Website und Rechtstexte zur iPhone-App Stapel, erreichbar unter https://stapel.nxtchange.app
(GitHub Pages, Branch `main`, Wurzelordner, Jekyll mit `jekyll-theme-primer` für die Markdown-Seiten;
`index.html` ist statisch).

- `index.html` Startseite (bis zum Launch ohne Store-Abzeichen; „Bald im App Store“ ist kein Link). Statisches HTML mit
  eingebettetem CSS, keine externen Schriften, Skripte oder Tracker; `en/index.html` hat dasselbe CSS, Änderungen daran
  in beiden Dateien gleich halten.
- `assets/img/icon-64.png` (Favicon) und `assets/img/icon-180.png` (Apple-Touch-Icon, Markenzeichen im Kopf) und
  `assets/press/stapel-icon-1024.png` (`og:image`, Pressebild): das App-Icon „Rechnung und Bon mit Akzent“ (G2), aus dem
  SVG mit `qlmanage -t -s 1024` gerendert und mit `sips -Z` verkleinert. Dasselbe 180-px-Bild liegt im Register
  nxtchange.app (Repo `apps-site`, `assets/stapel-180.png`).
- `datenschutz.md`, `nutzungsbedingungen.md`, `support.md` → `/datenschutz/`, `/nutzungsbedingungen/`, `/support/`
- `privacy.md`, `terms.md`, `en/index.html` → `/privacy/`, `/terms/`, `/en/` (Englisch, Rückfall für alle Länder außer DE/AT/CH)
- `CNAME` bindet die Subdomain; der DNS-Eintrag liegt bei Hostinger: CNAME `stapel` → `nxtchangeconsultingflexco.github.io`

Die Adressen stehen in der App in `App/AppModel.swift` (`Legal`) und gehören in App Store Connect.

## Design

Farben als CSS-Variablen in `:root` der beiden Startseiten, abgestimmt mit `Shared/Brand.swift` der App:
Papier `--bg:#f6f5f0`, Tinte `--fg:#1b2430`, Grau `--muted:#5d6570`, Linie `--line:#dfe2db`; Petrol `--accent:#1f6f78`
(Marke), `--accent-deep:#1a5f68` (Links, Hauptknopf), `--accent-bright:#2d939d` (Kicker, Schrittnummern, Diagramme),
`--accent-soft:#e3f0f1` (Balkengrund, Plus-Pille), Bernstein `--amber:#e0a43a` (Füllungen) und `--amber-text:#8f5a0d`
(Beträge als Text, Kontrast 5,3:1 auf Papier). Der Dunkelmodus überschreibt die Werte in
`@media (prefers-color-scheme:dark)`; `--on-deep` ist die Schrift auf dem Hauptknopf, `--bar1` der erste Balken (im
Dunkeln sonst gleich mit `--accent-bright`). Überschriften in Serifen („New York“, Georgia), Beträge in `ui-monospace` mit
`font-variant-numeric: tabular-nums` (Klassen `.mono`, `.betrag`). Hero und Abschnitte mit Beispielkarte zweispaltig,
unter 720 px untereinander.

Vorschau ohne Server: `qlmanage -t -s 1400 -o <ordner> index.html` (folgt dem Erscheinungsbild des Macs).

## Einrichten

1. Repo-Einstellungen › Pages › Source: Deploy from a branch, `main`, `/ (root)`. Custom domain
   `stapel.nxtchange.app`, „Enforce HTTPS“ nach dem ersten Zertifikat.
2. Hostinger: DNS-Eintrag wie oben. Prüfen mit `dig stapel.nxtchange.app CNAME`.
3. Vor dem Launch: Rechtstexte durch den Anwalt prüfen lassen. Sie sind aus den geprüften Texten von
   Auszeit abgeleitet und um Kamera, Fotos, Dateien, Cloud-Ablage, Sprachmodell auf dem Gerät und
   Mitteilungen ergänzt (Stand 12.10.2026, Entwurf); Stapel (Privat und Firma) und Übergabe an die Kanzlei
   ergänzt am 14.10.2026 (Entwurf, im Quelltext von `datenschutz.md` und `privacy.md` als Kommentar markiert).
4. Englische Fassungen: `privacy.md` → `/privacy/`, `terms.md` → `/terms/`, Support zweisprachig in `support.md`, Startseite `en/index.html`.
