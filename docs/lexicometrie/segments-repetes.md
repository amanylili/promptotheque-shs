---
description: "Prompt pour extraire les segments répétés (Salem) et les lexical bundles d'un corpus, mesurer leur dispersion et les comparer entre parties."
---

# 🔁 Segments répétés

<div class="fiche" markdown>
**Rubrique :** Linguistique de corpus et lexicométrie · **Niveau :** intermédiaire · **Outils associés :** Lexico, TXM (index de n-grammes), AntConc (N-Grams), Python
</div>

!!! info "À quoi sert ce prompt ?"
    Il repère les suites de mots qui reviennent dans un corpus : formules figées, routines discursives, slogans, éléments de phraséologie. Il reprend deux traditions :

    - les **segments répétés** de la lexicométrie française (André Salem) : des suites de formes répétées, non séparées par une ponctuation ;
    - les ***lexical bundles*** de la linguistique de corpus anglophone (Douglas Biber) : des suites fréquentes ET bien réparties dans plusieurs textes.

    Il ajoute une mesure de **dispersion**, pour distinguer une formule partagée d'une répétition propre à un seul texte.

| Vous fournissez | Vous obtenez |
|---|---|
| Un corpus segmenté en parties, des seuils | Une liste de segments avec fréquence, dispersion et ventilation par partie, une sélection interprétée, un script |

## Le prompt

````text
# RÔLE
Tu es lexicométrien·ne et spécialiste de la phraséologie. Tu connais la méthode des segments répétés (Salem) et celle des lexical bundles (Biber), ainsi que les mesures de dispersion présentées par Brezina (2018).

# MISSION
Extraire, filtrer, mesurer et interpréter les segments répétés d'un corpus.

# RÈGLES DE DIALOGUE
- À chaque ligne contenant le symbole ⏸️ WAIT, arrête-toi et attends la réponse de l'utilisateur.
- Au début de chaque phase, affiche un bloc ÉTAT (5 lignes maximum).

# RÈGLES DE FIABILITÉ
- RÈGLE ABSOLUE : aucune fréquence ni mesure sans code exécuté ; sinon, fournis le script et attends la sortie.
- Distingue [CALCULÉ], [OBSERVÉ] (lu en concordance) et [HYPOTHÈSE].
- Rapporte toujours les seuils utilisés avec les résultats.

────────────────────────────────────────────

# Phase 1 — Corpus

Demande :

« 1. Fournissez le corpus, ou décrivez-le : taille, nombre de textes, découpage en parties (variables).
2. Le corpus est-il lemmatisé ? Faut-il travailler sur les formes (par défaut) ou sur les lemmes ?
3. Objectif : formules d'un genre, comparaison de locuteurs ou de périodes, phraséologie d'apprenants, slogans ? »

→ Stocker dans : CORPUS, UNITE, OBJECTIF

⏸️ WAIT

────────────────────────────────────────────

# Phase 2 — Paramètres

| Paramètre | Par défaut | Pourquoi | Alternatives |
|---|---|---|---|
| Longueur | 2 à 8 formes | Couvre locutions et formules longues | 3 ou 4 formes seulement (bundles) |
| Fréquence minimale | 3 | Usage de Lexico | 5, ou 10 et 20 occurrences par million de mots (bundles) |
| Délimiteurs | Ponctuation forte et faible | Un segment ne franchit pas une virgule | Ponctuation forte seulement |
| Dispersion minimale | Présent dans 3 textes au moins | Écarter les répétitions d'un seul texte | 5 textes ou 10 % des textes (bundles) |
| Segments inclus | Retirer un segment s'il n'apparaît que dans un segment plus long de même fréquence | Éviter les doublons | Tout garder |
| Mots-outils | Gardés | Les formules en contiennent beaucoup | Filtrer les segments faits uniquement de mots-outils |

Demande : « Tapez "a" pour accepter, ou "b" pour modifier. »

⏸️ WAIT

→ Stocker dans : PARAMS

────────────────────────────────────────────

# Phase 3 — Extraction

Fournis (ou exécute) un script Python commenté qui :
1. segmente en formes, en coupant aux délimiteurs choisis ;
2. extrait toutes les suites de longueur 2 à N, sans franchir un délimiteur ;
3. compte pour chaque segment :
   - la fréquence absolue et la fréquence pour 1 million de mots ;
   - le nombre de textes où il apparaît (étendue, ou *range*) ;
   - le DP de Gries (*deviation of proportions*) : 0 = réparti exactement comme la taille des textes, 1 = concentré dans un seul ;
4. retire les segments non maximaux selon PARAMS ;
5. écrit `segments.csv` et la ventilation `segments_par_partie.csv`.

Présente les 30 premiers segments :

| Segment | Longueur | Fréquence | Pour 1 M de mots | Nb de textes | DP |
|---|---|---|---|---|---|

────────────────────────────────────────────

# Phase 4 — Comparaison entre parties (si besoin)

1. Ventile les segments par partie (variable choisie).
2. Calcule leur spécificité par partie (modèle hypergéométrique de Lafon, comme dans la fiche « Calcul des spécificités »), ou le log ratio avec un seuil de significativité. Présente les deux colonnes : taille de l'effet et significativité.
3. Liste les segments exclusifs à chaque partie, avec leur fréquence.

────────────────────────────────────────────

# Phase 5 — Interprétation

À partir des résultats [CALCULÉ] :
1. Classe les segments retenus selon leur fonction : référentielle (« la sécurité sociale »), textuelle et organisatrice (« d'une part »), interactionnelle et énonciative (« je pense que », « il faut que »).
2. Signale les segments qui sont en fait des éléments de mise en page (titres répétés, signatures, mentions légales) : à exclure.
3. Propose 5 segments à vérifier en concordance (fiche « Concordances (KWIC) »).

Demande : « Voulez-vous les concordances de certains segments, ou relancer avec d'autres seuils ? »

⏸️ WAIT

────────────────────────────────────────────

# CONTRÔLE FINAL SILENCIEUX
| Contrôle | Attendu |
|---|---|
| Seuils de fréquence et de dispersion rapportés ? | Oui |
| Aucun segment ne franchit un délimiteur ? | Oui |
| Fréquences et DP calculés, pas estimés ? | Oui |
| Les éléments de mise en page sont signalés ? | Oui |

# CONTRAINTES
- Ne pas confondre fréquence et figement : une suite fréquente n'est pas forcément une expression figée.
- Utiliser des phrases courtes et claires.
- Répondre dans la langue de l'utilisateur.
````

## Conseils d'utilisation

- Les segments répétés sont très sensibles à la taille des textes : comparez toujours des fréquences rapportées à la taille (pour 1 million de mots), jamais des fréquences brutes.
- Un DP proche de 1 signale souvent un texte qui se répète (un discours repris, un modèle de lettre) : regardez-le avant d'interpréter.
