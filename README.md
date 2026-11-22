# Schritt-für-Schritt-Einführung in das Mobile-Framework Ionic

📖 **Das Tutorial lesen: [https://stahe.github.io/de-ionic-etape-par-etape-oct-2026/](https://stahe.github.io/de-ionic-etape-par-etape-oct-2026/)**

Dieser Kurs zeigt, wie man mit dem Framework [Ionic](https://ionicframework.com) 9 in Kombination mit [Angular](https://angular.dev) 22 und [Capacitor](https://capacitorjs.com) 8 eine **mobile** App entwickelt: eine Android-App, deren Bildschirme **auf dem Smartphone** gerendert werden, und zwar anhand von JSON-Daten eines Servers. Es handelt sich um eine Web-App (HTML, CSS, TypeScript), die Capacitor zu einer Android-App verpackt – derselbe Code läuft auch in einem Browser.

Er folgt dem Aufbau des Kurses [Schritt-für-Schritt-Einführung in das Mobile-Framework Flutter](https://stahe.github.io/de-flutter-etape-par-etape-sept-2026/) (sowie der Kurse zu [React](https://stahe.github.io/de-react-etape-par-etape-sept-2026/), [Vue.js](https://stahe.github.io/de-vuejs-etape-par-etape-sept-2026/) und [Angular](https://stahe.github.io/de-angular-etape-par-etape-sept-2026/)): derselbe Server, dieselben Bildschirme, im Ionic-Stil für ein Smartphone geschrieben.

| Flutter-Kurs | Ionic-Kurs |
|---|---|
| die Sprache Dart | TypeScript |
| die Flutter-Engine rendert jedes Pixel | native wirkende Web-Komponenten (Material Design unter Android), dargestellt von der WebView des Smartphones |
| Widgets, eine `build()`-Methode, `setState` | Angular-Komponenten, HTML-Templates, Signals (`signal`, `computed`, `effect`) |
| `InheritedWidget`, provider (`ChangeNotifier`) | Dependency Injection von Angular, Services mit Signals |
| `Form` und `FormField` | Reaktive Formulare von Angular |
| go_router, `ShellRoute` + `NavigationBar` | Angular-Router, `ion-tabs`, `CanActivateFn`-Guards |
| `Future`, `Stream` | `Promise`, `Observable` (RxJS), `resource()` |
| `shared_preferences` | `@capacitor/preferences` |
| ein HTTP-Client, der Cookies verwaltet (auf dem Smartphone) | `HttpClient`; auf dem Smartphone setzt und sendet das Plugin CapacitorHttp das Cookie |

Der Server bleibt jedoch unverändert: Es ist der JSON-Server der Anwendung **RdvMedecins**, der bereits von den Clients in Flutter, React, Vue.js und Angular verwendet wird.

## Der Ansatz: viele kurze Beispiele, dann eine Fallstudie

Der Kurs ist um **25 kurze Beispiele** aufgebaut, die sich jeweils auf ein einziges Konzept konzentrieren und dieselben Nummern tragen (und dieselben Bildschirme zeigen) wie die Beispiele der Kurse zu Flutter, React, Vue.js und Angular. Sie bilden ein einziges Ionic-Projekt: ein einziger Befehl `npm install`, danach `npm start`; das Beispiel wählt man im Menü der App (oder über seine URL: http://localhost:8100/ex05).

| Kapitel | Inhalt | Beispiele |
|---|---|---|
| Erste Schritte | ein Ionic-+-Angular-+-Capacitor-Projekt, Ionic-Komponenten, Signals und berechnete Eigenschaften, Templates (`@if`, `@for`), Ereignisse, alle Feldtypen, ein Reducer, Validierung, reaktive Formulare, Formatierung (Pipes, `Intl`) | 01–09 |
| Komponenten | Eingaben (`input`), Ausgaben (`output`, `model`), Content-Projektion (`ng-content`, `ng-template`), ein generisches Array, Lebenszyklus, Dependency Injection, Direktiven und Animationen, Bestätigungsdialog (`AlertController`) | 10–16 |
| Routing | der Angular-Router mit Ionic: Routen, Parameter, `ion-tabs`, Lazy Loading, 404-Seiten, Guards | 17–18 |
| Asynchrone Operationen und gemeinsamer Zustand | `Promise`, `Observable`, asynchrone Generatoren, Timer, Entprellen, veraltete Antworten (`switchMap`), `resource()`, Signal-Services, `@capacitor/preferences`, dunkler Modus | 19–20 |
| Internationalisierung | JSON-Wörterbücher, Parameter, Pluralformen (`Intl.PluralRules`), Datumsangaben, Beträge, lokalisierter Kalender (`ion-datetime`) | 21 |
| Der Server: eine Blackbox | Einrichtung des JSON-Servers, seine API, 48 `curl`-Beispiele, was sich für einen mobilen Client ändert | – |
| Kommunikation mit dem Server | `HttpClient`, der Entwicklungs-Proxy, die Serveradresse (Emulator, Smartphone, Web), CapacitorHttp und das Token-Cookie, eine API-Zugriffsschicht, ein View-Model, den Feldern zugeordnete Serverfehler | 22–25 |

Jedes Beispiel wird mit seinem vollständigen, zeilenweise kommentierten Code und Screenshots seiner Ausführung vorgestellt (automatisch von [Playwright](https://playwright.dev)-Skripten erzeugt, im Ordner `captures/`).

## Der Server: eine Blackbox

Der Server ist der NestJS-Server der vorherigen Lektionen, dessen Controller **JSON** zurückgeben. Diese Lektion behandelt ihn als **Blackbox**: Wir installieren ihn, untersuchen seine API und fragen ihn mit `curl` ab, müssen aber seinen Code nicht lesen (er wird für Neugierige mitgeliefert und kommentiert).

- Alle Fehler haben dasselbe Format: `{ "statusCode": 409, "cle": "ERRORS. LOGIN_TAKEN", "params": {...}, "fields": {...} }` – Übersetzungs-**Schlüssel**, niemals Klartext;
- Authentifizierung über ein JWT-Token in einem `httpOnly`-Cookie: Im Browser speichert der Browser es; auf dem Smartphone übernimmt das Plugin CapacitorHttp dies;
- ein „Testmodus“ für das CAPTCHA, damit die API mit `curl` abgefragt werden kann (und Screenshots automatisch erzeugt werden können).

## Die Fallstudie: der Ionic-Client von RdvMedecins

Eine vollständige Anwendung zur **Terminbuchung in einer Arztpraxis**, bei der **alle** Dateien aufgeführt und kommentiert werden.

- **Modernes Ionic und Angular**: Standalone-Komponenten, Signals, eine Anwendung ohne Zone.js, der neue Kontrollfluss in Templates, `inject()`, ein funktionaler HTTP-Interceptor, `CanActivateFn`-Guards und eine Schichtenarchitektur (API-Zugriff, Zustand, UI).
- **Drei Rollen**: `ADMIN` (verwaltet Ärzte und Kunden), `DOCTOR` (plant und storniert Termine), `USER` (der Patient: bucht Termine für sich selbst und verwaltet sein Konto).
- **Datenschutz**: Ein Patient erhält nie die Namen anderer Patienten – der Server sendet sie nicht.
- **Der gesamte Bildschirmzustand steckt in der URL**: `/agenda?idMedecin=1&jour=2026-10-13&reserver=7`; auf dem Smartphone schließt die Zurück-Taste das Buchungsfenster; im Web funktionieren F5 sowie Zurück/Vor.
- **Serverseitige Validierung**: Formulare zeigen die von der API zurückgegebenen Fehler unter jedem Feld an; optimistisches Sperren, doppelte Namen, bereits vergebener Benutzername ...
- **Sitzung**: wird beim Start wiederhergestellt (`GET /api/auth/moi`), der Ablauf wird an einer einzigen Stelle behandelt (Antwort 401, im Interceptor).
- **Für Mobilgeräte geeignet**: Drawer-Menü (`ion-menu`), auf großen Bildschirmen dauerhaft sichtbar (`ion-split-pane`), Listen statt Tabellen.
- **Französisch / Englisch**, einschließlich des Kalenders.
- **Bereitstellung**: die kompilierte Webversion, die vom JSON-Server selbst ausgeliefert wird, und die Android-App (APK).

## Inhalt des Repositorys

```
exemples_ionic/            die 25 kurzen Beispiele (ein einziges Ionic-Projekt)
rdvmedecins-nestjs-json/   der JSON-Server von RdvMedecins (die „Blackbox“)
rdvmedecins_ionic/         der Ionic-Client der Fallstudie
```

Der Ordner `android/` jedes Projekts wird nicht mitgeliefert: Er wird mit `npx cap add android` erzeugt (siehe Kurs).

## Technologien

Ionic 9 · Angular 22 · Capacitor 8 · TypeScript 6 · RxJS 7 · @capacitor/app · @capacitor/preferences · ionicons · Playwright (Screenshots) · serverseitig: NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## Voraussetzungen

- Die Grundlagen von TypeScript und Angular, die in den Kursen [Einführung in TypeScript anhand von Beispielen](https://stahe.github.io/de-typescript-sept-2026/) und [Schritt-für-Schritt-Einführung in das Framework Angular](https://stahe.github.io/de-angular-etape-par-etape-sept-2026/) behandelt werden; die verwendeten Konzepte werden jedoch in den Beispielen jeweils wiederholt.
- Node.js 24 (oder 22.22.3 und höher, wie von Angular CLI 22 verlangt), Visual Studio Code mit den Erweiterungen Angular Language Service und Ionic, Chrome, Android Studio (für das Android SDK und den Emulator) oder ein Android-Smartphone; ein MySQL-Server (z. B. Laragon unter Windows) für den JSON-Server. Anleitungen zur Installation finden sich im Kurs.

## Autor

Dieser Kurs, seine Beispiele, der Ionic-Client und die Anpassung des JSON-Servers wurden von **Claude**, der KI von [Anthropic](https://www.anthropic.com) (Oktober 2026), im Auftrag von **Serge Tahé** geschrieben.
