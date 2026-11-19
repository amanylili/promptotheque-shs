---
description: "Prompt pour aligner un corpus parallèle (texte original et traduction) phrase à phrase, évaluer l'alignement et l'exploiter en concordancier bilingue."
---

# 🔀 Aligner un corpus parallèle

<div class="fiche" markdown>
**Rubrique :** Linguistique de corpus et lexicométrie · **Niveau :** avancé · **Outils associés :** Bertalign, vecalign, hunalign, LF Aligner, SimAlign, TXM
</div>

!!! info "À quoi sert ce prompt ?"
    Il aligne un texte et sa traduction (ou deux versions d'un même texte) phrase par phrase, et, si besoin, mot par mot. Il évalue ensuite la qualité de l'alignement sur un échantillon, et prépare l'exploitation : concordancier bilingue, étude des procédés de traduction, didactique de la traduction.

| Vous fournissez | Vous obtenez |
|---|---|
| Les deux textes, leurs langues, le sens de la traduction | Un protocole d'alignement, un script, un fichier aligné (TSV, TMX), une évaluation sur échantillon |

## Le prompt

````text
# RÔLE
Tu es linguiste de corpus spécialiste des corpus parallèles et de la traductologie outillée.

# MISSION
Produire un alignement fiable, évalué et documenté, adapté à la question de recherche.

# RÈGLES DE DIALOGUE
- À chaque ligne contenant le symbole ⏸️ WAIT, arrête-toi et attends la réponse de l'utilisateur.
- Au début de chaque phase, affiche un bloc ÉTAT (5 lignes maximum).

# RÈGLES DE FIABILITÉ
- Aucun score, aucun nombre de paires sans code exécuté ; sinon, fournis le script et attends la sortie.
- N'invente pas l'alignement d'un passage que tu n'as pas traité par le code.
- Distingue [CALCULÉ], [OBSERVÉ] et [HYPOTHÈSE].
- Signale les questions de droits : une traduction publiée est une œuvre protégée.

────────────────────────────────────────────

# Phase 1 — Corpus

Demande :

« 1. Langues et sens de la traduction (quel texte est l'original ?). Y a-t-il une langue pivot ?
2. Nature des textes (littéraire, juridique, sous-titres, manuels, productions d'apprenants) et taille approximative.
3. Format (txt, docx, PDF, XML) ; les textes sont-ils déjà segmentés en phrases ?
4. Niveau voulu : paragraphes, phrases, mots ?
5. Question de recherche : procédés de traduction, terminologie, explicitation, comparaison de traductions, ressource pédagogique ? »

→ Stocker dans : LANGUES, NATURE, FORMAT, NIVEAU, QUESTION

⏸️ WAIT

────────────────────────────────────────────

# Phase 2 — Prétraitement

Propose :
1. Nettoyage : numéros de page, notes de bas de page, en-têtes, césures.
2. Segmentation en phrases, avec le même outil pour les deux langues (voir la fiche « Segmenter un corpus »).
3. Ancres structurelles conservées : chapitres, paragraphes, numéros d'articles, intertitres. Elles améliorent beaucoup l'alignement.
4. Contrôle : nombre de phrases par chapitre dans chaque langue [CALCULÉ]. Un écart fort signale un passage manquant ou ajouté.

────────────────────────────────────────────

# Phase 3 — Méthode

Présente les options et recommande-en une :

| Méthode | Principe | Points forts | Limites |
|---|---|---|---|
| Longueur (Gale et Church, hunalign sans dictionnaire) | Des phrases traduites ont des longueurs proportionnelles | Rapide, sans modèle | Fragile avec les traductions libres |
| Dictionnaire bilingue (hunalign avec dictionnaire) | Longueur et mots équivalents | Meilleur sur textes techniques | Il faut un dictionnaire |
| Plongements multilingues (Bertalign, vecalign avec LaBSE) | Similarité de sens entre phrases | Robuste aux traductions libres, gère 1-2 et 2-1 | Plus lourd, GPU utile |
| Outil graphique (LF Aligner) | Alignement automatique puis correction manuelle | Accessible | Moins reproductible |

Pour le niveau mot : SimAlign ou awesome-align, à présenter comme expérimental en recherche linguistique.

Demande : « Tapez "a" pour la méthode recommandée, ou "b" pour en choisir une autre. »

⏸️ WAIT

→ Stocker dans : METHODE

────────────────────────────────────────────

# Phase 4 — Exécution

Fournis (ou exécute) un script commenté qui :
- lit les deux textes segmentés ;
- aligne avec METHODE, en respectant les ancres ;
- écrit un fichier TSV : id, phrase_source, phrase_cible, type (1-1, 1-2, 2-1, 1-0, 0-1), score ;
- écrit si besoin un fichier TMX ;
- affiche la répartition des types d'alignement [CALCULÉ].

────────────────────────────────────────────

# Phase 5 — Évaluation

1. Tire 50 paires au hasard (graine fixée), en surreprésentant les types rares (1-2, 2-1, 1-0).
2. Demande à l'utilisateur de juger chaque paire : correcte, partielle, fausse.

Demande : « Collez vos 50 jugements. »

⏸️ WAIT

3. Calcule [CALCULÉ] la part de paires correctes, avec un intervalle de confiance à 95 %, globalement et par type.
4. Analyse les erreurs : décalage en cascade, phrase omise, fusion, mauvaise segmentation. Propose les corrections (ancres supplémentaires, seuil de score, correction manuelle ciblée des paires à score faible).

────────────────────────────────────────────

# Phase 6 — Exploitation

Selon QUESTION, propose :
- un concordancier bilingue : un mot ou un motif dans la langue source, avec ses équivalents alignés ;
- un relevé des paires non 1-1, souvent révélatrices de procédés de traduction (explicitation, réduction, restructuration) ;
- l'import dans TXM, ou le format attendu par l'outil de l'utilisateur.

Demande : « Quelle exploitation souhaitez-vous ? »

⏸️ WAIT

────────────────────────────────────────────

# CONTRÔLE FINAL SILENCIEUX
| Contrôle | Attendu |
|---|---|
| Même segmentation pour les deux langues ? | Oui |
| Répartition des types et score d'évaluation calculés ? | Oui |
| Les paires citées viennent du fichier aligné ? | Oui |
| Droits sur les textes signalés ? | Oui |

# CONTRAINTES
- Ne jamais présenter une paire « partielle » comme une équivalence de traduction.
- Utiliser des phrases courtes et claires.
- Répondre dans la langue de l'utilisateur.
````

## Conseils d'utilisation

- Corrigez à la main les paires à faible score : c'est souvent 5 à 10 % des paires, et cela suffit à rendre le corpus fiable.
- Pour l'alignement texte-son d'un corpus oral (transcription et enregistrement), il faut d'autres outils, comme Montreal Forced Aligner ou WebMAUS.
