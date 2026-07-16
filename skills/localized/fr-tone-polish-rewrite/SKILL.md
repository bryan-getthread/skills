---
name: Reformulation soignée
description: Réécrire un texte brut de technicien en version soignée, prête pour le client — « écris ça proprement », « améliore ce message », « nettoie ça » — en préservant chaque fait à l'identique. (Rewrite rough technician text into polished, client-ready French, preserving every fact exactly.)
category: Localized
tools: []
---

# Reformulation soignée

Version française de `skills/communication/tone-polish-rewrite/SKILL.md` — maintenue à jour manuellement.

Prend le brouillon brut d'un technicien — laconique, abrupt ou tapé à la va-vite — et rend une version soignée au contenu factuel strictement identique. La demande de rédaction la plus fréquente sur un service desk.

## Quand l'utiliser

- « Écris ça proprement : <texte brut> »
- « Améliore ce message » / « rends ça professionnel »
- « Nettoie ça avant que je l'envoie » — avec un brouillon collé.
- Un message paraît sec, cassant ou confus et le technicien en veut une version plus cordiale.

## Étapes

1. Lire le texte brut et identifier chaque affirmation factuelle : ce qui s'est passé, ce qui a été fait, les horaires, les noms, les engagements. Tout cela est intouchable.
2. Réécrire pour le ton et la clarté selon le skill Email Baseline Standard (communication/email-baseline-standard) : la réponse d'abord, langage courant, zéro remplissage, 1 à 2 paragraphes courts sauf si la source en exige davantage.
3. Respecter à la lettre toute contrainte de style énoncée par le membre — « pas de tirets longs » signifie zéro tiret long ; « plus court » signifie plus court ; « orthographe rectifiée de 1990 » signifie orthographe rectifiée. Une contrainte énoncée une fois dans la session s'applique à toutes les réécritures de la session.
4. Conserver le sens et les engagements du membre à l'identique : ne pas adoucir un « non » en « peut-être », ne pas transformer un « devrait » en « va », ne pas ajouter de promesses, d'excuses ni d'étapes absentes de la source.
5. Rendre **uniquement le texte réécrit** — pas de préambule, pas de « Voici une version soignée : », pas de commentaire, pas de variantes sauf demande. La réponse est l'artefact ; le membre la copiera-collera.

## Garde-fous

- Ne jamais ajouter, supprimer ni altérer un fait. Le polissage porte sur le ton uniquement ; si la source est ambiguë, conserver l'ambiguïté plutôt que de trancher par une interprétation.
- Ne jamais inventer un prénom de salutation, un numéro de ticket ni un détail absent du texte source.
- Si la source contient quelque chose qui ne doit pas partir chez un client (identifiants, tarifs internes, reproches), l'écarter de la réécriture et ajouter un unique indicateur entre crochets à la fin : [Retiré : <quoi> — interne uniquement].
- Préserver la langue de la source sauf demande de traduction (pour le nettoyage de textes de non-francophones natifs, voir communication/esl-drafting-assistant).

## Conventions locales (français)

- Registre client : vouvoiement par défaut ; ne reprendre le tutoiement que si le texte source montre qu'il est déjà installé avec ce client.
- La reformulation en français passe souvent de tournures parlées (« du coup », « ça a sauté ») à un registre professionnel neutre — sans basculer dans l'administratif guindé.
- Typographie française : espaces insécables avant « : ; ? ! », guillemets « à chevrons » si le desk les utilise, dates en JJ/MM/AAAA, heures en format 24 h.
- Ne pas franciser les chaînes techniques anglaises (messages d'erreur, noms de services, commandes) : elles restent verbatim.

## Variante non assistée (Flows)

- Votre réponse entière est le texte réécrit, tel quel — pas de narration, pas d'étiquettes.
- Si l'entrée est vide ou ne contient aucune prose réécrivable, ne rien produire.
