---
name: Urenregistratie opschonen
description: Zet ruwe, rommelige tijdnotities om in nette, gestandaardiseerde urenregistraties — klantzichtbare en interne versies — waarbij alleen werk wordt vastgelegd dat de engineer expliciet als uitgevoerd heeft benoemd. (Turn rough time notes into clean, standardized time entries in Dutch — client-facing and internal versions.)
category: Localized
tools: [search_tickets, log_time_entry, add_ticket_note]
---

# Urenregistratie opschonen

Nederlandse versie van `skills/documentation/time-entry-cleanup/SKILL.md` — handmatig synchroon gehouden.

Normaliseer rommelige tijdnotities naar het standaard urenregistratieformat van de desk, met gescheiden klantzichtbare bewoording en intern detail, zonder ooit werk te verzinnen of op te waarderen.

## Wanneer gebruiken

- Een technicus plakt ruwe steno ("ww reset, getest ok, gebr geïnformeerd") en wil een nette urenregistratie.
- "Schoon mijn tijdnotities voor dit ticket op."
- Einde van de dag: meerdere ruwe registraties moeten gestandaardiseerd worden vóór de factuurcontrole.
- Een gespreksafronding moet een factureerbare registratie met een duur worden.

## Stappen

1. Lees de ruwe notities; haal omliggende context alleen uit de ticketthread met `search_tickets` om verwijzingen op te lossen (welk ticket, welke <user>, welk <device>) — nooit om werkonderdelen toe te voegen.
2. Herschrijf elke registratie in heldere verleden tijd, in de vaste veldvolgorde:
   1. **Probleem** — wat er gemeld is.
   2. **Uitgevoerd werk** — wat de engineer expliciet zei te hebben gedaan.
   3. **Resultaat** — de uitkomst zoals benoemd.
   4. **Volgende stappen** — alleen als de engineer er een noemde.
3. Pas de **nul-aannameregel** toe: neem alleen op wat de engineer expliciet zei te hebben gedaan. Zet nooit een aanbeveling, voornemen of "zou moeten" om in een voltooide actie ("herstart aanbevolen" mag NIET "herstart uitgevoerd" worden). Zijn notities dubbelzinnig, vraag het na of markeer het als `[onduidelijk — bevestigen]`.
4. Pas de verboden-woordvervangingen van de huisstijl toe — gangbare set: "gefikst" → "opgelost"; "probleem met die gast" → "probleem gemeld door <user>"; "verprutst" → "verkeerd geconfigureerd"; "zal wel goed zijn" → "werkend geverifieerd" **alleen als de verificatie is benoemd**, anders "in afwachting van bevestiging door de gebruiker". Honoreer de eigen verbodenlijst van de desk wanneer die is aangeleverd.
5. Lever op verzoek twee versies:
   - **Klantzichtbaar** — helder zakelijk taalgebruik, geen interne steno, geen verwijten, geen gemopper op leveranciers, platte tekst (PSA-veilig).
   - **Intern** — volledig technisch detail, hypotheses en opvolgmarkeringen.
6. Lever de opgeschoonde registraties met tijdsduur. Schrijf pas met `log_time_entry` / `add_ticket_note` nadat de technicus de tekst én de tijdsduur heeft bevestigd.

## Vangrails

- **Nul aanname is absoluut.** Geen afgeleid werk, geen afgeleide verificatie, geen afgeleide duur. Is een duur niet genoemd, vraag ernaar — schat niet stilzwijgend.
- Behoud de betekenis van de technicus; alleen de bewoording opschonen. Factureerbaar detail moet trouw blijven aan wat de notities beschrijven.
- Zet nooit interne opmerkingen (verwijten, klantfrictie, securityvermoedens) in de klantzichtbare versie.
- Bevestig vóór het vastleggen: registraties raken de facturatie. Bij twijfel: lever alleen tekst en laat de technicus zelf indienen.

## Lokale conventies (Nederlands)

- Duur in de gangbare desknotatie: "1,5 uur" of "45 min"; datums in DD-MM-JJJJ.
- De voltooid tegenwoordige tijd is de natuurlijke registratievorm ("Ik heb het wachtwoord gereset en de login geverifieerd").
- De klantzichtbare versie blijft in de u-vorm en vermijdt jargon-anglicismen waar een gangbaar Nederlands woord bestaat ("opnieuw opgestart" i.p.v. "gereboot"); de interne versie mag het gebruikelijke technische vocabulaire van de desk houden.
- Vertaal Engelse technische strings niet (foutmeldingen, servicenamen, commando's): die blijven letterlijk staan.

## Consolideert

De skills voor urenregistratiesamenvattingen, het herformatteren van ruwe tijdnotities, gespreksafrondingen en notitiestijlen met verboden woorden.
