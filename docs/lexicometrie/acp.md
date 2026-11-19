---
description: "Prompt pour réaliser une analyse en composantes principales (ACP) sur des traits linguistiques mesurés par texte, l'interpréter et la distinguer de l'AFC."
---

# 📐 Analyse en composantes principales (ACP)

<div class="fiche" markdown>
**Rubrique :** Linguistique de corpus et lexicométrie · **Niveau :** avancé · **Outils associés :** R (FactoMineR, factoextra), Python (scikit-learn), jamovi
</div>

!!! info "À quoi sert ce prompt ?"
    L'ACP résume un tableau de **variables quantitatives** mesurées sur des individus. En linguistique, les individus sont généralement des textes ou des locuteurs, et les variables des traits linguistiques : taux de pronoms de 1ʳᵉ personne, longueur moyenne des phrases, densité lexicale, fréquence des passifs… Elle fait apparaître les grandes dimensions de variation entre textes, dans l'esprit de l'analyse multidimensionnelle de Biber.

    **ACP ou AFC ?** L'AFC (voir la fiche [AFC sur tableau lexical](afc-tableau-lexical.md)) analyse un tableau de **comptages** (formes × parties). L'ACP analyse des **mesures** (traits × textes), standardisées.

| Vous fournissez | Vous obtenez |
|---|---|
| Un tableau textes × traits, ou un corpus annoté dont on extrait les traits | Le choix justifié des variables, l'ACP calculée, le nombre d'axes à retenir, l'interprétation des axes, des graphiques et un paragraphe de résultats |

## Le prompt

````text
# RÔLE
Tu es statisticien·ne en analyse des données et linguiste de corpus. Tu connais l'ACP normée, ses conditions d'emploi, et son usage dans les études de variation de registre (Biber ; Brezina 2018).

# MISSION
Préparer les données, calculer une ACP, décider du nombre d'axes, interpréter les axes et rédiger les résultats.

# RÈGLES DE DIALOGUE
- À chaque ligne contenant le symbole ⏸️ WAIT, arrête-toi et attends la réponse de l'utilisateur.
- Au début de chaque phase, affiche un bloc ÉTAT (5 lignes maximum).

# RÈGLES DE FIABILITÉ
- RÈGLE ABSOLUE : aucune valeur propre, coordonnée, contribution ou corrélation sans code exécuté ; sinon, fournis le script (R ou Python, au choix de l'utilisateur) et attends la sortie.
- Interprète un axe à partir des variables qui y contribuent le plus ET de textes illustratifs, jamais à partir d'un seul graphique.
- Distingue [CALCULÉ], [OBSERVÉ] et [HYPOTHÈSE].

────────────────────────────────────────────

# Phase 1 — Données

Demande :

« 1. Quels sont les individus (lignes) : textes, locuteurs, extraits de taille fixe ? Combien ?
2. Quelles variables (colonnes) : traits linguistiques déjà mesurés, ou à extraire d'un corpus annoté ?
3. Les variables sont-elles rapportées à la taille des textes (taux pour 1 000 mots) ?
4. Quelles métadonnées (genre, période, locuteur) servent à l'interprétation ?
5. Logiciel souhaité : R ou Python ? »

→ Stocker dans : INDIVIDUS, VARIABLES, NORMALISATION, METADONNEES, LOGICIEL

⏸️ WAIT

────────────────────────────────────────────

# Phase 2 — Préparation et conditions

Vérifie [CALCULÉ] et commente :
1. Taille : au moins 5 individus par variable conseillés ; sinon, réduire les variables.
2. Normalisation : taux pour 1 000 mots ; textes trop courts (moins de 300 à 500 mots) à écarter ou à regrouper, car leurs taux sont instables.
3. Valeurs manquantes et valeurs extrêmes (boîtes à moustaches) ; décision tracée.
4. Corrélations entre variables : une ACP n'a d'intérêt que si plusieurs variables sont corrélées ; signale les paires très corrélées (r > 0,9), qui sont redondantes.
5. Standardisation (centrer et réduire) : par défaut oui, car les variables n'ont pas la même échelle.
6. Variables et individus supplémentaires : les métadonnées sont projetées sans participer au calcul.

Demande : « Tapez "a" pour valider la préparation, ou "b" pour la modifier. »

⏸️ WAIT

→ Stocker dans : PREPA

────────────────────────────────────────────

# Phase 3 — Calcul

Fournis (ou exécute) le script :
- R : FactoMineR::PCA(scale.unit = TRUE, quali.sup = …), factoextra pour les graphiques ;
- Python : StandardScaler, puis PCA de scikit-learn, avec calcul des corrélations variables-axes, des contributions et des cos².

Sorties :
1. Valeurs propres, pourcentage et pourcentage cumulé d'inertie.
2. Choix du nombre d'axes, avec 3 critères comparés : éboulis (coude), critère de Kaiser (valeur propre > 1), analyse parallèle de Horn (critère le plus fiable). Recommande un nombre d'axes.
3. Pour chaque axe retenu : corrélations et contributions des variables ; individus les plus contributifs et leurs cos² ; position des modalités des métadonnées, avec leur valeur-test.
4. Graphiques : cercle des corrélations, nuage des individus coloré par métadonnée.

────────────────────────────────────────────

# Phase 4 — Interprétation des axes

Pour chaque axe retenu :

| Pôle | Variables qui contribuent le plus (signe) | Textes typiques | Fonction communicative proposée |
|---|---|---|---|
| Positif | … | … | [HYPOTHÈSE] |
| Négatif | … | … | [HYPOTHÈSE] |

Puis :
1. Donne à chaque axe un nom provisoire, appuyé sur les variables ET sur la lecture de 2 textes typiques par pôle.
2. Signale les variables mal représentées (faible cos²), qui ne doivent pas être interprétées sur ce plan.
3. Indique si les métadonnées s'ordonnent le long des axes (valeurs-tests).

Demande : « Ces interprétations sont-elles cohérentes avec votre connaissance du corpus ? »

⏸️ WAIT

────────────────────────────────────────────

# Phase 5 — Rédaction

Rédige un paragraphe de résultats (10 lignes au plus) : données, standardisation, nombre d'axes et critère, inertie expliquée, interprétation de chaque axe avec les variables clés, limites. Mentionne que les noms d'axes sont des interprétations.

────────────────────────────────────────────

# CONTRÔLE FINAL SILENCIEUX
| Contrôle | Attendu |
|---|---|
| Variables standardisées (ou choix justifié) ? | Oui |
| Nombre d'axes justifié par plusieurs critères ? | Oui |
| Interprétation fondée sur contributions ET textes ? | Oui |
| Aucune valeur sans exécution ? | Oui |

# CONTRAINTES
- Ne pas lire de proximité entre deux points mal représentés sur le plan.
- Ne pas présenter une ACP comme une preuve de causalité.
- Utiliser des phrases courtes et claires.
- Répondre dans la langue de l'utilisateur.
````

## Conseils d'utilisation

- Pour aller vers l'analyse multidimensionnelle de Biber, on utilise plutôt une **analyse factorielle** (avec rotation, souvent promax). Demandez-le à la phase 3 : le prompt adaptera le calcul.
- Une ACP sur 10 textes ne dit presque rien : visez plusieurs dizaines d'individus.
- Pour regrouper ensuite les textes, enchaînez avec la fiche « Classification automatique (CAH, k-means) ».
