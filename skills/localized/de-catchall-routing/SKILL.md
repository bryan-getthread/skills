---
name: Sammelpostfach-Routing
description: Für ein Ticket, das in einem Sammelpostfach (Catchall) oder ohne Firma gelandet ist, den richtigen Kunden und Kontakt bestimmen — einschließlich weitergeleiteter Mails und Herstellerwarnungen — und es dann neu zuweisen oder Kandidaten zurückgeben. (Route catchall/no-company tickets to the right client and contact — in German.)
category: Localized
tools: [search_tickets, search_clients, search_contacts, assign_contact, update_ticket, add_ticket_note, create_ticket]
---

# Sammelpostfach-Routing

Deutsche Version von `skills/triage-and-routing/catchall-routing/SKILL.md` — kept in sync manually.

Ein ohne Firma (oder auf einem Sammelkontakt) eingegangenes Ticket über eine ausdrückliche Beweisleiter zum echten Kunden und Kontakt routen — oder es in Ruhe lassen, wenn der Beweis fehlt.

## Wann einsetzen

- Ein Ticket hat keine Firma zugewiesen oder sitzt auf einem Sammel-/Ohne-Firma-Kontakt.
- Eine Mail wurde von einem Techniker oder Kunden weitergeleitet („WG:“, „Fwd:“, „FW:“), und das Ticket ist dem Weiterleitenden zugeschrieben, nicht dem ursprünglichen Absender.
- Eine Hersteller- oder Monitoring-Warnung ist im Sammelpostfach gelandet, und ihr Kunde muss aus dem Warnungstext extrahiert werden.
- Periodischer Durchgang durch das Sammel-/Eingangs-Board, um falsch zugeordnete Tickets neu zuzuordnen.

## Schritte

1. Das Ticket mit `search_tickets` lesen: Titel, Beschreibung, früheste Nachricht sowie vollständige Header/zitierter Text, falls vorhanden.
2. Spam-Vorprüfung: Ist die Nachricht eindeutig unaufgeforderte Werbung oder automatischer Müll ohne kundenidentifizierenden Inhalt und ohne Anzeichen, dass ein Mensch sie aus einem Grund weitergeleitet hat, stoppen und sie als Spam zur Prüfung markieren, statt sie zu routen.
3. Identitätshinweise in dieser Stärkereihenfolge extrahieren (die Beweisleiter):
   a. E-Mail-Domäne des wahren Absenders (am stärksten),
   b. ein im Text oder in Warnfeldern ausdrücklich genannter Firmenname,
   c. ein Personenname allein (am schwächsten — nie für sich genügend).
4. Beginnt der Betreff mit „WG:“, „Fwd:“ oder „FW:“, oder enthält der Text eine zitierte Ursprungsnachricht, die ursprüngliche „Von:“- / „From:“-Zeile auswerten und diesen Absender — nicht den Weiterleitenden — als Identitätsquelle nehmen.
5. Ist das Ticket eine Hersteller-/Monitoring-Warnung, die strukturierten Felder (Standortname, Organisation, Gerät, Tenant) aus dem Warnungstext extrahieren und als Firmenhinweis nutzen.
6. Die Firma mit `search_clients` über die Domäne oder den extrahierten Namen auflösen; dann den Kontakt mit `search_contacts`, eingegrenzt auf diese Firma, finden. Hat der Absender keinen Kontaktdatensatz, die nächstliegende Übereinstimmung vorschlagen oder vermerken, dass einer angelegt werden muss — nicht an einen Namensvetter anhängen.
7. Muss die aktuelle Fehlzuweisung erst gelöscht werden, bevor die richtige auf diesem Tenant greifen kann, zuerst die Zuweisung aufheben (das Ticket auf Ohne-Firma/Sammel zurücksetzen) und dann die richtige Firma und den richtigen Kontakt anwenden.
8. Die Übereinstimmung mit `update_ticket` und `assign_contact` nur dann anwenden, wenn genau eine Firmenkandidatin auf Domänen- oder Ausdrücklich-Name-Stärke passt. Andernfalls die Kandidaten vorlegen und nachfragen.
9. Lehnt die PSA-Synchronisation des Tenants Firmenänderungen an einem bestehenden Ticket ab, den Schließen-und-neu-anlegen-Weg nutzen: `create_ticket` unter der richtigen Firma mit dem Originaltext und einer Klartextnotiz, die beide Ticketnummern gegenseitig referenziert, dann das Original schließen — nur mit Bestätigung im beaufsichtigten Einsatz.
10. Eine interne Klartextnotiz mit `add_ticket_note` posten, die angibt, was zugeordnet wurde, welche Sprosse der Beweisleiter diente und was sich geändert hat.

## Leitplanken

- Niemals allein auf Namensähnlichkeit zuweisen. Ein übereinstimmender Vor-/Nachname ohne Domäne oder ausdrückliche Firmenbestätigung ist geringe Zuversicht → keine Änderung.
- Mehrdeutigkeit (zwei oder mehr plausible Firmen) → keine Änderung; stattdessen Kandidaten auflisten.
- Ein zu einem PSA synchronisiertes Ticket muss immer eine Firma haben; niemals ein PSA-gebundenes Ticket firmenlos lassen — ist keine Übereinstimmung möglich, es gemäß Desk-Richtlinie an den designierten internen/Sammel-Kunden routen und markieren.
- Schließen-und-neu-anlegen ist quasi-destruktiv: im beaufsichtigten Einsatz vorher bestätigen; den Originaltext bewahren und beide Nummern im Klartext gegenseitig referenzieren.
- Für das PSA bestimmte Notizen sind Klartext — kein Markdown, keine Emojis.
- Keine Firmen, Kontakte oder Ticketnummern erfinden. Konnten Suchen Ergebnisobergrenzen erreicht haben, das sagen.

## Lokale Konventionen (Deutsch)

- Weiterleitungs- und Antwortmarker im deutschsprachigen Umfeld: „WG:“ (Outlook-Weiterleitung DE), „Fwd:“ und „AW: / RE:“ — die Auswertung der Ursprungsnachricht muss diese Präfixe genauso erkennen wie „FW:“.
- Domänen deutscher Kunden nutzen oft .de, .com oder Umlaut-transliterierte Marken; Domänen ohne Rücksicht auf Umlaute im Firmennamen vergleichen („Müller & Co.“ ↔ mueller-co.de).
- Die interne Notiz bleibt auf Deutsch, aber aus einer Warnung extrahierte Felder (site name, organization, tenant) werden verbatim übernommen, ohne Übersetzung.
- Datum als TT.MM.JJJJ in den Notizen, die beide Tickets gegenseitig referenzieren.

## Unbeaufsichtigte Variante (Flows)

- Nur auf Domänen-Übereinstimmung oder ausdrücklicher Firma handeln; auf allem Schwächeren nichts ändern und eine einzige interne Klartextnotiz posten: `SAMMELPOSTFACH-ROUTING: keine sichere Übereinstimmung. Gefundene Hinweise: <Hinweise>. Zur menschlichen Prüfung unzugewiesen belassen.`
- Niemals den Schließen-und-neu-anlegen-Weg unbeaufsichtigt nutzen.
- Spam-Zweig: unbeaufsichtigt nicht schließen; nur per Notiz markieren.
- Eine Neuzuordnung pro Ticket pro Lauf; trägt das Ticket bereits eine Routing-Notiz dieses Skills, stoppen.
- Ihre gesamte Antwort ist die interne Notiz — keine Erzählstimme, keine Fragen.

## Konsolidiert

Die Skills für Sammel-zu-Kontakt-Zuordnung, Catchall-Reparatur, Neuzuordnung mit vorheriger Aufhebung, Spam-Vorprüfung bei der Annahme, Auswertung des Ursprungsabsenders hinter „FW:“, Feldextraktion aus Herstellerwarnungen, Firmen-/Kontaktidentifikation, Anreicherung ohne Firma, Schließen-und-neu-anlegen und Umleitung zum richtigen Kunden (28 geschürfte Varianten).
