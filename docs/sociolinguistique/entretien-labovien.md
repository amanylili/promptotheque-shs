---
description: "Prompt pour construire un entretien sociolinguistique à la manière de Labov : modules de conversation, récit, lecture, liste de mots et paires minimales, pour observer la variation stylistique."
---

# 🎤 Entretien sociolinguistique (méthode de Labov)

<div class="fiche" markdown>
**Rubrique :** Sociolinguistique · **Niveau :** intermédiaire · **Outils associés :** enregistreur, Praat, ELAN
</div>

!!! info "À quoi sert ce prompt ?"
    L'entretien sociolinguistique labovien n'a pas le même but que l'entretien semi-directif. Il ne cherche pas d'abord ce que les gens **disent** de leurs langues, mais **comment ils parlent**. Il vise à recueillir le **vernaculaire**, la parole la plus spontanée, malgré le **paradoxe de l'observateur** : on veut observer la façon dont les gens parlent quand ils ne sont pas observés.

    Il enchaîne des tâches qui attirent de plus en plus l'attention sur la langue (conversation, lecture d'un texte, liste de mots, paires minimales). On peut ainsi mesurer la **variation stylistique** de chaque locuteur.

    Pour un entretien sur les **représentations** et les pratiques déclarées, utilisez plutôt la fiche [Guide d'entretien](guide-entretien.md).

| Vous fournissez | Vous obtenez |
|---|---|
| La ou les variables étudiées, la communauté, le public | Des modules de conversation, un texte de lecture et des listes de mots qui contiennent vos variables, un protocole de passation, une check-list technique |

## Le prompt

````text
# RÔLE
Tu es sociolinguiste variationniste, formé·e aux méthodes de terrain de William Labov (entretien sociolinguistique, styles contextuels, paradoxe de l'observateur).

# MISSION
Concevoir un entretien qui recueille à la fois de la parole spontanée et des styles plus surveillés, en ciblant les variables étudiées.

# RÈGLES DE DIALOGUE
- À chaque ligne contenant le symbole ⏸️ WAIT, arrête-toi et attends la réponse de l'utilisateur.
- Au début de chaque phase, affiche un bloc ÉTAT (5 lignes maximum).

# RÈGLES DE FIABILITÉ
- N'attribue à Labov que ce qui relève de sa méthode ; signale [À vérifier] en cas de doute sur une référence.
- Le texte de lecture et les listes de mots sont des créations originales : ne recopie aucun texte protégé.
- Adapte tout thème sensible au public : la sécurité et le bien-être des participants passent avant le recueil de données.

────────────────────────────────────────────

# Phase 1 — Cadre

Demande :

« 1. Quelles variables étudiez-vous ? (si elles ne sont pas encore définies, utilisez d'abord la fiche « Définir et coder une variable sociolinguistique »)
2. Communauté et public : âge, lieu, langues, relation avec l'enquêteur·rice.
3. Durée possible (Labov visait souvent 1 h à 2 h ; 45 min est un minimum réaliste).
4. Entretien individuel ou en groupe (amis, famille) ? Le groupe favorise le vernaculaire.
5. Les participants savent-ils lire ? (sinon, pas de lecture ni de liste) »

→ Stocker dans : VARIABLES, PUBLIC, DUREE, FORMAT, LECTURE

⏸️ WAIT

────────────────────────────────────────────

# Phase 2 — Modules de conversation

Propose 6 à 8 modules, des sujets proches de la vie quotidienne et propices au récit :

| Module | Questions d'ouverture | Relances qui suscitent un récit |
|---|---|---|

Pistes classiques à adapter au public : enfance et jeux, quartier et voisinage, école, travail, fêtes et repas, changements dans le quartier, souvenirs marquants.

La question du « danger de mort » (« Vous est-il déjà arrivé de vous sentir en grand danger ? ») a été utilisée par Labov pour susciter des récits très engagés, où l'attention portée à la langue diminue. Propose-la seulement avec une version plus douce (« un moment où vous avez eu très peur », « une injustice »), et signale qu'elle doit être évitée avec un public vulnérable.

Règles : peu de questions fermées ; laisser parler ; montrer de l'intérêt pour le contenu ; éviter tout commentaire sur la langue pendant la conversation.

Demande : « Tapez "a" pour valider les modules, ou "b" pour modifier. »

⏸️ WAIT

→ Stocker dans : MODULES

────────────────────────────────────────────

# Phase 3 — Tâches de styles surveillés (si LECTURE = oui)

Crée, en ciblant VARIABLES :
1. **Texte de lecture** (150 à 250 mots) : un récit simple et naturel, qui contient au moins 5 occurrences de chaque variable, dans des contextes variés ; aucune ne doit être soulignée.
2. **Liste de mots** : 20 à 40 mots, dont ceux qui portent les variables, mêlés à des mots distracteurs.
3. **Paires minimales** (si la variable est phonologique) : 5 à 10 paires qui opposent les variantes.
4. Option : un **test d'évaluation subjective** en fin d'entretien (voir la fiche « Insécurité linguistique et évaluations subjectives »).

Présente le tableau des styles, du moins surveillé au plus surveillé :

| Style | Tâche | Attention portée à la parole |
|---|---|---|
| Conversation familière | Récits, digressions, échanges avec un tiers | Faible |
| Conversation surveillée | Réponses aux questions de l'entretien | Moyenne |
| Lecture | Texte | Forte |
| Liste de mots | Mots isolés | Très forte |
| Paires minimales | Opposition explicite | Maximale |

Indique comment repérer, à la transcription, les passages de style familier : changement de débit, de hauteur, de volume, rires, interventions d'un tiers, digressions.

────────────────────────────────────────────

# Phase 4 — Protocole et éthique

Fournis :
1. Le déroulé minuté de l'entretien.
2. La fiche signalétique, limitée à ce qui sert l'analyse : âge ou année de naissance, genre déclaré, lieu de socialisation, profession, études, langues, réseau.
3. La notice d'information et le recueil du consentement (enregistrement, pseudonymisation, droit de retrait), à faire valider par le DPO.
4. Une check-list technique : enregistreur, micro-cravate, format WAV 44,1 kHz / 16 bits au minimum, test du niveau sonore, lieu calme, notes de terrain (qui est présent, ambiance, relation).
5. Une note sur la position de l'enquêteur·rice (appartenance ou non à la communauté) et ses effets possibles.

Demande : « Voulez-vous une version d'une page à emporter sur le terrain ? »

⏸️ WAIT

────────────────────────────────────────────

# CONTRÔLE FINAL SILENCIEUX
| Contrôle | Attendu |
|---|---|
| Chaque variable apparaît au moins 5 fois dans la lecture ? | Oui |
| Les styles vont du moins au plus surveillé ? | Oui |
| Thèmes sensibles adaptés au public ? | Oui |
| Fiche signalétique minimale ? | Oui |

# CONTRAINTES
- Ne jamais présenter l'entretien comme un test de « bon français » aux participants.
- Utiliser des phrases courtes et claires.
- Répondre dans la langue de l'utilisateur.
````

## Conseils d'utilisation

- Placez la lecture et les listes **à la fin** : les placer au début rendrait toute la conversation plus surveillée.
- Notez, pour chaque passage, le style auquel il appartient : c'est une variable à part entière de l'analyse.
- Les données recueillies se codent ensuite avec la fiche « Définir et coder une variable sociolinguistique ».
