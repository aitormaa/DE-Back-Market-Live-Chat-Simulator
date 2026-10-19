Back Market - Live-Chat-Trainingssimulator
Ein browserbasiertes Trainingstool für Back Market-Kundendienstmitarbeiter. Zwei Personen verbinden sich mit derselben Live-Chat-Sitzung. Eine Person übernimmt die Rolle des Agenten, die andere die Rolle des Kunden.
Live-Demo
https://aitormaa.github.io/DE-Back-Market-Live-Chat-Simulator/
Funktionen
Live-Sitzungen mit zwei Personen

    Der Agent erstellt einen sechsstelligen Sitzungscode.
    Der Kunde tritt mit diesem Code bei.
    Firebase Realtime Database synchronisiert Sitzungen und Nachrichten zwischen Geräten.
    Firebase Anonymous Authentication schützt den Datenbankzugriff, ohne dass Benutzer ein Konto benötigen.
    PeerJS WebRTC kann zusätzlich eine direkte Verbindung herstellen, wenn das Netzwerk dies erlaubt.
    Wenn WebRTC durch ein Unternehmensnetzwerk blockiert wird, synchronisiert Firebase weiterhin die Sitzung und die Nachrichten.

52 Trainingsszenarien
Stufe	Codebereich	Anzahl	Schwerpunkte
Anfänger	B01-B11	11	Sendungsverfolgung, Rechnungen, Stornierungen, Garantien und Rückgaberecht
Fortgeschritten	I01-I30	30	Gerätemängel, Garantieansprüche, Rückerstattungen und technische Fehlersuche
Experte	A01-A11	11	Betrugsvorwürfe, DSGVO-Themen, schwierige Kunden, rechtliche Drohungen und besonders schutzbedürftige Kunden
Jedes Szenario enthält eine Kundenpersona, Geräte- und Bestelldaten, eine Eröffnungsnachricht, einen Lösungsweg, Hinweise zum Kundenverhalten und Übungsschwerpunkte.
Leistungs-Timer

    FRT: Erste Antwortzeit mit einem Ziel von 60 Sekunden.
    NRT: Zeit bis zur nächsten Antwort mit einem Ziel von 90 Sekunden.
    Sitzungstimer: gesamte Dauer der Sitzung.

Agenten-Tools

    Umschaltung zwischen Antwort und interner Notiz.
    Makro-Bibliothek für häufige Antworten.
    Datei- und Bildanhänge mit clientseitiger Bildkomprimierung.
    Ticket-Felder für Status, Priorität und Kategorie.
    QA-Bewertung und Empathie-Analyse.

Erste Schritte
Live-Version verwenden
Öffne die Live-Demo in zwei Browserfenstern oder auf zwei Geräten:
https://aitormaa.github.io/DE-Back-Market-Live-Chat-Simulator/
Der Agent und der Kunde können unterschiedliche Geräte und Netzwerke verwenden. Beide Benutzer sollten die deutsche Version verwenden. Deutsche Sitzungen werden unter sessions/de gespeichert.
Lokal ausführen
Das Projekt benötigt keinen Build-Schritt:

git clone https://github.com/aitormaa/DE-Back-Market-Live-Chat-Simulator.git
cd DE-Back-Market-Live-Chat-Simulator
python3 -m http.server 8080

Öffne anschließend diese Adresse im Browser:
http://localhost:8080
Ein lokaler Server ist zuverlässiger als das direkte Öffnen der Datei über file://.
Sitzung durchführen
Agent

    Simulator öffnen.
    Rolle "Agent" auswählen.
    Schwierigkeitsstufe auswählen.
    Namen eingeben.
    Den erzeugten sechsstelligen Sitzungscode teilen.
    Warten, bis der Kunde beitritt.

Kunde

    Simulator auf einem anderen Gerät oder in einem anderen Browser öffnen.
    Rolle "Kunde" auswählen.
    Sitzungscode des Agenten eingeben.
    Gespräch starten.

Dateistruktur

DE-Back-Market-Live-Chat-Simulator/
├── index.html
└── README.md

Die Firebase-Webkonfiguration ist in index.html enthalten. Wenn du stattdessen eine Version mit einer separaten firebase-config.js-Datei verwendest, muss diese Datei im selben Ordner wie index.html liegen.
Firebase-Konfiguration
Der Simulator verwendet:

    Firebase Anonymous Authentication
    Firebase Realtime Database
    Firebase als zentrale Quelle für Sitzungen und Nachrichten
    Einen lokalen Browser-Cache für bereits geladene Daten
    PeerJS WebRTC als optionale direkte Verbindung

Deutsche Sitzungen werden unter diesem Pfad gespeichert:

sessions/de/SESSION_CODE

Die Firebase-Webkonfiguration darf in einer Browseranwendung enthalten sein. Der Zugriff wird durch Authentication und die Realtime Database Rules geschützt.
Erforderliche Firebase-Einstellungen

    Anonymous Authentication ist aktiviert.
    Realtime Database ist aktiviert.
    aitormaa.github.io ist als autorisierte Domain eingetragen.
    Die Realtime Database Rules sind veröffentlicht.

Verwende diese Regeln:

Rules
Sessions
$locale
$code
.read
auth != null
.write
auth != null
.validate
newData.hasChildren(['scenario', 'closed'])

Die Regel darf messages nicht als Pflichtfeld verlangen. Eine neue Sitzung startet mit einer leeren Nachrichtenliste, die Firebase zunächst eventuell nicht als eigenen Datenbankknoten speichert.
Architektur

Agent-Browser                    Kunden-Browser
      |                                |
      +------ Firebase Database -------+
      |             und Auth            |
      +------ PeerJS WebRTC, optional --+

Firebase speichert Sitzungen und Nachrichten. Der Browser verwendet zusätzlich einen lokalen Cache. PeerJS kann eine direkte Verbindung herstellen, wenn das Netzwerk WebRTC unterstützt.
Wichtige Daten
Die Szenarien, Makros und automatisierten Nachrichten sind in index.html definiert.
Konstante	Beschreibung
SC	Trainingsszenarien
MACROS	Makrovorlagen für Agenten
AUTO	Automatische Nachrichten bei Inaktivität
PCFG	PeerJS-Verbindungseinstellungen
Bewertung
Empathie-Score
Der Simulator analysiert Agentennachrichten anhand von:

    Empathiebegriffen wie "verstehe", "Entschuldigung" und "helfe gerne".
    Verwendung des Vornamens des Kunden.
    Häufigkeit und Verteilung empathischer Formulierungen im Gespräch.

QA-Score
Die manuelle QA-Bewertung berücksichtigt unter anderem:

    Qualität der Begrüßung.
    Erkennung des Problems.
    Korrektheit der Lösung.
    Einhaltung der Richtlinien.
    Verständlichkeit der Kommunikation.
    Antwortgeschwindigkeit anhand von FRT und NRT.

Fehlerbehebung
Wenn die Browserkonsole folgende Meldung zeigt:

Firebase is not configured. Local-only mode is active.

Prüfe, ob die verwendete index.html die Firebase-Konfiguration enthält.
Wenn folgende Meldung erscheint:

PERMISSION_DENIED

Prüfe, ob Anonymous Authentication aktiviert und die Realtime Database Rules veröffentlicht sind.
Wenn folgende Meldung erscheint:

WebRTC: ICE failed

Konnte PeerJS keine direkte Verbindung herstellen. Firebase sollte die Sitzung und die Nachrichten trotzdem synchronisieren.
Roadmap

    Zusammenführung der vier Sprachrepositorys in ein gemeinsames Repository.
    Administrationsdashboard mit Teamkennzahlen.
    Exportierbarer PDF-Abschlussbericht.
    Speicherung großer Anhänge in Firebase Storage statt in der Realtime Database.

Mitwirkende
Entwickelt von den Back Market-Teams für Learning and Development und Customer Care.
Lizenz
Nur für den internen Gebrauch. Proprietäres Back Market-Tool. Keine öffentliche Weitergabe.
