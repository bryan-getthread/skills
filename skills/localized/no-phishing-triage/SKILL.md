---
name: Phishing-triage
description: En bruker har meldt en mistenkelig e-post, eller et phishing-meldingssak har landet på sikkerhetstavla — vurder den uten å røre nyttelasten, sjekk spredningsradiusen, inneslutt hvis skadelig, og svar melderen med en konklusjon. (Norwegian version of Phishing Triage — assess without touching the payload, check blast radius, contain, reply to the reporter.)
category: Localized
tools: [search_tickets, search_contacts, search_knowledge_base, add_ticket_note, update_ticket, view_openDraft]
---

# Phishing-triage

Norsk versjon av `skills/security/phishing-triage/SKILL.md` — kept in sync manually.

Fører en meldt mistenkelig e-post fra «er dette phishing?» til en dokumentert konklusjon: indikatorer fanget trygt, simuleringstrafikk filtrert bort, alle andre mottakere identifisert, innesluttning anbefalt der det trengs, og melderen besvart.

## Når skal den brukes

- «Er dette en phishing-e-post?» / en bruker har videresendt noe mistenkelig
- Et phishing-meldingssak (meldeknapp, postboks-tillegg eller manuell videresending) lander på sikkerhetstavla
- En tekniker vil ha en ny vurdering av en mistenkelig melding før den slippes ut eller slettes

## Fremgangsmåte

1. Fang indikatorene fra saken uten å samhandle med dem: avsenderadresse og visningsnavn, reply-to, emne, sendetidspunkt, hvert lenkemål nøyaktig slik det står skrevet, og navn og typer på vedlegg. Åpne aldri lenker eller vedlegg, og hent aldri en URL fra meldingen for å «se hva det er».
2. Simuleringsgren: sammenlign avsender- og lenkedomener mot kundens dokumenterte phishing-simuleringsplattform-domener (search_knowledge_base etter kundens simuleringsleverandør-oppføring). Matcher det en kjent simulator → klassifiser som simulering, lukk internt med et rentekst-notat som navngir det matchede domenet, og svar IKKE kunden — et svar skjevfordeler måltallene i simuleringsprogrammet deres. Stopp her.
3. Vurder indikatorene: etterligner- eller søskendomene, hastverks- og betalingsagn, legitimasjonshøstingslenke (avvik mellom vist tekst og faktisk mål), uventede vedleggstyper, kapring av en ekte tidligere samtaletråd. Er fulle headere limt inn, overlat dybdeanalysen til skillen email-header-analysis og innarbeid konklusjonen dens.
4. Spredningsradius-sjekk: search_tickets etter samme avsender eller emne på tvers av kundens boards og nylige saker. Fastslå hvem andre som mottok meldingen og — kritisk — om noen klikket, svarte, oppga legitimasjon eller åpnet et vedlegg.
5. Har noen samhandlet med en melding du vurderer som skadelig → eskaler umiddelbart og start skillen compromised-account-containment for den eller de berørte brukerne. Innesluttning går foran å fullføre rapporten: inneslutt raskt, undersøk etterpå.
6. Lever konklusjonen:
   - Skadelig → anbefal karantene/fjerning fra alle mottakerpostbokser, blokkering av avsender/domene på gatewayen, og en legitimasjonsnullstilling for alle som klikket.
   - Mistenkelig, men ubekreftet → si nøyaktig det, sammen med hva som ville bekreftet det.
   - Legitim → forklar signalene som frikjenner den, slik at melderen lærer.
7. Svar melderen med konklusjonen i vokabularet fra defensive-writing-standard (en mistenkelig e-post er en «meldt melding», ikke et «brudd»). Loggfør konklusjonen, bevisene og beslutningsresonnementet som et internt notat; klassifiser og sett status etter skillen soc-classification-tree. Takk melderen — å melde er atferden du vil ha gjentatt.

## Kjøreregler

- Åpne, klikk, hent eller gjengi aldri lenker eller vedlegg fra den meldte meldingen. Fang indikatorer kun som tekst.
- Sjekk simuleringsstatus begge veier: utløs aldri en ekte hendelse for en simulering, og lukk aldri en ekte phishing som simulering på en delvis domenematch — krev et nøyaktig, dokumentert simulatordomene.
- Svar aldri kunden på en bekreftet simulering; lukk kun internt.
- Klikket eller svart betyr eskaler umiddelbart — en falsk eskalering er billig, en oversett kompromittering er det ikke.
- Slå aldri fast «ingen andre mottok denne» ut fra et avkuttet søk; skriv «ingen andre meldinger funnet i de siste N gjennomsøkte sakene».
- Dokumenter beslutningen, ikke bare handlingen: notatet må si hvorfor konklusjonen ble nådd, ikke bare hva som ble gjort.
- Defensiv skriving hele veien: ingen «brudd», ingen «hacket», ingen påstander om konsekvenser før fakta er bekreftet.

## Lokale konvensjoner (norsk)

- Defensivt norsk vokabular: «meldt melding», «mistenkt phishing-forsøk», «uvanlig aktivitet» — aldri «hacking» eller «datalekkasje» før formell bekreftelse.
- Svaret til melderen holder du-formen og avsluttes med en eksplisitt takk: «Takk for at du meldte denne meldingen — det var akkurat riktig gjort.»
- Tidsstempler i tidslinjer i DD.MM.ÅÅÅÅ og 24-timersformat, med tidssone («15.07.2026 kl. 09:12 CET»).
- Tekniske indikatorer (headere, URL-er, filnavn, feilstrenger) gjengis verbatim på engelsk — oversett dem aldri, de tjener som bevis og søkenøkler.

## Konsoliderer

Skills for phishing-meldingstriage, identifisering av simuleringer, gjennomgang av meldt e-post, spredningsradius-feiing og svar til melder.
