# Konzept: June OS – Persönliches KI-Betriebssystem

> Status: Erstentwurf, Arbeitsdokument
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

---

## 6. Datenarchitektur

### 6.1 Datenarten

| Art | Beispiele | Anforderung |
|---|---|---|
| **Operative Daten** | Mandanten, Projekte, Aufgaben, Kontakte, Termine, Notizen | strukturierte Datenhaltung |
| **Wissensdaten** | Dokumente, Transkripte, Zusammenfassungen, Embeddings, Kontext | gehören in June Aware |
| **Agenten-Gedächtnis** | Anforderungen, Rückfragen, Entscheidungen, Begründungen, Architekturänderungen, Deployment-/Fehlerhistorie, offene Punkte | Operations-Gedächtnis, auditierbar |
| **Relay-Daten** | verschlüsselte Datenpakete, Snapshots, Cache-Stände, Replikations-/Aktualitätsmarker | **nur verschlüsselt** |
| **Temporäre Daten** | Zwischenergebnisse, LLM-Ausgaben, laufende Jobs, Queues | klare Lösch-/Lebenszyklusregeln |

### 6.2 Source of Truth & Konfliktbehandlung

Muss explizit geregelt sein:
- Was ist primär, was nur Cache, was Replik?
- Wer darf schreiben, wer nur lesen?
- Wie wird Aktualität markiert und erkannt?
- Welche Quelle gewinnt bei Konflikten?

**Schreibzugriffe offline** (z. B. Heimserver down, Client mobil): Lokale Schreibung mit späterer Synchronisation und Konfliktauflösung — Mechanismus ist **noch zu entscheiden** (siehe §8).

### 6.3 Auswirkung der E2E-Verschlüsselung

E2E ist gesetzt, macht aber komplexer:
- **Suche** und semantische Verarbeitung können nicht beliebig auf Relays stattfinden.
- Indizes und Metadaten brauchen eigene Strategie.
- Wissensverarbeitung läuft lokal, clientseitig oder in **explizit vertrauenswürdigen** Ausführungsumgebungen.

---

## 7. Agentenmodell

### 7.1 Rollen

| Rolle | Aufgabe |
|---|---|
| **Requirements-Agent** | Bedarf klären, Rückfragen, Entitäten erkennen, Prozesse verstehen, Felder vorschlagen, Spezifikation erstellen |
| **Development-Agent** | Anforderungen in Software übersetzen, feste Patterns nutzen, Coding-Systeme kapseln (z. B. Codex, Claude Code), mehrere Instanzen koordinieren |
| **Deployment-Agent** | Bauen, Deployen, Versionieren, Rollback, neue Funktionen registrieren, June Use informieren |
| **Infrastructure-Agent** | Services hoch-/runterfahren, Ressourcen, Skalierung, Self-Healing, Health-Checks, lokale + Cloud-Koordination |
| **Knowledge-Agent / June Aware** | Wissen speichern, Kontext liefern, Anforderungen & Entscheidungen dokumentieren, Agenten-Kommunikation sichtbar machen, Projektgedächtnis |

**Offen:** Ob diese Rollen als echte Agents, Skills, Workflows oder Services umgesetzt werden (siehe §8).

### 7.2 Gemeinsame Wissensbasis

Alle Rollen greifen auf dieselben Quellen zu und sehen die Konversationen/Entscheidungen der anderen. Sie enthält mindestens:

Nutzeranforderungen · Rückfragen & Antworten · Architekturentscheidungen · Datenmodelle · API-Spezifikationen · Deployment-Zustände · Fehlerhistorie · Agenten-Kommunikation · Service-Abhängigkeiten · Versionen · offene Entscheidungen.

Ziel: keine Informationsverluste, keine Widersprüche, gemeinsame Arbeitsgrundlage, Nachvollziehbarkeit, Auditierbarkeit, Wiederverwendung.

### 7.3 Autonomiestufen

Agenten dürfen Infrastruktur nicht beliebig verändern. Festzulegen sind Stufen:

1. **Nur Vorschlag**
2. **Plan erstellen**
3. **Simulation**
4. **Ausführung nach Freigabe**
5. **Automatische Ausführung innerhalb definierter Grenzen**

---

## 8. Kritische Architekturfragen / Offene Punkte

1. **Source of Truth** je Datenart eindeutig festlegen.
2. **Offline-Schreibzugriffe**: Mechanismus für Konflikterkennung, Versionierung, Auflösung.
3. **Aktualitätsmarker**: wie der Client zwischen aktuell / veraltet / Fallback unterscheidet.
4. **E2E vs. Funktionalität**: Strategie für Suche, semantische Analyse, Indizes, Metadaten, Schlüsselmanagement.
5. **Agenten-Autonomiestufen**: konkrete Grenzen je Rolle.
6. **Agent vs. Skill vs. Workflow vs. Service**: technische Form der Rollen aus §7.1.
7. **Modulgrenzen im Monolith**: wie wir Disziplin erzwingen, damit Module nicht verwachsen.

---

## 9. Roadmap der Detaildokumente

Das Konzept wird in fünf eigenständige Standards überführt:

| Nr. | Dokument | Inhalt (Kurz) |
|---|---|---|
| 1 | **June OS Zielarchitektur** | Vision, Systembild, Komponenten, Datenflüsse, lokale/Cloud-Ausführung, Relay-Strategie, Sicherheitsprinzipien |
| 2 | **June Code Application Standard** | API-first-Regeln, Datenmodell, Modul- und Service-Struktur, Jobs, Naming, Versionierung, Logging, Health-Checks, Deployment |
| 3 | **June Data Architecture** | Operative / Wissens- / Agenten- / Cache- / Relay-Daten, lokale vs. Cloud, Konfliktregeln, Verschlüsselung, Aktualitätslogik |
| 4 | **June Agent Operating Model** | Rollen, Kommunikationsregeln, gemeinsame Wissensbasis, Entscheidungsprotokolle, Verantwortlichkeiten, Eskalation |
| 5 | **June Infrastructure Standard** | Lokaler Server, lokaler Relay, Cloud-Relay, Cloud-Execution, Pipeline, Self-Healing, Self-Scaling, Backup, Monitoring, Recovery, Zero-Manual-Maintenance |

---

## 10. Empfohlener nächster Schritt

1. **Architekturentscheidung aus §5.1 festschreiben** (Modularer Monolith, hybrid).
2. **Dokument 2 — June Code Application Standard** als erstes ausarbeiten. Solange dieser Standard nicht existiert, darf keine KI Anwendungen erzeugen.
3. Parallel: offene Punkte aus §8 priorisieren und je Punkt verantwortliche Entscheidung festhalten.
