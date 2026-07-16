---
name: Routage de la boîte fourre-tout
description: Identifier le bon client et le bon contact pour un ticket arrivé dans une boîte fourre-tout (catchall) ou sans société — y compris les mails transférés et les alertes d'éditeurs — puis le réaffecter ou rendre des candidats. (Route catchall/no-company tickets to the right client and contact — in French.)
category: Localized
tools: [search_tickets, search_clients, search_contacts, assign_contact, update_ticket, add_ticket_note, create_ticket]
---

# Routage de la boîte fourre-tout

Version française de `skills/triage-and-routing/catchall-routing/SKILL.md` — maintenue à jour manuellement.

Router un ticket arrivé sans société (ou posé sur un contact fourre-tout) vers le vrai client et le vrai contact, au moyen d'une échelle de preuves explicite — ou n'y pas toucher quand la preuve n'y est pas.

## Quand l'utiliser

- Un ticket n'a aucune société affectée ou repose sur un contact fourre-tout/sans société.
- Un mail a été transféré (« TR : », « Fwd : », « FW: ») par un technicien ou un client et le ticket est attribué au transféreur, pas à l'expéditeur d'origine.
- Une alerte d'éditeur ou de supervision a atterri dans la boîte fourre-tout et son client doit être extrait du corps de l'alerte.
- Balayage périodique du tableau fourre-tout/d'entrée pour recartographier les tickets mal attribués.

## Étapes

1. Lire le ticket avec `search_tickets` : titre, description, tout premier message, et en-têtes/texte cité complets s'ils existent.
2. Pré-filtrage spam : si le message est manifestement de la prospection non sollicitée ou du courrier automatique sans aucun contenu identifiant un client et sans signe qu'un humain l'a transféré pour une raison, s'arrêter et le signaler comme spam pour revue au lieu de le router.
3. Extraire les indices d'identité dans cet ordre de force (l'échelle de preuves) :
   a. Le domaine e-mail du véritable expéditeur (le plus fort),
   b. Un nom de société énoncé explicitement dans le corps ou dans les champs d'alerte,
   c. Un nom de personne seul (le plus faible — jamais suffisant à lui seul).
4. Si le sujet commence par « TR : », « Fwd : » ou « FW: », ou si le corps contient un message d'origine cité, analyser la ligne « De : » / « From: » d'origine et prendre cet expéditeur — pas le transféreur — comme source d'identité.
5. Si le ticket est une alerte d'éditeur/supervision, extraire les champs structurés (nom de site, organisation, appareil, tenant) du corps de l'alerte et s'en servir comme indice de société.
6. Résoudre la société avec `search_clients` sur le domaine ou le nom extrait ; puis trouver le contact avec `search_contacts` restreint à cette société. Si l'expéditeur n'a pas de fiche contact, proposer la correspondance la plus proche ou noter qu'il faut en créer une — ne pas rattacher à un sosie.
7. Si l'affectation erronée actuelle doit être levée avant que la bonne puisse s'appliquer sur ce tenant, désaffecter d'abord (remettre le ticket en sans-société/fourre-tout), puis appliquer la bonne société et le bon contact.
8. N'appliquer la correspondance avec `update_ticket` et `assign_contact` que lorsqu'une seule société candidate correspond au niveau domaine ou nom explicite. Sinon, présenter les candidats et demander.
9. Si la synchronisation PSA du tenant refuse les changements de société sur un ticket existant, employer le circuit clore-et-recréer : `create_ticket` sous la bonne société avec le texte d'origine et une note en texte brut croisant les deux numéros de ticket, puis clore l'original — uniquement avec confirmation en usage assisté.
10. Publier une note interne en texte brut avec `add_ticket_note` indiquant ce qui a été apparié, quel barreau de l'échelle de preuves a servi, et ce qui a changé.

## Garde-fous

- Ne jamais affecter sur une simple similarité de nom. Un prénom/nom identique sans domaine ni confirmation explicite de société est une confiance faible → aucun changement.
- Ambiguïté (deux sociétés plausibles ou plus) → aucun changement ; lister les candidats à la place.
- Un ticket synchronisé vers un PSA doit toujours avoir une société ; ne jamais laisser un ticket lié au PSA sans société — si aucune correspondance n'est possible, l'orienter vers le client interne/fourre-tout désigné par la politique du desk et le signaler.
- Clore-et-recréer est quasi destructif : en usage assisté, confirmer avant de le faire ; préserver le texte d'origine et croiser les deux numéros en texte brut.
- Les notes destinées au PSA sont en texte brut — pas de markdown, pas d'émojis.
- Ne pas inventer de sociétés, de contacts ni de numéros de ticket. Si des recherches ont pu atteindre des plafonds de résultats, le dire.

## Conventions locales (français)

- Marqueurs de transfert et de réponse en environnement francophone : « TR : » (transfert Outlook FR), « Fwd : » et « RE : / Rép : » — l'analyse du message d'origine doit reconnaître ces préfixes au même titre que « FW: ».
- Les domaines des clients français utilisent souvent .fr, .com ou des marques accentuées translittérées ; comparer les domaines sans tenir compte des accents du nom de société (« Société Générale » ↔ societegenerale.fr).
- La note interne reste en français, mais les champs extraits d'une alerte (site name, organization, tenant) sont recopiés verbatim, sans traduction.
- Dates en JJ/MM/AAAA dans les notes croisant les deux tickets.

## Variante non assistée (Flows)

- N'agir qu'au niveau correspondance de domaine ou société explicite ; à tout niveau plus faible, ne rien changer et publier une seule note interne en texte brut : `CATCHALL ROUTING: aucune correspondance sûre. Indices relevés : <indices>. Laissé non affecté pour revue humaine.`
- Ne jamais employer le circuit clore-et-recréer en mode non assisté.
- Branche spam : ne pas clore en mode non assisté ; signaler par note uniquement.
- Un seul reroutage par ticket et par exécution ; si le ticket porte déjà une note de routage de ce skill, s'arrêter.
- Votre réponse entière est la note interne — pas de narration, pas de questions.

## Consolide

Les skills de rattachement fourre-tout → contact, de réparation de catchall, de reroutage avec désaffectation préalable, de pré-filtrage spam à l'admission, d'analyse d'expéditeur d'origine derrière « FW: », d'extraction de champs d'alertes d'éditeurs, d'identification société/contact, d'enrichissement sans-société, de clore-et-recréer et de reroutage vers le bon client (28 variantes minées).
