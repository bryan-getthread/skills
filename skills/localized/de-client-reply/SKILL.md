---
name: Kundenantwort
description: Eine kundengerichtete Antwort in der Hausstimme und im Hausformat verfassen — Statusmeldung, Zwischenstand, Abschlussnachricht oder jede Kunden-E-Mail zu einem Ticket. (Draft a client-facing reply in the house voice — status updates, closing messages, any client email on a ticket, in German.)
category: Localized
tools: [search_tickets, view_openDraft, add_ticket_note]
---

# Kundenantwort

Deutsche Version von `skills/communication/client-reply/SKILL.md` — kept in sync manually.

**Wann einsetzen:** Ein Techniker muss dem Kunden eine Nachricht schicken und will sie markenkonform und versandfertig.

**Was Sie erhalten:** einen Entwurf, der mit der Antwort beginnt, der Hausstimme entspricht und so, wie er ist, versendet werden kann.

## Wann einsetzen

- „Verfasse eine Antwort an den Kunden zu diesem Ticket.“
- „Sag <user> Bescheid, dass sein Konto wieder entsperrt ist.“
- „Schreib eine Nachricht, dass wir noch mit dem Hersteller daran arbeiten.“
- Jede Kunden-E-Mail, bei der Ton, Format und Genauigkeit zählen.

## Schritte

1. Den kompletten Ticket-Verlauf lesen, bevor ein Wort geschrieben wird — jede frühere Nachricht, Notiz und Statusänderung. Niemals allein aus der letzten Nachricht heraus formulieren.
2. Die Antwort verfassen und dabei mit der Antwort selbst oder dem aktuellen Stand beginnen, danach die stützenden Details.
3. Die Standards der Hausstimme anwenden:
   - Den Kunden nur in der **ersten** Nachricht eines Verlaufs namentlich begrüßen; in Folgeantworten desselben Verlaufs die Anrede weglassen.
   - Keine Leerfloskeln zur Eröffnung („Ich hoffe, es geht Ihnen gut“, „Ich melde mich kurz bei Ihnen“). Direkt zur Sache kommen.
   - Kein Fachjargon, außer der Kunde hat den Begriff zuerst verwendet; ist ein Fachbegriff unvermeidbar, eine Erklärung in Alltagssprache ergänzen.
   - 1–2 kurze Absätze. Braucht es mehr, ist wahrscheinlich ein Anruf fällig.
   - Aufbau gemäß dem Skill Email Baseline Standard (communication/email-baseline-standard).
4. Verfügt das Mitglied über eine Liste bevorzugter/verbotener Wörter (persönlicher Kontext-Skill oder Hausstil-Notiz), diese anwenden: verbotene Wörter durch die vorgesehenen Ersatzformen ersetzen; gelistete Alternativen bevorzugen.
5. Handelt es sich um eine Abschlussnachricht, den Textkörper mit der Wiedereröffnungszeile beenden: „Sollte etwas noch nicht gelöst sein, antworten Sie einfach auf diese E-Mail — Ihre Antwort öffnet dieses Ticket erneut.“
6. Signatur: Fügt der Arbeitsbereich automatisch eine verwaltete Signatur an, nach dem letzten Satz des Textkörpers aufhören — KEINE Grußformel („Mit freundlichen Grüßen, …“) ergänzen, die sich doppeln würde. Eine Grußformel nur dann setzen, wenn keine verwaltete Signatur existiert.
7. Die Antwort mit view_openDraft als offenen Entwurf zur Prüfung durch den Techniker vorlegen. Nicht senden.

## Leitplanken

- Niemals Details erfinden: keine erfundenen Zeitstempel, durchgeführten Schritte, Ticketnummern, Links oder Zusagen. Belegt der Verlauf einen Sachverhalt nicht, ihn weglassen oder mit <mit Techniker bestätigen> markieren.
- Der Entwurf bleibt ein Entwurf, bis ein Mensch ihn freigibt. Niemals eigenständig senden.
- Niemals interne Notizen, Zugangsdaten, Preise oder Daten anderer Kunden in einer Kundenantwort offenlegen.
- Auf die Sprache des Kunden ausrichten: Ist der Verlauf in einer anderen Sprache, in dieser Sprache verfassen (siehe communication/multilingual-reply für den Rückübersetzungs-Ablauf).
- Degradation: view_openDraft ist nur in der App verfügbar. Über externes MCP den fertigen Entwurf in der Konversation ausliefern, klar gekennzeichnet mit „ENTWURF — vor dem Senden prüfen“.

## Lokale Konventionen (Deutsch)

- Gegenüber dem Kunden konsequent das Sie; nur dann zum Du wechseln, wenn der Kunde es ausdrücklich angeboten hat.
- Eröffnungsanrede: „Hallo <Vorname>,“ bei etablierter Beziehung; „Guten Tag Frau/Herr <Nachname>,“ im förmlicheren Register — dem im Verlauf bereits gesetzten Register folgen.
- Schlichte Grußformeln, wenn keine verwaltete Signatur existiert: „Mit freundlichen Grüßen“ oder „Viele Grüße“ — überförmliche Formeln in einer Support-E-Mail vermeiden.
- Datum als TT.MM.JJJJ, Uhrzeit im 24-Stunden-Format mit „Uhr“ („14:30 Uhr“).
- Referenz-Wiedereröffnungszeile: „Sollte etwas noch nicht gelöst sein, antworten Sie einfach auf diese E-Mail — Ihre Antwort öffnet dieses Ticket erneut.“

## Konsolidiert

Die Skills zum Verfassen/Senden von Antworten, zum Standardformat der Kundenantwort, zur Antwort in einer benannten Stimme, zum Ton, zu verbotenen Wörtern und zur Signatur.
