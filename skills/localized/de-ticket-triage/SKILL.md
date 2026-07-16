---
name: Ticket-Triage
description: Ein neues oder nicht zugewiesenes Ticket klassifizieren, die Schwere einschätzen, Dubletten erkennen und vorschlagen, wohin es gehört, bevor es in die Warteschlange geht. (Classify a new/unassigned ticket, gauge severity, catch duplicates, recommend routing — in German.)
category: Localized
tools: [search_tickets, list_boards, list_ticket_statuses, list_ticket_priorities, search_clients, update_ticket]
---

# Ticket-Triage

Deutsche Version von `skills/triage-and-routing/ticket-triage/SKILL.md` — kept in sync manually.

Erste Sichtung eines neuen oder nicht zugewiesenen Tickets: klassifizieren, die richtige Schwere erzwingen, Dubletten erkennen und Board, Status und Priorität vorschlagen — und erst nach Bestätigung anwenden.

## Wann einsetzen

- Ein neues Ticket ist eben eingegangen und braucht eine erste Sichtung vor dem Dispatch.
- „Triagiere dieses Ticket“ / „klassifizier das“ / „wohin gehört das?“
- Morgendlicher Durchgang durch die nicht zugewiesenen Tickets auf dem Eingangs-Board.
- Ein Dispatcher will einen Vorschlag für Board, Status und Priorität mit einer Warteschlangen-Zusammenfassung in einer Zeile.

## Schritte

1. Den vollständigen Verlauf mit `search_tickets` lesen, einschließlich der frühesten Nachricht und jeder Antwort. Niemals allein aus dem Titel klassifizieren oder routen — Titel sind häufig veraltet, automatisch erzeugt oder schlicht falsch.
2. Das Anliegen in die Kategorien des Desks einordnen (z. B. Zugriff, Hardware, Software, Netzwerk, E-Mail, Sicherheit). Die Einschätzung auf den Inhalt der Anfrage stützen, nicht auf Schlagwörter im Betreff.
3. Die Regeln zur erzwungenen Schwere vor jeder Ermessensentscheidung anwenden: Alles Sicherheitsrelevante (Kompromittierung, Phishing, Malware, unerwartete MFA-/Anmeldeaktivität) oder was mehrere Nutzer oder einen ganzen Standort betrifft, ist standardmäßig hohe Priorität. Nur die eigenen, entschärfenden Worte des Anfragenden, mit dem Techniker bestätigt, können sie senken.
4. Auf Dubletten mit starken Kennungen prüfen, nicht mit Formulierungsähnlichkeit: dieselbe Alarm-ID, derselbe Geräte-/Asset-Name, dieselbe Monitoring-Referenz oder derselbe Kontakt + derselbe spezifische Fehler in einem jüngeren Zeitfenster. Je Kennung ein separates `search_tickets` ausführen; konnte eine Suche eine Ergebnisobergrenze erreicht haben, das sagen.
5. Den Kunden mit `search_clients` nachschlagen, um zu bestätigen, dass die Firma auf dem Ticket zum Anfragenden passt.
6. Vorschlagen: Board, Status, Priorität, Kategorie und eine Warteschlangen-Zusammenfassung in einer Zeile. Vermutete Dubletten mit Ticketnummern auflisten und warum sie übereinstimmten.
7. Erst nachdem der Prüfer den Vorschlag bestätigt hat, ihn mit `update_ticket` anwenden. Ändert er etwas, seine Fassung anwenden, nicht Ihre.

## Leitplanken

- Niemals allein aus dem Titel routen oder klassifizieren; immer zuerst den Verlauf lesen.
- Niemals das Ticket ändern, bevor die vorgeschlagene Klassifizierung bestätigt ist. Dieser Skill empfiehlt; ein Mensch (oder ein separat konfigurierter unbeaufsichtigter Skill) wendet an.
- Sicherheits- oder Mehrbenutzer-Auswirkung erzwingt hohe Priorität — ein ruhiger Ton in der Nachricht darf Sie nicht herunterreden.
- Ein Dubletten-Urteil verlangt eine starke Kennungsübereinstimmung. Ähnliche Formulierung ist ein Hinweis zum Erwähnen, niemals ein Grund zum Zusammenführen oder Schließen.
- Sind Board-/Status-/Prioritätsnamen für diesen Tenant mehrdeutig, sie mit `list_boards` / `list_ticket_statuses` / `list_ticket_priorities` abrufen, statt Bezeichnungen zu raten.
- Keine Ticketnummern, Kunden oder Kategorien erfinden. Lässt sich das Ticket nicht mit Zuversicht klassifizieren, das sagen und es einem Menschen überlassen.

## Lokale Konventionen (Deutsch)

- Warteschlangen-Zusammenfassungen und interne Notizen auf Deutsch, Datum als TT.MM.JJJJ, Uhrzeit im 24-Stunden-Format; Prioritäten und Status behalten ihre exakten Bezeichnungen, wie im Tenant konfiguriert (auch wenn sie englisch sind) — nicht spontan übersetzen.
- Deutsche Weiterleitungsmarker im Betreff erkennen: „WG:“ (weitergeleitet) und „AW:“ (Antwort) spielen dieselbe Rolle wie „FW:“ / „RE:“ — die zitierte Ursprungsnachricht zählt mehr als der Weiterleitende.
- Das Schwere-Vokabular Richtung Kunde bleibt maßvoll: „größere Störung“ statt „Katastrophe“; alarmistische Zuspitzungen erscheinen nie in einer für den Kunden sichtbaren Zusammenfassung.
- Englische technische Fehlerzeichenketten, die im Ticket zitiert werden, nicht übersetzen: sie dienen als Suchkennungen.

## Konsolidiert

Die Skills für Morgen-Triage, Dispatch-Unterstützung, P1-Triage, dublettenbewusste Annahme und generisches „klassifizier dieses Ticket“.
