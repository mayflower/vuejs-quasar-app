# Eine einfache Wetter-App, um VueJs und Quasar kennen zu lernen

In diesem Projekt kannst Du Schritt für Schritt eine Wetter-App mit Quasar (auf Basis von VueJS) bauen, die sowohl im Browser, als auch als App auf Deinem Smartphone (mit Hilfe von Corodva) oder als vollwärtige Desktop-App (mit Hilfe von Elektron) ausführbar ist.

Die Idee ist, dass wir gemeinsam ein kleines VueJS-Projekt aufsetzen und Du kann dieses Repository zum "Spicken" verwenden kannst. Dieses Repository ist aufgeteilt in Branches, die dem jeweilgen Blog-Artikeln auf Mayflower entsprechen.

## Installation
1. Installiere Quasar: `yarn global add quasar-cli`
2. Erstelle ein leeres Quasar-Projekt, an dem wir jetzt gemeinsam arbeiten werden mit `quasar init <ORDNER_NAME>`
3. Um diesen Branch lauffähig zu bekommen, installiere die javascript-Abhängigkeiten mit `yarn` oder `npm install`

Bei mir sieht die Ausgabe dann so aus (ich habe das Projekt in einem bereits bestehenden Ordner installiert):

``` bash
 Running command: vue init 'quasarframework/quasar-starter-kit' .

? Generate project in current directory? Yes
? Project name (internal usage for dev) vue-simple-app
? Project product name (official name) Vueather
? Project description A simple VueJS / Quasar weather app to dabble in those technologies
? Author Florian Fackler <florian.fackler@mayflower.de>
? Check the features needed for your project: ESLint, Axios, IE11 support
? Pick an ESLint preset Standard
? Cordova id (disregard if not building mobile apps) de.mayflower.vueather
? Should we run `npm install` for you after the project has been created? (recommended) yarn
```

Jetzt bist Du bereit, ein wenig mit Deiner neuen App herum zu experimentieren, bevor wir weiter machen.
Mit `quasar dev` (im neu erstellten Projekt-Ordner) startest Du eine Voransicht in Deinem Standard-Browser (http://localhost:8080/#/) und bei jedem Speichervorgang aktualisiert sich die Ansicht im Browser auch sofort. Wenn Du also 2 Bildschirme zur Verfügung hast, kannst Du der App beim Enstehen in Echtzeit zusehen.

Viel Spaß damit, wir sehen uns im nächsten Branch 02-output-geolocation mit `git checkout 02-output-geolocation`!
