---
name: Synthèse de ticket et note de clôture
description: Produire une synthèse propre de ce qui s'est passé sur un ticket — note de résolution, note de clôture ou synthèse de passation P1/P2 sur gabarit — dans le format et le point de vue demandés. (Ticket summary, closure note, or templated P1/P2 handoff summary — in French.)
category: Localized
tools: [search_tickets, add_ticket_note]
---

# Synthèse de ticket et note de clôture

Version française de `skills/documentation/ticket-summary-and-closure-note/SKILL.md` — maintenue à jour manuellement.

Synthétiser le fil d'un ticket dans la note exacte qu'exige la situation : note de résolution/clôture, récapitulatif « ce qui a été fait », ou synthèse de passation d'escalade sur gabarit.

## Quand l'utiliser

- « Rédige une note de clôture pour ce ticket » / « résume ce qui a été fait. »
- « Donne-moi une note de résolution dans notre format standard. »
- Un P1/P2 passe à un autre technicien ou à l'équipe d'astreinte et exige une synthèse de passation structurée.
- « Résume ça à la première personne pour ma saisie de temps / ma note PSA. »

## Étapes

1. Lire le fil complet avec `search_tickets` : réponses, notes internes et saisies de temps. Établir problème → actions menées → résultat → reste à faire.
2. Choisir le format qu'appelle la demande :
   - **Note de résolution / clôture** — Problème, Actions menées, Résultat, et (seulement s'il reste du travail) une Prochaine étape claire avec responsable et échéance.
   - **Synthèse de passation P1/P2** — gabarit fixe : État actuel ; Impact (qui/quoi est indisponible) ; Chronologie des événements clés horodatés ; Actions menées à ce stade ; Ce qui a été écarté ; Prochaine action + responsable ; État de la communication client (qui a été informé de quoi, et quand).
   - **Récapitulatif simple** — courte prose pour un message de clôture visible du client.
3. Appliquer le point de vue demandé : troisième personne par défaut, ou **première personne du technicien** (« J'ai réinitialisé le profil et vérifié la connexion ») sur demande — écrire comme le technicien affecté, pas comme une IA.
4. Appliquer les règles de mise en forme du skill **note-format-standard** du desk s'il est chargé. Si la note se synchronise vers un PSA, utiliser du **texte brut uniquement** : pas de markdown, pas d'émojis, pas de guillemets typographiques, pas de tirets longs.
5. Livrer la note dans la conversation pour relecture. Ne la publier avec `add_ticket_note` que sur demande explicite, et publier les synthèses de passation/QA en notes **internes** sauf instruction contraire.

## Garde-fous

- Ne pas inventer de détail que le fil ne soutient pas — pas de confirmations, d'horodatages ni de causes racines fabriqués. Si le résultat est incertain, écrire « résultat non confirmé dans le fil » plutôt que d'affirmer la résolution.
- Suivre le format demandé à la lettre, y compris « sans puces » ou des limites de mots quand elles sont précisées.
- La première personne ne change que la voix — ne jamais ajouter un travail que le technicien n'a pas consigné.
- Dans les synthèses de passation, séparer explicitement le fait de l'hypothèse (« Confirmé : » vs « Suspecté : »).
- Ne jamais reprendre dans une synthèse des identifiants ou codes à usage unique trouvés dans le fil.

## Conventions locales (français)

- Chronologies en JJ/MM/AAAA et heures 24 h (« 15/07/2026 14 h 30 ») ; toujours préciser le fuseau si le desk couvre plusieurs zones.
- Une note visible du client reste au vouvoiement et au registre professionnel sobre ; la clôture type se termine par : « Si quelque chose n'est pas résolu, il vous suffit de répondre à cet e-mail — votre réponse rouvrira ce ticket. »
- Les intitulés de gabarit peuvent rester bilingues si le PSA du desk est anglophone (« Current Status / État actuel ») — suivre l'usage établi du tenant plutôt que d'imposer une traduction.
- Les chaînes d'erreur techniques citées restent en anglais, verbatim.

## Consolide

Les demandes de note de clôture, les synthèses de résolution, les gabarits de passation P1/P2 et les skills de synthèse situationnelle « prochaine étape ».
