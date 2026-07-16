---
name: Tickettriage
description: Classificeer een nieuw of niet-toegewezen ticket, weeg de ernst, vang duplicaten af en adviseer waar het heen moet voordat het de wachtrij ingaat. (Classify a new/unassigned ticket, gauge severity, catch duplicates, recommend routing — in Dutch.)
category: Localized
tools: [search_tickets, list_boards, list_ticket_statuses, list_ticket_priorities, search_clients, update_ticket]
---

# Tickettriage

Nederlandse versie van `skills/triage-and-routing/ticket-triage/SKILL.md` — handmatig synchroon gehouden.

Eerste lezing van een nieuw of niet-toegewezen ticket: classificeren, de juiste ernst afdwingen, duplicaten afvangen en board, status en prioriteit voorstellen — en pas toepassen na bevestiging.

## Wanneer gebruiken

- Er is net een nieuw ticket binnengekomen dat een eerste beoordeling nodig heeft vóór dispatch.
- "Triageer dit ticket" / "classificeer dit" / "waar moet dit heen?"
- Ochtendronde langs niet-toegewezen tickets op het intakeboard.
- Een dispatcher wil een voorgesteld board, status en prioriteit met een wachtrijsamenvatting van één regel.

## Stappen

1. Lees de volledige thread met `search_tickets`, inclusief het vroegste bericht en elke reactie. Classificeer of routeer nooit op de titel alleen — titels zijn vaak verouderd, automatisch gegenereerd of fout.
2. Classificeer het probleem in de categorieën van de desk (bv. toegang, hardware, software, netwerk, e-mail, security). Baseer het oordeel op de inhoud van het verzoek, niet op trefwoorden in het onderwerp.
3. Pas de dwingende ernstregels toe vóór elke eigen afweging: alles wat security-gerelateerd is (compromittering, phishing, malware, onverwachte MFA-/inlogactiviteit) of meerdere gebruikers dan wel een hele locatie raakt, krijgt standaard hoge prioriteit. Alleen de eigen woorden van de aanvrager die het afschalen, bevestigd met de technicus, mogen dat verlagen.
4. Controleer op duplicaten met sterke identificatoren, niet met tekstgelijkenis: zelfde alert-ID, zelfde apparaat-/assetnaam, zelfde monitoringreferentie, of zelfde contact + zelfde specifieke foutmelding binnen een recent venster. Draai een aparte `search_tickets` per identificator; kan een zoekopdracht een resultaatlimiet hebben geraakt, meld dat dan.
5. Zoek de klant op met `search_clients` om te bevestigen dat het bedrijf op het ticket bij de aanvrager hoort.
6. Stel voor: board, status, prioriteit, categorie en een wachtrijsamenvatting van één regel. Vermeld vermoede duplicaten met ticketnummers en de reden van de match.
7. Pas pas ná bevestiging door de beoordelaar toe met `update_ticket`. Wijzigt die iets, pas dan zijn of haar versie toe, niet de jouwe.

## Vangrails

- Routeer of classificeer nooit op de titel alleen; lees altijd eerst de thread.
- Wijzig het ticket nooit voordat de voorgestelde classificatie is bevestigd. Deze skill adviseert; een mens (of een apart geconfigureerde onbeheerde skill) past toe.
- Security of impact op meerdere gebruikers dwingt hoge prioriteit af — laat je niet omlaagpraten door een kalme toon in het bericht.
- Een duplicaatoordeel vereist een match op een sterke identificator. Vergelijkbare bewoording is een aanwijzing om te noemen, nooit een grond om samen te voegen of te sluiten.
- Zijn board-/status-/prioriteitsnamen voor deze tenant onduidelijk, haal ze dan op met `list_boards` / `list_ticket_statuses` / `list_ticket_priorities` in plaats van labels te raden.
- Verzin geen ticketnummers, klanten of categorieën. Kan het ticket niet met vertrouwen worden geclassificeerd, zeg dat dan en laat het over aan een mens.

## Lokale conventies (Nederlands)

- Wachtrijsamenvattingen en interne notities in het Nederlands, datums in DD-MM-JJJJ en tijden in 24-uursnotatie; prioriteiten en statussen behouden hun exacte labels zoals geconfigureerd in de tenant (ook als die Engels zijn) — vertaal ze niet ter plekke.
- Herken Nederlandse doorstuur- en antwoordmarkeringen in onderwerpen: "FW:", "Fwd:" en "Doorst.:" voor doorgestuurde mail, "RE:" en "Antw.:" voor reacties — het geciteerde origineel weegt zwaarder dan de doorstuurder.
- Klantzichtbare ernsttaal blijft nuchter: "verstoring" of "storing" in plaats van dramatische termen; alarmerende kwalificaties horen nooit in een klantzichtbare samenvatting.
- Vertaal geciteerde Engelse foutmeldingen in het ticket niet: ze dienen als zoekidentificatoren.

## Consolideert

De skills voor ochtendtriage, dispatchondersteuning, P1-triage, intake met duplicaatdetectie en generiek "classificeer dit ticket".
