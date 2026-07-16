---
name: Synthèse quotidienne
description: Un technicien demande un récapitulatif de ses tickets ouverts — qui attend une réponse, ce qui est urgent, ce qui est planifié aujourd'hui — lisible en moins d'une minute, avec une variante ultra-courte en 3 lignes. (French daily digest of a technician's open tickets — replies due, urgent items, today's schedule.)
category: Localized
tools: [search_tickets, search_members]
---

# Synthèse quotidienne

Version française de `skills/personal-productivity/daily-digest/SKILL.md` — maintenue à jour manuellement.

La lecture du matin : tout ce que vous avez sur le feu, trié selon ce qui a besoin de vous maintenant. Conçue pour être parcourue en moins d'une minute et pour nommer la toute première chose à faire. C'est aussi le schéma que la plupart des partenaires exécutent en skill planifié du matin.

## Quand l'utiliser

- « Fais-moi un récap de mes tickets ouverts » / « qui attend une réponse, y a-t-il des urgences ? »
- « Synthèse du matin » / « à quoi ressemble ma journée ? »
- « Version courte » — la variante en 3 lignes
- Intégrée dans un Flow comme briefing planifié de début de journée

## Étapes

1. Récupérer tous les tickets ouverts affectés au membre demandeur avec search_tickets. Si un plafond de résultats a pu tronquer la liste, le dire d'emblée (« 50 affichés — il peut y en avoir davantage ») plutôt que de présenter la synthèse comme exhaustive.
2. Classer chaque ticket dans exactement une rubrique, dans cet ordre de priorité (un ticket tombe dans la première rubrique à laquelle il correspond) :
   - **En attente de votre réponse** — le client a répondu en dernier et attend votre retour. Trier par durée d'attente.
   - **Urgent / à risque** — priorité haute, SLA en dépassement ou proche du dépassement, ou fil à sentiment négatif. Trier par gravité.
   - **Planifié aujourd'hui** — tickets avec une entrée d'agenda ou un suivi engagé pour aujourd'hui, dans l'ordre horaire.
   - **En attente de tiers** — le client, un fournisseur ou une escalade a la main. Signaler ceux silencieux depuis 3 jours ou plus (candidats à une relance), sans laisser cette rubrique envahir le haut de page.
   - **Tout le reste** — un total et une caractérisation en une ligne, sans détail ticket par ticket.
3. Chaque ligne de ticket tient sur une seule ligne : numéro, client (abrégé), état en 5 à 8 mots, et la prochaine action (« #1234 <client> — imprimante HS depuis 2 j, le client a répondu hier → confirmer que le correctif fonctionne »).
4. Ouvrir la synthèse par **Commencez ici :** le ticket le plus important et une phrase expliquant pourquoi (la réponse client qui attend depuis le plus longtemps prime sur une urgence vague ; un dépassement ferme de SLA prime sur tout).
5. Conclure par la physionomie de la journée : totaux par rubrique et une phrase (« 2 réponses et 1 urgence avant votre intervention planifiée de 10 h 00 dégagent l'essentiel de la pression »).
6. Si la version courte est demandée (ou « 3 lignes »), produire exactement trois lignes : (1) Commencez ici + pourquoi, (2) les compteurs — X réponses attendues / Y urgents / Z planifiés, (3) la seule chose qui fera mal si elle est ignorée aujourd'hui. Ni en-têtes, ni préambule.
7. Proposer la suite naturelle : « Voulez-vous des brouillons de réponse pour les tickets qui vous attendent ? »

## Garde-fous

- Se limiter strictement aux tickets du membre demandeur ; ne jamais inclure les files des autres techniciens sauf demande explicite (c'est un rapport de responsable, pas une synthèse personnelle).
- Lecture seule : la synthèse ne modifie rien — pas de changement de statut, pas de note, pas de rappel — sauf si le membre le demande ensuite.
- Ne jamais inventer de numéros de ticket, d'échéances SLA ni de réponses client ; si la dernière activité est ambiguë, classer le ticket dans la rubrique la plus prudente (En attente de votre réponse) plutôt que de l'enfouir dans En attente de tiers.
- Chaque ligne de rubrique doit être actionnable — un état sans prochaine action est une ligne gaspillée.
- Garder la synthèse complète parcourable : si elle ne tient pas sur un écran, resserrer les lignes ; ne jamais délayer.

## Conventions locales (français)

- Dates au format JJ/MM/AAAA (« 15/07/2026 ») ; heures en format 24 h avec l'usage français (« 10 h 00 », « 14 h 30 »).
- La synthèse est un document interne : le tutoiement entre collègues est acceptable si c'est l'usage du desk, mais tout extrait destiné au client reste au vouvoiement.
- Abréviations usuelles du desk francophone : « HS » (hors service), « RAS » (rien à signaler), « càd » — utilisables dans les lignes internes, jamais dans un texte client.

## Variante non assistée (Flows)

- Votre réponse entière est livrée telle quelle comme briefing du matin — pas de narration, pas de questions, pas de « n'hésitez pas à ».
- Toujours inclure la ligne Commencez ici et les compteurs par rubrique ; omettre la proposition de suite.
- Si la file est vide, produire exactement : « Aucun ticket ouvert ne vous est affecté. Profitez de l'ardoise vierge. »
- Si la recherche de tickets échoue ou revient invraisemblablement vide pour un technicien actif, produire une seule ligne indiquant que la synthèse n'a pas pu être générée — jamais une synthèse fabriquée.
