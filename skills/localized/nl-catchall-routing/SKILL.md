---
name: Catchall-routering
description: Bepaal de juiste klant en contactpersoon voor een ticket dat in een catchall- of bedrijfsloze mailbox belandde — inclusief doorgestuurde mail en leveranciersalerts — en wijs het opnieuw toe of geef kandidaten terug. (Route catchall/no-company tickets to the right client and contact — in Dutch.)
category: Localized
tools: [search_tickets, search_clients, search_contacts, assign_contact, update_ticket, add_ticket_note, create_ticket]
---

# Catchall-routering

Nederlandse versie van `skills/triage-and-routing/catchall-routing/SKILL.md` — handmatig synchroon gehouden.

Routeer een ticket dat zonder bedrijf binnenkwam (of op een catchall-contact staat) naar de echte klant en contactpersoon, via een expliciete bewijsladder — of laat het met rust wanneer het bewijs er niet is.

## Wanneer gebruiken

- Een ticket heeft geen bedrijf toegewezen of staat op een catchall-/bedrijfsloos contact.
- Mail is doorgestuurd ("FW:", "Fwd:", "Doorst.:") door een technicus of klant en het ticket staat op naam van de doorstuurder, niet van de oorspronkelijke afzender.
- Een leveranciers- of monitoringalert is in de catchall beland en de klant moet uit de alerttekst worden gehaald.
- Periodieke ronde langs het catchall-/intakeboard om verkeerd toegewezen tickets om te hangen.

## Stappen

1. Lees het ticket met `search_tickets`: titel, omschrijving, vroegste bericht, en volledige headers/geciteerde tekst indien aanwezig.
2. Spamvoorcontrole: is het bericht overduidelijk ongevraagde marketing of geautomatiseerde rommel zonder klantidentificerende inhoud en zonder teken dat een mens het bewust doorstuurde, stop dan en markeer het als spam ter beoordeling in plaats van het te routeren.
3. Haal identiteitsaanwijzingen op in deze volgorde van sterkte (de bewijsladder):
   a. Het e-maildomein van de werkelijke afzender (sterkst),
   b. Een expliciete bedrijfsnaam in de tekst of in alertvelden,
   c. Alleen een persoonsnaam (zwakst — nooit voldoende op zichzelf).
4. Begint het onderwerp met "FW:", "Fwd:" of "Doorst.:", of bevat de tekst een geciteerd origineel bericht, parseer dan de oorspronkelijke "Van:"/"From:"-regel en gebruik die afzender — niet de doorstuurder — als identiteitsbron.
5. Is het ticket een leveranciers-/monitoringalert, haal dan de gestructureerde velden (sitenaam, organisatie, apparaat, tenant) uit de alerttekst en gebruik die als bedrijfsaanwijzing.
6. Los het bedrijf op met `search_clients` op het domein of de gevonden naam; zoek daarna de contactpersoon met `search_contacts` binnen dat bedrijf. Heeft de afzender geen contactrecord, stel dan de dichtstbijzijnde match voor of noteer dat er een aangemaakt moet worden — koppel niet aan een naamgenoot.
7. Moet de huidige foute toewijzing eerst worden losgemaakt voordat de juiste kan worden toegepast op deze tenant, maak dan eerst los (zet het ticket terug op geen-bedrijf/catchall) en pas daarna het juiste bedrijf en contact toe.
8. Pas de match alleen toe met `update_ticket` en `assign_contact` wanneer precies één bedrijfskandidaat past op domein- of expliciete-naamsterkte. Presenteer anders de kandidaten en vraag het na.
9. Weigert de PSA-synchronisatie van de tenant bedrijfswijzigingen op een bestaand ticket, gebruik dan het sluiten-en-heraanmakenpad: `create_ticket` onder het juiste bedrijf met de oorspronkelijke tekst en een platte-tekstnotitie die beide ticketnummers kruisverwijst, en sluit daarna het origineel — in begeleid gebruik alleen met bevestiging.
10. Plaats een interne platte-tekstnotitie met `add_ticket_note` die vermeldt wat er gematcht is, welke sport van de bewijsladder is gebruikt, en wat er is gewijzigd.

## Vangrails

- Wijs nooit toe op naamgelijkenis alleen. Een overeenkomende voor-/achternaam zonder domein of expliciete bedrijfsbevestiging is laag vertrouwen → geen wijziging.
- Dubbelzinnigheid (twee of meer plausibele bedrijven) → geen wijziging; toon in plaats daarvan de kandidaten.
- Een naar een PSA gesynchroniseerd ticket moet altijd een bedrijf hebben; laat een PSA-gebonden ticket nooit bedrijfsloos achter — is geen match mogelijk, routeer het dan naar de aangewezen interne/catchall-klant volgens deskbeleid en markeer het.
- Sluiten-en-heraanmaken grenst aan destructief: bevestig in begeleid gebruik vóór uitvoering; bewaar de oorspronkelijke tekst en kruisverwijs beide nummers in platte tekst.
- Notities die naar de PSA gaan zijn platte tekst — geen markdown, geen emoji's.
- Verzin geen bedrijven, contactpersonen of ticketnummers. Kunnen zoekopdrachten resultaatlimieten hebben geraakt, meld dat dan.

## Lokale conventies (Nederlands)

- Nederlandse doorstuur- en antwoordmarkeringen: "Doorst.:" en "Fwd:" naast "FW:"; "Antw.:" naast "RE:" — de parser van het originele bericht moet deze voorvoegsels net zo behandelen als "FW:".
- Nederlandse en Belgische klantdomeinen eindigen vaak op .nl of .be; let op bedrijfsnamen met tussenvoegsels ("Van der Berg IT") en vergelijk domeinen zonder spaties en tussenvoegselvariaties (vanderbergit.nl).
- De interne notitie blijft in het Nederlands, maar uit een alert gehaalde velden (site name, organization, tenant) worden letterlijk overgenomen, zonder vertaling.
- Datums in DD-MM-JJJJ in de kruisverwijzende notities.

## Onbeheerde variant (Flows)

- Handel alleen op domeinmatch- of expliciete-bedrijfssterkte; bij alles zwakkers: wijzig niets en plaats één interne platte-tekstnotitie: `CATCHALL ROUTING: geen betrouwbare match. Gevonden aanwijzingen: <aanwijzingen>. Niet toegewezen gelaten voor menselijke beoordeling.`
- Gebruik het sluiten-en-heraanmakenpad nooit onbeheerd.
- Spamtak: sluit niet onbeheerd; markeer alleen via een notitie.
- Eén omhanging per ticket per run; draagt het ticket al een routeringsnotitie van deze skill, stop dan.
- Je volledige antwoord is de interne notitie — geen vertelstem, geen vragen.

## Consolideert

De skills voor catchall-naar-contactmapping, catchall-reparatie, eerst-losmaken-omhangingen, spamvoorcontrole bij intake, "FW:"-parsing van de oorspronkelijke afzender, veldextractie uit leveranciersalerts, bedrijfs-/contactidentificatie, verrijking zonder bedrijf, sluiten-en-heraanmaken en herrouteren-naar-de-juiste-klant (28 gedolven varianten).
