# Eine einfache Wetter-App, um VueJs und Quasar kennen zu lernen

In diesem Projekt bauen wir gemeinsam Schritt für Schritt eine Wetter-App mit Quasar auf. Quasar ist ein Framework um ... ein Framework. Lang vorbei sind die Zeiten, in denen man komplexe Anwendungingen mit purem Javascript erstellt. Googles AngularJS, Facebooks ReactJS und auch das hier verwendete VueJS-Framework kapseln die Hürden, die Du als Entwickler sonst nehmen müsstest elegant weg und erlauben Dir, Dich auf Dein Projekt zu konzentrieren, ohne Browser spezifischen Fehlern nachzujagen.
Quasar erlaubt uns Apps basierend auf VueJS zu ertellen, die dann wahlweise im Browser, als auch als App auf Deinem Smartphone (mit Hilfe von Corodva) oder als vollwärtige Desktop-App (mit Hilfe von Elektron) ausführbar sind.

Lass uns ein neues Quasar-Projekt aufsetzen! Dieses Repository kannst Du gerne zum "Spicken" verwenden. Es ist aufgeteilt in Branches, auf die im entsprechenden Mayflower-Blogartikel verwiesen werden. Technologien wie npm, yarn, git, die Konsole und Betriebssystem abhängige Abweichungen werden hier nicht gesondert erklärt. Auf Linux und Mac gibt es quasi keine Unterschiede und mit Windows bin ich selbst nicht vertraut.

## Installation
1. Installiere Quasar global: `yarn global add quasar-cli`
2. Initialisiere ein neues leeres Quasar-Projekt, an dem wir jetzt gemeinsam arbeiten werden mit `quasar init <ORDNER_NAME>`
3. Um diesen Branch lauffähig zu bekommen, installiere die javascript-Abhängigkeiten mit `yarn` oder `npm install`

Die Ausgabe bei Dir sollte dann in etwa so aussehen (ich habe das Projekt in einem bereits bestehenden Ordner installiert):

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

## Weiter geht's'
Viel Spaß damit, wir sehen uns im nächsten Branch 02-output-geolocation mit `git checkout 02-output-geolocation`!
