---
name: Dagelijkse samenvatting
description: Een technicus vraagt om een overzicht van zijn of haar open tickets — wat wacht op antwoord, wat is urgent, wat staat vandaag gepland — te scannen in minder dan een minuut, met een ultrakorte 3-regelvariant. (Dutch daily digest of a technician's open tickets — replies due, urgent items, today's schedule.)
category: Localized
tools: [search_tickets, search_members]
---

# Dagelijkse samenvatting

Nederlandse versie van `skills/personal-productivity/daily-digest/SKILL.md` — handmatig synchroon gehouden.

De ochtendleesbeurt: alles wat op je bord ligt, gesorteerd op wat nú aandacht nodig heeft. Gebouwd om in minder dan een minuut te scannen en om de allereerste actie van de dag bij naam te noemen. Ook het patroon dat de meeste partners als geplande ochtendskill draaien.

## Wanneer gebruiken

- "Geef me een overzicht van mijn open tickets" / "wie wacht op antwoord, is er iets urgents?"
- "Ochtendoverzicht" / "hoe ziet mijn dag eruit?"
- "Korte versie" — de 3-regelvariant
- Ingebed in een Flow als gepland dagstartbriefing

## Stappen

1. Haal alle open tickets van het aanvragende teamlid op met search_tickets. Als een resultaatlimiet de lijst kan hebben afgekapt, meld dat meteen ("50 getoond — er kunnen er meer zijn") in plaats van de samenvatting als volledig te presenteren.
2. Sorteer elk ticket in precies één categorie, in deze prioriteitsvolgorde (een ticket valt in de eerste categorie waarvoor het kwalificeert):
   - **Wacht op jouw antwoord** — de klant reageerde als laatste en wacht op jou. Sorteer op wachttijd.
   - **Urgent / risico** — hoge prioriteit, SLA op of nabij overschrijding, of threads met negatief sentiment. Sorteer op ernst.
   - **Vandaag gepland** — tickets met een agenda-item of toegezegde opvolging vandaag, in tijdsvolgorde.
   - **Wachten op anderen** — de klant, een leverancier of een escalatie is aan zet. Markeer alles dat 3+ dagen stil is (kandidaat voor een herinnering), maar laat deze categorie de top niet verdringen.
   - **De rest** — alleen een aantal en een typering in één regel; geen regels per ticket.
3. Elke ticketregel is één regel: nummer, klant (kort), status in 5–8 woorden, en de volgende actie ("#1234 <client> — printer 2 d offline, klant reageerde gisteren → bevestigen dat de fix werkt").
4. Open de samenvatting met **Begin hier:** het belangrijkste ticket en één zin waarom (het langst wachtende klantantwoord wint van vage urgentie; een harde SLA-overschrijding wint van alles).
5. Sluit af met de vorm van de dag: totalen per categorie en één zin ("2 antwoorden en 1 urgent ticket vóór je geplande bezoek van 10:00 haalt de meeste druk eraf").
6. Als om de korte versie wordt gevraagd (of "3 regels"), lever exact drie regels: (1) Begin hier + waarom, (2) de tellingen — X wachten op antwoord / Y urgent / Z gepland, (3) het ene ding dat vandaag pijn gaat doen als het genegeerd wordt. Geen koppen, geen inleiding.
7. Bied het natuurlijke vervolg aan: "Wil je conceptantwoorden voor de tickets die op jou wachten?"

## Vangrails

- Beperk je strikt tot de eigen tickets van het aanvragende teamlid; neem nooit de wachtrijen van andere technici mee tenzij expliciet gevraagd (dat is een teamleadrapport, geen persoonlijk overzicht).
- Alleen-lezen: de samenvatting verandert niets — geen statusupdates, geen notities, geen herinneringen — tenzij het teamlid daar als vervolg om vraagt.
- Verzin nooit ticketnummers, SLA-tijden of klantreacties; is de laatste activiteit onduidelijk, zet het ticket dan in de veiligere categorie (Wacht op jouw antwoord) in plaats van het te verstoppen onder Wachten op anderen.
- Elke categorieregel moet actiegericht zijn — een status zonder volgende actie is een verspilde regel.
- Houd de volledige samenvatting scanbaar: past hij niet op één scherm, maak de regels dan strakker; nooit opvullen.

## Lokale conventies (Nederlands)

- Datums in DD-MM-JJJJ ("15-07-2026"); tijden in 24-uursnotatie ("10:00", "14:30").
- De samenvatting is een intern document: tussen collega's is "je" gangbaar op Nederlandse en Vlaamse desks; alles wat richting de klant gaat, blijft standaard in de u-vorm tot de klant zelf tutoyeert.
- Gebruikelijke interne afkortingen ("z.s.m.", "n.a.v.", "t.b.v.") mogen in interne regels, nooit in klanttekst.

## Onbeheerde variant (Flows)

- Je volledige antwoord wordt letterlijk als ochtendbriefing afgeleverd — geen vertelstem, geen vragen, geen "laat maar weten".
- Neem altijd de Begin hier-regel en de tellingen per categorie op; laat het vervolgaanbod weg.
- Is de wachtrij leeg, lever dan exact: "Geen open tickets aan jou toegewezen. Geniet van de schone lei."
- Als de ticketzoekopdracht mislukt of onwaarschijnlijk leeg terugkomt voor een actieve technicus, lever dan één regel dat de samenvatting niet gegenereerd kon worden — nooit een verzonnen samenvatting.
