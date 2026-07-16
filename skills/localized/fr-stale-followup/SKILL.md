---
name: Cadence de relance des tickets en attente
description: Piloter une cadence de relance configurable (par exemple 24/48/72 heures, trois tentatives) sur les tickets en attente d'une réponse client — rédiger chaque relance cordiale, compter les tentatives documentées et épargner les tickets en attente légitime. (Follow-up cadence on waiting-on-client tickets, with French client-facing nudges.)
category: Localized
tools: [search_tickets, add_ticket_note, update_ticket, schedule_ticket]
---

# Cadence de relance des tickets en attente

Version française de `skills/qa-and-closure/stale-ticket-followup-cadence/SKILL.md` — maintenue à jour manuellement.

Fait avancer les tickets en attente client sur un rythme prévisible. Compte les tentatives déjà documentées dans le fil, rédige la relance suivante d'une voix cordiale tournée vers le client, et sait quand un ticket attend pour une bonne raison et doit être laissé tranquille.

## Quand l'utiliser

- « Relance mes tickets en attente » / « qui ne nous a pas répondu ? »
- Exécution de la cadence standard du desk (24/48/72 heures, ou la variante maison) sur une file.
- Un technicien veut la prochaine relance rédigée pour un ticket silencieux précis.
- Intégré dans un Flow sur planification ou sur minuterie de statut d'attente (voir la variante non assistée).

## Étapes

1. Confirmer la cadence à appliquer. Par défaut : première relance 24 heures après le dernier message sortant visible du client, deuxième à 48, troisième à 72 — trois tentatives au total, puis passage au skill de séquence de clôture sans réponse (No-Response Closure Sequence). Respecter une cadence propre au desk si l'utilisateur en énonce une (« 3 jours / 3 tentatives / 3 e-mails » est une règle maison courante).
2. Trouver les candidats avec `search_tickets` : tickets ouverts en statut d'attente client dont le dernier message est sortant et plus ancien que l'échéance de cadence en cours.
3. Pour chaque candidat, compter les **tentatives documentées** : messages de relance visibles du client depuis sa dernière réponse. Seuls comptent les messages que le client a réellement pu voir — les notes internes ne sont pas des tentatives.
4. Épargner les attentes légitimes : un rendez-vous planifié à venir (date `schedule_ticket`), un délai annoncé par le client (« de retour de congés lundi »), une dépendance fournisseur/tiers, ou une approbation en cours. Ne pas relancer quelqu'un qui vous a dit quand il répondrait.
5. Rédiger la relance de chaque ticket restant dans la voix client : cordiale, courte, rappelant le sujet initial en une ligne, précisant ce qui est attendu de lui, et offrant une porte de sortie facile (« si c'est résolu de votre côté, dites-le-nous et nous clôturerons le ticket »). Faire monter le ton en douceur au fil des tentatives — la troisième indique que le ticket sera bientôt clôturé sans réponse, sans jamais paraître agacée.
6. Présenter les brouillons pour relecture, puis publier chaque relance approuvée en note visible du client avec `add_ticket_note` et positionner le statut d'attente approprié avec `update_ticket`.
7. Produire un tableau récapitulatif : ticket, client, numéro de la tentative envoyée (ou « épargné » et pourquoi), et la date d'échéance de la prochaine étape de cadence.

## Garde-fous

- Ne jamais dépasser le nombre de tentatives : si le fil montre déjà trois tentatives documentées, orienter vers la séquence de clôture au lieu d'une quatrième relance.
- Ne compter que ce qui est documenté — ne jamais supposer qu'une tentative téléphonique a eu lieu si aucune note ne l'enregistre.
- Les brouillons client ne contiennent ni jargon interne, ni boilerplate d'outil de ticketing, ni détails inventés sur le problème.
- La liste des épargnés est sacrée : un client relancé à tort pendant ses congés ou avant une intervention planifiée, c'est de la confiance perdue. Si la légitimité de l'attente est incertaine, épargner et signaler plutôt qu'envoyer.
- Ne jamais envoyer de relances en masse sans relecture des brouillons par l'utilisateur.
- Garder une langue simple et localisable ; s'aligner sur la langue du client quand le fil n'est pas en français.

## Conventions locales (français)

- Vouvoiement systématique dans les relances ; ton cordial mais direct (« Nous restons dans l'attente de votre retour » plutôt que des circonvolutions).
- Périodes sensibles en France : congés d'été (fin juillet–août) et fêtes de fin d'année — un silence client pendant ces fenêtres est plus souvent une attente légitime ; vérifier les messages d'absence avant de compter une tentative.
- Dates en JJ/MM/AAAA ; « sous 48 h » est la tournure usuelle pour les délais.
- Porte de sortie type : « Si le problème est résolu de votre côté, un simple mot et nous clôturerons le ticket. » Rappel de clôture (3e tentative) : « Sans retour de votre part d'ici le <date>, nous clôturerons ce ticket — vous pourrez le rouvrir à tout moment en répondant à cet e-mail. »

## Variante non assistée (Flows)

- Déclencheur : planification (p. ex. balayage horaire) ou minuterie d'ancienneté de statut d'attente.
- Votre réponse entière est le message de relance visible du client, publié tel quel — pas de narration, pas de compteur de tentatives, pas de commentaire interne. N'écrire rien d'autre.
- Règles déterministes : n'envoyer que si (a) le ticket est en statut d'attente client, (b) le dernier message est sortant, (c) l'étape de cadence est échue, et (d) les tentatives documentées sont sous le maximum. Si une condition échoue, ou si l'attente paraît légitime, ne rien produire et s'arrêter.
- Ne jamais rédiger la troisième tentative ou au-delà en mode non assisté si le desk exige une validation humaine avant les derniers avis — vérifier le plafond de tentatives configuré et s'arrêter en dessous.
