---
name: Nettoyage des saisies de temps
description: Transformer des notes de temps brutes en saisies propres et normalisées — versions client et interne — en ne consignant que le travail que le technicien a explicitement déclaré avoir fait. (Turn rough time notes into clean, standardized time entries in French — client-facing and internal versions.)
category: Localized
tools: [search_tickets, log_time_entry, add_ticket_note]
---

# Nettoyage des saisies de temps

Version française de `skills/documentation/time-entry-cleanup/SKILL.md` — maintenue à jour manuellement.

Normaliser des notes de temps désordonnées dans le format de saisie standard du desk, en séparant la formulation visible du client du détail interne, sans jamais inventer ni surclasser le travail.

## Quand l'utiliser

- Un technicien colle du raccourci brut (« reinit mdp, testé ok, prévenu l'utilisateur ») et veut une saisie de temps propre.
- « Nettoie mes notes de temps sur ce ticket. »
- Fin de journée : plusieurs saisies brutes à normaliser avant la revue de facturation.
- Un compte rendu d'appel doit devenir une saisie facturable avec une durée.

## Étapes

1. Lire les notes brutes ; ne puiser dans le fil du ticket avec `search_tickets` que pour résoudre les références (quel ticket, quel <user>, quel <device>) — jamais pour ajouter des éléments de travail.
2. Réécrire chaque saisie au passé, en langage clair, dans l'ordre de champs fixe :
   1. **Problème** — ce qui a été signalé.
   2. **Travail effectué** — ce que le technicien a explicitement déclaré avoir fait.
   3. **Résultat** — l'issue telle qu'énoncée.
   4. **Prochaines étapes** — uniquement si le technicien en a énoncé une.
3. Appliquer la **règle du zéro postulat** : n'inclure que ce que le technicien a explicitement dit avoir fait. Ne jamais convertir une recommandation, une intention ou un « devrait » en action accomplie (« a recommandé un redémarrage » ne doit PAS devenir « a redémarré »). Si les notes sont ambiguës, demander ou marquer `[à clarifier — confirmer]`.
4. Appliquer les remplacements de mots bannis du style maison — jeu typique : « réparé » → « résolu » ; « problème avec le gars » → « problème signalé par <user> » ; « c'était en vrac » → « était mal configuré » ; « ça devrait être bon » → « vérifié fonctionnel » **uniquement si la vérification a été énoncée**, sinon « en attente de confirmation utilisateur ». Respecter la liste bannie propre au desk quand elle est fournie.
5. Produire deux versions sur demande :
   - **Visible du client** — langage professionnel simple, sans raccourci interne, sans mise en cause, sans récriminations contre les éditeurs, texte brut (compatible PSA).
   - **Interne** — détail technique complet, hypothèses et points de suivi.
6. Livrer les saisies nettoyées avec leurs durées. N'écrire avec `log_time_entry` / `add_ticket_note` qu'après confirmation du texte et du temps par le technicien.

## Garde-fous

- **Le zéro postulat est absolu.** Aucun travail inféré, aucune vérification inférée, aucune durée inférée. Si une durée n'a pas été énoncée, demander — ne pas estimer en silence.
- Préserver le sens du technicien ; nettoyer la formulation seulement. Le détail facturable doit rester fidèle à ce que décrivent les notes.
- Ne jamais mettre de remarques internes (mises en cause, frictions client, soupçons de sécurité) dans la version visible du client.
- Confirmer avant d'enregistrer : ces saisies touchent la facturation. Dans le doute, ne livrer que le texte et laisser le technicien soumettre.

## Conventions locales (français)

- Durées au format usuel du desk francophone : « 1 h 30 » ou « 45 min » ; dates en JJ/MM/AAAA.
- Le passé composé est le temps naturel de la saisie (« J'ai réinitialisé le mot de passe et vérifié la connexion ») ; éviter le passé simple, trop littéraire pour une note d'intervention.
- La version visible du client reste au vouvoiement et sans anglicismes de jargon (« redémarré » plutôt que « rebooté ») ; la version interne peut garder le vocabulaire technique courant du desk.
- Ne pas traduire les chaînes techniques anglaises (messages d'erreur, noms de services, commandes) : elles restent verbatim.

## Consolide

Les skills de résumé de saisie de temps, de reformatage de notes brutes, de compte rendu d'appel et de style de note à mots bannis.
