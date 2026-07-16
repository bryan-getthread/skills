---
name: Ruting fra catchall-postkasse
description: Identifiser riktig kunde og kontakt for en sak som havnet i en catchall- eller uten-selskap-postkasse — inkludert videresendt e-post og leverandørvarsler — og tildel den på nytt eller lever kandidatene tilbake. (Norwegian version of Catchall Routing — evidence-ladder routing of no-company tickets.)
category: Localized
tools: [search_tickets, search_clients, search_contacts, assign_contact, update_ticket, add_ticket_note, create_ticket]
---

# Ruting fra catchall-postkasse

Norsk versjon av `skills/triage-and-routing/catchall-routing` — kept in sync manually.

Ruter en sak som kom inn uten selskap (eller på en catchall-kontakt) til riktig kunde og kontakt, ved hjelp av en eksplisitt bevistrapp — eller lar den ligge når beviset ikke holder.

## Når skal den brukes

- En sak mangler selskap eller ligger på en catchall-/uten-selskap-kontakt.
- E-post ble videresendt inn («FW:» / «VS:») av en tekniker eller kunde, og saken er tilskrevet videresenderen, ikke den opprinnelige avsenderen.
- Et leverandør- eller overvåkningsvarsel havnet i catchall og kunden må hentes ut av varselteksten.
- Periodisk feiing av catchall-/inntaks-boardet for å flytte feiltilskrevne saker.

## Fremgangsmåte

1. Les saken med `search_tickets`: tittel, beskrivelse, tidligste melding og fullstendige headere/sitert tekst der det finnes.
2. Spam-forhåndssjekk: er meldingen åpenbart uønsket markedsføring eller automatisert søppel uten kundeidentifiserende innhold og uten tegn på at et menneske videresendte den av en grunn, stopp og flagg den som spam for gjennomgang i stedet for å rute den.
3. Trekk ut identitetsspor i denne styrkerekkefølgen (bevistrappen):
   a. E-postdomenet til den egentlige avsenderen (sterkest),
   b. Et eksplisitt selskapsnavn oppgitt i teksten eller i varselfeltene,
   c. Et personnavn alene (svakest — aldri tilstrekkelig i seg selv).
4. Starter emnet med «FW:» eller «VS:», eller inneholder teksten en sitert originalmelding, parse den opprinnelige «From:»-/«Fra:»-linjen og bruk den avsenderen — ikke videresenderen — som identitetskilde.
5. Er saken et leverandør-/overvåkningsvarsel, hent ut de strukturerte feltene (stedsnavn, organisasjon, enhet, tenant) fra varselteksten og bruk dem som selskapsspor.
6. Finn selskapet med `search_clients` på domenet eller det uttrukne navnet; finn deretter kontakten med `search_contacts` avgrenset til det selskapet. Har avsenderen ingen kontaktoppføring, foreslå nærmeste match eller noter at en må opprettes — ikke koble til en som bare ligner.
7. Må den gale tildelingen fjernes før den riktige kan settes på denne tenanten, fjern tildelingen først (sett saken tilbake til uten-selskap/catchall) og sett deretter riktig selskap og kontakt.
8. Utfør matchen med `update_ticket` og `assign_contact` kun når nøyaktig ett selskapskandidat passer med domene- eller eksplisitt-navn-styrke. Ellers: presenter kandidatene og spør.
9. Avviser tenantens PSA-synkronisering selskapsendringer på en eksisterende sak, bruk lukk-og-gjenopprett-veien: `create_ticket` under riktig selskap med originalteksten og et ren-tekst-notat som kryssrefererer begge saksnumrene, og lukk deretter originalen — kun med bekreftelse i betjent bruk.
10. Publiser et internt ren-tekst-notat med `add_ticket_note` som sier hva som ble matchet, hvilket trinn i bevistrappen som ble brukt, og hva som ble endret.

## Kjøreregler

- Tildel aldri på navnelikhet alene. Matchende for- og etternavn uten domene eller eksplisitt selskapsbekreftelse er lav sikkerhet → gjør ingen endring.
- Tvetydighet (to eller flere plausible selskaper) → ingen endring; list opp kandidatene i stedet.
- En sak synkronisert til en PSA må alltid ha et selskap; la aldri en PSA-koblet sak stå uten selskap — er ingen match mulig, rut den til den utpekte interne/catchall-kunden etter deskens policy og flagg den.
- Lukk-og-gjenopprett grenser mot det destruktive: i betjent bruk, bekreft før du gjør det; bevar originalteksten og kryssreferer begge numrene i ren tekst.
- Notater som skal til PSA-en er ren tekst — ingen markdown, ingen emojier.
- Finn ikke på selskaper, kontakter eller saksnumre. Kan søkene ha truffet resultattak, si det.

## Lokale konvensjoner (norsk)

- Norske e-postklienter (særlig Outlook på norsk) merker videresendinger som «VS:» og svar som «SV:»; behandle «VS:» etter samme regel som «FW:», og se etter «Fra:»-linjen i tillegg til «From:» i sitert tekst.
- Vær obs på æ/ø/å i domenenavn og selskapsnavn: selskapet «Sørby AS» kan ha domenet «sorby.no» — normaliser norske tegn før du sammenligner navn mot domener.
- Private domener (gmail.com, hotmail.no, online.no) er ikke selskapsbevis — behandle dem som trinn c (svakt), ikke trinn a.
- Suffikser som AS, ASA, ANS og ENK skiller juridiske enheter; «Berg AS» og «Berg Consulting AS» er ikke samme kandidat.
- Det interne notatet kan skrives på norsk, men behold det engelske markørprefikset `CATCHALL ROUTING:` — senere kjøringer bruker det til å oppdage at saken allerede er behandlet.

## Uovervåket variant (Flows)

- Handle kun ved domene-match eller eksplisitt-selskap-styrke; ved noe svakere, gjør ingen endring og publiser ett internt ren-tekst-notat: `CATCHALL ROUTING: ingen sikker match. Funnet holdepunkter: <clues>. Latt stå utildelt for manuell gjennomgang.`
- Bruk aldri lukk-og-gjenopprett-veien uovervåket.
- Spam-gren: ikke lukk uovervåket; flagg kun via notat.
- Én omtildeling per sak per kjøring; bærer saken allerede et rutingnotat fra denne skillen, stopp.
- Hele svaret ditt er det interne notatet — ingen fortellerstemme, ingen spørsmål.

## Konsoliderer

Skills for catchall-til-kontakt-mapping, catchall-reparasjon, fjern-tildeling-først-omtildelinger, spam-forhåndssjekk ved inntak, parsing av original avsender i FW:-headere, feltuttrekk fra leverandørvarsler, selskaps-/kontaktidentifisering, uten-selskap-beriking, lukk-og-gjenopprett-omtildelinger og rut-til-riktig-kunde (28 utvunnede varianter).
