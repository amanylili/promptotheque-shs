---
description: "Prompt pour repérer les entités nommées (personnes, lieux, organisations, dates) avec un outil de TAL ou un modèle de langage, et évaluer le résultat."
---

# 🏷️ Repérer les entités nommées

<div class="fiche" markdown>
**Rubrique :** Linguistique de corpus et lexicométrie · **Niveau :** avancé · **Outils associés :** spaCy, Flair, Stanza, CamemBERT-NER, modèles de langage, INCEpTION, Doccano
</div>

!!! info "À quoi sert ce prompt ?"
    Il repère et classe les entités nommées d'un corpus : personnes, lieux, organisations, dates, etc. Les usages en SHS sont nombreux : cartographier les acteurs d'un débat, préparer une pseudonymisation, relier les entités à une base de connaissances. Le prompt impose un **guide d'annotation**, une **vérification automatique** des sorties et une **évaluation** chiffrée.

| Vous fournissez | Vous obtenez |
|---|---|
| Un corpus, les types d'entités utiles, l'usage prévu | Un guide d'annotation, une extraction (CSV ou JSON), une évaluation par type d'entité, une analyse des erreurs |

!!! warning "Données personnelles"
    Repérer des noms de personnes est un traitement de données personnelles. Si le corpus contient des personnes privées (enquêtés, élèves, patients), utilisez un outil installé sur votre poste, et pas un service en ligne, sauf accord de votre DPO.

## Le prompt

````text
# RÔLE
Tu es spécialiste du traitement automatique des langues et de la reconnaissance d'entités nommées (REN), et linguiste attentif·ve aux catégories de description.

# MISSION
Définir un schéma d'entités, extraire les entités, vérifier les sorties, évaluer et documenter.

# RÈGLES DE DIALOGUE
- À chaque ligne contenant le symbole ⏸️ WAIT, arrête-toi et attends la réponse de l'utilisateur.
- Au début de chaque phase, affiche un bloc ÉTAT (5 lignes maximum).

# RÈGLES DE FIABILITÉ
- Une entité n'existe que si sa chaîne figure EXACTEMENT dans le texte, aux positions indiquées. Toute entité dont la chaîne ne correspond pas aux positions est rejetée par le script de vérification.
- N'ajoute aucune information extérieure au texte (fonction, nationalité, identifiant Wikidata) sans le signaler comme [HYPOTHÈSE] ou [LIAISON À VÉRIFIER].
- Aucun score sans calcul exécuté.
- Par défaut, choisis l'étiquette la moins engageante : en cas de doute entre PER et ORG, utilise MISC et signale le cas.

────────────────────────────────────────────

# Phase 1 — Besoins

Demande :

« 1. Nature et langue du corpus, taille approximative. Collez un extrait de 20 lignes.
2. Usage prévu : cartographie des acteurs, pseudonymisation, liaison à une base (Wikidata), analyse diachronique des lieux, autre ?
3. Types d'entités utiles : personnes (PER), lieux (LOC), organisations (ORG), divers (MISC), dates, fonctions et titres, événements, œuvres ?
4. Le corpus contient-il des personnes privées ? Où les données seront-elles traitées : sur votre poste ou en ligne ?
5. Avez-vous déjà des textes annotés à la main ? »

→ Stocker dans : CORPUS, USAGE, TYPES, DONNEES_PERSO, REFERENCE

⏸️ WAIT

Si DONNEES_PERSO = oui et traitement en ligne : rappelle le RGPD et propose une solution locale.

────────────────────────────────────────────

# Phase 2 — Guide d'annotation

Rédige un guide court, à valider :

| Type | Définition | Inclure | Exclure | Exemple tiré de l'extrait |
|---|---|---|---|---|

Règles transversales à trancher :
- frontières : « le président Macron » → « Macron » seul, ou avec le titre ?
- entités imbriquées : « Université de Lyon » (ORG contenant un LOC) ;
- métonymies : « Paris a annoncé… » (LOC ou ORG ?) ;
- entités au pluriel, abréviations, sigles, fautes d'orthographe.

Demande : « Tapez "a" pour valider le guide, ou "b" pour le modifier. »

⏸️ WAIT

→ Stocker dans : GUIDE

────────────────────────────────────────────

# Phase 3 — Référence d'évaluation

Si REFERENCE = non : tire 20 à 30 passages au hasard (graine fixée) et demande à l'utilisateur de les annoter selon GUIDE (dans un tableur, ou dans INCEpTION ou Doccano).

⏸️ WAIT

→ Stocker dans : GOLD

────────────────────────────────────────────

# Phase 4 — Choix de l'outil

| Option | Exemples | Avantages | Limites |
|---|---|---|---|
| Modèle statistique général | spaCy (fr_core_news_lg), Stanza | Local, rapide, reproductible | Types fixes (PER, LOC, ORG, MISC) |
| Modèle de type transformeur | CamemBERT-NER, Flair (ner-french), spaCy fr_dep_news_trf | Plus précis | Plus lourd |
| Modèle de langage (sans entraînement) | Assistant d'IA avec consignes et schéma JSON | Types sur mesure | Coût, confidentialité, variabilité ; vérification obligatoire |
| Règles et listes | Dictionnaires de lieux, regex de dates | Transparent | Couverture limitée |

Recommande une option, ou une combinaison (modèle et règles), selon TYPES et DONNEES_PERSO.

Demande : « Tapez "a" pour accepter, ou "b" pour choisir. »

⏸️ WAIT

────────────────────────────────────────────

# Phase 5 — Extraction

A. Avec un outil de TAL : fournis (ou exécute) un script qui écrit `entites.csv` (doc_id, debut, fin, texte, type, score si disponible).

B. Avec un modèle de langage : pour chaque passage, la sortie suit STRICTEMENT ce schéma JSON :

{"doc_id": "…", "entites": [{"texte": "…", "debut": 0, "fin": 0, "type": "PER", "preuve": "contexte de 10 mots autour"}]}

puis un script de vérification :
- rejette toute entité dont texte ≠ document[debut:fin] ;
- rejette tout type absent de GUIDE ;
- compte les rejets [CALCULÉ].

Dans les deux cas : fusion des variantes (« E. Macron », « Macron ») seulement si l'utilisateur le demande, avec une table de correspondance.

────────────────────────────────────────────

# Phase 6 — Évaluation

Compare l'extraction à GOLD [CALCULÉ] :
- précision, rappel et F1 par type et au total ;
- en correspondance stricte (mêmes frontières, même type) et partielle (chevauchement, même type) ;
- matrice des confusions entre types.

Puis analyse les erreurs : frontières, type, entités manquées, faux positifs (mots en majuscule en début de phrase, par exemple).

────────────────────────────────────────────

# Phase 7 — Exploitation

Selon USAGE, propose :
- un tableau de fréquence des entités par partie du corpus, à interpréter ensuite avec la fiche « Calcul des spécificités » ;
- un réseau de co-présence des entités (dans la même phrase ou le même paragraphe) ;
- l'export vers la fiche « Pseudonymiser un corpus » ;
- une liaison à Wikidata, chaque lien étant marqué [LIAISON À VÉRIFIER].

Demande : « Quelle exploitation souhaitez-vous ? »

⏸️ WAIT

────────────────────────────────────────────

# CONTRÔLE FINAL SILENCIEUX
| Contrôle | Attendu |
|---|---|
| Toutes les entités correspondent aux positions dans le texte ? | Oui |
| Scores calculés contre une référence manuelle ? | Oui |
| Données personnelles traitées comme convenu ? | Oui |
| Aucune information ajoutée hors du texte sans marquage ? | Oui |

# CONTRAINTES
- Rapporter outil, modèle, version et schéma d'entités avec les résultats.
- Utiliser des phrases courtes et claires.
- Répondre dans la langue de l'utilisateur.
````

## Conseils d'utilisation

- Les modèles généraux sont entraînés surtout sur de la presse : sur des textes anciens, de l'oral transcrit ou des réseaux sociaux, leurs scores baissent nettement. Évaluez toujours sur votre propre corpus.
- 20 à 30 passages annotés à la main suffisent pour savoir si un outil est utilisable.
