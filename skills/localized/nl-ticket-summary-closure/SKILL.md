---
name: Ticketsamenvatting en afsluitnotitie
description: Maak een heldere samenvatting van wat er op een ticket gebeurde — oplossingsnotitie, afsluitnotitie of een gesjabloneerde P1/P2-overdrachtssamenvatting — in het gevraagde format en perspectief. (Ticket summary, closure note, or templated P1/P2 handoff summary — in Dutch.)
category: Localized
tools: [search_tickets, add_ticket_note]
---

# Ticketsamenvatting en afsluitnotitie

Nederlandse versie van `skills/documentation/ticket-summary-and-closure-note/SKILL.md` — handmatig synchroon gehouden.

Vat een ticketthread samen tot precies de notitie die de situatie vraagt: een oplossings-/afsluitnotitie, een "wat is er gedaan"-recap, of een gesjabloneerde escalatie-overdrachtssamenvatting.

## Wanneer gebruiken

- "Schrijf een afsluitnotitie voor dit ticket" / "vat samen wat er is gedaan."
- "Geef me een oplossingsnotitie in ons standaardformat."
- Een P1/P2 gaat over naar een andere technicus of het avond-/nachtteam en heeft een gestructureerde overdrachtssamenvatting nodig.
- "Vat dit in de eerste persoon samen voor mijn urenregistratie / PSA-notitie."

## Stappen

1. Lees de volledige thread met `search_tickets`: reacties, interne notities en urenregistraties. Stel vast: probleem → uitgevoerde acties → resultaat → wat er nog openstaat.
2. Kies het format dat het verzoek vraagt:
   - **Oplossings-/afsluitnotitie** — Probleem, Uitgevoerde acties, Resultaat, en (alleen als er werk overblijft) één duidelijke Volgende stap met eigenaar en deadline.
   - **P1/P2-overdrachtssamenvatting** — vast sjabloon: Huidige status; Impact (wie/wat ligt eruit); Tijdlijn van sleutelmomenten met tijdstempels; Tot nu toe uitgevoerde acties; Wat is uitgesloten; Volgende actie + eigenaar; Stand van de klantcommunicatie (wie is wat verteld, en wanneer).
   - **Korte recap** — korte lopende tekst voor een klantzichtbaar afsluitbericht.
3. Pas het gevraagde perspectief toe: standaard derde persoon, of op verzoek **eerste persoon vanuit de technicus** ("Ik heb het profiel gereset en de login geverifieerd") — schrijf als de toegewezen technicus, niet als een AI.
4. Pas de opmaakregels van de desk-skill **note-format-standard** toe als die geladen is. Synchroniseert de notitie naar een PSA, gebruik dan **uitsluitend platte tekst**: geen markdown, geen emoji's, geen typografische aanhalingstekens, geen gedachtestreepjes.
5. Lever de notitie in de chat ter controle. Plaats hem alleen met `add_ticket_note` wanneer daar expliciet om is gevraagd, en plaats overdrachts-/QA-samenvattingen als **interne** notities tenzij anders opgedragen.

## Vangrails

- Verzin geen details die de thread niet onderbouwt — geen gefabriceerde bevestigingen, tijdstempels of grondoorzaken. Is de uitkomst onduidelijk, schrijf dan "uitkomst niet bevestigd in de thread" in plaats van oplossing te claimen.
- Volg het gevraagde format precies, inclusief "geen opsommingstekens" of woordlimieten wanneer die zijn opgegeven.
- Eerste persoon verandert alleen de stem — voeg nooit werk toe dat de technicus niet heeft vastgelegd.
- Scheid in overdrachtssamenvattingen feit en hypothese expliciet ("Bevestigd:" vs. "Vermoed:").
- Neem nooit inloggegevens of eenmalige codes uit de thread op in welke samenvatting dan ook.

## Lokale conventies (Nederlands)

- Tijdlijnen in DD-MM-JJJJ en 24-uursnotatie ("15-07-2026 14:30"); vermeld de tijdzone als de desk meerdere zones bedient ("CET/CEST").
- Een klantzichtbare notitie blijft in de u-vorm en nuchter zakelijk; de standaardafsluiting eindigt met: "Is er toch iets niet opgelost? Beantwoord dan gewoon deze e-mail — uw reactie heropent dit ticket."
- Sjabloonkoppen mogen tweetalig blijven als de PSA van de desk Engelstalig is ("Current Status / Huidige status") — volg het gevestigde gebruik van de tenant in plaats van een vertaling op te dringen.
- Geciteerde Engelse technische foutmeldingen blijven letterlijk in het Engels staan.

## Consolideert

Afsluitnotitieverzoeken, oplossingssamenvattingen, P1/P2-overdrachtssjablonen en "volgende stap"-situatiesamenvattingen.
