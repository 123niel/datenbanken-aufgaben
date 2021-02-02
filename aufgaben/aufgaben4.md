# Aufgaben 4

## Aufgabe 4.1

Stellen Sie fest, ob die folgenden Aussagen wahr oder falsch sind:

- Gruppenfunktionen arbeiten mit einer Vielzahl von Zeilen einer Tabelle, um ein "verdichtetes" Ergebnis zu erzeugen. ✔
- Gruppenfunktionen berücksichtigen NULL-Werte. ✔
- Die WHERE Klausel schränkt die Anzahl der zu bearbeitenden Zeilen ein bevor die Gruppenberechnung stattfindet. ✔

## Aufgabe 4.2

Zeigen Sie Minimum, Maximum, Summe und Durchschnitt der aktuellen Beiträge pro Abteilung an. Geben Sie dazu neben den genannten Werten die Abteilungsnummer, den Abteilungsnamen sowie die Anzahl der Werte an.

```sql
SELECT a.ABT_NR, a.ABT_BEZ, MIN(MI_BEITR_AKT) AS MIN, MAX(MI_BEITR_AKT) as MAX, SUM(MI_BEITR_AKT) as SUMME, AVG(MI_BEITR_AKT) AS DURCHSCHNITT, COUNT(*) AS ANZAHL
FROM abteilungen a, mitglieder m
WHERE m.ABT_NR = a.ABT_NR
GROUP BY a.ABT_NR, a.ABT_BEZ
```

## Aufgabe 4.3

Zeigen Sie Minimum, Maximum, Summe und Durchschnitt der Aufwandsentschädigungen pro Abteilung an. Geben Sie dazu neben den genannten Werten die Abteilungsnummer, den Abteilungsnamen sowie die Anzahl der Werte an. Überlegen Sie sich eine sinnvolle Behandlung der Mitglieder-Datensätze, die keine Aufwandsentschädigung enthalten.

```sql
SELECT a.ABT_NR, a.ABT_BEZ, MIN(m.MI_AUFENT) AS MIN, MAX(m.MI_AUFENT) as MAX, SUM(m.MI_AUFENT) as SUMME, AVG(m.MI_AUFENT) AS DURCHSCHNITT, COUNT(*) AS ANZAHL
FROM abteilungen a, mitglieder m
WHERE m.ABT_NR = a.ABT_NR AND m.MI_AUFENT is not NULL
GROUP BY a.ABT_NR, a.ABT_BEZ
```

## Aufgabe 4.4

Schreiben Sie ein Kommando, mit dem Sie für jede vorhandene Aufwandsentschädigung die Anzahl der Mitglieder angeben, die diese Aufwandsentschädigung erhalten. Mitglieder, die keine Aufwandsentschädigung erhalten, sollen mit Aufwandsentschädigung 0 berücksichtigt werden. Sortieren Sie fallend nach Aufwandsentschädigung.

```sql
SELECT NVL(m.MI_AUFENT, 0) AS AUFENT, COUNT(*)
FROM mitglieder m
GROUP BY m.MI_AUFENT
ORDER BY AUFENT DESC
```

## Aufgabe 4.5

Berechnen Sie die Anzahl der Mitglieder, die andere Mitglieder koordinieren.

```sql
SELECT COUNT(DISTINCT(MIT_KOORD))
FROM mitglieder
```

## Aufgabe 4.6

Schreiben Sie eine Abfrage, die den Unterschied zwischen der geringsten und der höchsten vorhandenen Aufwandsentschädigung berechnet. Geben Sie den Wert unter der Spaltenbezeichnung "Unterschied" aus

```sql
SELECT MAX(MI_AUFENT) - MIN(MI_AUFENT) as Unterschied 
FROM mitglieder
```

## Aufgabe 4.7

Schreiben Sie eine Abfrage, die für die Koordinatoren den jeweils geringsten aktuellen Beitrag eines von ihm/ihr koordinierten Mitglieds anzeigt (dabei soll nur die jeweils nächste Hierarchie- stufe betrachtet werden). Mitglieder, die selbst nicht koordiniert werden, sollen ausgeschlossen werden

```sql
SELECT k.MI_NR, MIN(m.MI_BEITR_AKT)
FROM mitglieder m, mitglieder k
WHERE k.MI_NR = m.MIT_KOORD
AND m.MIT_KOORD IS NOT NULL
GROUP BY k.MI_NR
```
