---
name: Relève de quart
description: Produire une page unique et parcourable de fin de quart ou de fin de journée récapitulant le travail ouvert, groupé par statut, pour l'équipe suivante ou pour un technicien receveur nommé. (End-of-shift/EOD one-pager of open work for the next shift — in French.)
category: Localized
tools: [search_tickets, add_ticket_note, log_time_entry, search_members]
---

# Relève de quart

Version française de `skills/escalation/shift-handoff/SKILL.md` — maintenue à jour manuellement.

Construit une page de relève lisible en une minute : chaque fil ouvert groupé par statut, avec ce qui a été fait, ce qui suit, ce qui bloque et le risque. Se restreint à un seul technicien receveur quand il est nommé.

## Quand l'utiliser

- Fin de quart ou fin de journée : « passe mon travail ouvert à l'équipe de nuit. »
- « Rédige une relève pour <member> — il couvre ma file demain. »
- Un responsable veut le travail ouvert de l'équipe résumé avant le changement de quart.

## Étapes

1. Établir le périmètre. Si un technicien receveur unique est nommé, ne transmettre que ce sur quoi cette personne doit agir (les tickets ouverts/en cours du technicien sortant, plus tout ce qui lui est explicitement signalé) — pas la file entière de l'équipe. Sinon, couvrir le travail ouvert de l'équipe pour le quart entrant.
2. Récupérer les tickets ouverts et en cours via search_tickets. Si une recherche atteint un plafond de résultats, le dire dans la sortie plutôt que de présenter la liste comme complète ; scinder les recherches par tableau ou par statut lors d'un balayage large.
3. Grouper la page par statut (p. ex. En cours / En attente client / En attente fournisseur / Planifié / Nouveau non touché), pour que le lecteur voie d'un coup d'œil ce qui est actionnable et ce qui est en pause.
4. Pour chaque fil, employer l'entrée fixe en quatre lignes : **Fait** (ce qui a été accompli pendant ce quart), **Suivant** (l'unique prochaine action et son responsable), **Attente** (qui ou quoi bloque, et depuis quand), **Risque** (proximité SLA, sentiment, VIP, ou « aucun »).
5. Coiffer la page d'une section d'alerte : tout ce qui est critique dans les prochaines heures, les tickets proches du SLA et les rappels promis.
6. Pour une fin de journée, proposer d'enregistrer le temps restant via log_time_entry et de publier des notes de relève par ticket (texte brut) pour tout ce qui n'est pas résolu — publier uniquement sur confirmation.

## Garde-fous

- Optimiser la vitesse de lecture : le receveur doit savoir quoi reprendre en moins d'une minute. Agréger ; ne pas coller les fils de tickets.
- Ne jamais omettre des blocages ou des risques SLA pour raccourcir le résumé.
- Divulguer les plafonds de résultats — ne pas présenter une recherche tronquée comme la file complète.
- Les notes de ticket sont en texte brut ; la page de relève elle-même est une sortie de conversation sauf demande contraire.
- Pour transférer un appel vif en cours, employer le skill Live Call Transfer Brief — ce skill-ci sert aux relèves de quart/fin de journée.

## Conventions locales (français)

- Horaires en format 24 h (« intervention prévue à 18 h 00 », « astreinte jusqu'à 22 h ») ; dates en JJ/MM/AAAA.
- « Relève », « passation » et « consignes » sont les termes usuels ; la section d'alerte peut s'intituler « À surveiller cette nuit » pour un quart de nuit.
- Rappeler les fuseaux si le desk couvre la métropole et l'outre-mer ou des clients hors de France (« 18 h 00 CET »).
- Entre collègues, le tutoiement dans les consignes est acceptable si c'est l'usage du desk ; toute note susceptible d'être vue du client repasse au vouvoiement.

## Consolide

Les skills de résumé de relève de quart, de bilan de fin de journée et de briefing d'équipe suivante.
