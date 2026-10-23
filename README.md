# Weihnachten im Park 2026 – Helferplanung Käsehütte

Zeitraum: **4.–6. Dezember 2026**. Mobile Anmeldeseite, Zeitwünsche, Änderungen, persönlicher Fixplan und Admin-Dienstplan.

## Zeitfenster für Helfer:innen

- **Freitag, 04.12.2026:** 13:00–23:00 Uhr
- **Samstag, 05.12.2026:** 14:00–23:00 Uhr
- **Sonntag, 06.12.2026:** 13:00–23:00 Uhr

Helfer:innen geben ihre tatsächlich verfügbaren Teilzeiten innerhalb dieser Tagesfenster an. Die Webseite zeigt diese Zeiten im Kopfbereich und direkt an den Eingabefeldern; zusätzliche Prüfungen verhindern die Speicherung außerhalb des Fensters über die Weboberfläche. Bestehende ältere Einteilungen werden nicht eigenmächtig verändert, sondern bei abweichenden Zeiten in der Adminoberfläche markiert.



## Architektur
- `index.html`: öffentliche Oberfläche über GitHub Pages (ohne Google-Balken)
- `config.js`: Adresse der Google-Apps-Script-Webapp (kein Passwort hineinschreiben!)
- `Code.gs`: Backend auf Google Apps Script; schreibt in Google Sheets
- Datenbank: https://docs.google.com/spreadsheets/d/1-wyGW6BNzV9eq2Xks07e1bieB4dfVa16sSebLN0jE5Q/edit

## Einmalige Einrichtung

1. https://script.google.com/home/projects öffnen, **Neues Projekt** erstellen.
2. `Code.gs` aus diesem Repository vollständig in die Datei `Code.gs` kopieren.
3. Funktion `setupWIP` wählen → **Ausführen** → Berechtigungen gewähren.
4. Im Ausführungsprotokoll den **Admin-Schlüssel** kopieren und privat aufbewahren. Alternativ in den Skripteigenschaften `WIP_ADMIN_KEY` ansehen. **Niemals bei GitHub speichern.**
5. Bereitstellen → **Neue Bereitstellung** → Typ **Web-App** → Ausführen als **Ich** → Zugriff **Jeder** → Bereitstellen. Die `/exec`-URL kopieren.
6. In `config.js` die URL zwischen die Anführungszeichen eintragen und auf GitHub speichern.
7. GitHub Pages für das verwendete öffentliche Repository aktivieren: Einstellungen → Pages → Deploy from a branch → `main`, `/(root)`. Der Ordner wird unter `https://michaelmejtsky.github.io/Weihnachten-im-Park/` erreichbar, **sofern GitHub Pages dort aktiviert ist**.

## Benutzung

Helfer senden Verfügbarkeiten und Wünsche ohne Konto, erhalten auf demselben Gerät einen persönlichen Bearbeitungszugang (Browser-Speicher). Im Reiter „Meine Einteilung“ sehen sie nach der Veröffentlichung ihre fixen Zeiten. Als Organisator unter „Verwaltung“ den geheimen Admin-Schlüssel eingeben, Dienste als Entwurf anlegen und über „Plan veröffentlichen“ fixieren.

**Datenschutz:** Personen mit dem Anmelde-Link können den veröffentlichten Dienstplan sehen. Namen werden nur angezeigt, wenn die jeweilige Person beim Eintragen ausdrücklich zugestimmt hat; sonst steht „Helfer:in“. Telefonnummern und Notizen stehen nur im Adminbereich. Das Backend benötigt die öffentliche Apps-Script-Webapp; bitte nicht den Google-Sheet-Link oder den Admin-Schlüssel in der WhatsApp-Gruppe teilen. Verfügbarkeit ist keine automatische fixe Zusage.

**Hinweis:** Ohne Script-Bereitstellung und URL in `config.js` ist die Seite nur eine Vorschau und nimmt keine Anmeldungen entgegen. GitHub Pages braucht möglicherweise wenige Minuten zum Aktualisieren.

## Erweiterung Oktober 2026 – Abbau, Notizen und Inventar

- Montag, **7. Dezember 2026**, Abbauarbeiten **ab 07:30 Uhr**, kein festgelegtes Veranstaltungsende. Freiwillige Unterstützung für die Feuerwehr: „Viele Hände, schnelles Ende“.
- Helfer:innen können für den Abbautag ein voraussichtliches Zeitfenster ab 07:30 Uhr angeben; die technische Tagesgrenze 23:59 Uhr ist kein angekündigtes Enddatum/-zeit.
- Im geschützten Bereich **Verwaltung** gibt es **Notizen & Aufgaben** (Titel, Text, offen/erledigt) und **Inventar & Einkauf** (Artikel, Kategorie, Menge, Einheit, Status, Anmerkung).
- Die Daten werden in den Tabellen **Notizen** und **Inventar** desselben Google Sheets gespeichert, nicht öffentlich angezeigt.

### Nach Backend-Änderungen unbedingt aktualisieren

1. Aktuellen Inhalt von `Code.gs` vollständig in **dem bereits bestehenden** Google-Apps-Script-Projekt ersetzen (keinen zweiten Google-Apps-Script-Dienst anlegen).
2. **Speichern**, Funktion **`setupWIP` einmal ausführen** (bestehender Admin-Schlüssel bleibt dabei unverändert).
3. **Bereitstellen → Bereitstellungen verwalten → Web-App bearbeiten (Stiftsymbol) → Version: Neue Version → Bereitstellen**. Nicht nur speichern: Die bestehende `/exec`-URL läuft andernfalls weiter mit der alten Version.
4. Die GitHub-Webapp aktualisieren und im Adminbereich Notizen/Inventar prüfen. Den privaten Admin-Schlüssel nicht in GitHub hinterlegen.

Hinweis: Die Weboberfläche sperrt Einträge außerhalb der konfigurierten Tagesfenster; die aktualisierte Backend-Version überprüft dies ebenfalls. Bereits vorhandene Schichten werden nicht automatisch überschrieben oder gelöscht.

## Inventar: PDF, Ausdruck und Schnellbestätigung

Im geschützten Bereich **Verwaltung → Inventar & Einkauf** stehen zur Verfügung:

- **PDF herunterladen:** erzeugt einen A4-Inventarauszug aus der aktuell gefilterten Liste (Kategorien, Artikel, Menge, Status, Anmerkungen, Anzahl offener Prüfungen), mit mehrseitiger Gestaltung. Benötigt die jsPDF-Bibliothek über CDN.
- **Druckansicht:** öffnet einen für A4 gestalteten Bericht. Dort kann gedruckt oder über den Browser **Als PDF speichern** gewählt werden; dies funktioniert auch als Fallback, falls die externe PDF-Bibliothek nicht geladen werden kann.
- **Schnellbestätigung:** „Weitergeleitet“, „Besorgt“, bei Prüfeinträgen außerdem „Vorhanden bestätigt“. Vor dem Speichern erscheint eine Bestätigung.
- Statusänderungen benötigen den Admin-Schlüssel. Das Backend schreibt **nur die Statusspalte und den Zeitpunkt**, ohne Menge oder Notizen versehentlich zu überschreiben. Erledigte Einträge bleiben erhalten und können nötigenfalls über Bearbeiten erneut geöffnet werden.
- Die neuen Status „Weitergeleitet“ und „Besorgt“ werden auch im Formular unterstützt.

**Wichtig:** Die neue Aktion `setInventoryStatus` benötigt ein frisches Apps-Script-Deployment. Deshalb die aktualisierte `Code.gs` speichern und die bestehende Web-App über **Bereitstellungen verwalten → Neue Version → Bereitstellen** aktualisieren. Der reine GitHub-Upload genügt nicht für den neuen Speicher-Endpoint.

