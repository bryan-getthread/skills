---
name: Sakstriagering
description: Klassifiser en ny eller utildelt sak, vurder alvorlighet, fang opp duplikater og anbefal hvor den skal, før den går inn i køen. (Norwegian version of Ticket Triage — first-pass classification, severity, duplicates, and routing proposal.)
category: Localized
tools: [search_tickets, list_boards, list_ticket_statuses, list_ticket_priorities, search_clients, update_ticket]
---

# Sakstriagering

Norsk versjon av `skills/triage-and-routing/ticket-triage` — kept in sync manually.

Førstegjennomgang av en ny eller utildelt sak: klassifiser den, tving frem riktig alvorlighet, fang opp duplikater og foreslå board, status og prioritet — og utfør først etter bekreftelse.

## Når skal den brukes

- En ny sak har nettopp kommet inn og trenger en førstegjennomgang før fordeling.
- «Triager denne saken» / «klassifiser dette» / «hvor skal denne?»
- Morgengjennomgang av utildelte saker på inntaks-boardet.
- En dispatcher vil ha forslag til board, status og prioritet med en énlinjes køoppsummering.

## Fremgangsmåte

1. Les hele tråden med `search_tickets`, inkludert den tidligste meldingen og hvert svar. Ikke klassifiser eller rut ut fra tittelen alene — titler er ofte utdaterte, autogenererte eller feil.
2. Klassifiser saken i deskens kategorier (f.eks. tilgang, maskinvare, programvare, nettverk, e-post, sikkerhet). Baser vurderingen på innholdet i henvendelsen, ikke nøkkelord i emnefeltet.
3. Bruk reglene for tvungen alvorlighet før enhver skjønnsvurdering: alt sikkerhetsrelatert (kompromittering, phishing, skadevare, uventet MFA-/påloggingsaktivitet) eller som rammer flere brukere eller et helt kontorsted, er høy prioritet som standard. Bare innmelderens egne ord som nedgraderer det, bekreftet med teknikeren, kan senke den.
4. Sjekk duplikater med sterke identifikatorer, ikke lignende ordlyd: samme varsel-ID, samme enhets-/ressursnavn, samme overvåkningsreferanse, eller samme kontakt + samme spesifikke feil innenfor et nylig tidsvindu. Kjør et separat `search_tickets`-søk per identifikator; hvis et søk kan ha truffet resultattaket, si det.
5. Slå opp kunden med `search_clients` for å bekrefte at selskapet på saken stemmer med innmelderen.
6. Foreslå: board, status, prioritet, kategori og en énlinjes køoppsummering. List opp mistenkte duplikater med saksnumre og hvorfor de matchet.
7. Først etter at den som gjennomgår har bekreftet forslaget, utfør det med `update_ticket`. Endrer de noe, utfør deres versjon, ikke din.

## Kjøreregler

- Rut eller klassifiser aldri ut fra tittelen alene; les alltid tråden først.
- Endre aldri saken før den foreslåtte klassifiseringen er bekreftet. Denne skillen anbefaler; et menneske (eller en separat konfigurert uovervåket skill) utfører.
- Sikkerhet eller flerbruker-påvirkning tvinger høy prioritet — la ikke en rolig tone i meldingen prate deg ned.
- Duplikatvurderinger krever match på en sterk identifikator. Lignende ordlyd er et spor å nevne, aldri grunnlag for å slå sammen eller lukke.
- Hvis navnene på board/status/prioritet er tvetydige for denne tenanten, hent dem med `list_boards` / `list_ticket_statuses` / `list_ticket_priorities` i stedet for å gjette etiketter.
- Finn ikke på saksnumre, kunder eller kategorier. Kan ikke saken klassifiseres med rimelig sikkerhet, si det og la den ligge til et menneske.

## Lokale konvensjoner (norsk)

- Norske tenants blander ofte norske og engelske etiketter på boards og statuser («Venter på kunde» ved siden av «In Progress») — hent alltid de faktiske etikettene med listeverktøyene; oversett aldri statusnavn på egen hånd.
- I énlinjes-oppsummeringen: datoer som DD.MM og klokkeslett i 24-timersformat.
- Vær obs på norske tegn (æ/ø/å) i selskaps- og kontaktnavn når du matcher duplikater og kunder — «Sørlie» og «Sorlie» kan være samme entitet skrevet med og uten norske tegn; et personnavn alene er uansett aldri nok.
- Behold produkttermene på engelsk (board, status, priority) når du refererer til verktøyfelt; skriv naturlig norsk ellers.

## Konsoliderer

Skills for morgentriagering, dispatch-triagehjelp, P1-triagering, duplikatbevisst inntak og generisk «klassifiser denne saken».
