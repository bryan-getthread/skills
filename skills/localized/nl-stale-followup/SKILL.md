---
name: Opvolgcadans wachtende tickets
description: Draai een instelbare opvolgcadans (bijvoorbeeld 24/48/72 uur, drie pogingen) op tickets die op een klantreactie wachten — stel elke vriendelijke herinnering op, tel gedocumenteerde pogingen en sla tickets met een legitieme wachtreden over. (Follow-up cadence on waiting-on-client tickets, with Dutch client-facing nudges.)
category: Localized
tools: [search_tickets, add_ticket_note, update_ticket, schedule_ticket]
---

# Opvolgcadans wachtende tickets

Nederlandse versie van `skills/qa-and-closure/stale-ticket-followup-cadence/SKILL.md` — handmatig synchroon gehouden.

Houdt wacht-op-klant-tickets in beweging op een voorspelbaar ritme. Telt de pogingen die al in de thread gedocumenteerd zijn, stelt de volgende herinnering op in een vriendelijke klantgerichte toon, en weet wanneer een ticket met goede reden wacht en met rust gelaten moet worden.

## Wanneer gebruiken

- "Volg mijn wachtende tickets op" / "wie heeft ons nog niet geantwoord?"
- Het draaien van de standaardcadans van de desk (24/48/72 uur, of de huisvariant) over een wachtrij.
- Een technicus wil de volgende herinnering opgesteld hebben voor één specifiek stil ticket.
- Ingebed in een Flow op een schema of op een wachtstatus-timer (zie de onbeheerde variant).

## Stappen

1. Bevestig de toe te passen cadans. Standaard: eerste opvolging 24 uur na het laatste uitgaande klantzichtbare bericht, tweede op 48, derde op 72 — drie pogingen in totaal, daarna overdracht aan de skill No-Response Closure Sequence. Honoreer een deskspecifieke cadans als de gebruiker die noemt ("3 dagen / 3 pogingen / 3 e-mails" is een gangbare huisregel).
2. Vind kandidaten met `search_tickets`: open tickets in wacht-op-klant-statussen waarvan het laatste bericht uitgaand is en ouder dan de huidige cadansstap.
3. Tel per kandidaat de **gedocumenteerde pogingen**: klantzichtbare opvolgberichten sinds de klant voor het laatst reageerde. Alleen berichten die de klant daadwerkelijk kon zien tellen — interne notities zijn geen pogingen.
4. Sla legitieme wachtredenen over: een toekomstige geplande afspraak (`schedule_ticket`-datum), een door de klant genoemde termijn ("maandag terug van vakantie"), een leveranciers-/derdenafhankelijkheid, of een lopende goedkeuring. Herinner niemand die je heeft verteld wanneer hij of zij reageert.
5. Stel voor elk overgebleven ticket de opvolging op in de klantgerichte toon: vriendelijk, kort, verwijst in één regel naar het oorspronkelijke probleem, benoemt wat er van de klant nodig is, en biedt een makkelijke uitweg ("is het inmiddels opgelost, laat het ons weten en dan sluiten we het ticket"). Laat de toon per poging voorzichtig oplopen — poging drie meldt dat het ticket binnenkort zonder reactie wordt gesloten, en klinkt nooit geïrriteerd.
6. Presenteer de concepten ter controle en plaats daarna elke goedgekeurde opvolging als klantzichtbare notitie met `add_ticket_note`; zet de passende wachtstatus met `update_ticket`.
7. Lever een samenvattingstabel: ticket, klant, zojuist verstuurd pogingnummer (of overgeslagen en waarom), en de datum waarop de volgende cadansstap vervalt.

## Vangrails

- Overschrijd nooit het aantal pogingen: toont de thread al drie gedocumenteerde pogingen, stuur dan door naar de afsluitprocedure in plaats van een vierde herinnering.
- Tel alleen wat gedocumenteerd is — ga er nooit van uit dat er telefonisch is opgevolgd tenzij een notitie dat vastlegt.
- Klantzichtbare concepten bevatten geen intern jargon, geen ticketsysteem-boilerplate en geen verzonnen details over het probleem.
- De overslaanlijst is heilig: een ten onrechte herinnerde klant op vakantie of vlak voor een gepland bezoek kost vertrouwen. Is de legitimiteit van het wachten onduidelijk, sla over en markeer in plaats van te versturen.
- Verstuur nooit bulk-opvolgingen zonder dat de gebruiker de concepten heeft beoordeeld.
- Houd de taal eenvoudig en lokaliseerbaar; volg de taal van de klant wanneer de thread niet in het Nederlands is.

## Lokale conventies (Nederlands)

- U-vorm in herinneringen totdat de klant zelf tutoyeert; toon vriendelijk maar direct ("Wij wachten nog op uw reactie" mag gewoon gezegd worden — Nederlandse zakelijke communicatie waardeert directheid).
- Gevoelige periodes: de bouwvak en zomervakanties (juli–augustus, regionaal gespreid), meivakantie en de feestdagen — stilte in die vensters is vaker een legitieme wachtreden; controleer afwezigheidsberichten voordat je een poging telt.
- Datums in DD-MM-JJJJ; "binnen 48 uur" is de gangbare termijnformulering.
- Standaard-uitweg: "Is het probleem inmiddels opgelost? Een kort berichtje volstaat en wij sluiten het ticket." Afsluitwaarschuwing (3e poging): "Zonder reactie vóór <datum> sluiten wij dit ticket — u kunt het altijd heropenen door deze e-mail te beantwoorden."

## Onbeheerde variant (Flows)

- Trigger: schema (bv. uurlijkse ronde) of wachtstatus-leeftijdstimer.
- Je volledige antwoord is het klantzichtbare opvolgbericht, letterlijk geplaatst — geen vertelstem, geen pogingtellers, geen intern commentaar. Schrijf niets anders.
- Deterministische regels: verstuur alleen wanneer (a) het ticket in een wacht-op-klant-status staat, (b) het laatste bericht uitgaand is, (c) de cadansstap vervallen is, en (d) de gedocumenteerde pogingen onder het maximum liggen. Faalt een voorwaarde, of oogt het wachten legitiem, lever dan niets en stop.
- Stel poging drie of hoger nooit onbeheerd op als de desk menselijke goedkeuring vereist vóór laatste kennisgevingen — controleer het geconfigureerde pogingenplafond en stop eronder.
