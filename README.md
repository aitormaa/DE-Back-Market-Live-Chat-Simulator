Back Market – Live Chat Training Simulator
Ein interaktives, browserbasiertes Trainingstool für Back Market-Kundendienst-Mitarbeiter. Zwei Benutzer verbinden sich in Echtzeit: einer spielt den Agenten, der andere den Kunden, um den Live-Chat-Support anhand realistischer Szenarien zu üben.

    Live-Demo: https://aitormaa.github.io/DE-Back-Market-Live-Chat-Simulator/

Funktionen
Multiplayer für zwei Benutzer

    Zwei Teilnehmer verbinden sich über einen gemeinsamen Sitzungscode
    Ein Benutzer wählt den Schwierigkeitsgrad und übernimmt die Rolle des Agenten
    Der andere tritt über den Code bei und wird zum Kunden
    Echtzeit-Synchronisation via PeerJS (WebRTC) mit localStorage + BroadcastChannel als Fallback
    Funktioniert in zwei Browser-Tabs auf demselben Gerät oder auf zwei verschiedenen Geräten

52 Trainingsszenarien – 3 Schwierigkeitsstufen
Stufe	Codebereich	Szenarien	Schwerpunkte
Anfänger	B01–B11	11	Sendungsverfolgung, Rechnungen, Stornierungen, Garantien, Rückgaberecht
Fortgeschritten	I01–I30	30	Geräteschäden, Garantieansprüche, Rückerstattungsstreitigkeiten, technische Fehlersuche
Experte	A01–A11	11	Betrugsvorwürfe, DSGVO-Verstöße, feindseligen Kunden, rechtliche Drohungen, vulnerable Nutzer
Jedes Szenario enthält:

    Kundenpersona (Name, Gerät, Bestelldetails, Stimmung)
    Eröffnungsnachricht des Kunden
    Lösungsweg (nur für den Agenten sichtbar)
    Hinweise zum Kundenverhalten
    Kernherausforderungen zum Üben

Leistungs-Timer

    FRT – Erste Antwortzeit (Ziel: unter 60 Sek.), läuft ab Sitzungsbeginn automatisch
    NRT – Nächste Antwortzeit (Ziel: unter 90 Sek.), wird bei jeder Kundennachricht zurückgesetzt
    Sitzungs-Timer – gesamte verstrichene Zeit

Automatisiertes Kundenverhalten

    Keine Agentenantwort nach 60 Sekunden: automatische Warteschlangen-Nachricht
    Noch immer keine Antwort nach 120 Sekunden: Chat wird automatisch in den asynchronen Modus übertragen

Agenten-Tools

    ANTWORT / NOTIZ-Umschalter (interne Notizen sind für den Kunden nicht sichtbar)
    Makro-Bibliothek (8 vorgefertigte Antworten: Begrüßung, Warteschleife, Eskalation, Abschluss, Zusammenfassung usw.)
    Datei- und Bild-Upload mit clientseitiger Bildkomprimierung (max. 600px, JPEG 65 %)
    Ticket-Felder: Status, Priorität, Kategorie

QA-Bewertung und Empathie-Analyse

    Automatischer Empathie-Score aus Agentennachrichten (Schlüsselworterkennung, Nutzung des Kundennamens, Häufigkeit)
    Manuelles QA-Checklisten-Scoring mit Gewichtung
    Vollständiger Abschlussbericht beim Schließen des Chats

Erste Schritte
Option 1 – Live-Version verwenden
Keine Installation nötig. Öffne https://aitormaa.github.io/DE-Back-Market-Live-Chat-Simulator/ in zwei Browser-Fenstern oder auf zwei Geräten.
Option 2 – Lokal ausführen
Da es sich um eine einzelne HTML-Datei ohne Build-Schritt handelt:

git clone https://github.com/aitormaa/backmarket-chat-simulator.git
cd backmarket-chat-simulator
open index.html

    Hinweis: Einige Browser schränken localStorage und BroadcastChannel bei file://-URLs ein. Für beste Ergebnisse einen lokalen Server verwenden:

npx serve .
# oder
python3 -m http.server 8080

Sitzungsablauf
Agent (Trainer oder Trainee)

    Simulator öffnen
    Schwierigkeitsgrad wählen: Anfänger, Fortgeschritten oder Experte
    Namen eingeben
    Ein 6-stelliger Sitzungscode wird generiert – diesen mit dem Partner teilen
    Auf den Kunden warten, dann beginnt der Chat

Kunde (Rollenspieler)

    Simulator in einem anderen Tab oder Gerät öffnen
    Auf „Als Kunde beitreten" klicken
    Den vom Agenten geteilten Sitzungscode eingeben
    Das Szenario lädt automatisch – Chat kann beginnen

Dateistruktur

backmarket-chat-simulator/
└── index.html        # Gesamte Anwendung – eigenständig, keine Abhängigkeiten

Die App nutzt CDN-Bibliotheken (kein npm erforderlich):

    React 18 – UI-Framework
    Tailwind CSS – Styling
    Babel Standalone – JSX-Transpilierung im Browser
    PeerJS 1.5 – WebRTC Peer-to-Peer-Verbindung

Architektur

AGENT (Tab / Gerät A)                    KUNDE (Tab / Gerät B)

Erstellt Sitzungscode                    Tritt per Sitzungscode bei
Erhält Szenario + Hinweise               Erhält Szenario (ohne Hinweise)
Sieht QA-Bewertung                       Sieht nur den kundenseitigen Chat

          |                                        |
          +------------- PeerJS WebRTC ------------+
               BroadcastChannel + localStorage (Fallback)

Sitzungsdatenfluss:

    Agent erstellt Sitzung – Szenario wird unter dem Sitzungscode in localStorage gespeichert
    PeerJS sendet INIT-Payload bei Verbindungsaufbau an den Kunden
    Nachrichten werden über WebRTC DataChannel + localStorage übertragen
    BroadcastChannel hält Tabs auf demselben Gerät synchron (400-ms-Polling als Fallback)

Konfiguration
Alle Szenarien, Makros und automatisierten Nachrichten sind als einfache JavaScript-Arrays am Anfang von index.html definiert – kein Backend oder Datenbank erforderlich.
Konstante	Beschreibung
SC	Array aller 52 Trainingsszenarien
MACROS	Agenten-Makrovorlagen (unterstützt {CUSTOMER_FNAME}, {CURRENT_USER_FNAME})
AUTO	Automatische Nachrichten bei Agenteninaktivität nach 60 und 120 Sekunden
PCFG	PeerJS-Konfiguration (STUN-Server)
Bewertungsdetails
Empathie-Score (automatisch berechnet, max. 25 Punkte)
Analysiert alle Agentennachrichten auf:

    Empathieschlüsselwörter (verstehe, entschuldigung, tut mir leid, helfe gerne, usw.)
    Verwendung des Vornamens des Kunden
    Häufigkeit und Konsistenz im Gesprächsverlauf

QA-Score (manuelle Checkliste)
Bewerter prüfen Punkte in folgenden Kategorien:

    Qualität der Begrüßung
    Problemidentifikation
    Korrektheit der Lösung
    Einhaltung der Richtlinien
    Kommunikationsklarheit
    Antwortgeschwindigkeit (FRT- und NRT-Ziele)

Roadmap

    Firebase / Supabase-Backend für persistente Sitzungshistorie
    Admin-Dashboard mit teamweiter Analyse
    Spanische Sprachversion (index-es.html)
    Trainer-gesteuerte Szenario-Einspielung während der Sitzung
    Exportierbarer PDF-Abschlussbericht

Mitwirkende
Entwickelt vom Back Market Learning & Development / Customer Care Team.
Lizenz
Nur für den internen Gebrauch – proprietäres Back Market-Tool. Keine öffentliche Weitergabe.


writeWell
