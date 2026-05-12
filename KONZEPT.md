# Konzept: June OS – Persönliches KI-Betriebssystem

> Status: Arbeitsdokument, v0.2
> Sprache: Deutsch
> Ziel: Gemeinsame Grundlage für alle weiteren Entscheidungen, bevor irgendeine KI Code erzeugen darf.

---

## 1. Vision

June OS ist kein klassisches Betriebssystem, sondern eine **persönliche KI-Infrastruktur**.

Sie nimmt Eingaben über mehrere Kanäle entgegen — primär **Sprache**, daneben Text, Bild und Chat — und macht daraus systematisch nutzbare Lösungen. Aus einer formulierten Anforderung entstehen automatisch:

- strukturierte Anforderungen
- Datenmodelle und APIs
- Services oder Module
- Deployment
- neue Funktionen in der einheitlichen Oberfläche June Use

**Beispiel:** „Ich möchte Mandanten und Projekte verwalten." → June versteht, fragt nach, entwirft Entitäten und Felder, leitet APIs ab, erzeugt das Modul, deployt es und macht es in June Use verfügbar.

June OS bildet außerdem das **persönliche Arbeitsgedächtnis** seines Nutzers ab:

- Was weiß ich?
- Was habe ich notiert?
- Was ist offen?
- Wer muss was tun?
- Worauf warte ich?
- Was wurde entschieden?
- Was muss ich nachverfolgen?
- Welche Information gehört zu welchem Kontext?

---

## 2. Grundlage: Bestehendes June-Projekt

Aufgesetzt wird auf den vorhandenen Bausteinen:

- **Client-Architektur**
- **Wissensmanagement**
- **June Use** – Mobile- und Desktop-Oberfläche (zentraler Interaktionspunkt, primär Chat im Thread)
- **June Aware** – Wissensdatenbank / Wissenssystem

Das Konzept erweitert June zu einer persönlichen Plattform für Wissen, Anwendungen, Agenten, Infrastruktur, Deployment und Automatisierung.

---

## 3. Leitprinzipien

### 3.1 Local-first, Cloud-assisted, Relay-supported

- Primäre Instanz läuft **lokal** beim Nutzer.
- Rechenintensive Aufgaben **können** in die Cloud ausgelagert werden.
- Bei Internetausfall arbeitet das System **lokal weiter**.
- **Relay-Server** im Internet erhöhen die weltweite Erreichbarkeit.

### 3.2 Ende-zu-Ende-Verschlüsselung

- Ver- und Entschlüsselung **ausschließlich auf dem Client**.
- Relay- und Cloud-Komponenten transportieren und cachen nur verschlüsselte Daten.
- Schlüsselverwaltung ist zentrales Architekturthema.

### 3.3 Standards vor Automatisierung

> Erst Standards definieren, dann Automatisierung erlauben.

Die KI entwickelt nicht frei, sondern füllt feste Artefakte aus (Anforderungsspezifikation, Datenmodell, API-Modell, Architekturentscheidung, Deployment-Plan, Sicherheitsbewertung, Betriebsmodell).

### 3.4 Modularer Monolith als Default

Architekturentscheidung (siehe §5.1): Neue Anwendungen entstehen standardmäßig als **Module im June Core**. Eigenständige Services nur in Ausnahmefällen (Skalierung, Isolation, Laufzeit, Sicherheit). Beide Varianten folgen demselben Standard und erscheinen einheitlich in June Use.

### 3.5 Notes & Tasks als systemweite Objekte

> June OS behandelt **Notes** und **Tasks** als zentrale, systemweite Objekte. Notizen bilden das persönliche Wiki und die Wissensbasis des Nutzers. Aufgaben bilden die operative Handlungsebene des Systems. Beide Objekttypen können manuell entstehen, durch KI vorgeschlagen werden und mit Projekten, Mandanten, Personen, Gesprächen und Wissen verknüpft werden.

Beide sind **keine optionalen Apps**, sondern **Foundation Modules** im June Core (siehe §5.3, Details in §6).

---

## 4. Systemkomponenten

### 4.1 Lokaler Hauptserver

Primäre Instanz und **Source of Truth**. Beherbergt:

- zentrale Services
- Hauptdatenhaltung
- June Aware / Wissensdatenbank
- lokale Ausführungsumgebung
- lokale Steuerung
- lokalen Relay-Server

### 4.2 Relay-Server

Passives Verfügbarkeitsnetz an verschiedenen Standorten im Internet.

**Aufgaben:**
- Inhalte vom Hauptserver holen
- verschlüsselte Pakete zwischenspeichern
- Inhalte ausliefern, wenn Hauptserver nicht erreichbar ist
- weltweiten Zugriff ermöglichen

**Abgrenzung:**
- **Kein** CDN (bewusste Namenswahl).
- **Keine** intelligenten Datenhalter.
- **Können nicht** entschlüsseln.
- Reine Fallback- und Verfügbarkeitskomponenten.

### 4.3 Client (June Use) – intelligenter Zugriffspunkt

Der Client trifft die Zugriffsentscheidungen:

- Welche Quelle ist erreichbar (lokaler Hauptserver, lokaler Relay, Cloud-Relay)?
- Welche Quelle hat die aktuellsten Daten?
- Was ist Source of Truth, was nur Fallback?
- Gibt es Konflikte? Wann wird synchronisiert?

June Use ist damit **Oberfläche + intelligente Zugriffslogik**.

---

## 5. June Code – Entwicklungs- und Service-Standard

June Code ist nicht nur Techstack, sondern **verbindlicher Standard** für alle Anwendungen.

Er definiert:
Architektur-Patterns, Service-Templates, API-Konventionen, Datenbank-Konventionen, Deployment, Security, Logging, Monitoring, Skalierung, Hintergrundjobs, Service-Kommunikation und Regeln für KI-generierte Software.

### 5.1 Architekturentscheidung: Modularer Monolith (hybrid)

> June Code basiert initial auf einem modularen Core-System. Neue Anwendungen werden standardmäßig als Module innerhalb dieses Core-Systems umgesetzt. Nur Anwendungen mit besonderen Anforderungen an Skalierung, Isolation, Laufzeit oder Sicherheit werden als eigenständige Services umgesetzt. Beide Varianten folgen demselben API-first-Standard und erscheinen einheitlich in June Use.

| Variante | Wann | Vorteile | Nachteile |
|---|---|---|---|
| **Modul im June Core** (Default) | Standardfall | einfach, schnelle Entwicklung, gemeinsame Daten, lokal lauffähig, KI-wartbar | weniger feingranulare Skalierung, Modulgrenzen müssen diszipliniert sein |
| **Eigenständiger Service** (Ausnahme) | besondere Skalierung / Isolation / Laufzeit / Sicherheit | klare Trennung, isolierter Betrieb, eigene Skalierung | mehr Infra-Aufwand, mehr Monitoring, mehr Service-Kommunikation |

### 5.2 Techstack-Anforderungen

Minimal, einheitlich, stabil, API-first, lokal lauffähig, cloudfähig, gut automatisierbar, **gut geeignet für KI-generierte Software**, schneller Weg von Datenmodell zu API, geeignet für Hintergrundjobs, Self-Healing, Skalierung – mit **wenig manuellem Wartungsaufwand**.

View bleibt einheitlich: **June Use Mobile + Desktop**.

### 5.3 Foundation Modules im June Core

Der June Core enthält initial die folgenden Foundation Modules. Andere Anwendungen (eigene Module oder Services) dürfen darauf zugreifen, aber sie nicht ersetzen.

| # | Modul | Zweck |
|---|---|---|
| 1 | **Identity & Access** | Nutzer, Geräte, Schlüssel, Berechtigungen |
| 2 | **Knowledge / June Aware** | Wissensbasis, Embeddings, Kontext |
| 3 | **Notes** | Persönliches Wiki, dauerhafte Wissensartefakte (Detail §6) |
| 4 | **Tasks** | Operative Handlungsebene, Follow-ups (Detail §6) |
| 5 | **People** | Personen, Beziehungen, Verantwortlichkeiten |
| 6 | **Projects** | Projekte, Mandanten, Arbeitskontexte |
| 7 | **Context & Conversations** | Threads, Transkripte, Eingabekanäle |
| 8 | **Agent Memory** | Anforderungen, Entscheidungen, Agenten-Kommunikation |
| 9 | **Events / Timeline** | Zeitliche Abfolge, Historie, Audit |
| 10 | **Notifications / Reminders** | Erinnerungen, Eskalationen, Follow-ups |

Notes und Tasks werden im nächsten Abschnitt eigens vertieft, weil sie die **stärksten Querverbindungen** im System haben.

---

## 6. Notes & Tasks im Detail

### 6.1 Aufgaben (Tasks)

**Operative Handlungsebene** des Systems.

**Quellen / Erstellung**
- manuell durch den Nutzer
- automatisch aus Gesprächen, Notizen, Dokumenten, Anforderungen
- KI-Vorschlag
- aus Projekten / Mandaten / Workflows
- aus Deadlines, Zusagen, offenen Punkten

**Zuweisung**
- an sich selbst
- an Dritte (auch wenn diese **nicht** im System arbeiten)
- Zweck der externen Zuweisung: Tracking, Verantwortlichkeit, Follow-up, Nachhaken, offene Rückmeldungen

**Automatische Erkennung soll erfassen:**
mögliche Aufgaben · offene Punkte · Deadlines · Fristen · zugesagte nächste Schritte · Verantwortlichkeiten · Follow-up-Bedarfe · Erinnerungen · Abhängigkeiten zwischen Aufgaben.

**Beispiel:** Im Gespräch fällt „Ich schicke dir das bis Freitag." → June erkennt: erwartete Lieferung, dritte Person verantwortlich, Deadline Freitag, automatisches Follow-up bei Ausbleiben.

**Feldvorschlag (Task-Objekt):**
Titel · Beschreibung · Status · Priorität · Verantwortliche Person · Zugewiesen an · Erstellt von · Quelle · Kontext · Projektbezug · Mandatsbezug · Deadline · Follow-up-Datum · Erinnerungslogik · Abhängigkeiten · Historie · KI-Begründung (bei automatisch vorgeschlagenen Aufgaben).

### 6.2 Notizen (Notes)

**Persönliches Wiki** und Wissensbasis des Nutzers — nicht bloße Textablage.

**Inhalte (offen, nicht abschließend):**
Gedanken · Gesprächsnotizen · Projekt-/Mandatsinformationen · technische Konzepte · Architekturentscheidungen · persönliche Erkenntnisse · Meeting-Zusammenfassungen · Prozesswissen · Rechercheergebnisse · Ideen · offene Fragen · Entscheidungen · Lerninhalte.

**Anforderungen:**
- dauerhaft speicherbar
- verlinkbar (untereinander und zu anderen Foundation-Modules)
- durchsuchbar
- verknüpfbar mit Projekten, Mandanten, Personen, Aufgaben
- KI-verständlich und wiederverwendbar
- integriert in **June Aware**
- manuell **und** automatisch erstellbar

### 6.3 Querverbindungen (Pflicht)

Notes und Tasks dürfen **nicht isoliert** existieren. Sie verbinden sich mit:

June Use · June Aware · June Code · Mandanten · Projekten · Personen · Gesprächen · Dokumenten · Anforderungen · Agenten-Kommunikation · Architekturentscheidungen · Deployments · Erinnerungen · Kalendern · Workflows.

### 6.4 Offene Frage: Grad der KI-Unterstützung

Hier ist **kritische Festlegung** nötig, bevor das Modul standardisiert wird:

- Soll June Notizen automatisch strukturieren?
- Automatisch Zusammenfassungen erstellen?
- Wiki-Seiten aus Gesprächen erzeugen?
- Verknüpfungen zwischen Notizen erkennen?
- Aufgaben aus Notizen ableiten?
- Wissen automatisch aktualisieren?
- Veraltete oder widersprüchliche Informationen erkennen?
- **Vorschläge machen statt direkt ändern?** (Default-Empfehlung: ja)

Empfehlung: Diese Fragen werden in **Dokument 4 (Agent Operating Model)** verbindlich entschieden und an die Autonomiestufen (§7.3) gekoppelt.

### 6.5 Begründung der Foundation-Modul-Position

Notes und Tasks starten **nicht** als separate Services, sondern als Foundation Modules im June Core, weil:

- sie für fast alle anderen Module relevant sind,
- sie viele Querverbindungen haben,
- sie zusammen mit Wissen, Projekten und Personen den Arbeitskontext bilden,
- sie tief in June Use integriert sein müssen,
- sie Grundlage für KI-Assistenz, Follow-ups und persönliches Wissensmanagement sind.

---

## 7. Datenarchitektur

### 7.1 Datenarten

| Art | Beispiele | Anforderung |
|---|---|---|
| **Operative Daten** | Mandanten, Projekte, **Tasks**, Kontakte, Termine | strukturierte Datenhaltung |
| **Wissensdaten** | **Notes**, Dokumente, Transkripte, Zusammenfassungen, Embeddings, Kontext | gehören in June Aware |
| **Agenten-Gedächtnis** | Anforderungen, Rückfragen, Entscheidungen, Begründungen, Architekturänderungen, Deployment-/Fehlerhistorie, offene Punkte | Operations-Gedächtnis, auditierbar |
| **Relay-Daten** | verschlüsselte Datenpakete, Snapshots, Cache-Stände, Replikations-/Aktualitätsmarker | **nur verschlüsselt** |
| **Temporäre Daten** | Zwischenergebnisse, LLM-Ausgaben, laufende Jobs, Queues | klare Lösch-/Lebenszyklusregeln |

### 7.2 Source of Truth & Konfliktbehandlung

Muss explizit geregelt sein:
- Was ist primär, was nur Cache, was Replik?
- Wer darf schreiben, wer nur lesen?
- Wie wird Aktualität markiert und erkannt?
- Welche Quelle gewinnt bei Konflikten?

**Schreibzugriffe offline** (z. B. Heimserver down, Client mobil): Lokale Schreibung mit späterer Synchronisation und Konfliktauflösung — Mechanismus ist **noch zu entscheiden** (siehe §9).

### 7.3 Auswirkung der E2E-Verschlüsselung

E2E ist gesetzt, macht aber komplexer:
- **Suche** und semantische Verarbeitung können nicht beliebig auf Relays stattfinden.
- Indizes und Metadaten brauchen eigene Strategie.
- Wissensverarbeitung läuft lokal, clientseitig oder in **explizit vertrauenswürdigen** Ausführungsumgebungen.

---

## 8. Agentenmodell

### 8.1 Rollen

| Rolle | Aufgabe |
|---|---|
| **Requirements-Agent** | Bedarf klären, Rückfragen, Entitäten erkennen, Prozesse verstehen, Felder vorschlagen, Spezifikation erstellen |
| **Development-Agent** | Anforderungen in Software übersetzen, feste Patterns nutzen, Coding-Systeme kapseln (z. B. Codex, Claude Code), mehrere Instanzen koordinieren |
| **Deployment-Agent** | Bauen, Deployen, Versionieren, Rollback, neue Funktionen registrieren, June Use informieren |
| **Infrastructure-Agent** | Services hoch-/runterfahren, Ressourcen, Skalierung, Self-Healing, Health-Checks, lokale + Cloud-Koordination |
| **Knowledge-Agent / June Aware** | Wissen speichern, Kontext liefern, Anforderungen & Entscheidungen dokumentieren, Agenten-Kommunikation sichtbar machen, Projektgedächtnis |

**Offen:** Ob diese Rollen als echte Agents, Skills, Workflows oder Services umgesetzt werden (siehe §9).

### 8.2 Gemeinsame Wissensbasis

Alle Rollen greifen auf dieselben Quellen zu und sehen die Konversationen/Entscheidungen der anderen. Sie enthält mindestens:

Nutzeranforderungen · Rückfragen & Antworten · Architekturentscheidungen · Datenmodelle · API-Spezifikationen · Deployment-Zustände · Fehlerhistorie · Agenten-Kommunikation · Service-Abhängigkeiten · Versionen · offene Entscheidungen.

Ziel: keine Informationsverluste, keine Widersprüche, gemeinsame Arbeitsgrundlage, Nachvollziehbarkeit, Auditierbarkeit, Wiederverwendung.

### 8.3 Autonomiestufen

Agenten dürfen Infrastruktur nicht beliebig verändern. Festzulegen sind Stufen:

1. **Nur Vorschlag**
2. **Plan erstellen**
3. **Simulation**
4. **Ausführung nach Freigabe**
5. **Automatische Ausführung innerhalb definierter Grenzen**

Die KI-Eingriffe auf Notes/Tasks (§6.4) werden an diese Stufen gekoppelt — Default ist **Stufe 1 (Vorschlag)**, automatische Ableitung von Tasks aus Gesprächen erfordert explizite Freigabegrenzen.

---

## 9. Kritische Architekturfragen / Offene Punkte

1. **Source of Truth** je Datenart eindeutig festlegen.
2. **Offline-Schreibzugriffe**: Mechanismus für Konflikterkennung, Versionierung, Auflösung.
3. **Aktualitätsmarker**: wie der Client zwischen aktuell / veraltet / Fallback unterscheidet.
4. **E2E vs. Funktionalität**: Strategie für Suche, semantische Analyse, Indizes, Metadaten, Schlüsselmanagement.
5. **Agenten-Autonomiestufen**: konkrete Grenzen je Rolle.
6. **Agent vs. Skill vs. Workflow vs. Service**: technische Form der Rollen aus §8.1.
7. **Modulgrenzen im Monolith**: wie wir Disziplin erzwingen, damit Module nicht verwachsen.
8. **Grad der KI-Unterstützung für Notes/Tasks** (§6.4): welche automatischen Aktionen erlaubt sind, was nur Vorschlag bleibt.
9. **Notes ↔ June Aware**: wie genau Notizen in die Wissensbasis integriert werden (Speicherung, Embedding, Versionierung, Rückkanäle).
10. **Task-Erkennung aus Gesprächen**: Trigger, Konfidenzschwellen, Rückfragepflicht, Sichtbarkeit für den Nutzer.

---

## 10. Roadmap der Detaildokumente

Das Konzept wird in fünf eigenständige Standards überführt:

| Nr. | Dokument | Inhalt (Kurz) |
|---|---|---|
| 1 | **June OS Zielarchitektur** | Vision, Systembild, Komponenten, Datenflüsse, lokale/Cloud-Ausführung, Relay-Strategie, Sicherheitsprinzipien |
| 2 | **June Code Application Standard** | API-first-Regeln, Datenmodell, Modul- und Service-Struktur, Jobs, Naming, Versionierung, Logging, Health-Checks, Deployment, **Foundation-Module-Schnittstellen** |
| 3 | **June Data Architecture** | Operative / Wissens- / Agenten- / Cache- / Relay-Daten, lokale vs. Cloud, Konfliktregeln, Verschlüsselung, Aktualitätslogik |
| 4 | **June Agent Operating Model** | Rollen, Kommunikationsregeln, gemeinsame Wissensbasis, Entscheidungsprotokolle, Verantwortlichkeiten, Eskalation, **KI-Eingriffsgrenzen bei Notes/Tasks** |
| 5 | **June Infrastructure Standard** | Lokaler Server, lokaler Relay, Cloud-Relay, Cloud-Execution, Pipeline, Self-Healing, Self-Scaling, Backup, Monitoring, Recovery, Zero-Manual-Maintenance |

Ergänzend werden je Foundation Module aus §5.3 **eigene Modul-Spezifikationen** erstellt, beginnend mit **Notes** und **Tasks**.

---

## 11. Empfohlener nächster Schritt

1. **Architekturentscheidungen festschreiben:** §5.1 (Modularer Monolith) und §3.5 (Notes/Tasks als systemweite Objekte).
2. **Dokument 2 — June Code Application Standard** ausarbeiten. Solange dieser Standard nicht existiert, darf keine KI Anwendungen erzeugen.
3. **Modul-Spezifikationen Notes und Tasks** parallel beginnen (Datenmodell, API, Verknüpfungen, KI-Eingriffsgrenzen — letzteres erst nach Klärung der Fragen aus §6.4).
4. Offene Punkte aus §9 priorisieren und je Punkt verantwortliche Entscheidung festhalten.
