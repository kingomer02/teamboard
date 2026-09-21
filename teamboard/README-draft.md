# Teamboard

## Konzeptentwurf

Teamboard ist ein geschuetztes Kanban-Ticketboard fuer Teams, die geschaeftliche Aufgaben zentral erfassen, verteilen und bearbeiten. Tickets werden auf einem Board nach ihrem Bearbeitungsstatus organisiert und koennen einzelnen Teammitgliedern zugewiesen werden.

## Ziel

Das System soll den gesamten Lebenszyklus eines Geschaeftstickets sichtbar machen:

1. Ein Ticket wird erstellt.
2. Es wird einer verantwortlichen Person zugewiesen.
3. Die verantwortliche Person bearbeitet das Ticket.
4. Nach Abschluss wird das Ticket als erledigt markiert.

Dadurch ist jederzeit erkennbar, welche Aufgaben offen sind, woran gearbeitet wird und was bereits abgeschlossen wurde.

## Kernfunktionen

### Anmeldung und Sicherheit

- Anmeldung mit Benutzername oder E-Mail-Adresse und Passwort
- Zwei-Faktor-Authentifizierung (2FA) als zusaetzliche Sicherheitsstufe
- Geschuetzter Zugriff auf das Board erst nach erfolgreicher Anmeldung
- Abmeldung aus jeder geschuetzten Ansicht
- Rollen- und Berechtigungspruefung fuer administrative Aktionen

### Kanban-Board

Das Board besteht aus drei Spalten:

- **To Do**: Neue und noch nicht gestartete Tickets
- **In Progress**: Tickets, die aktuell bearbeitet werden
- **Done**: Abgeschlossene Tickets

Tickets sollen zwischen den Spalten verschoben werden koennen. Beim Verschieben wird der Status des Tickets aktualisiert.

### Ticketverwaltung

Ein Ticket kann mindestens folgende Informationen enthalten:

- Titel
- Beschreibung
- Status
- Verantwortliche Person
- Ersteller
- Erstellungsdatum
- Letzte Aktualisierung

Vorgesehene Aktionen:

- Ticket erstellen
- Ticket anzeigen
- Ticket bearbeiten
- Ticket einer Person zuweisen
- Status aendern
- Ticket abschliessen
- Ticket loeschen, sofern die Berechtigung vorhanden ist

## Rollen und Berechtigungen

### Teammitglied

- Tickets anzeigen
- Tickets erstellen
- Eigene Tickets bearbeiten
- Zugewiesene Tickets bearbeiten
- Status von Tickets aktualisieren

### Administrator

- Alle Teammitglied-Funktionen
- Tickets loeschen
- Tickets beliebigen Personen zuweisen
- Benutzer verwalten
- Rollen und 2FA-Status verwalten

Die genauen Berechtigungen sollten vor der technischen Umsetzung bestaetigt werden.

## Vorgeschlagene Benutzeroberflaeche

### Login

- Eingabefelder fuer E-Mail-Adresse und Passwort
- 2FA-Schritt nach erfolgreicher Passwortpruefung
- Verstaendliche Fehlermeldungen bei ungueltigen Zugangsdaten oder Codes

### Board-Ansicht

- Kopfbereich mit Projektname, angemeldetem Benutzer und Abmeldung
- Drei klar getrennte Kanban-Spalten
- Ticketkarten mit Titel, Status und zugewiesener Person
- Schaltflaeche zum Erstellen eines neuen Tickets
- Filter nach verantwortlicher Person und Status als moegliche Erweiterung

### Ticket-Detailansicht

- Vollstaendige Ticketinformationen
- Bearbeiten- und Zuweisen-Aktion
- Statusauswahl
- Anzeige von Ersteller und Zeitstempeln

## Beispielablauf

1. Ein Benutzer meldet sich mit Passwort an.
2. Der Benutzer bestaetigt die Anmeldung mit dem 2FA-Code.
3. Auf dem Board erstellt er ein neues Ticket mit Titel und Beschreibung.
4. Das Ticket erscheint in `To Do`.
5. Ein Teammitglied wird dem Ticket zugewiesen.
6. Die Bearbeitung startet und das Ticket wechselt nach `In Progress`.
7. Nach Abschluss wird das Ticket nach `Done` verschoben.

## Datenmodell, erster Entwurf

### User

- `id`
- `name`
- `email`
- `passwordHash`
- `role`
- `twoFactorEnabled`
- `twoFactorSecret` (verschluesselt speichern)
- `createdAt`

### Ticket

- `id`
- `title`
- `description`
- `status`
- `assigneeId`
- `createdById`
- `createdAt`
- `updatedAt`
- `completedAt`

Der Status sollte technisch als kontrollierter Enum-Wert gespeichert werden, zum Beispiel `TODO`, `IN_PROGRESS` und `DONE`, statt als frei editierbarer Text.

## Sicherheitsanforderungen

- Passwoerter niemals im Klartext speichern
- Passwort-Hashing mit einem etablierten Verfahren
- 2FA-Secrets verschluesselt speichern
- Sitzungen sicher verwalten und bei Abmeldung invalidieren
- Autorisierung serverseitig pruefen, nicht nur in der Benutzeroberflaeche
- Eingaben validieren und gegen typische Injection-Angriffe absichern
- Schutz vor CSRF und Brute-Force-Anmeldeversuchen vorsehen
- Sicherheitsrelevante Aktionen protokollieren

## MVP-Umfang

Die erste Version sollte enthalten:

- Benutzeranmeldung
- 2FA-Anmeldung
- Rollen Teammitglied und Administrator
- Kanban-Board mit den drei Statuswerten
- Ticket erstellen, bearbeiten und anzeigen
- Ticket zuweisen
- Status aendern
- Sicher abmelden

Nicht zwingend fuer das MVP sind Kommentare, Dateianhaenge, Benachrichtigungen, Volltextsuche und umfangreiche Filter. Diese Funktionen koennen spaeter ergaenzt werden.

## Erfolgskriterien

Das MVP gilt als funktional, wenn:

- nur angemeldete und per 2FA bestaetigte Benutzer das Board sehen;
- ein berechtigter Benutzer ein Ticket erstellen kann;
- Tickets eindeutig einem Status zugeordnet sind;
- Tickets einer verantwortlichen Person zugewiesen werden koennen;
- Statusaenderungen auf dem Board sichtbar und dauerhaft gespeichert werden;
- abgeschlossene Tickets in `Done` erscheinen;
- unberechtigte Aktionen serverseitig abgewiesen werden.

## Offene Entscheidungen

Vor Beginn der Umsetzung sollten folgende Punkte geklaert werden:

- Welche Technologie und Datenbank werden verwendet?
- Ist 2FA per Authenticator-App, E-Mail oder SMS vorgesehen?
- Duerfen Teammitglieder Tickets beliebiger Personen bearbeiten?
- Soll ein Ticket geloescht oder nur archiviert werden koennen?
- Wird das Board fuer ein einzelnes Team oder mehrere Teams verwendet?
- Werden Prioritaet, Faelligkeitsdatum oder Labels benoetigt?
- Soll es eine Historie fuer Status- und Zuweisungsaenderungen geben?
