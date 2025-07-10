# Tag 1  
## Thema: Einführung in Datenbanken & ER-Modelle

---

### Was ist eine Datenbank?

Eine strukturierte Sammlung von Daten, die effizient gespeichert, verwaltet und abgefragt werden kann.  
Ziel: Daten zentral und sicher speichern, Redundanz vermeiden, Datenintegrität gewährleisten.

---

### Grundbegriffe

- **Entität:** Ein reales Objekt oder Konzept (z. B. Person, Auto).  
- **Attribut:** Eigenschaften einer Entität (z. B. Name, Alter).  
- **Schlüssel:** Attribut oder Attributkombination, die eine Entität eindeutig identifiziert.

---

### ER-Modell (Entity-Relationship-Modell)

Ein grafisches Modell zur Darstellung von Daten und deren Beziehungen.

- **Entitäten** werden als Rechtecke dargestellt.  
- **Attribute** als Ovale.  
- **Beziehungen** als Rauten.  
- **Schlüsselattribute** unterstrichen.

---

### Beispiel ER-Diagramm (textuell)

**Entität:** Person  
- Name (Schlüssel)  
- Alter  
- Adresse  

**Entität:** Auto  
- Kennzeichen (Schlüssel)  
- Marke  
- Baujahr  

**Beziehung:** Besitzt  
- Person – Auto (1:n)

---

### Warum ER-Modelle?

- Visuelle Strukturierung von Daten  
- Einfache Kommunikation zwischen Entwicklern und Fachabteilungen  
- Grundlage für Datenbankentwurf

---

### Normalisierung (kurz)

- Prozess zur Minimierung von Redundanz und Inkonsistenzen  
- Typische Normalformen: 1NF, 2NF, 3NF

---

### Fazit Tag 1

Versteh, was deine Daten sind, wie sie zusammenhängen und wie man das klar darstellt.

---

### TL;DR

Datenbank = ordentliches Datenlager.  
ER-Modell = visuelle Landkarte deiner Daten.  
Normalisierung = Aufräumen, bevor’s chaotisch wird.

---


# Tag 2  
## Thema: Generalisierung & Spezialisierung in Datenbanken

---

### Was ist Generalisierung / Spezialisierung?

**Generalisierung:**  
Zusammenfassung gemeinsamer Attribute mehrerer Entitäten in eine übergeordnete Entität.  
→ Ziel: Wiederverwendbarkeit, Redundanz vermeiden, saubere Struktur.

**Spezialisierung:**  
Erweiterung einer allgemeinen Entität um spezielle Attribute.  
→ Ziel: Genauere Modellierung, spezifische Eigenschaften abbilden.

**Merkregel:**  
„Ein Fahrer ist eine Person.“ = **is-a-Beziehung**

---

### Beispiel aus dem Unterricht

#### Ohne Generalisierung

- **Fahrer**: Name, Vorname, Telefonnummer, Führerscheinklasse  
- **Disponent**: Name, Vorname, Telefonnummer, Planungsbereich  

Problem: Wiederholte Attribute → Redundanz

#### Mit Generalisierung

**Person** (Oberklasse):  
- Name  
- Vorname  
- Telefonnummer  

**Fahrer** (Unterklasse):  
- Führerscheinklasse  

**Disponent** (Unterklasse):  
- Planungsbereich  

---

### ER-Modell (Textuell)

**Entitäten:**

**Person**  
- ID (PK)  
- Name  
- Vorname  
- Telefonnummer  

**Fahrer**  
- ID (PK, FK → Person.ID)  
- Führerscheinklasse  

**Disponent**  
- ID (PK, FK → Person.ID)  
- Planungsbereich  

---

### Eigenes Beispiel

**Tier** (Oberklasse)  
- Name  
- Alter  

**Hund** (Unterklasse)  
- Rasse  

**Katze** (Unterklasse)  
- Lieblingsplatz  

---

### Weitere Klassenbeispiele

| Oberklasse  | Unterklassen             |
|-------------|--------------------------|
| Fahrzeug    | PKW, LKW                 |
| Mitarbeiter | Verkäufer, Techniker      |
| Produkt     | Lebensmittel, Elektronik |
| Benutzer    | Admin, Standardnutzer    |
| Tier        | Hund, Katze              |
| Konto       | Girokonto, Sparkonto     |

---

### SQL-Code zur Umsetzung

```sql
-- Oberklasse
CREATE TABLE Person (
  ID INT PRIMARY KEY,
  Name VARCHAR(50),
  Vorname VARCHAR(30),
  Telefonnummer VARCHAR(12)
);

-- Unterklasse Fahrer
CREATE TABLE Fahrer (
  ID INT PRIMARY KEY,
  Fuehrerscheinklasse VARCHAR(10),
  FOREIGN KEY (ID) REFERENCES Person(ID)
);

-- Unterklasse Disponent
CREATE TABLE Disponent (
  ID INT PRIMARY KEY,
  Planungsbereich VARCHAR(50),
  FOREIGN KEY (ID) REFERENCES Person(ID)
);
```

# Tag 3

---

## Datentypen in MariaDB/MySQL

| Datentyp                   | Beschreibung                              | Beispiel              |
|----------------------------|------------------------------------------|-----------------------|
| Ganze Zahlen               | INT, TINYINT, SMALLINT, MEDIUMINT, BIGINT | -                     |
| Natürliche Zahlen          | UNSIGNED Versionen der ganzen Zahlen     | -                     |
| Festkommazahlen (Decimal)  | DECIMAL(M,D), M = Stellen, D = Nachkommastellen | DECIMAL(6,2) = 1234.56 |
| Aufzählungstypen           | ENUM, SET                                | -                     |
| Boolean (logisch)          | BOOL oder TINYINT(1)                     | -                     |
| Zeichen (einzelnes Zeichen)| CHAR(1)                                 | -                     |
| Gleitkommazahlen           | FLOAT, DOUBLE                           | -                     |
| Zeichenkette fester Länge  | CHAR(n)                                | -                     |
| Zeichenkette variabler Länge| VARCHAR(n)                             | -                     |
| Datum und/oder Zeit        | DATE, TIME, DATETIME, TIMESTAMP, YEAR  | -                     |
| Binäre Datenobjekte        | BLOB, VARBINARY                         | -                     |
| JSON                       | JSON Datentyp                           | -                     |

---

## Rekursion und Hierarchien in Datenbanken

### 1) Mehrfachbeziehungen  
- Zwei Tabellen können mehrere unabhängige Beziehungen zueinander haben  
- Jede Beziehung braucht eine eindeutige Bezeichnung (Rolle)  
- Beispiel: tbl_Fahrten und tbl_Orte mit mehreren Beziehungstypen (1:n, m:n)  
- Für m:n-Beziehungen benötigt man Transformationstabellen

### 2) Rekursion (strenge Hierarchie)  
- Tabelle bezieht sich auf sich selbst via Fremdschlüssel (z.B. Person → Vorgesetzter)  
- Klassische Firmenhierarchie: Jeder hat genau einen Vorgesetzten (außer der oberste)  
- Fremdschlüssel darf NULL sein (für Chef ohne Vorgesetzten)  
- Beziehung: c:mc (1 Chef für mehrere Mitarbeiter)

### 3) Einfache Hierarchie mit Zwischentabelle  
- Wenn mehrere Vorgesetzte möglich sind → mc:mc-Beziehung  
- Transformationstabelle mit zwei Fremdschlüsseln auf dieselbe Tabelle, unterschiedliche Rollen („ist_Vorgesetzter_von“, „ist_Mitarbeiter_von“)  
- Beispiel: Netzwerkstruktur in der Firmenorganisation

### 4) Stücklistenproblem (Rekursion in der Praxis)  
- Produkte können aus anderen Produkten bestehen (modulare Struktur)  
- Beide – Aggregate und Einzelteile – sind in derselben Produkttabelle  
- Zur Darstellung wird eine zusätzliche Tabelle nötig, die die Zusammensetzung (Komponenten) abbildet  
- Ziel: Auflösen bis auf elementare Einzelteile (Explosionsproblem)

---

## SQL SELECT Basics (DQL)

- `SELECT * FROM tabelle;` → alle Daten anzeigen  
- `SELECT spalte1, spalte2 FROM tabelle;` → nur bestimmte Spalten  
- `WHERE` zum Filtern (`WHERE spalte = wert`)  
- Funktionen: `ROUND()`, `IF()`, `POWER()`, `ABS()`, `SIN()`, `PI()`  
- Variablen: `SET @var = wert;` und Nutzung in Queries  
- Joins zum Kombinieren von Tabellen (nicht im Detail hier)

---

## Wichtige Konzepte

- **NULL in Hierarchie:** Bedeutet kein Vorgesetzter (Wurzelelement)  
- **Referentielle Integrität:** Fremdschlüssel müssen auf gültige Einträge zeigen oder NULL sein  
- **Transformationstabellen:** Werden für m:n-Beziehungen genutzt  
- **Rekursion:** Selbsterreferenzierende Beziehungen in Tabellen

---

*Fazit:*  
Datenbanken brauchen saubere Struktur und klare Regeln für Beziehungen — vor allem bei Hierarchien und Mehrfachbeziehungen. Datentypen richtig zu wählen, ist essenziell für Performance und Korrektheit.



# Tag 4 - Datenbanken Essentials: FK, Constraints & Mengenlehre

---

## Fremdschlüssel (FK)

- FK verweist auf PK der Mastertabelle  
- FK-Constraints sichern referentielle Integrität  
- FK kann NULL sein (außer bei NOT NULL Constraint)  
- Für 1:1 Beziehung: FK + UNIQUE Constraint auf der FK-Spalte  

---

## Constraints

- **NOT NULL:** Wert darf nicht fehlen  
- **UNIQUE:** Wert darf nur einmal vorkommen  
- **FOREIGN KEY:** FK muss gültigen PK referenzieren  

---

## Daten einfügen & Reihenfolge

- Erst Mastertabelle füllen  
- Dann Detailtabelle mit gültigen FK-Werten füllen  
- Fehler wenn FK auf nicht existierenden PK zeigt  

---

## Mengenlehre & JOINs

| Symbol | Bedeutung                  |
|--------|----------------------------|
| ∈      | Element gehört zur Menge    |
| ∉      | Element gehört nicht zur Menge |
| ∩      | Schnittmenge (JOIN)         |
| ∪      | Vereinigung                 |
| \      | Differenz                   |

- Tabellen = Mengen  
- JOIN = Schnittmenge (INNER JOIN)  
- LEFT/RIGHT JOIN = alle von links/rechts + passende von der anderen Seite  

---

## Wichtig

- m:n-Beziehungen brauchen Transformationstabellen  
- Forward Engineering erzeugt DB-Struktur aus Modell  
- FK-Constraints verhindern inkonsistente Daten  


# Tag 5 – Daten löschen, Integrität & Aggregatsfunktionen

---

## Löschen in DBs

- DELETE oft tabu (verlust von Infos + Bruch von Relationen)  
- Statt löschen: Datensatz als „inaktiv“ markieren oder Austrittsdatum  
- Referentielle Integrität schützt vor ungewolltem Löschen  
- Historie ist King (Audit, Recht, Buchhaltung)  

---

## Datenintegrität (4 Regeln)

- Entitätsintegrität: PK eindeutig & nicht leer  
- Referentielle Integrität: FK muss auf existierenden PK zeigen  
- Bereichswertintegrität: passende Datentypen & Werte  
- Benutzerdefinierte Integrität: eigene Regeln (z.B. Emailformat)

---

## FK-Löschregeln (ON DELETE)

| Regel       | Wirkung                              |
|-------------|------------------------------------|
| RESTRICT    | Löschen blockiert, wenn FK existiert (Standard)  |
| CASCADE     | Löschen löscht verknüpfte Datensätze mit  |
| SET NULL    | FK wird NULL gesetzt (wenn FK NULL erlaubt) |

---

## SQL Alias

- Alias für Spalten und Tabellen, nur temporär in Query  
- `SELECT name AS MitarbeiterName`  
- `FROM mitarbeiter AS m` → `m.name`

---

## Aggregatsfunktionen

| Funktion | Beschreibung          |
|----------|----------------------|
| COUNT    | Anzahl Zeilen/Werte   |
| SUM      | Summe                |
| AVG      | Durchschnitt         |
| MIN      | Minimum              |
| MAX      | Maximum              |

---

## GROUP BY

- Gruppiert Zeilen  
- Aggregiert Werte pro Gruppe (z.B. SUM, COUNT)

---

## HAVING

- Filtert Ergebnisse nach GROUP BY  
- Filter auf Aggregatsfunktionen

---

## Merksatz SELECT Aufbau  
**SELECT - FROM - WHERE - GROUP BY - HAVING - ORDER BY - LIMIT**


## Tag 6 ##

# Tag 7 – Datenbanken erstellen & Daten einfügen

---

## DB Backup – Datensicherung von Datenbanken

- Datenbanken sind essenziell für Websites & Unternehmenssoftware  
- Datenverlust = fettes Problem → Ausfall, Daten weg, Kunden vertrauen nicht mehr  
- Ursache meist Hardware-Pech oder User-Fehler, nicht Hacker  
- Backup = Sicherheitskopie, um Daten wiederherzustellen  

---

## Backup-Arten

| Typ                  | Beschreibung                                                                                  |
|----------------------|----------------------------------------------------------------------------------------------|
| Voll-Backup          | Sichert alle Daten + Strukturen komplett → viel Speicher, einfache Wiederherstellung          |
| Differentielles Backup | Erst Vollbackup, dann nur Änderungen seit Vollbackup → spart Speicher, braucht Voll + letztes diff Backup |
| Inkrementelles Backup | Erst Vollbackup, danach nur Änderungen seit letztem Backup (egal ob Voll oder inkrementell) → spart Speicher, braucht alle Backups für Restore |

---

## Backup-Methoden

- **Online-Backup:** DB bleibt an, sichert live, aber komplexer  
- **Offline-Backup:** DB runterfahren → Backup erstellen → DB wieder hoch → einfach, aber Downtime nötig (nacht etc.)

---

## Backup-Tools Beispiele

- **mysqldump:** Shell Tool, schnell, aber nicht immer vom Hoster erlaubt  
- **phpMyAdmin:** Einfach, aber limitiert bei großen DBs (2 MB Limit)  
- **BigDump:** Für große Backups, kann große Dumps importieren, keine Backup-Funktion  
- **HeidiSQL:** Windows, gut für große DBs, aber kein automatischer Backup-Job  
- **Mariabackup:** Open Source, physische Online Backups (Linux & Windows)

---

## Backup-User Beispiel

```sql
GRANT RELOAD, PROCESS, LOCK TABLES, REPLICATION CLIENT ON *.* TO 'backupuser'@'localhost' IDENTIFIED BY 'backup123';
```

# Tag 8 – Recap & Weiterarbeit an Datenbanken

---

## Recap / Q&A Tag 7  
- Wiederholung der Backup-Themen & Normalisierung  

---

Weiterarbeit an Aufgabe „Freifächer“ von Tag 7  
**Wichtig:** Vorbereitung auf LB2! Alle Punkte sauber durcharbeiten! Berechtigungen klären, damit LB2 nicht scheitert.

---

### Aufgabe DB_Freifaecher (Erinnerung)

1. Erste Normalform der Excel-Tabelle erstellen und als CSV exportieren  
   - Atomare Felder, Redundanz bewusst erzeugt  
2. Logisches ERD erstellen  
   - Mind. 5 Tabellen, mindestens 2. Normalform  
   - Kardinalitäten bestimmen  
3. Physisches ERD erstellen  
   - NN- und UQ-Constraints setzen  
   - FK-Constraints definieren  
   - SQL-DDL-Script durch Forward Engineering erzeugen  
4. Daten aus CSV per `LOAD DATA LOCAL INFILE` in DB importieren  
   - FK-Beziehungen überwachen  
5. Daten bereinigen (Redundanz, leere Werte, Inkonsistenzen)  
   - Scripte aus Tag 6 ggf. nutzen  
6. Daten testen  
   - Vergleich CSV vs. DB-Selects → Ausgabe muss identisch sein  
7. 290 neue Schüler-Datensätze mit Testdatengenerator erstellen  
   - Werte anpassen/vermischen in Excel  
   - Punkte 4-6 wiederholen  
8. SELECT-Aufgaben:  
   - Wie viele Teilnehmer hat Inge Sommer in ihrem Freifach?  
   - Liste aller Klassen mit Anzahl SchülerInnen in Freifächern, sortiert nach Klassen  
   - Welche SchülerInnen besuchen „Chor“ oder „Elektronik“?  
9. Ergebnisse von Punkt 8 in Datei speichern mittels `SELECT INTO OUTFILE`  
10. Backup der Datenbank „Freifaecher“ erstellen

---

## Freifächer Tipps

- 1.NF: Atomare Felder, Redundanz bewusst zulassen  
- Logisches ERD in 2.NF  
- Physisches ERD mit Constraints (2.NF)  
- Testdatengenerator: Zufallszahlen, vertikale Verschiebung der Spalten in Excel, Korrektur mit Suchen-Ersetzen  
- 290 Datensätze SchülerInnen generieren

---

### Aufgabe: Steuerdaten Stadt Zürich

- Website Opendata Stadt Zürich besuchen  
- CSV „Median-Einkommen steuerpflichtiger natürlicher Personen“ herunterladen  

---

### Aufgaben Steuerdaten

1. CSV analysieren und normalisieren  
2. Attribute identifizieren & Datentypen festlegen  
3. Quelldatei bereinigen (1.NF, überflüssige Daten entfernen)  
4. Physisches ERD + DDL-Script erstellen  
5. Steuerdaten per Bulkimport in DB laden  
6. DML-Skript schreiben  
7. Felder _p25, _p50, _p75 erklären (Grundlagen Statistik)  
8. Beantworten:  
   - a) Quartier mit maximalem Steuereinkommen bei _p75?  
   - b) Quartier mit niedrigstem Steuereinkommen bei _p50?  
   - c) Quartier mit höchstem Steuereinkommen bei _p50?  
9. Backup der DB erstellen

---

### Aufgabe: Bildungsdaten Bundesamt für Statistik

- Bildungsindikatoren im internationalen Vergleich herunterladen (Excel)  

---

### Aufgaben Bildungsdaten

1. Excel analysieren & normalisieren  
2. Attribute & Datentypen festlegen  
3. Quelldatei bereinigen (1.NF, überflüssige Daten raus)  
4. Mind. 2017 & 2018 berücksichtigen, CSVs exportieren  
5. Physisches ERD + DDL-Script erstellen  
6. CSV-Daten per Bulkimport laden  
7. DML-Skript schreiben  
8. Analyse:  
   - a) Länder mit Schulbesuchsquote 15-19 J unter OECD-Mittel (pro Jahr)  
   - b) Land mit höchster tertiärer Ausbildung über alle Jahre  
   - c) Land mit größter Steigerung abgeschlossener obligatorischer Schulen in 1 Jahr  
9. Backup der DB erstellen

---

### Aufgabe: Eigene DB aus Opendata-Quelle

1. Eigene Opendata-Quelle suchen & Daten herunterladen  
2. Normalisieren & physisches ERD + DDL-Script erstellen  
3. Daten per Bulkimport importieren  
4. DML-Skript schreiben  
5. Daten analysieren & Queries erstellen  
6. Backup der DB erstellen

---

## Merke  
- Saubere Normalisierung + ERD sind Schlüssel  
- Bulkimport erleichtert große Datenmengen  
- Backup nach jedem großen Schritt  
- Analyse mit passenden Queries hilft, den Wert der Daten zu checken


