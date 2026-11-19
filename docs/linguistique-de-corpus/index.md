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
| [Segmenter un corpus](segmentation.md) | Définir et évaluer le découpage en mots, phrases, énoncés, tours de parole ou parties | Intermédiaire |

### 🔍 Rechercher dans le corpus

Prompts pour retrouver, compter, extraire ou transformer des formes dans un corpus, avec des **expressions régulières** (regex) ou des **requêtes structurées**. Chaque prompt teste ses expressions sur des exemples à trouver et à ne pas trouver avant de les livrer.

| Prompt | Pour… | Niveau |
|---|---|---|
| [Du langage courant à l’expression régulière](expressions-regulieres.md) | Décrire en français ce qu’on cherche, obtenir une regex expliquée, testée et adaptée à son outil | Débutant à avancé |
| [Repérer un phénomène linguistique par regex](regex-phenomenes.md) | Morphologie, graphies non standard, écriture inclusive dans un corpus non annoté, avec précision et rappel mesurés | Avancé |
| [Extraire des données structurées par regex](regex-extraction.md) | Locuteurs, horodatages, variables IRaMuTeQ, citations : du texte au tableau, avec rapport de validation | Avancé |
| [Transformer un corpus par remplacements en série](regex-remplacements.md) | Chaîne de rechercher-remplacer testée, essai à blanc, journal des modifications | Avancé |
| [Interroger un corpus : CQL et SQL](requetes-cql-sql.md) | Traduire une question en requête structurée et mesurer son bruit et son silence | Avancé |
| [Construire un concordancier](concordancier.md) | Programmer un concordancier KWIC sur mesure, testé, avec tri et export | Avancé |

## 2. Analyser le corpus : lexicométrie

Tous ces prompts imposent un **calcul réel**, par code exécuté ou par script fourni. L'assistant aide ensuite à **interpréter** les résultats.

| Prompt | Pour… | Niveau |
|---|---|---|
| [Comptage de fréquences](../lexicometrie/comptage-frequences.md) | Dictionnaire de fréquences, hapax, richesse lexicale, loi de Zipf | Débutant |
| [Calcul des spécificités](../lexicometrie/calcul-specificites.md) | Mots sur- et sous-employés dans une partie du corpus (modèle hypergéométrique de Lafon) | Intermédiaire |
| [AFC sur tableau lexical](../lexicometrie/afc-tableau-lexical.md) | Analyse factorielle des correspondances formes × parties | Avancé |
| [Cooccurrences](../lexicometrie/cooccurrences.md) | Attractions lexicales autour d'un pôle | Intermédiaire |
| [Classification (méthode Reinert)](../lexicometrie/classification-reinert.md) | Préparer, paramétrer et interpréter une classification descendante hiérarchique | Intermédiaire |
| [Segments répétés](../lexicometrie/segments-repetes.md) | Suites de mots récurrentes (Salem) et lexical bundles, avec dispersion | Intermédiaire |
| [Motifs textuels](../lexicometrie/motifs-textuels.md) | Suites récurrentes mêlant formes, lemmes et catégories | Avancé |
| [Analyse en composantes principales (ACP)](../lexicometrie/acp.md) | Dimensions de variation entre textes à partir de traits mesurés | Avancé |
| [Classification automatique (CAH, k-means)](../lexicometrie/classification-automatique.md) | Regrouper textes ou locuteurs, choisir le nombre de classes, tester la stabilité | Avancé |
| [Choisir et rapporter une statistique](../lexicometrie/statistiques-corpus.md) | Mesures, tests et tailles d’effet adaptés à la question (d’après Brezina 2018) | Avancé |

## 3. TAL et apprentissage automatique

Prompts pour enrichir et exploiter le corpus avec les outils du traitement automatique des langues. Tous imposent une **évaluation** sur un échantillon corrigé à la main.

| Prompt | Pour… | Niveau |
|---|---|---|
| [Repérer les entités nommées](entites-nommees.md) | Personnes, lieux, organisations, dates : extraction vérifiée et évaluée | Avancé |
| [Aligner un corpus parallèle](alignement.md) | Aligner un texte et sa traduction, évaluer, construire un concordancier bilingue | Avancé |
| [Apprentissage profond appliqué au texte](apprentissage-profond.md) | Classification, plongements, BERTopic, annotation par modèle de langage | Avancé |

!!! tip "Ordre conseillé"
    Constituer → Nettoyer → Segmenter → Pseudonymiser → Annoter → Interroger et concordances → Fréquences, segments répétés, motifs → Spécificités et cooccurrences → AFC, ACP, classifications → retour au texte par les concordances. La fiche « Choisir et rapporter une statistique » sert à chaque étape de calcul.

!!! warning "Rappel"
    Un résultat lexicométrique n'est pas une interprétation. Revenez toujours au texte, par les concordances, avant de conclure.
