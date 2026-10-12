# stapel-legal

Website und Rechtstexte zur iPhone-App Stapel, erreichbar unter https://stapel.nxtchange.app
(GitHub Pages, Branch `main`, Wurzelordner, Jekyll mit `jekyll-theme-primer` für die Markdown-Seiten;
`index.html` ist statisch).

- `index.html` Startseite (Platzhalter bis zum Launch: ohne Store-Abzeichen und Bilder)
- `datenschutz.md`, `nutzungsbedingungen.md`, `support.md` → `/datenschutz/`, `/nutzungsbedingungen/`, `/support/`
- `CNAME` bindet die Subdomain; der DNS-Eintrag liegt bei Hostinger: CNAME `stapel` → `nxtchangeconsultingflexco.github.io`

Die Adressen stehen in der App in `App/AppModel.swift` (`Legal`) und gehören in App Store Connect.

## Einrichten

1. Repo-Einstellungen › Pages › Source: Deploy from a branch, `main`, `/ (root)`. Custom domain
   `stapel.nxtchange.app`, „Enforce HTTPS“ nach dem ersten Zertifikat.
2. Hostinger: DNS-Eintrag wie oben. Prüfen mit `dig stapel.nxtchange.app CNAME`.
3. Vor dem Launch: Rechtstexte durch den Anwalt prüfen lassen. Sie sind aus den geprüften Texten von
   Auszeit abgeleitet und um Kamera, Fotos, Dateien, Cloud-Ablage, Sprachmodell auf dem Gerät und
   Mitteilungen ergänzt (Stand 12.10.2026, Entwurf).
4. Englische Fassungen (`privacy.md`, `terms.md`, `en/`) folgen mit der englischen Oberfläche der App.
