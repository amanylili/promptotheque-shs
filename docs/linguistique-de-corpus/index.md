---
description: "Prompts de linguistique de corpus et de lexicométrie : constituer, nettoyer, pseudonymiser et annoter un corpus, extraire des concordances, puis l'analyser (fréquences, spécificités, AFC, cooccurrences, Reinert)."
---

# 📚 Linguistique de corpus et lexicométrie

Prompts pour tout le travail sur corpus, de sa constitution jusqu'à son analyse statistique. La lexicométrie (ou textométrie) est la partie de la linguistique de corpus qui mesure et compare le vocabulaire des textes.

## 1. Préparer et explorer le corpus

| Prompt | Pour… | Niveau |
|---|---|---|
| [Constituer un corpus](constitution-corpus.md) | Rédiger un protocole de constitution, choisir les métadonnées, préparer le plan de gestion des données | Débutant |
| [Nettoyer un corpus](nettoyage-corpus.md) | Diagnostiquer et corriger encodage, césures, en-têtes, balises ; formater pour TXM, IRaMuTeQ ou AntConc | Débutant |
| [Pseudonymiser un corpus](pseudonymisation-corpus.md) | Remplacer noms, lieux, institutions par des codes cohérents, avec table de correspondance | Débutant |
| [Annoter (lemmes, catégories)](annotation-morphosyntaxique.md) | Lemmatiser et étiqueter morphosyntaxiquement, puis évaluer l'annotation | Intermédiaire |
| [Concordances (KWIC)](concordances-kwic.md) | Extraire et analyser les contextes d'un mot ou d'un motif | Débutant |

## 2. Analyser le corpus : lexicométrie

Tous ces prompts imposent un **calcul réel**, par code exécuté ou par script fourni. L'assistant aide ensuite à **interpréter** les résultats.

| Prompt | Pour… | Niveau |
|---|---|---|
| [Comptage de fréquences](../lexicometrie/comptage-frequences.md) | Dictionnaire de fréquences, hapax, richesse lexicale, loi de Zipf | Débutant |
| [Calcul des spécificités](../lexicometrie/calcul-specificites.md) | Mots sur- et sous-employés dans une partie du corpus (modèle hypergéométrique de Lafon) | Intermédiaire |
| [AFC sur tableau lexical](../lexicometrie/afc-tableau-lexical.md) | Analyse factorielle des correspondances formes × parties | Avancé |
| [Cooccurrences](../lexicometrie/cooccurrences.md) | Attractions lexicales autour d'un pôle | Intermédiaire |
| [Classification (méthode Reinert)](../lexicometrie/classification-reinert.md) | Préparer, paramétrer et interpréter une classification descendante hiérarchique | Intermédiaire |

!!! tip "Ordre conseillé"
    Constituer → Nettoyer → Pseudonymiser → Annoter → Concordances → Fréquences → Spécificités et cooccurrences → AFC ou classification de Reinert → retour au texte par les concordances.

!!! warning "Rappel"
    Un résultat lexicométrique n'est pas une interprétation. Revenez toujours au texte, par les concordances, avant de conclure.
