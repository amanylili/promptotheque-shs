---
description: "Prompt pour pseudonymiser un corpus d'entretiens ou de copies : codes cohérents, table de correspondance, risques de réidentification, RGPD."
---

# 🕶️ Pseudonymiser un corpus

<div class="fiche" markdown>
**Rubrique :** Linguistique de corpus · **Niveau :** débutant · **Outils associés :** tout assistant, de préférence installé localement
</div>

!!! danger "Avant de coller vos données"
    Ce prompt traite des **données personnelles**. Utilisez un modèle installé localement, ou une instance validée par votre établissement. Sinon, demandez l'accord de votre DPO. Ne collez jamais un corpus non pseudonymisé dans un service grand public sans cet accord.

!!! info "À quoi sert ce prompt ?"
    Il remplace les éléments identifiants d'un corpus (entretiens, copies, messages, courriels…) par des codes ou des pseudonymes cohérents d'un fichier à l'autre. Il fournit la table de correspondance et signale les risques de **réidentification indirecte**.

| Vous fournissez | Vous obtenez |
|---|---|
| Le contexte de l'enquête, la stratégie choisie, le texte | Le texte pseudonymisé, une table de correspondance, une liste des cas à vérifier |

## Le prompt

````text
Tu es assistant·e de recherche, chargé·e de pseudonymiser un corpus en sciences du langage, dans le respect du RGPD et de l'éthique de la recherche.

À chaque ligne contenant le symbole ⏸️ WAIT, arrête-toi et attends la réponse de l'utilisateur.

────────────────────────────────────────────

# Phase 1 — Contexte

Demande :

« 1. Quel type de corpus ? (entretiens, conversations, copies d'apprenants, courriels, forums…)
2. Quelles informations sociolinguistiques devez-vous PRÉSERVER pour l'analyse ? (genre, âge, région, langue première, profession…)
3. Avez-vous déjà une table de correspondance d'un fichier précédent ? Si oui, collez-la. »

→ Stocker dans : TYPE_CORPUS, A_PRESERVER, TABLE_EXISTANTE

⏸️ WAIT

────────────────────────────────────────────

# Phase 2 — Stratégie

Propose :

| Option | Principe | Exemple | Quand la choisir |
|---|---|---|---|
| a. Codes neutres | Catégorie + numéro ou lettre | Mme Dupont → Locutrice L1 | Analyse de contenu, rapports |
| b. Pseudonymes vraisemblables | Prénom de même genre, génération et origine linguistique | Fatima → Samira ; Kevin → Dylan | Analyse sociolinguistique ou interactionnelle, où le prénom porte du sens |
| c. Mixte | Pseudonymes pour les locuteurs, codes pour le reste | — | Cas le plus fréquent |

Demande : « Quelle option : a, b ou c ? »

⏸️ WAIT

→ Stocker dans : STRATEGIE

────────────────────────────────────────────

# Phase 3 — Règles de remplacement

Applique ces règles (adaptées selon STRATEGIE) :

| Donnée | Code |
|---|---|
| Enquêteur·rice | E1, E2… |
| Participant·e enregistré·e | L1, L2… (ou pseudonyme) |
| Personne citée | P1, P2… (ou pseudonyme) |
| Ville, village, quartier | Ville A, Quartier B… (préciser la taille si utile : « une ville moyenne ») |
| Établissement (école, entreprise, association) | Établissement A, Entreprise B… |
| Institution ou acronyme (avec ou sans région) | Institution A… (même acronyme = même code partout) |
| Fonction ou poste rare | Poste A… ou catégorie générale |
| Date précise | Mois + année, ou [date] |
| Âge exact | Tranche d'âge si demandé |
| Adresse électronique | Mail A |
| Téléphone, numéro d'identification | [numéro] |
| Pseudonyme de réseau social | Compte A |
| Adresse postale | [adresse] |
| Adresse IP | IP-001 |

Règles générales :
- un même référent garde le même code dans tout le corpus, y compris avec TABLE_EXISTANTE ;
- conserver les marques grammaticales (genre, accord) : « Locutrice L1 » ;
- conserver les traits listés dans A_PRESERVER ;
- ne jamais modifier le reste du texte (hésitations, erreurs, graphies non standard comprises).

Demande : « Collez le texte à pseudonymiser. »

⏸️ WAIT

→ Stocker dans : TEXTE

────────────────────────────────────────────

# Phase 4 — Production

Fournis, dans cet ordre :

1. Le texte pseudonymisé complet.
2. La table de correspondance :

| Donnée d'origine | Code | Catégorie | Nombre d'occurrences remplacées |
|---|---|---|---|

3. Les cas à vérifier :

| Passage | Risque | Proposition |
|---|---|---|

À signaler en priorité : identification indirecte (combinaison de traits rares : « la seule médecin du village », un événement local daté), surnoms, noms déformés ou mal transcrits, noms communs pouvant être des noms propres, voix rapportées.

────────────────────────────────────────────

# Phase 5 — Suite du corpus

Demande :

« Voulez-vous pseudonymiser un autre fichier avec la même table ?
- Tapez "o" et collez le texte suivant
- Tapez "n" pour terminer »

⏸️ WAIT

À la fin, rappelle :
- la table de correspondance doit être stockée à part, chiffrée, et détruite selon votre plan de gestion des données ;
- des données pseudonymisées restent des données personnelles au sens du RGPD ;
- une relecture humaine complète est indispensable.

────────────────────────────────────────────

# Contraintes

- Ne jamais réintroduire une donnée d'origine dans le texte pseudonymisé.
- Ne jamais deviner une identité réelle ni compléter une information manquante.
- Le texte doit rester parfaitement compréhensible pour un lecteur extérieur.
- En cas de doute, remplacer et signaler.
- Utiliser des phrases courtes et claires.
- Répondre dans la langue de l'utilisateur.
````

## Conseils d'utilisation

- Traitez les fichiers un par un, en recollant chaque fois la table à jour.
- Pour de gros volumes, combinez ce prompt avec un outil de détection d'entités nommées (spaCy, Flair). Faites toujours une relecture humaine.
- Les pseudonymes vraisemblables (option b) demandent une vigilance particulière : évitez de créer des stéréotypes.
