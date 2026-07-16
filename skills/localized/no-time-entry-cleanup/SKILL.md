---
name: Opprydding i timeføringer
description: Gjør grove, rå timenotater om til rene, standardiserte timeføringer — kunderettet og intern versjon — og registrer kun arbeid ingeniøren eksplisitt oppga å ha utført. (Norwegian version of Time Entry Cleanup — zero-assumption time entry normalization.)
category: Localized
tools: [search_tickets, log_time_entry, add_ticket_note]
---

# Opprydding i timeføringer

Norsk versjon av `skills/documentation/time-entry-cleanup` — kept in sync manually.

Normaliserer rotete timenotater til deskens standardformat for timeføring, skiller kunderettet ordlyd fra interne detaljer, uten noensinne å finne på eller blåse opp utført arbeid.

## Når skal den brukes

- En tekniker limer inn rå stikkord («rst pw, testet ok, sa fra til bruker») og vil ha en skikkelig timeføring.
- «Rydd opp i timenotatene mine for denne saken.»
- Slutten av dagen: flere rå føringer må standardiseres før fakturagjennomgang.
- En samtaleavslutning skal bli en fakturerbar føring med varighet.

## Fremgangsmåte

1. Les de rå notatene; hent kontekst fra sakstråden med `search_tickets` kun for å oppklare referanser (hvilken sak, hvilken <user>, hvilken <device>) — aldri for å legge til arbeidsposter.
2. Skriv om hver føring i tydelig fortid med den faste feltrekkefølgen:
   1. **Problem** — hva som ble meldt inn.
   2. **Utført arbeid** — det ingeniøren eksplisitt oppga å ha gjort.
   3. **Resultat** — utfallet slik det ble oppgitt.
   4. **Neste steg** — kun hvis ingeniøren oppga ett.
3. Bruk **null-antakelse-regelen**: ta bare med det ingeniøren eksplisitt sa var gjort. Gjør aldri en anbefaling, en intensjon eller et «burde» om til en fullført handling («anbefalte omstart» skal IKKE bli «startet om»). Er notatene tvetydige, spør eller merk `[uklart — bekreft]`.
4. Bruk erstatninger for forbudte ord etter husstilen — typisk sett: «fikset» → «løst»; «trøbbelet til fyren» → «problem meldt inn av <user>»; «rotet til» → «feilkonfigurert»; «burde være greit» → «verifisert i orden» **kun hvis verifisering ble oppgitt**, ellers «avventer bekreftelse fra bruker». Følg deskens egen forbudsliste når den finnes.
5. Lag to versjoner når det bes om det:
   - **Kunderettet** — nøkternt profesjonelt språk, ingen interne stikkord, ingen skyldfordeling, ingen leverandørsyting, ren tekst (PSA-trygt).
   - **Intern** — alle tekniske detaljer, hypoteser og oppfølgingsflagg.
6. Lever de ryddede føringene med varigheter. Skriv med `log_time_entry` / `add_ticket_note` først etter at teknikeren har bekreftet teksten og tidsbruken.

## Kjøreregler

- **Null antakelse er absolutt.** Ingen antatt arbeid, ingen antatt verifisering, ingen antatte varigheter. Ble ikke varigheten oppgitt, spør — ikke anslå i stillhet.
- Bevar teknikerens mening; rydd kun i ordlyden. Fakturerbare detaljer må forbli tro mot det notatene beskriver.
- Flytt aldri rent interne bemerkninger (skyldfordeling, kundefriksjon, sikkerhetsmistanker) inn i den kunderettede versjonen.
- Bekreft før du fører: føringene påvirker fakturering. Ved tvil, lever kun tekst og la teknikeren sende inn.

## Lokale konvensjoner (norsk)

- Varigheter i deskens format: «1 t 30 min» eller desimalt («1,5 t» — desimalkomma, ikke punktum) etter hva PSA-en forventer; spør om det er uklart.
- Den kunderettede versjonen i nøktern, aktiv du-form-kultur: «Tilbakestilte passordet og verifiserte pålogging» — kort og saklig er normen, ikke omstendelig høflighet.
- Ren tekst til PSA: behold æ/ø/å, men ingen markdown, emojier, typografiske anførselstegn eller tankestreker.
- Datoer på føringene i DD.MM.ÅÅÅÅ og klokkeslett i 24-timersformat.

## Konsoliderer

Skills for timeføringssammendrag, omformatering av rå timenotater, samtaleavslutning og notatstil med forbudte ord.
