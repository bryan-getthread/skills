---
name: Zeiteinträge bereinigen
description: Rohe, grobe Zeitnotizen in saubere, standardisierte Zeiteinträge überführen — kundensichtbare und interne Fassung — und dabei nur Arbeit erfassen, die der Techniker ausdrücklich als erledigt genannt hat. (Turn rough time notes into clean, standardized time entries in German — client-facing and internal versions.)
category: Localized
tools: [search_tickets, log_time_entry, add_ticket_note]
---

# Zeiteinträge bereinigen

Deutsche Version von `skills/documentation/time-entry-cleanup/SKILL.md` — kept in sync manually.

Unordentliche Zeitnotizen in das Standard-Zeiteintragsformat des Desks normalisieren, kundensichtbare Formulierung vom internen Detail trennen, ohne jemals Arbeit zu erfinden oder aufzuwerten.

## Wann einsetzen

- Ein Techniker fügt grobes Kürzel ein („pw zurückg, getestet ok, usr benachr“) und will einen ordentlichen Zeiteintrag.
- „Bereinige meine Zeitnotizen zu diesem Ticket.“
- Feierabend: mehrere Roheinträge müssen vor der Abrechnungsprüfung standardisiert werden.
- Ein Anruf-Nachbericht soll ein abrechenbarer Eintrag mit Dauer werden.

## Schritte

1. Die Rohnotizen lesen; Kontext aus dem Ticket-Verlauf mit `search_tickets` nur heranziehen, um Referenzen aufzulösen (welches Ticket, welcher <user>, welches <device>) — niemals, um Arbeitspositionen zu ergänzen.
2. Jeden Eintrag in klarem Vergangenheitstempus in fester Feldreihenfolge umschreiben:
   1. **Problem** — was gemeldet wurde.
   2. **Durchgeführte Arbeit** — was der Techniker ausdrücklich als getan angegeben hat.
   3. **Ergebnis** — der Ausgang, wie genannt.
   4. **Nächste Schritte** — nur, wenn der Techniker einen genannt hat.
3. Die **Null-Annahme-Regel** anwenden: nur aufnehmen, was der Techniker ausdrücklich als getan gesagt hat. Niemals eine Empfehlung, Absicht oder ein „sollte“ in eine erledigte Handlung verwandeln („Neustart empfohlen“ darf NICHT zu „neu gestartet“ werden). Sind die Notizen mehrdeutig, nachfragen oder mit `[unklar — bestätigen]` markieren.
4. Ersetzungen für verbotene Wörter gemäß Hausstil anwenden — typischer Satz: „repariert“ → „behoben“; „Problem mit dem Typen“ → „von <user> gemeldetes Problem“; „verbockt“ → „fehlkonfiguriert“; „müsste passen“ → „funktionsfähig verifiziert“ **nur wenn die Verifikation genannt wurde**, sonst „ausstehende Nutzerbestätigung“. Die eigene Verbotsliste des Desks respektieren, wenn bereitgestellt.
5. Auf Wunsch zwei Fassungen erstellen:
   - **Kundensichtbar** — schlichte professionelle Sprache, kein internes Kürzel, keine Schuldzuweisung, kein Herstellergrummeln, Klartext (PSA-tauglich).
   - **Intern** — volles technisches Detail, Hypothesen und Nachverfolgungsmarker.
6. Die bereinigten Einträge mit Dauern ausgeben. Erst nach Bestätigung von Text und Zeitmenge durch den Techniker mit `log_time_entry` / `add_ticket_note` schreiben.

## Leitplanken

- **Die Null-Annahme ist absolut.** Keine erschlossene Arbeit, keine erschlossene Verifikation, keine erschlossenen Dauern. Wurde eine Dauer nicht genannt, nachfragen — nicht still schätzen.
- Den Sinn des Technikers bewahren; nur die Formulierung säubern. Das abrechenbare Detail muss dem entsprechen, was die Notizen beschreiben.
- Niemals rein interne Bemerkungen (Schuldzuweisungen, Kundenreibung, Sicherheitsverdacht) in die kundensichtbare Fassung geben.
- Vor dem Erfassen bestätigen: Einträge wirken auf die Abrechnung. Im Zweifel nur den Text ausgeben und den Techniker einreichen lassen.

## Lokale Konventionen (Deutsch)

- Dauern im üblichen Format des Desks: „1,5 Std.“ oder „45 Min.“; Datum als TT.MM.JJJJ.
- Das Perfekt ist das natürliche Tempus des Eintrags („Ich habe das Passwort zurückgesetzt und die Anmeldung geprüft“); das Präteritum wirkt in einer Einsatznotiz zu literarisch.
- Die kundensichtbare Fassung bleibt beim Sie und ohne Jargon-Anglizismen („neu gestartet“ statt „gerebootet“); die interne Fassung darf das gängige technische Vokabular des Desks behalten.
- Englische technische Zeichenketten (Fehlermeldungen, Dienstnamen, Befehle) nicht übersetzen: sie bleiben verbatim.

## Konsolidiert

Die Skills für Zeiteintrags-Zusammenfassung, Umformatierung roher Zeitnotizen, Anruf-Nachbericht und Notizstil mit verbotenen Wörtern.
