---
name: Daglig oversikt
description: En tekniker ber om en oppsummering av sine åpne saker — hva som trenger svar, hva som haster, hva som er planlagt i dag — lesbar på under ett minutt, med en ultrakort 3-linjers variant. (Norwegian version of Daily Digest — skimmable morning summary of the requesting member's open tickets.)
category: Localized
tools: [search_tickets, search_members]
---

# Daglig oversikt

Norsk versjon av `skills/personal-productivity/daily-digest` — kept in sync manually.

Morgenlesningen: alt du har på bordet, sortert etter hva som trenger deg nå. Laget for å kunne skumleses på under ett minutt og for å peke ut den aller første oppgaven for dagen. Dette er også mønsteret flest partnere kjører som planlagt morgenskill.

## Når skal den brukes

- «Gi meg en oppsummering av mine åpne saker» / «hva trenger svar, haster noe?»
- «Morgenoversikt» / «hvordan ser dagen min ut?»
- «Kortversjonen» — 3-linjers-varianten
- Innebygd i en Flow som planlagt briefing ved arbeidsdagens start

## Fremgangsmåte

1. Hent alle åpne saker tildelt medlemmet som spør, med search_tickets. Hvis et resultattak kan ha kuttet listen, si det med en gang («viser 50 — det kan finnes flere») i stedet for å presentere oversikten som komplett.
2. Sorter hver sak i nøyaktig én bøtte, i denne prioritetsrekkefølgen (saken havner i den første bøtta den kvalifiserer for):
   - **Venter på svar fra deg** — kunden svarte sist og venter på deg. Sorter etter hvor lenge de har ventet.
   - **Haster / i faresonen** — høy prioritet, SLA på eller nær brudd, eller tråder med negativ tone. Sorter etter alvorlighet.
   - **Planlagt i dag** — saker med en kalenderoppføring eller en lovet oppfølging i dag, i tidsrekkefølge.
   - **Venter på andre** — kunden, en leverandør eller en eskalering har ballen. Flagg saker som har vært stille i 3+ dager (kandidater for en purring), men ikke la denne bøtta ta over toppen.
   - **Alt annet** — kun antall og en énlinjes karakteristikk; ingen linjer per sak.
3. Hver sak får én linje: nummer, kunde (kort), tilstand på 5–8 ord og neste handling («#1234 <kunde> — skriver frakoblet 2 d, kunden svarte i går → bekreft at løsningen fungerer»).
4. Innled oversikten med **Start her:** den viktigste enkeltsaken og én setning om hvorfor (kundesvaret som har ventet lengst slår vag hastefølelse; et hardt SLA-brudd slår alt).
5. Avslutt med dagens fasong: totaler per bøtte og én setning («2 svar og 1 hastesak før det planlagte besøket ditt kl. 10.00 letter det meste av trykket»).
6. Hvis noen ber om kortversjonen (eller «3 linjer»), lever nøyaktig tre linjer: (1) Start her + hvorfor, (2) antall — X trenger svar / Y haster / Z planlagt, (3) den ene tingen som biter deg i dag hvis den ignoreres. Ingen overskrifter, ingen innledning.
7. Tilby den naturlige oppfølgingen: «Vil du ha svarutkast for sakene som venter på deg?»

## Kjøreregler

- Avgrens strengt til sakene til medlemmet som spør; ta aldri med andre teknikeres køer med mindre det bes om eksplisitt (det er en leders rapport, ikke en personlig oversikt).
- Kun lesing: oversikten endrer ingenting — ingen statusendringer, ingen notater, ingen påminnelser — med mindre medlemmet ber om det som oppfølging.
- Finn aldri på saksnumre, SLA-frister eller kundesvar; hvis siste-aktivitet-data er tvetydig, legg saken i den tryggeste bøtta (Venter på svar fra deg) i stedet for å gjemme den i Venter på andre.
- Hver bøttelinje må være handlingsrettet — en tilstand uten neste handling er en bortkastet linje.
- Hold hele oversikten skumlesbar: hvis den ikke får plass på én skjerm, stram inn linjene; fyll aldri ut.

## Lokale konvensjoner (norsk)

- Datoer i formatet DD.MM.ÅÅÅÅ («15.07.2026»); i løpende tekst gjerne «tirsdag 15. juli».
- Klokkeslett i 24-timersformat med «kl.» («kl. 10.00» eller «kl. 10:00» — følg deskens husstil konsekvent).
- Du-form hele veien — det er standard i norsk arbeidsliv, både internt og mot kunder. «De»-formen er utdatert og skal ikke brukes.
- Bokmål, ikke nynorsk, med mindre desken eksplisitt ber om noe annet. Behold etablerte fagtermer (SLA, VIP) og produktbegreper på engelsk.

## Uovervåket variant (Flows)

- Hele svaret ditt leveres ordrett som morgenbriefingen — ingen fortellerstemme, ingen spørsmål, ingen «si fra hvis...».
- Ta alltid med Start her-linjen og antallene per bøtte; dropp oppfølgingstilbudet.
- Hvis køen er tom, lever nøyaktig: «Ingen åpne saker tildelt deg. Nyt den rene lista.»
- Hvis sakssøket feiler eller returnerer et usannsynlig tomt resultat for en aktiv tekniker, lever én enkelt linje om at oversikten ikke kunne lages — aldri en fabrikkert oversikt.
