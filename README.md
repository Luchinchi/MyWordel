# MyWordel
Selbstgemachtes bekanntes Wordle game, in welchem man versucht ein 5-stelliges Wort zu raten.

### Aufbau
- Logik des Spiels in JavaScript programmiert
- mithilfe von Vue.js eine UI erstellt

### Weitere Funktionen
- Streak System
- Anzahl gewinne, Anzahl verlorener Runden, sowie durchschnittliche Gewinnchance in einem Info Block aktuell gehalten
- Gewinnanimation
- Bootstrap Grid System, responsive design




## Anleitung zum Ausführen
Um das Projekt zu starten müssen folgende Schritte befolgt werden. Eine Vorraussetzung, um das Projekt auszuführen ist Node.js.
Falls Node.js bereits installiert wurde kann man direkt zum Unterkapitel "Projekt Ausführen" springen.

### Node.js installieren (Windows)
1. Besuche die [offizielle Node.js Webseite](https://nodejs.org/)
2. Lade den Installer für Windows herunter (LTS-Version)
3. Führe die Datei aus und folge den Installationsanweisungen
4. Überprüfe die Installation von Node.js mit folgendem befehl:
    ```bash
    node -v
    ```
5. Überprüfe die Installation von npm mit folgendem befehl:
    ```bash
    npm -v
    ```
### Projekt Ausführen
1. Klone das repository
   ```bash
   git clone https://github.com/Luchinchi/MyWordel.git
   ```
2. Wechsel in den "MyWordel" Ordner
3. Installiere die Abhängigkeiten mit npm:
    ```bash
    npm install
    ```
4. Starte die Anwendung:
    ```bash
    npm run serve
    ```
    Dieser Befehl startet einen lokalen Entwicklungsserver und öffnet die Anwendung normalerweise unter http://localhost:8080 in deinem Browser.
