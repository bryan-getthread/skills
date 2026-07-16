---
name: Nachfass-Kadenz für stille Tickets
description: Eine konfigurierbare Nachfass-Kadenz (etwa 24/48/72 Stunden, drei Versuche) auf Tickets fahren, die auf eine Kundenantwort warten — jede freundliche Erinnerung entwerfen, dokumentierte Versuche zählen und Tickets in berechtigter Wartestellung auslassen. (Follow-up cadence on waiting-on-client tickets, with German client-facing nudges.)
category: Localized
tools: [search_tickets, add_ticket_note, update_ticket, schedule_ticket]
---

# Nachfass-Kadenz für stille Tickets

Deutsche Version von `skills/qa-and-closure/stale-ticket-followup-cadence/SKILL.md` — kept in sync manually.

Hält auf den Kunden wartende Tickets in einem berechenbaren Rhythmus in Bewegung. Zählt die im Verlauf bereits dokumentierten Versuche, entwirft die nächste Nachfassung in einer freundlichen, kundengerichteten Stimme und weiß, wann ein Ticket aus gutem Grund wartet und in Ruhe gelassen werden sollte.

## Wann einsetzen

- „Fass meine wartenden Tickets nach“ / „wer hat uns nicht geantwortet?“
- Ausführen der Standard-Kadenz des Desks (24/48/72 Stunden oder die Hausvariante) über eine Warteschlange.
- Ein Techniker will die nächste Erinnerung für ein bestimmtes stilles Ticket entworfen haben.
- Manuell auf Abruf oder aus einem externen Zeitplaner heraus (siehe Abschnitt „Unbeaufsichtigt ausführen“).

## Schritte

1. Die anzuwendende Kadenz bestätigen. Standard: erste Nachfassung 24 Stunden nach der letzten ausgehenden, kundensichtbaren Nachricht, zweite nach 48, dritte nach 72 — insgesamt drei Versuche, dann Übergabe an den Skill No-Response Closure Sequence. Eine deskspezifische Kadenz respektieren, wenn der Nutzer eine nennt („3 Tage / 3 Versuche / 3 E-Mails“ ist eine gängige Hausregel).
2. Kandidaten mit `search_tickets` finden: offene Tickets in Warten-auf-Kunde-Status, deren letzte Nachricht ausgehend und älter als die aktuelle Kadenzstufe ist.
3. Für jeden Kandidaten die **dokumentierten Versuche** zählen: kundensichtbare Nachfassnachrichten seit der letzten Kundenantwort. Nur Nachrichten, die der Kunde tatsächlich sehen konnte, zählen — interne Notizen sind keine Versuche.
4. Berechtigte Wartestellungen auslassen: ein künftiger geplanter Termin (`schedule_ticket`-Datum), eine vom Kunden genannte Frist („ab Montag zurück aus dem Urlaub“), eine Hersteller-/Drittabhängigkeit oder eine ausstehende Freigabe. Wer Ihnen gesagt hat, wann er antwortet, wird nicht erinnert.
5. Die Nachfassung für jedes verbleibende Ticket in der Kundenstimme entwerfen: freundlich, kurz, das ursprüngliche Anliegen in einer Zeile aufgreifend, klar sagend, was von ihm gebraucht wird, und einen einfachen Ausweg bietend („falls das erledigt ist, sagen Sie uns kurz Bescheid, dann schließen wir das Ticket“). Den Ton über die Versuche hinweg sanft steigern — der dritte weist darauf hin, dass das Ticket ohne Antwort bald geschlossen wird, und klingt nie verärgert.
6. Die Entwürfe zur Prüfung vorlegen, dann jede freigegebene Nachfassung als kundensichtbare Notiz mit `add_ticket_note` posten und den passenden Warten-Status mit `update_ticket` setzen.
7. Eine Übersichtstabelle ausgeben: Ticket, Kunde, soeben gesendete Versuchsnummer (oder „ausgelassen“ und warum) und das Datum, an dem die nächste Kadenzstufe fällig ist.

## Leitplanken

- Niemals die Versuchszahl überschreiten: Zeigt der Verlauf bereits drei dokumentierte Versuche, an die Schließsequenz übergeben, statt ein viertes Mal zu erinnern.
- Nur zählen, was dokumentiert ist — niemals annehmen, ein Versuch sei telefonisch erfolgt, wenn keine Notiz ihn festhält.
- Kundenentwürfe enthalten keinen internen Jargon, keinen Ticketsystem-Boilerplate und keine erfundenen Details zum Anliegen.
- Die Ausnahmeliste ist heilig: Ein zu Unrecht erinnerter Kunde im Urlaub oder vor einem geplanten Einsatz kostet Vertrauen. Ist die Berechtigung der Wartestellung unklar, auslassen und markieren, statt zu senden.
- Niemals Nachfassungen in Masse senden, ohne dass der Nutzer die Entwürfe geprüft hat.
- Die Sprache einfach und lokalisierbar halten; sich an der Sprache des Kunden ausrichten, wenn der Verlauf nicht auf Deutsch ist.

## Lokale Konventionen (Deutsch)

- Konsequent das Sie in den Nachfassungen; freundlicher, aber direkter Ton („Wir warten noch auf Ihre Rückmeldung“ statt Umschweifen).
- Sensible Zeiten im DACH-Raum: Sommerferien (regional gestaffelt, Juli–September) und die Feiertage zum Jahreswechsel — Kundenstille in diesen Fenstern ist häufiger eine berechtigte Wartestellung; Abwesenheitsnotizen prüfen, bevor ein Versuch gezählt wird.
- Datum als TT.MM.JJJJ; „innerhalb von 48 Stunden“ ist die übliche Wendung für Fristen.
- Typischer Ausweg: „Falls sich das bei Ihnen erledigt hat, genügt eine kurze Nachricht und wir schließen das Ticket.“ Schließhinweis (3. Versuch): „Ohne Ihre Rückmeldung bis zum <Datum> schließen wir dieses Ticket — Sie können es jederzeit wieder öffnen, indem Sie auf diese E-Mail antworten.“

## Unbeaufsichtigt ausführen

> **Flows können dies nicht planen oder zeitgesteuert auslösen.** Thread-Flows feuern ausschließlich auf Ticket-*Ereignisse* und Bedingungen — es gibt keinen Zeitplan, kein Cron, keinen Ticketalter- oder Zeitablauf-Trigger. Dies ist ein Kadenz-/Durchgangs-Skill, also **manuell** auf Abruf ausführen oder aus einem externen Zeitplaner, der Super Magic aufruft. Ein Flow erreicht ihn nur über **Run Skill** bei einem qualifizierenden Ticket-Ereignis, niemals „jeden Morgen“ oder „nach N Stunden“. Die Ausgabedisziplin unten gilt, wann immer er unbeaufsichtigt läuft.

- Ihre gesamte Antwort ist die kundensichtbare Nachfassnachricht, wortwörtlich gepostet — keine Erzählstimme, keine Versuchszähler, kein interner Kommentar. Sonst nichts schreiben.
- Deterministische Regeln: nur senden, wenn (a) das Ticket in einem Warten-auf-Kunde-Status ist, (b) die letzte Nachricht ausgehend ist, (c) die Kadenzstufe fällig ist und (d) die dokumentierten Versuche unter dem Maximum liegen. Scheitert eine Bedingung, oder wirkt die Wartestellung berechtigt, keine Ausgabe erzeugen und stoppen.
- Niemals den dritten Versuch oder darüber hinaus unbeaufsichtigt entwerfen, wenn der Desk eine menschliche Freigabe vor den letzten Mahnungen verlangt — die konfigurierte Versuchsobergrenze prüfen und darunter stoppen.
