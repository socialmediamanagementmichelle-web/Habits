# Mein Habit Tracker

Persönliche iPad-Web-App (PWA) ohne Login und ohne Backend. Alle Daten liegen im Browser (localStorage).

## Projektstruktur

```
habit-tracker/
├── index.html            # Die komplette App: Layout, Design (CSS) und Logik (JS)
├── manifest.webmanifest  # PWA-Infos: Name, Icon, Farben, Standalone-Modus
├── sw.js                 # Service Worker → App funktioniert offline
├── icon-180.png          # iOS Home-Bildschirm-Icon
├── icon-192.png
├── icon-512.png
└── README.md
```

Alles bewusst in einer Datei (`index.html`), damit die erste Version einfach bleibt. Der Code ist in nummerierte Abschnitte gegliedert (Datenmodell, Datum, Scoring, Rendering je Ansicht, Export/Import), sodass er sich später sauber in Module aufteilen lässt.

## Auf dem Mac/PC testen

1. Terminal im Ordner `habit-tracker` öffnen.
2. `python3 -m http.server 8080` ausführen.
3. Im Browser `http://localhost:8080` öffnen.

## Auf dem iPad nutzen (empfohlener Weg)

Safari braucht für eine „richtige" PWA (Offline + Home-Bildschirm) eine https-Adresse. Am einfachsten geht das kostenlos mit Netlify Drop oder GitHub Pages:

**Netlify Drop (ca. 2 Minuten, kein Account nötig)**
1. https://app.netlify.com/drop öffnen.
2. Den Ordner `habit-tracker` per Drag & Drop hochladen.
3. Du bekommst eine Adresse wie `https://xyz.netlify.app`.
4. Auf dem iPad in Safari öffnen → Teilen-Symbol → „Zum Home-Bildschirm".

**GitHub Pages**
1. Neues Repository anlegen, alle Dateien hochladen.
2. Settings → Pages → Branch `main` wählen.
3. Adresse `https://DEINNAME.github.io/REPONAME/` auf dem iPad öffnen und zum Home-Bildschirm hinzufügen.

Danach startet die App ohne Safari-Leiste, funktioniert offline und merkt sich deine Daten.

## Wichtig zu den Daten

- Die Daten liegen nur auf diesem Gerät in Safari. Wenn du Safari-Daten löschst oder die App vom Home-Bildschirm entfernst, sind sie weg.
- Deshalb: regelmäßig unter **Einstellungen → Daten sichern** ein JSON exportieren (in iCloud Drive speichern). Mit „JSON importieren" lässt sich alles wiederherstellen.
- CSV eignet sich für Excel/Numbers; zum Wiederherstellen bitte JSON verwenden.

## Code aktualisieren

Wenn du `index.html` änderst, in `sw.js` die `CACHE_VERSION` hochzählen (z. B. `habits-v2`), damit das iPad die neue Version lädt.

## Spätere Integrationen (Garmin, FatSecret)

In `DEFAULT_SETTINGS.integrations` ist der Platzhalter angelegt. Beide Dienste brauchen OAuth und API-Schlüssel, die nicht im Browser liegen dürfen – dafür wäre später ein kleines Backend (z. B. eine Serverless-Funktion) nötig, das Schritte/Kalorien abholt und in `state.days[datum].steps` bzw. `.calories` schreibt.
