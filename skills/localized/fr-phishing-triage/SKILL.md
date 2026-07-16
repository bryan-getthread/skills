---
name: Triage d'hameçonnage
description: Un utilisateur a signalé un e-mail suspect ou un ticket de signalement d'hameçonnage est arrivé sur le tableau sécurité — l'évaluer sans toucher à la charge, mesurer le rayon d'exposition, contenir si malveillant, et répondre au déclarant avec un verdict. (Phishing triage — assess without touching the payload, check blast radius, contain, reply to the reporter — in French.)
category: Localized
tools: [search_tickets, search_contacts, search_knowledge_base, add_ticket_note, update_ticket, view_openDraft]
---

# Triage d'hameçonnage

Version française de `skills/security/phishing-triage/SKILL.md` — maintenue à jour manuellement.

Mener un e-mail suspect signalé de « est-ce de l'hameçonnage ? » jusqu'à un verdict documenté : indicateurs capturés sans risque, trafic de simulation écarté, tous les autres destinataires identifiés, confinement conseillé si nécessaire, et réponse apportée au déclarant.

## Quand l'utiliser

- « Est-ce un e-mail d'hameçonnage ? » / un utilisateur a transféré quelque chose de louche
- Un ticket de signalement d'hameçonnage (bouton de signalement, module de messagerie ou transfert manuel) arrive sur le tableau sécurité
- Un technicien veut un second avis sur un message suspect avant de le libérer ou de le supprimer

## Étapes

1. Capturer les indicateurs depuis le ticket sans interagir avec eux : adresse et nom d'affichage de l'expéditeur, reply-to, sujet, heure d'envoi, chaque cible de lien exactement telle qu'écrite, et noms et types des pièces jointes. Ne jamais ouvrir de lien ni de pièce jointe, et ne jamais récupérer une URL du message pour « voir ce que c'est ».
2. Branche simulation : comparer l'expéditeur et les domaines des liens aux domaines documentés de la plateforme de simulation d'hameçonnage du client (search_knowledge_base pour la fiche du fournisseur de simulation du client). Correspondance avec un simulateur connu → classer en simulation, clore en interne avec une note en texte brut nommant le domaine apparié, et ne PAS répondre au client — une réponse fausse les métriques de son programme de simulation. S'arrêter là.
3. Évaluer les indicateurs : domaine sosie ou cousin, leviers d'urgence et de paiement, lien de collecte d'identifiants (décalage entre le texte affiché et la cible réelle), types de pièces jointes inattendus, détournement d'un fil de conversation réel antérieur. Si les en-têtes complets ont été collés, confier l'analyse fine au skill email-header-analysis et intégrer son verdict.
4. Vérification du rayon d'exposition : search_tickets sur le même expéditeur ou le même sujet à travers les tableaux du client et les tickets récents. Déterminer qui d'autre a reçu le message et — point critique — si quelqu'un a cliqué, répondu, saisi des identifiants ou ouvert une pièce jointe.
5. Si quelqu'un a interagi avec un message que vous jugez malveillant → escalader immédiatement et lancer le skill compromised-account-containment pour le ou les utilisateurs touchés. Le confinement prime sur la fin de la rédaction : contenir vite, investiguer ensuite.
6. Rendre le verdict :
   - Malveillant → conseiller la mise en quarantaine/le retrait de toutes les boîtes destinataires, le blocage de l'expéditeur/du domaine à la passerelle, et une réinitialisation d'identifiants pour quiconque a cliqué.
   - Suspect mais non confirmé → le dire exactement ainsi, avec ce qui permettrait de confirmer.
   - Légitime → expliquer les signaux qui l'innocentent, pour que le déclarant apprenne.
7. Répondre au déclarant avec le verdict, dans le vocabulaire du standard d'écriture défensive (un e-mail suspect est un « message signalé », pas une « brèche »). Consigner le verdict, les preuves et le raisonnement de décision en note interne ; classifier et positionner le statut selon le skill soc-classification-tree. Remercier le déclarant — le signalement est le comportement à encourager.

## Garde-fous

- Ne jamais ouvrir, cliquer, récupérer ni afficher des liens ou pièces jointes du message signalé. Capturer les indicateurs sous forme de texte uniquement.
- Vérifier le statut de simulation dans les deux sens : ne jamais déclencher un incident réel pour une simulation, et ne jamais clore un vrai hameçonnage comme simulation sur une correspondance de domaine partielle — exiger un domaine de simulateur documenté et exact.
- Ne jamais répondre au client sur une simulation confirmée ; clore en interne uniquement.
- Un clic ou une réponse signifie escalade immédiate — une fausse escalade ne coûte rien, une compromission manquée si.
- Ne jamais affirmer « personne d'autre ne l'a reçu » à partir d'une recherche plafonnée ; écrire « aucun autre signalement trouvé dans les N derniers tickets parcourus ».
- Documenter la décision, pas seulement l'action : la note doit dire pourquoi le verdict a été rendu, pas seulement ce qui a été fait.
- Écriture défensive de bout en bout : pas de « brèche », pas de « piraté », aucune affirmation d'impact avant confirmation des faits.

## Conventions locales (français)

- Vocabulaire défensif français : « message signalé », « tentative d'hameçonnage présumée », « activité inhabituelle » — jamais « piratage » ni « fuite de données » avant confirmation formelle.
- La réponse au déclarant reste au vouvoiement et se conclut par un remerciement explicite : « Merci d'avoir signalé ce message — c'est exactement le bon réflexe. »
- Horodatages des chronologies en JJ/MM/AAAA et format 24 h, fuseau précisé (« 15/07/2026 09 h 12 CET »).
- Les indicateurs techniques (en-têtes, URL, noms de fichiers, chaînes d'erreur) sont recopiés verbatim en anglais — ne jamais les traduire, ils servent de preuves et de clés de recherche.

## Consolide

Les skills de triage de signalement d'hameçonnage, d'identification des simulations, de revue d'e-mail signalé, de balayage du rayon d'exposition et de réponse au déclarant.
