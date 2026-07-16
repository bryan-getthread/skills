---
name: Triage de ticket
description: Classifier un ticket nouveau ou non affecté, évaluer la gravité, détecter les doublons et proposer son orientation avant qu'il n'entre en file. (Classify a new/unassigned ticket, gauge severity, catch duplicates, recommend routing — in French.)
category: Localized
tools: [search_tickets, list_boards, list_ticket_statuses, list_ticket_priorities, search_clients, update_ticket]
---

# Triage de ticket

Version française de `skills/triage-and-routing/ticket-triage/SKILL.md` — maintenue à jour manuellement.

Première lecture d'un ticket nouveau ou non affecté : le classifier, imposer la bonne gravité, repérer les doublons et proposer tableau, statut et priorité — puis n'appliquer qu'après confirmation.

## Quand l'utiliser

- Un ticket vient d'arriver et a besoin d'une première passe avant dispatch.
- « Trie ce ticket » / « classifie ça » / « où doit-il aller ? »
- Balayage matinal des tickets non affectés sur le tableau d'entrée.
- Un dispatcheur veut une proposition de tableau, statut et priorité avec un résumé de file en une ligne.

## Étapes

1. Lire le fil complet avec `search_tickets`, y compris le tout premier message et chaque réponse. Ne jamais classifier ni router à partir du seul titre — les titres sont souvent périmés, auto-générés ou faux.
2. Classifier le problème dans les catégories du desk (p. ex. accès, matériel, logiciel, réseau, messagerie, sécurité). Fonder le jugement sur le corps de la demande, pas sur des mots-clés du sujet.
3. Appliquer les règles de gravité forcée avant tout jugement : tout ce qui touche à la sécurité (compromission, hameçonnage, malware, activité MFA/connexion inattendue) ou qui affecte plusieurs utilisateurs ou un site entier est en priorité haute par défaut. Seuls les propres mots du demandeur qui dédramatisent, confirmés avec le technicien, peuvent l'abaisser.
4. Chercher les doublons avec des identifiants forts, pas des similarités de formulation : même ID d'alerte, même nom d'appareil/actif, même référence de supervision, ou même contact + même erreur précise dans une fenêtre récente. Lancer un `search_tickets` distinct par identifiant ; si une recherche a pu atteindre un plafond de résultats, le signaler.
5. Vérifier le client avec `search_clients` pour confirmer que la société du ticket correspond au demandeur.
6. Proposer : tableau, statut, priorité, catégorie et un résumé de file en une ligne. Lister les doublons suspectés avec leurs numéros de ticket et la raison de la correspondance.
7. N'appliquer avec `update_ticket` qu'après confirmation du relecteur. S'il modifie quoi que ce soit, appliquer sa version, pas la vôtre.

## Garde-fous

- Ne jamais router ni classifier sur le seul titre ; toujours lire le fil d'abord.
- Ne jamais modifier le ticket avant confirmation de la classification proposée. Ce skill recommande ; une personne (ou un skill non assisté configuré séparément) applique.
- Sécurité ou impact multi-utilisateurs impose la priorité haute — ne pas se laisser rassurer par le ton calme du message.
- Un verdict de doublon exige une correspondance d'identifiant fort. Une formulation semblable est une piste à mentionner, jamais un motif de fusion ni de clôture.
- Si les noms de tableaux/statuts/priorités sont ambigus pour ce tenant, les récupérer avec `list_boards` / `list_ticket_statuses` / `list_ticket_priorities` plutôt que de deviner les libellés.
- Ne pas inventer de numéros de ticket, de clients ni de catégories. Si le ticket ne peut pas être classifié avec confiance, le dire et le laisser à un humain.

## Conventions locales (français)

- Résumés de file et notes internes en français, dates en JJ/MM/AAAA et heures en format 24 h ; les priorités et statuts gardent leurs libellés exacts tels que configurés dans le tenant (même s'ils sont en anglais) — ne pas les traduire à la volée.
- Repérer les marqueurs de transfert français dans les sujets : « TR : » et « Fwd : » jouent le même rôle que « FW: » — le message d'origine cité prime sur l'expéditeur du transfert.
- Le vocabulaire de gravité côté client reste mesuré : « incident majeur » plutôt que « catastrophe » ; les qualifications alarmistes n'apparaissent jamais dans un résumé visible du client.
- Ne pas traduire les chaînes d'erreur techniques anglaises citées dans le ticket : elles servent d'identifiants de recherche.

## Consolide

Les skills de triage matinal, d'assistance au dispatch, de triage P1, d'admission avec détection de doublons et de « classifie ce ticket » génériques.
