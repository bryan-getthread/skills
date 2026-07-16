---
name: Tagesübersicht
description: Ein Techniker bittet um eine Zusammenfassung seiner offenen Tickets — was auf Antwort wartet, was dringend ist, was heute geplant ist — in unter einer Minute überfliegbar, mit einer ultrakurzen 3-Zeilen-Variante. (German daily digest of a technician's open tickets — replies due, urgent items, today's schedule.)
category: Localized
tools: [search_tickets, search_members]
---

# Tagesübersicht

Deutsche Version von `skills/personal-productivity/daily-digest/SKILL.md` — wird manuell synchron gehalten.

Die Morgenlektüre: alles, was auf dem Tisch liegt, sortiert danach, was jetzt Aufmerksamkeit braucht. Gebaut, um in unter einer Minute überflogen zu werden und die allererste Aufgabe des Tages beim Namen zu nennen. Zugleich das Muster, das die meisten Partner als geplanten Morgen-Skill laufen lassen.

## Wann einsetzen

- „Gib mir eine Übersicht über meine offenen Tickets“ / „wer wartet auf Antwort, gibt es etwas Dringendes?“
- „Morgenübersicht“ / „wie sieht mein Tag aus?“
- „Kurzfassung“ — die 3-Zeilen-Variante
- Eingebettet in einen Flow als geplantes Briefing zum Arbeitsbeginn

## Schritte

1. Alle offenen Tickets des anfragenden Mitglieds mit search_tickets abrufen. Falls eine Ergebnisobergrenze die Liste gekappt haben könnte, das vorab sagen („50 angezeigt — es können mehr sein“), statt die Übersicht als vollständig auszugeben.
2. Jedes Ticket in genau eine Rubrik einsortieren, in dieser Prioritätsreihenfolge (ein Ticket landet in der ersten Rubrik, für die es sich qualifiziert):
   - **Wartet auf Ihre Antwort** — der Kunde hat zuletzt geantwortet und wartet. Nach Wartedauer sortieren.
   - **Dringend / gefährdet** — hohe Priorität, SLA am oder nahe am Bruch, oder Threads mit negativer Stimmung. Nach Schwere sortieren.
   - **Heute geplant** — Tickets mit Termineintrag oder zugesagter Nachverfolgung heute, in zeitlicher Reihenfolge.
   - **Wartet auf Dritte** — Kunde, Hersteller oder Eskalation ist am Zug. Alles markieren, was 3+ Tage still ist (Kandidat für eine Erinnerung), ohne dass diese Rubrik den Kopf der Seite verdrängt.
   - **Alles Übrige** — nur Anzahl und eine Charakterisierung in einer Zeile; keine Einzelzeilen pro Ticket.
3. Jede Ticketzeile ist eine einzige Zeile: Nummer, Kunde (kurz), Zustand in 5–8 Wörtern und die nächste Aktion („#1234 <client> — Drucker seit 2 T offline, Kunde hat gestern geantwortet → prüfen, ob der Fix greift“).
4. Die Übersicht mit **Hier anfangen:** eröffnen — das wichtigste Ticket und ein Satz, warum (die am längsten wartende Kundenantwort schlägt vage Dringlichkeit; ein harter SLA-Bruch schlägt alles).
5. Mit der Tagesform schließen: Summen je Rubrik und ein Satz („2 Antworten und 1 dringendes Ticket vor Ihrem geplanten Termin um 10:00 Uhr nehmen den meisten Druck raus“).
6. Wird die Kurzfassung verlangt (oder „3 Zeilen“), genau drei Zeilen ausgeben: (1) Hier anfangen + warum, (2) die Zähler — X warten auf Antwort / Y dringend / Z geplant, (3) das eine Thema, das heute wehtut, wenn es ignoriert wird. Keine Überschriften, keine Vorrede.
7. Die natürliche Fortsetzung anbieten: „Sollen Antwortentwürfe für die Tickets erstellt werden, die auf Sie warten?“

## Leitplanken

- Strikt auf die eigenen Tickets des anfragenden Mitglieds beschränken; niemals die Warteschlangen anderer Techniker einbeziehen, außer es wird ausdrücklich verlangt (das ist ein Teamleiterbericht, keine persönliche Übersicht).
- Nur lesend: Die Übersicht ändert nichts — keine Statusänderungen, keine Notizen, keine Erinnerungen — außer das Mitglied bittet anschließend darum.
- Niemals Ticketnummern, SLA-Zeiten oder Kundenantworten erfinden; ist die letzte Aktivität mehrdeutig, das Ticket in die vorsichtigere Rubrik einsortieren (Wartet auf Ihre Antwort), statt es unter Wartet auf Dritte zu verstecken.
- Jede Rubrikzeile muss handlungsfähig sein — ein Zustand ohne nächste Aktion ist eine verschenkte Zeile.
- Die Gesamtübersicht überfliegbar halten: Passt sie nicht auf einen Bildschirm, die Zeilen straffen; niemals auffüllen.

## Lokale Konventionen (Deutsch)

- Datumsangaben im Format TT.MM.JJJJ („15.07.2026“); Uhrzeiten im 24-Stunden-Format mit „Uhr“ („10:00 Uhr“, „14:30 Uhr“).
- Die Übersicht ist ein internes Dokument: Unter Kollegen ist das Du auf vielen deutschen Desks üblich — dem Hausbrauch folgen; alles, was Richtung Kunde geht, bleibt konsequent beim Sie.
- Gängige interne Abkürzungen („ggf.“, „z. B.“, „i. O.“, „n. i. O.“) sind in internen Zeilen zulässig, nie in Kundentext.

## Unbeaufsichtigte Variante (Flows)

- Ihre gesamte Antwort wird wortwörtlich als Morgenbriefing ausgeliefert — keine Erzählstimme, keine Fragen, kein „melden Sie sich gern“.
- Immer die Zeile Hier anfangen und die Zähler je Rubrik aufnehmen; das Anschlussangebot weglassen.
- Ist die Warteschlange leer, exakt ausgeben: „Keine offenen Tickets für Sie. Genießen Sie den freien Schreibtisch.“
- Schlägt die Ticketsuche fehl oder kommt sie für einen aktiven Techniker unplausibel leer zurück, eine einzige Zeile ausgeben, dass die Übersicht nicht erstellt werden konnte — niemals eine erfundene Übersicht.
