---
name: Vaktoverlevering
description: Lag en skumlesbar énsides oversikt over åpent arbeid ved vaktslutt eller dagens slutt, gruppert etter status, til neste vakt eller til én navngitt mottakende tekniker. (Norwegian version of Shift Handoff — end-of-shift one-pager of open work.)
category: Localized
tools: [search_tickets, add_ticket_note, log_time_entry, search_members]
---

# Vaktoverlevering

Norsk versjon av `skills/escalation/shift-handoff` — kept in sync manually.

Bygger en overleverings-énsider som leses på ett minutt: hver åpen tråd gruppert etter status, hver med hva som ble gjort, hva som er neste, hva den venter på og risikoen. Avgrenses til én mottakende tekniker når en er navngitt.

## Når skal den brukes

- Vaktslutt eller dagens slutt: «overlever det åpne arbeidet mitt til nattevakta.»
- «Skriv en overlevering til <member> — de dekker køen min i morgen.»
- En leder vil ha teamets åpne arbeid oppsummert før vaktskiftet.

## Fremgangsmåte

1. Fastsett omfanget. Er én mottakende tekniker navngitt, overlever kun det den personen må handle på (den avtroppende teknikerens åpne/pågående saker, pluss alt som er eksplisitt flagget til vedkommende) — ikke hele teamets kø. Ellers dekk teamets åpne arbeid for påtroppende vakt.
2. Hent åpne og pågående saker med search_tickets. Treffer et søk resultattaket, si det i utdataene i stedet for å presentere listen som komplett; del opp søkene per board eller per status når du feier bredt.
3. Grupper énsideren etter status (f.eks. Pågår / Venter på kunde / Venter på leverandør / Planlagt / Ny-urørt), slik at leseren umiddelbart ser hva som kan handles på og hva som er parkert.
4. Bruk den faste firelinjersoppføringen for hver tråd: **Arbeid** (hva som ble gjort denne vakten), **Neste** (den ene neste handlingen og dens eier), **Venter** (hvem eller hva den er blokkert av, og siden når), **Risiko** (SLA-nærhet, stemning, VIP, eller «ingen»).
5. Topp siden med en varselseksjon: alt tidskritisk de neste timene, saker som nærmer seg SLA, og lovede tilbakeringinger.
6. Ved dagens slutt, tilby å føre utestående tid med log_time_entry og å publisere overleveringsnotater per sak (ren tekst) for alt uløst — publiser kun etter bekreftelse.

## Kjøreregler

- Optimaliser for lesehastighet: mottakeren skal vite hva som må plukkes opp på under ett minutt. Aggreger; ikke lim inn sakstråder.
- Utelat aldri blokkeringer eller SLA-risikosaker for å gjøre sammendraget kortere.
- Oppgi resultattak — presenter aldri et avkuttet søk som hele køen.
- Saksnotater er ren tekst; selve énsideren er chat-utdata med mindre annet bes om.
- For å overføre en pågående, levende samtale, bruk skillen Live Call Transfer Brief — denne skillen er for vakt-/dagsslutt-overleveringer.

## Lokale konvensjoner (norsk)

- Klokkeslett alltid i 24-timersformat («lovet tilbakeringing kl. 18:00»); tvetydige 12-timersklokkeslett i en nattoverlevering er en operasjonell risiko.
- Datoer i DD.MM; for overleveringer som krysser midnatt, skriv full dato og klokkeslett («16.07 kl. 02:00»).
- Direkte du-form kollegaer imellom gjennom hele énsideren — kort og saklig; kunderettet innhold følger kundesvar-konvensjonene.
- Norge følger CET/CEST; jobber desken på tvers av tidssoner (f.eks. med nordiske nabokontorer), oppgi sonen ved hvert kritiske klokkeslett.

## Konsoliderer

Skills for vaktoverleveringssammendrag, dagsslutt-oppsummering og briefing til neste vakt.
