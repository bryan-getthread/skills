---
name: Réponse client
description: Rédiger une réponse externe au client dans la voix et le format maison — point d'avancement, note de statut, message de clôture ou tout e-mail client sur un ticket. (Draft a client-facing reply in the house voice — status updates, closing messages, any client email on a ticket, in French.)
category: Localized
tools: [search_tickets, view_openDraft, add_ticket_note]
---

# Réponse client

Version française de `skills/communication/client-reply/SKILL.md` — maintenue à jour manuellement.

**Quand l'utiliser :** un technicien doit envoyer un message au client et le veut conforme à la charte, prêt à partir.

**Ce que vous obtenez :** un brouillon qui commence par la réponse, respecte la voix maison et peut être envoyé tel quel.

## Quand l'utiliser

- « Rédige une réponse au client sur ce ticket. »
- « Préviens <user> que son compte est débloqué. »
- « Écris un message pour dire que nous travaillons toujours avec l'éditeur. »
- Tout e-mail destiné au client où le ton, le format et l'exactitude comptent.

## Étapes

1. Lire l'intégralité du fil du ticket avant d'écrire un mot — chaque message, note et changement de statut antérieurs. Ne jamais rédiger à partir du seul dernier message.
2. Rédiger la réponse en commençant par la réponse elle-même ou l'état actuel, puis les détails à l'appui.
3. Appliquer les standards de la voix maison :
   - Saluer le client nommément sur le **premier** message d'un fil uniquement ; omettre la salutation dans les réponses suivantes du même fil.
   - Aucune formule creuse en ouverture (« J'espère que vous allez bien », « Je me permets de revenir vers vous »). Entrer directement dans le vif.
   - Aucun jargon technique sauf si le client a employé le terme en premier ; si un terme technique est inévitable, ajouter une explication en langage courant.
   - 1 à 2 paragraphes courts. S'il en faut davantage, c'est probablement qu'il faut un appel.
   - Structure conforme au skill Email Baseline Standard (communication/email-baseline-standard).
4. Si le membre dispose d'une liste de mots préférés/bannis (skill de contexte personnel ou note de style maison), l'appliquer : remplacer les mots bannis par leurs substituts désignés ; privilégier les alternatives listées.
5. S'il s'agit d'un message de clôture, terminer le corps par la ligne de réouverture : « Si quelque chose n'est pas résolu, il vous suffit de répondre à cet e-mail — votre réponse rouvrira ce ticket. »
6. Signature : si l'espace de travail applique automatiquement une signature gérée, s'arrêter après la dernière phrase du corps — ne PAS ajouter de formule de politesse (« Cordialement, ... ») qui ferait doublon. N'ajouter une formule de clôture que lorsqu'aucune signature gérée n'existe.
7. Présenter la réponse en brouillon ouvert avec view_openDraft pour relecture par le technicien. Ne pas envoyer.

## Garde-fous

- Ne jamais inventer de détails : pas d'horodatages, d'étapes réalisées, de numéros de ticket, de liens ni de promesses fabriqués. Si le fil n'établit pas un fait, l'omettre ou le marquer <à confirmer avec le technicien>.
- Le brouillon reste un brouillon tant qu'un humain ne l'a pas approuvé. Ne jamais envoyer de façon autonome.
- Ne jamais exposer de notes internes, d'identifiants, de tarifs ni de données d'autres clients dans une réponse destinée au client.
- S'aligner sur la langue du client : si le fil est dans une autre langue, rédiger dans cette langue (voir communication/multilingual-reply pour le circuit de rétro-traduction).
- Dégradation : view_openDraft n'est disponible que dans l'application. Via MCP externe, livrer le brouillon fini dans la conversation, clairement étiqueté « BROUILLON — à relire avant envoi ».

## Conventions locales (français)

- Vouvoiement systématique avec le client ; ne passer au tutoiement que si le client l'a explicitement instauré.
- Salutation d'ouverture : « Bonjour <Prénom>, » si la relation est établie ; « Bonjour Madame/Monsieur <Nom>, » en registre plus formel — suivre le registre déjà installé dans le fil.
- Formules de clôture sobres quand aucune signature gérée n'existe : « Cordialement, » ou « Bien cordialement, » — éviter les formules longues (« Je vous prie d'agréer... ») dans un e-mail de support.
- Dates en JJ/MM/AAAA, heures en format 24 h (« 14 h 30 ») ; espace insécable avant « : », « ? », « ! » et « ; » selon la typographie française.
- Ligne de réouverture de référence : « Si quelque chose n'est pas résolu, il vous suffit de répondre à cet e-mail — votre réponse rouvrira ce ticket. »

## Consolide

Les skills de rédaction/envoi de réponse, de format de réponse client standard, de réponse dans une voix nommée, de ton, de mots bannis et de signature.
