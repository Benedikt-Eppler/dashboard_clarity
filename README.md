# ChatGPT-Clone – Microsoft Clarity Testbed

Eine einzelne, in sich geschlossene `index.html`, die eine GPT-artige Chat-Oberfläche
simuliert. Zweck: eine Testseite mit vielen klickbaren Elementen und scrollbarem
Inhalt, damit **Microsoft Clarity** aussagekräftige Verhaltensdaten sammeln kann
(Heatmaps, Rage/Dead Clicks, Scroll Depth, Device/Browser-Splits).

- **Kein Backend, keine API-Keys, keine externen Libraries/CDNs.**
- Antworten sind **simuliert**: Typing-Indicator → zufällige Antwort aus einem Textpool.
- State ist rein in-memory (JS-Variablen), kein `localStorage`.

**🔗 Live:** https://benedikt-eppler.github.io/dashboard_clarity/

---

## Schnellstart (lokal)

Die Seite braucht keinen Server – Doppelklick auf `index.html` genügt.
Im Terminal:

```bash
open index.html
```

> Tipp: Willst du sie stattdessen über einen lokalen Server ansehen (näher an
> „echt", z. B. damit Clarity später sauber lädt):
>
> ```bash
> python3 -m http.server 8000
> # dann im Browser: http://localhost:8000
> ```

---

## Projekt später wieder öffnen (nachdem alles geschlossen ist)

Der gesamte Zustand liegt im Ordner **`/Users/ben/Projects/dashboard_clarity`**
und im GitHub-Repo. So kommst du zurück:

```bash
cd /Users/ben/Projects/dashboard_clarity   # in den Projektordner
open index.html                            # Seite lokal ansehen
```

Falls der Ordner mal weg ist, klonst du ihn neu:

```bash
git clone https://github.com/Benedikt-Eppler/dashboard_clarity.git
cd dashboard_clarity
open index.html
```

---

## Microsoft Clarity einbauen

1. In Clarity ein Projekt anlegen und die **Project-ID** kopieren
   (Clarity → *Settings → Setup → Install tracking code manually*).
2. In `index.html` im `<head>` den markierten Platzhalter suchen:

   ```html
   <!-- ===== MICROSOFT CLARITY SNIPPET HIER EINFÜGEN ===== -->
   ```

3. Den Kommentar durch dein echtes Snippet ersetzen – Vorlage (nur `DEINE_PROJECT_ID` ersetzen):

   ```html
   <script type="text/javascript">
     (function(c,l,a,r,i,t,y){
       c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
       t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/"+i;
       y=l.getElementsByTagName(r)[0];y.parentNode.insertBefore(t,y);
     })(window, document, "clarity", "script", "DEINE_PROJECT_ID");
   </script>
   ```

4. Änderung deployen (siehe unten). Nach ~1 Min die Live-URL öffnen, ein paar
   Aktionen durchklicken → in Clarity erscheinen nach kurzer Verzögerung die
   ersten Sessions.

> ⚠️ Sobald die öffentliche Seite Clarity lädt, werden **alle** Besucher
> aufgezeichnet. Nur zum Testen nutzen – keine echten personenbezogenen Daten
> ins Chatfeld tippen.

---

## Deployen / Änderungen veröffentlichen

Das Repo ist mit GitHub Pages verbunden (Branch `main`, Root). Jeder Push baut
die Seite automatisch neu:

```bash
git add -A
git commit -m "Beschreibung der Änderung"
git push
```

Nach ~1 Min ist die neue Version unter der Live-URL erreichbar.

Build-/Deploy-Status prüfen:

```bash
gh run list --limit 3      # letzte Pages-Builds
```

---

## Was ist alles klickbar? (für Clarity relevant)

- **New chat** – legt ein neues leeres Gespräch an
- **Sidebar** mit 6 fiktiven Chat-Verläufen (aktiver Zustand)
- **Modell-Dropdown** oben (GPT-4o / GPT-4 / GPT-3.5)
- **Empty-State** mit 4 anklickbaren Prompt-Vorschlag-Karten
- **Eingabefeld** unten (Enter = senden, Shift+Enter = Zeilenumbruch)
- Pro Assistant-Nachricht: **Copy / 👍 / 👎 / Regenerate**
- Sidebar-Footer: **Einstellungen / Upgrade**, dazu Suchen-, Teilen-, Anhängen-Buttons

## Qualität

Responsive bis Mobile (Off-Canvas-Sidebar), sichtbarer Keyboard-Focus,
`prefers-reduced-motion` wird respektiert.
