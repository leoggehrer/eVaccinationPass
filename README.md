# eVaccinationPass

**Inhaltsverzeichnis:**

- [eVaccinationPass](#evaccinationpass)
  - [Einleitung](#einleitung)
  - [Datenstruktur](#datenstruktur)
  - [Backend](#backend)
    - [Authentifizierung](#authentifizierung)
  - [Fontend](#fontend)
  - [Überprüfung der österreichischen Sozialversicherungsnummer (SVNR)](#überprüfung-der-österreichischen-sozialversicherungsnummer-svnr)
    - [Prüfziffer-Berechnung (Mod-11-Verfahren)](#prüfziffer-berechnung-mod-11-verfahren)
    - [Beispiel](#beispiel)
    - [Gültige SVNR](#gültige-svnr)

---

## Einleitung

Das Projekt **eVaccinationPass** ist ein elektronischer Impfpass für die Ausweisung von Impfungen. Zu diesen Impfdaten haben die Einrichtungen Impfbehörde (Vaccination Office) und Executive mit unterschidlichen Rechten Zugriffe. Die Definition der Rechte und Rollen ist im Abschnitt **Authentifizierung** beschrieben.

## Datenstruktur

Die Datenstruktur vom **eVaccinationPass** ist einfach und besteht im Wesentlichen aus einer Komponente welche in der folgenden Tabelle zusammengefasst ist:

| Name          | Type     | Nullable | MaxLength | Beschreibung                          |
|---------------|----------|----------|-----------|---------------------------------------|
| Date          | DateTime | No       | ---       | Datum der Impfung                     |
| Vaccine       | String   | No       | 512       | Verabreichter Impfstoff               |
| SocialNumber  | String   | No       | 10        | Sozialversicherungsnummer             |
| FirstName     | String   | No       | 64        | Vorname des Patienten                 |
| LastName      | String   | No       | 64        | Nachname des Patienten                |
| Doctor        | String   | No       | 64        | Arzt der die Impfung verabreicht hat  |
| Note          | String   | Yes      | 1024      | Anmerkungen                           |

> **Hinweis:** Die Sozialversicherungsnummer muss geprüft werden. Sie finden die Prüfung im Anhang *Überprüfung der österreichischen Sozialversicherungsnummer (SVNR)*

---

## Backend

Erstellen Sie den Backend-Service für das **eVaccinationPass**. Die Umsetzung erfolgt mit Hilfe der Vorlage **SETemplate**. Diese Vorlage ist eine einfache, aber mächtige Vorlage, die Ihnen hilft, schnell und effizient ein Backend-System zu erstellen.
Eine ausführliche Bedienungsanleitung für das Programm **TemplateTools** finden Sie [hier](https://github.com/leoggehrer/SETemplate?tab=readme-ov-file#bedienungsanleitung).

### Authentifizierung

Im letzten Abschnitt wird das Thema **Authentifizierung** behandelt. Nähere Informationen zum Thema Authentifizierung finden Sie [hier](https://github.com/leoggehrer/SETemplate?tab=readme-ov-file#authentifizierung).

In der folgenden Tabelle finden Sie die Definitionen der einzelnen Rollen und deren Möglichkeitetn:

| Entität/Rolle            | Anonym | SysAdmin | VaccinationOffice | Executive |
|--------------------------|--------|----------|-------------------|-----------|
| **Vaccination**          | ---    |   CRUD   |   CRUD            |    R      |

**CRUD...** Create, Read, Update, Delete

## Fontend

Erstellen Sie eine einfache Web-Anwendung in Angular für das **Anzeigen**, **Anlegen**, **Ändern** und **Löschen** von Impfungen. Die Übersichtsseite (Anzeigen) soll mit einer Filtermöglichkeit ausgestatet sein. Die Web-Anwendung soll die Daten vom Backend-Service verwenden.

## Überprüfung der österreichischen Sozialversicherungsnummer (SVNR)

Die österreichische Sozialversicherungsnummer (SVNR) besteht aus **10 Ziffern** im Format:

```text
NNN P DD MM JJ
```

- **NNN**: Laufnummer (dreistellig, beginnt nicht mit 0)
- **P**: Prüfziffer (eine Ziffer)
- **DD MM JJ**: Tag, Monat und Jahr des Geburtsdatums (zweistellig)

---

### Prüfziffer-Berechnung (Mod-11-Verfahren)

Zur Berechnung der Prüfziffer werden die ersten 9 Ziffern (NNN P DDMMJJ) mit festen Gewichten multipliziert:

| Position      | 1 | 2 | 3 | P | 4 | 5 | 6 | 7 | 8 | 9 |
|---------------|---|---|---|---|---|---|---|---|---|---|
| Gewichtung    | 3 | 7 | 9 | - | 5 | 8 | 4 | 2 | 1 | 6 |

1. Multipliziere jede der 9 Ziffern mit ihrem Gewicht.
2. Summiere alle Produkte.
3. Berechne den Rest der Summe durch Division mit 11 (`Summe % 11`).
4. Dieser Rest ist die Prüfziffer **P**.
5. Falls der Rest **10** ist, ist die Kombination **ungültig** – erhöhe die Laufnummer um 1 und versuche es erneut.

---

### Beispiel

Gegeben:

- Laufnummer: `782`
- Geburtsdatum: `28.07.1955` → `280755`

Ziffernfolge (ohne Prüfziffer): `7 8 2 2 8 0 7 5 5`

**Berechnung:**

```text
7×3  = 21
8×7  = 56
2×9  = 18
2×5  = 10
8×8  = 64
0×4  = 0
7×2  = 14
5×1  = 5
5×6  = 30
--------------------
Summe = 218
218 mod 11 = 9 → Prüfziffer = 9
```

**Ergebnis:** Die vollständige SVNR lautet:

```text
7829280755
```

---

### Gültige SVNR

```text
6851030379
0144211253
9492220974
6938101133
9807241279
0379300541
5481140663
4002201082
7017080467
9498200296
6741271294
3190080251
1432030443
1053090788
9714150394
7507251169
1186271005
0042080942
5344060544
4339080204
```

> **Viel Erfolg!**
