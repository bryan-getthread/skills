---
name: Schichtübergabe
description: Eine überfliegbare Einseiter-Übergabe zum Schicht- oder Tagesende über die offene Arbeit erstellen, nach Status gruppiert, für die nächste Schicht oder einen einzelnen namentlich benannten übernehmenden Techniker. (End-of-shift/EOD one-pager of open work for the next shift — in German.)
category: Localized
tools: [search_tickets, add_ticket_note, log_time_entry, search_members]
---

# Schichtübergabe

Deutsche Version von `skills/escalation/shift-handoff/SKILL.md` — kept in sync manually.

Baut einen in einer Minute lesbaren Übergabe-Einseiter: jeder offene Vorgang nach Status gruppiert, jeweils mit dem, was getan wurde, was als Nächstes ansteht, worauf er wartet und dem Risiko. Grenzt auf einen einzelnen übernehmenden Techniker ein, wenn einer benannt ist.

## Wann einsetzen

- Schicht- oder Tagesende: „übergib meine offene Arbeit an die Nachtschicht.“
- „Schreib eine Übergabe für <member> — er deckt morgen meine Warteschlange ab.“
- Eine Führungskraft will die offene Arbeit des Teams vor dem Schichtwechsel zusammengefasst haben.

## Schritte

1. Den Umfang festlegen. Ist ein einzelner übernehmender Techniker benannt, nur das übergeben, worauf diese Person handeln muss (die offenen/laufenden Tickets des abgebenden Technikers plus alles ausdrücklich für ihn Markierte) — nicht die gesamte Warteschlange des Teams. Andernfalls die offene Arbeit des Teams für die eintreffende Schicht abdecken.
2. Offene und laufende Tickets über search_tickets abrufen. Erreicht eine Suche eine Ergebnisobergrenze, das in der Ausgabe sagen, statt die Liste als vollständig darzustellen; bei breitem Durchgang die Suchen pro Board oder pro Status aufteilen.
3. Den Einseiter nach Status gruppieren (z. B. In Bearbeitung / Warten auf Kunde / Warten auf Hersteller / Geplant / Neu-unberührt), damit der Leser auf einen Blick sieht, was handlungsfähig und was geparkt ist.
4. Für jeden Vorgang den festen Vierzeiler nutzen: **Getan** (was in dieser Schicht erledigt wurde), **Nächstes** (die eine nächste Aktion und ihr Verantwortlicher), **Wartet** (wer oder was blockiert, und seit wann), **Risiko** (SLA-Nähe, Stimmung, VIP oder „keins“).
5. Die Seite mit einem Hinweisblock überschreiben: alles Zeitkritische in den nächsten Stunden, Tickets nahe am SLA und zugesagte Rückrufe.
6. Zum Tagesende anbieten, offene Zeit mit log_time_entry zu erfassen und Übergabenotizen pro Ticket (Klartext) für alles Ungelöste zu posten — nur nach Bestätigung posten.

## Leitplanken

- Auf Lesegeschwindigkeit optimieren: Der Übernehmende soll in unter einer Minute wissen, was aufzugreifen ist. Aggregieren; keine Ticket-Verläufe einfügen.
- Niemals Blocker oder SLA-Risiko-Punkte weglassen, um die Zusammenfassung zu kürzen.
- Ergebnisobergrenzen offenlegen — eine gekappte Suche nicht als vollständige Warteschlange darstellen.
- Ticketnotizen sind Klartext; der Einseiter selbst ist Konversationsausgabe, sofern nicht anders verlangt.
- Zum Übergeben eines laufenden, aktiven Gesprächs den Skill Live Call Transfer Brief nutzen — dieser Skill dient Schicht-/Tagesende-Übergaben.

## Lokale Konventionen (Deutsch)

- Uhrzeiten stets im 24-Stunden-Format („Einsatz geplant für 18:00 Uhr“, „Bereitschaft bis 22:00 Uhr“); Datum als TT.MM.JJJJ.
- „Übergabe“ und „Schichtübergabe“ sind die üblichen Begriffe; der Hinweisblock kann für eine Nachtschicht „Heute Nacht im Auge behalten“ heißen.
- Bei über die DACH-Region hinaus oder für Kunden außerhalb Deutschlands arbeitenden Desks die Zeitzone nennen („18:00 Uhr CET“).
- Unter Kollegen ist das Du in den Übergabezeilen zulässig, wenn es Hausbrauch ist; jede möglicherweise kundensichtbare Notiz wechselt zurück zum Sie.

## Konsolidiert

Die Skills für Schichtübergabe-Zusammenfassung, Tagesende-Bilanz und Briefing der nächsten Schicht.
