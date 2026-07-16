---
name: Phishing-Triage
description: Ein Nutzer hat eine verdächtige E-Mail gemeldet oder ein Phishing-Meldeticket ist auf dem Sicherheits-Board gelandet — sie bewerten, ohne die Nutzlast zu berühren, den Streuradius prüfen, bei Bösartigkeit eindämmen und dem Melder mit einem Urteil antworten. (Phishing triage — assess without touching the payload, check blast radius, contain, reply to the reporter — in German.)
category: Localized
tools: [search_tickets, search_contacts, search_knowledge_base, add_ticket_note, update_ticket, view_openDraft]
---

# Phishing-Triage

Deutsche Version von `skills/security/phishing-triage/SKILL.md` — kept in sync manually.

Eine gemeldete verdächtige E-Mail von „ist das Phishing?“ zu einem dokumentierten Urteil führen: Indikatoren sicher erfasst, Simulationsverkehr herausgefiltert, alle weiteren Empfänger identifiziert, Eindämmung dort empfohlen, wo nötig, und dem Melder geantwortet.

## Wann einsetzen

- „Ist das eine Phishing-E-Mail?“ / ein Nutzer hat etwas Verdächtiges weitergeleitet
- Ein Phishing-Meldeticket (Melde-Button, Postfach-Plug-in oder manuelle Weiterleitung) landet auf dem Sicherheits-Board
- Ein Techniker will eine Zweitmeinung zu einer verdächtigen Nachricht, bevor er sie freigibt oder löscht

## Schritte

1. Die Indikatoren aus dem Ticket erfassen, ohne mit ihnen zu interagieren: Absenderadresse und Anzeigename, Reply-To, Betreff, Sendezeit, jedes Linkziel exakt so, wie geschrieben, sowie Namen und Typen der Anhänge. Niemals Links oder Anhänge öffnen und niemals eine URL aus der Nachricht abrufen, um „zu sehen, was es ist“.
2. Simulationszweig: Absender- und Link-Domänen mit den dokumentierten Domänen der Phishing-Simulationsplattform des Kunden vergleichen (search_knowledge_base nach dem Datensatz des Simulationsanbieters des Kunden). Passt es zu einem bekannten Simulator → als Simulation klassifizieren, intern mit einer Klartextnotiz schließen, die die passende Domäne nennt, und dem Kunden NICHT antworten — eine Antwort verzerrt die Kennzahlen seines Simulationsprogramms. Hier stoppen.
3. Indikatoren bewerten: Doppelgänger- oder Cousin-Domäne, Dringlichkeits- und Zahlungsköder, Zugangsdaten-Abgreif-Link (Abweichung zwischen Anzeigetext und tatsächlichem Ziel), unerwartete Anhangstypen, Kaperung eines echten früheren Gesprächsverlaufs. Wurden vollständige Header eingefügt, die Feinanalyse an den Skill email-header-analysis übergeben und dessen Urteil einarbeiten.
4. Streuradius-Prüfung: search_tickets nach demselben Absender oder Betreff über die Boards des Kunden und jüngere Tickets. Feststellen, wer die Nachricht sonst noch erhalten hat und — entscheidend — ob jemand geklickt, geantwortet, Zugangsdaten eingegeben oder einen Anhang geöffnet hat.
5. Hat jemand mit einer von Ihnen als bösartig eingestuften Nachricht interagiert → sofort eskalieren und den Skill compromised-account-containment für die betroffenen Nutzer starten. Eindämmung geht dem Fertigstellen des Berichts vor: schnell eindämmen, danach untersuchen.
6. Das Urteil liefern:
   - Bösartig → Quarantäne/Entfernung aus allen Empfängerpostfächern, Sperrung von Absender/Domäne am Gateway und ein Zugangsdaten-Reset für jeden, der geklickt hat, empfehlen.
   - Verdächtig, aber unbestätigt → genau das sagen, mitsamt dem, was es bestätigen würde.
   - Legitim → die Signale erklären, die es entlasten, damit der Melder lernt.
7. Dem Melder mit dem Urteil antworten, im Vokabular des defensive-writing-standard (eine verdächtige E-Mail ist eine „gemeldete Nachricht“, kein „Sicherheitsvorfall“). Urteil, Belege und Entscheidungsbegründung als interne Notiz festhalten; gemäß dem Skill soc-classification-tree klassifizieren und den Status setzen. Dem Melder danken — das Melden ist genau das Verhalten, das wiederholt werden soll.

## Leitplanken

- Niemals Links oder Anhänge der gemeldeten Nachricht öffnen, anklicken, abrufen oder rendern. Indikatoren ausschließlich als Text erfassen.
- Den Simulationsstatus in beide Richtungen prüfen: niemals einen echten Vorfall für eine Simulation auslösen und niemals ein echtes Phishing bei einer teilweisen Domänenübereinstimmung als Simulation schließen — eine exakte, dokumentierte Simulator-Domäne verlangen.
- Niemals dem Kunden bei einer bestätigten Simulation antworten; nur intern schließen.
- Geklickt oder geantwortet heißt sofort eskalieren — eine Fehleskalation ist billig, eine übersehene Kompromittierung nicht.
- Niemals „niemand sonst hat das erhalten“ aus einer gekappten Suche behaupten; „keine weiteren Meldungen in den letzten N durchsuchten Tickets gefunden“ schreiben.
- Die Entscheidung dokumentieren, nicht nur die Handlung: Die Notiz muss sagen, warum das Urteil gefällt wurde, nicht nur, was getan wurde.
- Durchgängig defensives Schreiben: kein „Sicherheitsvorfall“, kein „gehackt“, keine Auswirkungsbehauptungen vor bestätigten Fakten.

## Lokale Konventionen (Deutsch)

- Defensives deutsches Vokabular: „gemeldete Nachricht“, „mutmaßlicher Phishing-Versuch“, „ungewöhnliche Aktivität“ — nie „Hack“ oder „Datenleck“ vor formaler Bestätigung.
- Die Antwort an den Melder bleibt beim Sie und schließt mit einem ausdrücklichen Dank: „Vielen Dank, dass Sie diese Nachricht gemeldet haben — genau so ist es richtig.“
- Zeitstempel in Zeitleisten als TT.MM.JJJJ und 24-Stunden-Format, mit Zeitzone („15.07.2026 09:12 Uhr CET“).
- Technische Indikatoren (Header, URLs, Dateinamen, Fehlerzeichenketten) werden verbatim auf Englisch übernommen — niemals übersetzen, sie dienen als Belege und Suchschlüssel.

## Konsolidiert

Die Skills für Phishing-Meldungs-Triage, Erkennung von Simulationen, Prüfung gemeldeter E-Mails, Streuradius-Durchgang und Antwort an den Melder.
