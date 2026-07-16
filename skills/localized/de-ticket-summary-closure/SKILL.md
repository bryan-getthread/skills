---
name: Ticket-Zusammenfassung und Abschlussnotiz
description: Eine saubere Zusammenfassung des Ticket-Geschehens erstellen — Lösungsnotiz, Abschlussnotiz oder eine P1/P2-Übergabezusammenfassung nach Vorlage — im gewünschten Format und Blickwinkel. (Ticket summary, closure note, or templated P1/P2 handoff summary — in German.)
category: Localized
tools: [search_tickets, add_ticket_note]
---

# Ticket-Zusammenfassung und Abschlussnotiz

Deutsche Version von `skills/documentation/ticket-summary-and-closure-note/SKILL.md` — kept in sync manually.

Den Ticket-Verlauf zu genau der Notiz verdichten, die die Situation verlangt: eine Lösungs-/Abschlussnotiz, ein „Was wurde getan“-Recap oder eine Eskalations-Übergabezusammenfassung nach Vorlage.

## Wann einsetzen

- „Schreib eine Abschlussnotiz für dieses Ticket“ / „fass zusammen, was gemacht wurde.“
- „Gib mir eine Lösungsnotiz in unserem Standardformat.“
- Ein P1/P2 wird an einen anderen Techniker oder ein Bereitschaftsteam übergeben und braucht eine strukturierte Übergabezusammenfassung.
- „Fass das in der Ich-Form für meine Zeiterfassung / PSA-Notiz zusammen.“

## Schritte

1. Den vollständigen Verlauf mit `search_tickets` lesen: Antworten, interne Notizen und Zeiteinträge. Herstellen: Problem → durchgeführte Maßnahmen → Ergebnis → was noch offen ist.
2. Das von der Anfrage geforderte Format wählen:
   - **Lösungs-/Abschlussnotiz** — Problem, Durchgeführte Maßnahmen, Ergebnis und (nur wenn Arbeit verbleibt) einen klaren Nächsten Schritt mit Verantwortlichem und Frist.
   - **P1/P2-Übergabezusammenfassung** — feste Vorlage: Aktueller Status; Auswirkung (wer/was ist ausgefallen); Zeitleiste der Schlüsselereignisse mit Zeitstempeln; Bisher durchgeführte Maßnahmen; Was ausgeschlossen wurde; Nächste Aktion + Verantwortlicher; Stand der Kundenkommunikation (wer wurde wann worüber informiert).
   - **Schlichtes Recap** — kurzer Fließtext für eine kundensichtbare Abschlussnachricht.
3. Den verlangten Blickwinkel anwenden: standardmäßig dritte Person, oder **Ich-Form aus Technikersicht** („Ich habe das Profil zurückgesetzt und die Anmeldung geprüft“), wenn verlangt — als der zugewiesene Techniker schreiben, nicht als KI.
4. Die Formatierungsregeln des Desk-Skills **note-format-standard** anwenden, falls geladen. Synchronisiert die Notiz zu einem PSA, **nur Klartext** verwenden: kein Markdown, keine Emojis, keine typografischen Anführungszeichen, keine Gedankenstriche.
5. Die Notiz zur Prüfung in der Konversation ausgeben. Nur mit `add_ticket_note` posten, wenn ausdrücklich verlangt, und Übergabe-/QA-Zusammenfassungen als **interne** Notizen posten, sofern nicht anders angewiesen.

## Leitplanken

- Kein Detail erfinden, das der Verlauf nicht trägt — keine fabrizierten Bestätigungen, Zeitstempel oder Ursachen. Ist das Ergebnis unklar, „Ergebnis im Verlauf nicht bestätigt“ schreiben, statt eine Lösung zu behaupten.
- Dem verlangten Format genau folgen, einschließlich „keine Aufzählungspunkte“ oder Wortgrenzen, wenn angegeben.
- Die Ich-Form ändert nur die Stimme — niemals Arbeit ergänzen, die der Techniker nicht dokumentiert hat.
- In Übergabezusammenfassungen Fakt und Hypothese ausdrücklich trennen („Bestätigt:“ vs. „Vermutet:“).
- Niemals Zugangsdaten oder Einmalcodes aus dem Verlauf in eine Zusammenfassung übernehmen.

## Lokale Konventionen (Deutsch)

- Zeitleisten als TT.MM.JJJJ und 24-Stunden-Format („15.07.2026 14:30 Uhr“); immer die Zeitzone angeben, wenn der Desk mehrere Zonen abdeckt.
- Eine kundensichtbare Notiz bleibt beim Sie und im sachlich-professionellen Register; der Standardabschluss endet mit: „Sollte etwas noch nicht gelöst sein, antworten Sie einfach auf diese E-Mail — Ihre Antwort öffnet dieses Ticket erneut.“
- Vorlagenüberschriften dürfen zweisprachig bleiben, wenn das PSA des Desks englischsprachig ist („Current Status / Aktueller Status“) — dem etablierten Gebrauch des Tenants folgen, statt eine Übersetzung aufzuzwingen.
- Zitierte englische technische Fehlerzeichenketten bleiben englisch, verbatim.

## Konsolidiert

Die Anfragen nach Abschlussnotizen, Lösungszusammenfassungen, P1/P2-Übergabevorlagen und die situationsbezogenen „Nächster Schritt“-Zusammenfassungs-Skills.
