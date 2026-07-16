---
name: Oppsummering og avslutningsnotat
description: Lag en ryddig oppsummering av hva som skjedde i en sak — løsningsnotat, avslutningsnotat eller et malbasert P1/P2-overleveringssammendrag — i ønsket format og synsvinkel. (Norwegian version of Ticket Summary & Closure Note — resolution, closure, and handoff summaries.)
category: Localized
tools: [search_tickets, add_ticket_note]
---

# Oppsummering og avslutningsnotat

Norsk versjon av `skills/documentation/ticket-summary-and-closure-note` — kept in sync manually.

Oppsummerer en sakstråd til nøyaktig det notatet situasjonen krever: et løsnings-/avslutningsnotat, en «hva ble gjort»-gjenfortelling, eller et malbasert overleveringssammendrag ved eskalering.

## Når skal den brukes

- «Skriv et avslutningsnotat for denne saken» / «oppsummer hva som ble gjort.»
- «Gi meg et løsningsnotat i standardformatet vårt.»
- En P1/P2 overleveres til en annen tekniker eller et vaktlag utenfor arbeidstid og trenger et strukturert overleveringssammendrag.
- «Oppsummer dette i førsteperson til timeføringen min / PSA-notatet.»

## Fremgangsmåte

1. Les hele tråden med `search_tickets`: svar, interne notater og timeføringer. Fastslå problem → utførte handlinger → utfall → eventuelle utestående punkter.
2. Velg formatet forespørselen ber om:
   - **Løsnings-/avslutningsnotat** — Problem, Utførte handlinger, Utfall, og (kun hvis arbeid gjenstår) ett tydelig Neste steg med ansvarlig og frist.
   - **P1/P2-overleveringssammendrag** — fast mal: Nåværende status; Påvirkning (hvem/hva er nede); Tidslinje over nøkkelhendelser med tidsstempler; Handlinger utført så langt; Hva som er utelukket; Neste handling + ansvarlig; Status for kundekommunikasjon (hvem fikk vite hva, når).
   - **Enkel gjenfortelling** — kort prosa til en kundesynlig avslutningsmelding.
3. Bruk den ønskede synsvinkelen: tredjeperson som standard, eller **teknikerens førsteperson** («Jeg tilbakestilte profilen og verifiserte påloggingen») når det bes om det — skriv som den tildelte teknikeren, ikke som en KI.
4. Bruk formatreglene fra deskens **note-format-standard**-skill hvis den er lastet. Skal notatet synkroniseres til en PSA, bruk **kun ren tekst**: ingen markdown, ingen emojier, ingen typografiske anførselstegn, ingen tankestreker.
5. Vis notatet i chatten for gjennomgang. Publiser det med `add_ticket_note` kun når det bes om eksplisitt, og publiser overleverings-/QA-sammendrag som **interne** notater med mindre annet er sagt.

## Kjøreregler

- Finn ikke på detaljer tråden ikke understøtter — ingen fabrikkerte bekreftelser, tidsstempler eller rotårsaker. Er utfallet uklart, skriv «utfall ikke bekreftet i tråden» i stedet for å påstå løsning.
- Følg det ønskede formatet presist, inkludert «ingen punktlister» eller ordgrenser når det er spesifisert.
- Førsteperson endrer kun stemmen — legg aldri til arbeid teknikeren ikke har dokumentert.
- I overleveringssammendrag: skill eksplisitt mellom fakta og hypotese («Bekreftet:» kontra «Mistenkt:»).
- Ta aldri med påloggingsdetaljer eller engangskoder fra tråden i noen oppsummering.

## Lokale konvensjoner (norsk)

- Tidsstempler i tidslinjen: DD.MM.ÅÅÅÅ kl. HH:MM i 24-timersformat («14.07.2026 kl. 09:45»). Krysser saken tidssoner, oppgi sonen («09:45 CEST»).
- Kundesynlige notater i du-form og klarspråk; interne overleveringer kan være mer tekniske og direkte.
- Ren tekst til PSA: behold æ/ø/å (det er rettskriving, ikke pynt), men dropp typografiske anførselstegn og tankestreker som eldre PSA-er kan ødelegge.
- Bruk konsekvente norske maletiketter i hele desken («Bekreftet:» / «Mistenkt:»; «Neste handling:»); ikke bland inn de engelske etikettene i samme notat.

## Konsoliderer

Forespørsler om avslutningsnotater, løsningssammendrag, P1/P2-overleveringsmaler og «neste steg»-situasjonssammendrag.
