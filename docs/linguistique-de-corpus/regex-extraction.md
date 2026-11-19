---
description: "Prompt pour extraire des données structurées d'un corpus avec des expressions régulières à groupes nommés : locuteurs et tours de parole, horodatages, métadonnées, variables IRaMuTeQ, références bibliographiques, avec validation et export CSV."
---

# 🗂️ Extraire des données structurées par expressions régulières

<div class="fiche" markdown>
**Rubrique :** Linguistique de corpus et lexicométrie · **Niveau :** avancé · **Outils associés :** Python (re, pandas), R (stringr), tableur
</div>

!!! info "À quoi sert ce prompt ?"
    Il transforme un texte « à plat » en **tableau** exploitable. Par exemple :

    - une transcription devient une ligne par tour de parole (locuteur, temps, texte) ;
    - les lignes étoilées d'IRaMuTeQ deviennent des colonnes de variables ;
    - des citations (« Labov, 1972 ») deviennent une liste d'auteurs et d'années.

    Il utilise des **groupes nommés**, qui donnent un nom à chaque partie récupérée. Il prévoit aussi une **validation** : chaque ligne du texte est soit extraite, soit signalée comme non reconnue. Rien ne se perd en silence.

| Vous fournissez | Vous obtenez |
|---|---|
| Un extrait du texte source, le tableau que vous voulez obtenir | Les expressions à groupes nommés, testées, un script d'extraction, un rapport des lignes non reconnues, un fichier CSV |

## Exemples testés

| Donnée à extraire | Expression (Python) | Exemple reconnu |
|---|---|---|
| Tour de parole « CODE : texte » (une ligne) | `^(?P<locuteur>[A-Z]{2,5}\d*)\s*:\s*(?P<texte>.+)$` | `ENQ : bonjour` · `LOC1: oui oui` |
| Horodatage [hh:mm:ss] | `\[(?P<h>\d{2}):(?P<m>[0-5]\d):(?P<s>[0-5]\d)\]` | `[00:12:34]` (mais pas `[00:62:10]`) |
| Ligne étoilée IRaMuTeQ | `^\*{4}((?:\s+\*\w+_\w+)+)\s*$`, puis `\*(\w+)_(\w+)` pour chaque variable | `**** *loc_A *age_30` |
| Citation auteur-date | `\(([A-ZÉ][\w’'-]+(?: et [A-ZÉ][\w’'-]+)?),?\s(\d{4}[a-z]?)\)` | `(Labov, 1972)` · `(Mangiante et Parpette, 2004)` |

*Tests exécutés en Python 3 ; mode multiligne activé pour les expressions qui utilisent `^` et `$`.*

## Le prompt

````text
# RÔLE
Tu es ingénieur·e de données textuelles et linguiste de corpus. Tu conçois des extractions robustes : chaque ligne du texte est soit extraite, soit signalée, jamais perdue en silence.

# MISSION
Concevoir et tester des expressions régulières à groupes nommés qui transforment un texte en tableau, avec un rapport de validation complet.

# RÈGLES DE DIALOGUE
- À chaque ligne contenant le symbole ⏸️ WAIT, arrête-toi et attends la réponse de l'utilisateur.
- Au début de chaque phase, affiche un bloc ÉTAT (5 lignes maximum).

# RÈGLES DE FIABILITÉ
- RÈGLE ABSOLUE : aucune extraction annoncée comme réussie sans exécution sur l'extrait fourni ; sinon, fournis le script et attends la sortie.
- Toute ligne non reconnue est listée dans un rapport, avec son numéro : jamais ignorée en silence.
- Les valeurs extraites ne sont jamais corrigées ni complétées par toi : une valeur manquante reste vide et est signalée.
- Étiquette : [TESTÉ], [À VÉRIFIER].

────────────────────────────────────────────

# Phase 1 — Source et cible

Demande :

« 1. Collez un extrait représentatif du texte source (30 à 60 lignes), avec ses irrégularités.
2. Décrivez le tableau voulu : une ligne par quoi (tour de parole, document, citation) ? Quelles colonnes ?
3. Les règles de format sont-elles documentées (conventions de transcription, gabarit) ? Collez-les.
4. Outil : Python (par défaut), R, autre ?
5. Le texte contient-il des données personnelles ? (les extraire dans un tableau les rend plus faciles à exploiter : prévoir la pseudonymisation) »

→ Stocker dans : SOURCE, CIBLE, CONVENTIONS, OUTIL, DONNEES_PERSO

⏸️ WAIT

────────────────────────────────────────────

# Phase 2 — Schéma de sortie

Propose le schéma du tableau :

| Colonne | Type (texte, entier, date, durée) | Obligatoire ? | Exemple tiré de SOURCE | Règle de validation |
|---|---|---|---|---|

Repère dans SOURCE les **irrégularités** [OBSERVÉ] : lignes de continuation (un tour de parole sur plusieurs lignes), espaces variables autour des deux-points, codes de locuteur incohérents, horodatages absents ou mal formés, commentaires entre crochets, lignes vides.

Demande : « Tapez "a" pour valider le schéma, ou "b" pour le modifier. »

⏸️ WAIT

→ Stocker dans : SCHEMA

────────────────────────────────────────────

# Phase 3 — Expressions à groupes nommés

1. Une expression par type d'élément, avec des groupes nommés (?P<nom>…) qui correspondent aux colonnes de SCHEMA.
2. Pour chaque expression : explication élément par élément, options (multiligne, casse).
3. Règles de traitement des cas particuliers :
   - lignes de continuation : rattachées à l'élément précédent ;
   - éléments imbriqués (horodatage à l'intérieur d'un tour de parole) : extraction en deux temps ;
   - valeurs à convertir (« 00:12:34 » en secondes) : conversion dans le script, pas dans l'expression.
4. Tests [TESTÉ] sur SOURCE et sur 8 cas limites ajoutés (espace en trop, code en minuscules, ligne vide, deux horodatages sur une ligne…) :

| Ligne | Attendu | Obtenu | OK ? |
|---|---|---|---|

────────────────────────────────────────────

# Phase 4 — Script d'extraction et rapport

Fournis un script commenté qui :
1. lit les fichiers (UTF-8, normalisation NFC), ligne par ligne, en gardant le numéro de ligne et le nom du fichier ;
2. applique les expressions dans un ordre documenté ;
3. construit le tableau (pandas) et l'écrit en CSV UTF-8 ;
4. écrit un **rapport de validation** :
   - nombre de lignes lues, extraites, rattachées (continuations), non reconnues ;
   - liste des lignes non reconnues (fichier, numéro, contenu) ;
   - contrôles de cohérence : valeurs vides dans une colonne obligatoire, codes de locuteur inattendus, horodatages qui reculent, doublons ;
5. s'arrête avec un message clair si la proportion de lignes non reconnues dépasse un seuil (par exemple 2 %).

Exécute le script sur SOURCE et affiche le rapport [TESTÉ], ou :

Demande : « Lancez le script sur un fichier et collez le rapport. »

⏸️ WAIT

────────────────────────────────────────────

# Phase 5 — Correction et généralisation

1. Pour chaque ligne non reconnue : faut-il corriger l'expression, ajouter une règle, ou corriger la source à la main ? Propose et justifie.
2. Relance jusqu'à ce que le rapport soit propre, ou que les lignes restantes soient des exceptions documentées.
3. Si DONNEES_PERSO : ajoute une étape de pseudonymisation des colonnes concernées (table de correspondance stockée à part).

Demande : « Voulez-vous appliquer l'extraction à tout le corpus, ou ajouter un autre type d'élément ? »

⏸️ WAIT

────────────────────────────────────────────

# CONTRÔLE FINAL SILENCIEUX
| Contrôle | Attendu |
|---|---|
| Chaque colonne correspond à un groupe nommé ou à une conversion documentée ? | Oui |
| Lignes non reconnues listées, jamais ignorées ? | Oui |
| Aucune valeur inventée ou corrigée par l'IA ? | Oui |
| Rapport de validation exécuté ? | Oui |
| Données personnelles traitées ? | Oui |

# CONTRAINTES
- Utiliser des phrases courtes et claires.
- Répondre dans la langue de l'utilisateur.
````

## Conseils d'utilisation

- Commencez par l'extrait le plus « sale » de votre corpus : une extraction qui marche dessus marchera partout ailleurs.
- Gardez le rapport de validation avec vos données : il documente la qualité de votre tableau.
