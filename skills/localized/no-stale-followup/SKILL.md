---
name: Oppfølgingskadens for stille saker
description: Kjør en konfigurerbar oppfølgingskadens (for eksempel 24/48/72 timer, tre forsøk) på saker som venter på svar fra kunden — skriv hvert vennlige purreutkast, tell dokumenterte forsøk og hopp over saker i legitim venting. (Norwegian version of Stale Ticket Follow-Up Cadence.)
category: Localized
tools: [search_tickets, add_ticket_note, update_ticket, schedule_ticket]
---

# Oppfølgingskadens for stille saker

Norsk versjon av `skills/qa-and-closure/stale-ticket-followup-cadence` — kept in sync manually.

Holder venter-på-kunde-saker i bevegelse med en forutsigbar rytme. Teller forsøkene som allerede er dokumentert i tråden, skriver utkast til neste purring i en vennlig kunderettet stemme, og vet når en sak venter av en god grunn og skal få være i fred.

## Når skal den brukes

- «Følg opp sakene mine som venter» / «hvem har ikke svart oss?»
- Kjøre deskens standardkadens (24/48/72 timer, eller husvarianten) over en kø.
- En tekniker vil ha neste purring skrevet for én bestemt stille sak.
- Innebygd i en Flow på tidsplan eller på en ventestatustimer (se uovervåket variant).

## Fremgangsmåte

1. Bekreft kadensen som skal brukes. Standard: første oppfølging 24 timer etter siste utgående kunderettede melding, andre etter 48, tredje etter 72 — tre forsøk totalt, deretter overlevering til skillen No-Response Closure Sequence. Følg en desk-spesifikk kadens hvis brukeren oppgir en («3 dager / 3 forsøk / 3 e-poster» er en vanlig husregel).
2. Finn kandidater med `search_tickets`: åpne saker i venter-på-kunde-statuser der siste melding er utgående og eldre enn gjeldende kadenssteg.
3. Tell **dokumenterte forsøk** for hver kandidat: kundesynlige oppfølgingsmeldinger siden kunden sist svarte. Bare meldinger kunden faktisk kunne se, teller — interne notater er ikke forsøk.
4. Hopp over legitim venting: en fremtidig planlagt avtale (`schedule_ticket`-dato), en frist kunden selv har oppgitt («tilbake fra ferie mandag»), en leverandør-/tredjepartsavhengighet, eller en godkjenning som venter. Ikke purr på noen som har fortalt deg når de svarer.
5. Skriv oppfølgingen for hver gjenværende sak i den kunderettede stemmen: vennlig, kort, viser til den opprinnelige saken i én linje, sier hva som trengs fra dem, og tilbyr en enkel utvei («hvis dette er løst, si fra, så lukker vi saken»). Skjerp tonen forsiktig for hvert forsøk — tredje forsøk nevner at saken snart lukkes uten svar, og høres aldri irritert ut.
6. Presenter utkastene for gjennomgang, og publiser deretter hver godkjente oppfølging som et kundesynlig notat med `add_ticket_note` og sett riktig ventestatus med `update_ticket`.
7. Lever en oppsummeringstabell: sak, kunde, forsøksnummer som nettopp ble sendt (eller hoppet over og hvorfor), og datoen neste kadenssteg forfaller.

## Kjøreregler

- Overskrid aldri forsøkstallet: viser tråden allerede tre dokumenterte forsøk, send saken til avslutningssekvensen i stedet for å purre en fjerde gang.
- Tell bare det som er dokumentert — anta aldri at et forsøk skjedde per telefon med mindre et notat bekrefter det.
- Kunderettede utkast inneholder ingen intern sjargong, ingen standardfraser fra sakssystemet og ingen oppdiktede detaljer om saken.
- Hopp-over-listen er hellig: en feilpurret kunde på ferie eller i påvente av et planlagt besøk svekker tilliten. Er ventingens legitimitet uklar, hopp over og flagg i stedet for å sende.
- Send aldri oppfølginger i bulk uten at brukeren har gått gjennom utkastene.
- Enkelt og oversettbart språk; skriv på kundens språk der tråden ikke er på norsk.

## Lokale konvensjoner (norsk)

- Du-form og lav terskel i purringene: «Vi ville bare høre om du har fått sett på forrige melding» er naturlig norsk; unngå stive fraser som «Vi tillater oss å purre».
- Tredje forsøk formuleres positivt: «Hører vi ikke noe, lukker vi saken i løpet av de neste dagene — er noe fortsatt uløst, er det bare å svare på denne e-posten, så gjenåpnes saken.»
- Datoer i utkastene: «mandag 20. juli», ikke bare 20.07; i den interne oppsummeringstabellen er DD.MM greit.
- Regn frister i virkedager der deskens policy sier det; fellesferien (juli) og bevegelige helligdager forlenger legitim venting — ved tvil, hopp over og flagg.

## Uovervåket variant (Flows)

- Utløser: tidsplan (f.eks. timesvis feiing) eller alderstimer på ventestatus.
- Hele svaret ditt er den kunderettede oppfølgingsmeldingen, publisert ordrett — ingen fortellerstemme, ingen forsøkstellere, ingen interne kommentarer. Skriv ingenting annet.
- Deterministiske regler: send bare når (a) saken står i en venter-på-kunde-status, (b) siste melding er utgående, (c) kadenssteget er forfalt og (d) dokumenterte forsøk er under maksimum. Feiler noen betingelse, eller ventingen ser legitim ut, produser ingen utdata og stopp.
- Skriv aldri tredje forsøk eller senere uovervåket hvis desken krever menneskelig godkjenning før siste varsler — sjekk det konfigurerte forsøkstaket og stopp under det.
