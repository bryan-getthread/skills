---
name: Phishingtriage
description: Een gebruiker meldde een verdachte e-mail of er landde een phishingmelding op het securityboard — beoordeel zonder de payload aan te raken, controleer de verspreiding, isoleer bij kwaadaardigheid en geef de melder een oordeel terug. (Phishing triage — assess without touching the payload, check blast radius, contain, reply to the reporter — in Dutch.)
category: Localized
tools: [search_tickets, search_contacts, search_knowledge_base, add_ticket_note, update_ticket, view_openDraft]
---

# Phishingtriage

Nederlandse versie van `skills/security/phishing-triage/SKILL.md` — handmatig synchroon gehouden.

Breng een gemelde verdachte e-mail van "is dit phishing?" naar een gedocumenteerd oordeel: indicatoren veilig vastgelegd, simulatieverkeer eruit gefilterd, alle andere ontvangers geïdentificeerd, isolatie geadviseerd waar nodig, en de melder beantwoord.

## Wanneer gebruiken

- "Is dit een phishingmail?" / een gebruiker stuurde iets verdachts door
- Een phishingmeldingsticket (meldknop, mailboxplugin of handmatig doorsturen) landt op het securityboard
- Een technicus wil een second opinion over een verdacht bericht voordat het wordt vrijgegeven of verwijderd

## Stappen

1. Leg de indicatoren vast vanuit het ticket zonder ermee te interacteren: afzenderadres en weergavenaam, reply-to, onderwerp, verzendtijd, elk linkdoel exact zoals geschreven, en bijlagenamen en -types. Open nooit links of bijlagen, en haal nooit een URL uit het bericht op om "even te kijken wat het is".
2. Simulatietak: vergelijk de afzender- en linkdomeinen met de gedocumenteerde domeinen van het phishingsimulatieplatform van de klant (search_knowledge_base voor het simulatieleveranciersrecord van de klant). Matcht het met een bekende simulator → classificeer als simulatie, sluit intern af met een platte-tekstnotitie die het gematchte domein noemt, en antwoord NIET aan de klant — een antwoord vertekent de metrics van hun simulatieprogramma. Stop hier.
3. Beoordeel de indicatoren: lookalike- of neefdomein, urgentie- en betalingslokkers, credential-harvestlink (weergavetekst wijkt af van het echte doel), onverwachte bijlagetypes, kaping van een echte eerdere conversatie. Zijn de volledige headers geplakt, geef de diepe analyse dan aan de skill email-header-analysis en verwerk diens oordeel.
4. Verspreidingscontrole: search_tickets op dezelfde afzender of hetzelfde onderwerp over de boards en recente tickets van de klant. Stel vast wie het bericht nog meer ontving en — cruciaal — of iemand klikte, antwoordde, inloggegevens invulde of een bijlage opende.
5. Heeft iemand geïnteracteerd met een bericht dat jij als kwaadaardig beoordeelt → escaleer onmiddellijk en start de skill compromised-account-containment voor de getroffen gebruiker(s). Isolatie gaat vóór het afmaken van het verslag: eerst indammen, daarna onderzoeken.
6. Lever het oordeel:
   - Kwaadaardig → adviseer quarantaine/verwijdering uit alle ontvangende mailboxen, blokkade van afzender/domein op de gateway, en een credentialreset voor iedereen die klikte.
   - Verdacht maar onbevestigd → zeg dat precies zo, met wat het zou bevestigen.
   - Legitiem → leg de signalen uit die het vrijpleiten, zodat de melder ervan leert.
7. Antwoord de melder met het oordeel in het vocabulaire van de defensive-writing-standard (een verdachte e-mail is een "gemeld bericht", geen "datalek"). Leg het oordeel, het bewijs en de beslisredenering vast als interne notitie; classificeer en zet de status volgens de skill soc-classification-tree. Bedank de melder — melden is precies het gedrag dat je herhaald wilt zien.

## Vangrails

- Open, klik, download of render nooit links of bijlagen uit het gemelde bericht. Leg indicatoren uitsluitend als tekst vast.
- Controleer de simulatiestatus in beide richtingen: maak nooit een echt incident aan voor een simulatie, en sluit nooit echte phishing af als simulatie op een gedeeltelijke domeinmatch — eis een exact gedocumenteerd simulatordomein.
- Antwoord de klant nooit op een bevestigde simulatie; sluit uitsluitend intern af.
- Geklikt of geantwoord betekent onmiddellijk escaleren — een valse escalatie is goedkoop, een gemiste compromittering niet.
- Beweer nooit "niemand anders heeft dit ontvangen" op basis van een begrensde zoekopdracht; rapporteer "geen andere meldingen gevonden in de laatste N doorzochte tickets".
- Documenteer de beslissing, niet alleen de actie: de notitie moet zeggen waaróm het oordeel is geveld, niet alleen wat er is gedaan.
- Defensief schrijven van begin tot eind: geen "datalek", geen "gehackt", geen impactclaims voordat de feiten bevestigd zijn.

## Lokale conventies (Nederlands)

- Defensief Nederlands vocabulaire: "gemeld bericht", "vermoedelijke phishingpoging", "ongebruikelijke activiteit" — nooit "hack" of "datalek" vóór formele bevestiging. Let op: "datalek" heeft onder de AVG een specifieke juridische lading en meldplicht — gebruik dat woord pas wanneer het incident daadwerkelijk als zodanig is vastgesteld.
- Het antwoord aan de melder blijft in de u-vorm en sluit af met een expliciet bedankje: "Bedankt voor het melden van dit bericht — dit is precies de juiste reactie."
- Tijdstempels in tijdlijnen in DD-MM-JJJJ en 24-uursnotatie, met tijdzone ("15-07-2026 09:12 CEST").
- Technische indicatoren (headers, URL's, bestandsnamen, foutmeldingen) worden letterlijk in het Engels overgenomen — nooit vertalen; ze dienen als bewijs en zoeksleutel.

## Consolideert

De skills voor phishingmeldingstriage, simulatie-identificatie, beoordeling van gemelde e-mails, verspreidingsrondes en melderantwoorden.
