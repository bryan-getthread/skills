---
name: Dienstoverdracht
description: Maak een scanbaar einde-dienst- of einde-dag-overzicht van één pagina met al het open werk, gegroepeerd op status, voor de volgende dienst of één benoemde ontvangende technicus. (End-of-shift/EOD one-pager of open work for the next shift — in Dutch.)
category: Localized
tools: [search_tickets, add_ticket_note, log_time_entry, search_members]
---

# Dienstoverdracht

Nederlandse versie van `skills/escalation/shift-handoff/SKILL.md` — handmatig synchroon gehouden.

Bouwt een overdrachtspagina die in één minuut te lezen is: elke open thread gegroepeerd op status, telkens met wat er is gedaan, wat er volgt, waarop het wacht en het risico. Beperkt zich tot één ontvangende technicus wanneer die met naam genoemd is.

## Wanneer gebruiken

- Einde dienst of einde dag: "draag mijn open werk over aan de nachtdienst."
- "Schrijf een overdracht voor <member> — die neemt morgen mijn wachtrij over."
- Een teamlead wil het open werk van het team samengevat vóór de dienstwissel.

## Stappen

1. Bepaal de scope. Is er één ontvangende technicus genoemd, draag dan alleen over waar die persoon op moet handelen (de open/lopende tickets van de vertrekkende technicus, plus alles wat expliciet voor hem of haar gemarkeerd is) — niet de hele teamwachtrij. Dek anders het open werk van het team voor de inkomende dienst.
2. Haal open en lopende tickets op via search_tickets. Raakt een zoekopdracht een resultaatlimiet, meld dat dan in de uitvoer in plaats van de lijst als volledig te presenteren; splits zoekopdrachten per board of per status bij een brede ronde.
3. Groepeer de pagina op status (bv. In behandeling / Wacht op klant / Wacht op leverancier / Gepland / Nieuw-onaangeraakt), zodat de lezer in één oogopslag ziet wat actiegericht is en wat geparkeerd staat.
4. Gebruik per thread de vaste vierregelige notatie: **Gedaan** (wat er deze dienst is uitgevoerd), **Volgende** (de ene volgende actie en de eigenaar), **Wacht op** (wie of wat blokkeert, en sinds wanneer), **Risico** (SLA-nabijheid, sentiment, VIP, of "geen").
5. Zet bovenaan een signaleringsblok: alles wat tijdkritisch is in de komende uren, tickets die de SLA naderen en toegezegde terugbelafspraken.
6. Bied bij einde dag aan om openstaande uren te loggen via log_time_entry en om per ticket overdrachtsnotities (platte tekst) te plaatsen voor alles wat onopgelost is — plaats alleen na bevestiging.

## Vangrails

- Optimaliseer op leessnelheid: de ontvanger moet in minder dan een minuut weten wat op te pakken. Aggregeer; plak geen ticketthreads.
- Laat blokkades of SLA-risico's nooit weg om de samenvatting kort te houden.
- Maak resultaatlimieten expliciet — presenteer een afgekapte zoekopdracht niet als de volledige wachtrij.
- Ticketnotities zijn platte tekst; het overzicht zelf is chatuitvoer tenzij anders gevraagd.
- Voor het overdragen van een lopend, live gesprek gebruik je de skill Live Call Transfer Brief — deze skill is voor dienst-/einde-dag-overdrachten.

## Lokale conventies (Nederlands)

- Tijden in 24-uursnotatie ("bezoek gepland om 18:00", "storingsdienst tot 22:00"); datums in DD-MM-JJJJ.
- "Overdracht" en "dienstwissel" zijn de gangbare termen; het signaleringsblok kan "Let op vannacht" heten voor een nachtdienst.
- Vermeld tijdzones wanneer de desk klanten buiten CET/CEST bedient ("18:00 CET").
- Tussen collega's is "je" in de overdracht gangbaar; elke notitie die de klant kan zien, gaat terug naar de u-vorm.

## Consolideert

De skills voor dienstoverdrachtssamenvattingen, einde-dag-afrondingen en briefings voor de volgende dienst.
