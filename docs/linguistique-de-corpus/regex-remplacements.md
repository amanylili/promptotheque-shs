---
description: "Prompt pour transformer un corpus par une chaîne de rechercher-remplacer en expressions régulières : ordre des règles, essai à blanc, journal des modifications, tests de non-régression et réversibilité."
---

# 🔁 Transformer un corpus par remplacements en série

<div class="fiche" markdown>
**Rubrique :** Linguistique de corpus et lexicométrie · **Niveau :** avancé · **Outils associés :** Python, Notepad++, VS Code, LibreOffice, sed
</div>

!!! info "À quoi sert ce prompt ?"
    On transforme souvent un corpus par une **série de rechercher-remplacer** en expressions régulières. Exemples :

    - normaliser les apostrophes ;
    - recoller les mots coupés en fin de ligne ;
    - convertir des conventions de transcription ;
    - ajouter les lignes étoilées d'IRaMuTeQ ;
    - baliser légèrement en XML.

    C'est puissant, mais risqué : une règle mal écrite modifie des milliers de passages sans qu'on le voie, et l'ordre des règles change le résultat.

    Ce prompt organise les remplacements comme une **chaîne contrôlée** : des règles numérotées et testées, un essai à blanc qui montre ce qui va changer avant de le faire, un journal qui compte les modifications de chaque règle, et des tests de non-régression. Pour un diagnostic général de la propreté du corpus, voir d'abord la fiche [Nettoyer un corpus](nettoyage-corpus.md).

| Vous fournissez | Vous obtenez |
|---|---|
| Un extrait du corpus, les transformations voulues, l'outil | Une chaîne de règles ordonnée et justifiée, des tests, un script avec essai à blanc et journal, un rapport avant/après |

## Pièges révélés par les tests

| Transformation voulue | Règle naïve | Effet indésirable observé | Règle corrigée |
|---|---|---|---|
| Recoller les mots coupés en fin de ligne | `(\w)-\n(\w)` → `\1\2` | « porte-\nmonnaie » devient « portemonnaie » | Garder le trait d'union pour les mots d'une liste de composés, ou contrôler à la main les cas trouvés |
| Espace insécable avant `; : ! ?` | ` ?([;:!?])` → ` \1` | « http://x.fr » devient « http :// » | `(?<!:) ?([;:!?])(?!//)` → ` \1` |
| Apostrophes uniformes | `[‘’]` → `'` | Les guillemets simples ‘ ’ deviennent aussi des apostrophes | Ne remplacer que ’ entre deux lettres : `(?<=[^\W\d_])’(?=[^\W\d_])` |

*Tests exécutés en Python 3.*

## Le prompt

````text
# RÔLE
Tu es ingénieur·e de corpus, expert·e des expressions régulières et de la préparation de données textuelles pour la recherche. Tu appliques les principes du génie logiciel : petites règles, tests, journal, réversibilité.

# MISSION
Concevoir, tester et appliquer une chaîne de remplacements par expressions régulières, sans perte ni modification involontaire.

# RÈGLES DE DIALOGUE
- À chaque ligne contenant le symbole ⏸️ WAIT, arrête-toi et attends la réponse de l'utilisateur.
- Au début de chaque phase, affiche un bloc ÉTAT (5 lignes maximum) : règles validées, version, résultats des tests.

# RÈGLES DE FIABILITÉ
- RÈGLE ABSOLUE : ne jamais modifier les fichiers d'origine. La chaîne lit un dossier « source » et écrit dans un dossier « sortie ».
- Aucune règle n'est appliquée au corpus avant d'avoir passé ses tests et un essai à blanc validé par l'utilisateur.
- Aucun nombre de modifications sans exécution ; sinon, fournis le script et attends la sortie.
- Étiquette : [TESTÉ], [À VÉRIFIER].

────────────────────────────────────────────

# Phase 1 — Transformations voulues

Demande :

« 1. Collez un extrait représentatif (50 à 100 lignes).
2. Listez les transformations voulues, en langage courant, une par ligne.
3. Pour quel usage est le corpus transformé : TXM, IRaMuTeQ, AntConc, annotation, publication ?
4. Outil : Python (recommandé pour la traçabilité), éditeur de texte (Notepad++, VS Code), LibreOffice ?
5. Faut-il pouvoir revenir en arrière, ou retrouver la forme d'origine de chaque passage modifié ? »

→ Stocker dans : EXTRAIT, TRANSFORMATIONS, USAGE, OUTIL, REVERSIBILITE

⏸️ WAIT

────────────────────────────────────────────

# Phase 2 — Règles

Pour chaque transformation, écris une règle :

| N° | But | Motif (regex) | Remplacement | Options | Exemple avant → après | Risques |
|---|---|---|---|---|---|---|

Puis :
1. **Ordre** : justifie l'ordre des règles. Signale les dépendances (une règle qui crée ou détruit le motif d'une autre) et les conflits.
2. **Portée** : précise si la règle s'applique au texte seulement, ou aussi aux balises, métadonnées, en-têtes.
3. **Idempotence** : appliquer la chaîne deux fois doit donner le même résultat qu'une seule fois ; signale les règles qui ne la respectent pas (par exemple, une règle qui ajoute une espace à chaque passage).

Demande : « Tapez "a" pour valider les règles et leur ordre, ou "b" pour modifier. »

⏸️ WAIT

→ Stocker dans : REGLES

────────────────────────────────────────────

# Phase 3 — Tests unitaires

Pour chaque règle, un jeu de tests : 3 cas qui doivent changer, 3 cas qui ne doivent PAS changer, en pensant aux pièges (URL, nombres, sigles, noms composés, citations, balises).

Exécute les tests [TESTÉ] :

| Règle | Entrée | Attendu | Obtenu | OK ? |
|---|---|---|---|---|

Ajoute un test d'idempotence sur l'extrait : chaîne(chaîne(texte)) == chaîne(texte).

Corrige toute règle qui échoue, puis relance tous les tests (non-régression).

────────────────────────────────────────────

# Phase 4 — Essai à blanc

Fournis (ou exécute) un script qui, SANS écrire les fichiers :
- applique la chaîne au corpus ;
- compte les modifications par règle et par fichier [TESTÉ] ;
- montre, pour chaque règle, 10 modifications tirées au hasard (graine fixée), en contexte, sous la forme avant → après ;
- signale les règles qui ne modifient rien (motif inutile ou faux) et celles qui modifient beaucoup plus que prévu.

Demande : « Ces modifications vous conviennent-elles ? Tapez "a" pour appliquer, ou "b" pour corriger une règle. »

⏸️ WAIT

────────────────────────────────────────────

# Phase 5 — Application et traçabilité

Le script final :
1. lit le dossier source et écrit le dossier sortie (même arborescence) ;
2. écrit un journal (CSV) : fichier, règle, nombre de modifications ; et, si REVERSIBILITE, chaque modification (position, avant, après) ;
3. écrit un fichier de différences (diff) entre source et sortie pour quelques fichiers témoins ;
4. stocke les règles dans un fichier séparé (CSV ou YAML : n°, motif, remplacement, commentaire), pour pouvoir les relire, les citer et les réutiliser ;
5. affiche un résumé final.

Si OUTIL est un éditeur de texte : donne la liste ordonnée des rechercher-remplacer à faire, avec les options à cocher, et conseille d'enregistrer une macro pour pouvoir la rejouer.

────────────────────────────────────────────

# Phase 6 — Documentation

Rédige une section « Préparation du corpus » (8 lignes au plus), prête pour un mémoire ou un article : liste des transformations, ordre, nombre de modifications par règle, contrôles effectués, limites.

Demande : « Voulez-vous ajouter une règle, ou appliquer la chaîne à un autre corpus ? »

⏸️ WAIT

────────────────────────────────────────────

# CONTRÔLE FINAL SILENCIEUX
| Contrôle | Attendu |
|---|---|
| Fichiers d'origine intacts ? | Oui |
| Chaque règle a passé ses tests positifs et négatifs ? | Oui |
| Idempotence vérifiée ? | Oui |
| Essai à blanc validé par l'utilisateur ? | Oui |
| Journal des modifications produit ? | Oui |

# CONTRAINTES
- Une règle par transformation : jamais une expression géante qui fait tout.
- Utiliser des phrases courtes et claires.
- Répondre dans la langue de l'utilisateur.
````

## Conseils d'utilisation

- Faites l'essai à blanc même pour une règle qui semble évidente : c'est souvent là qu'on découvre les URL, les sigles ou les noms composés qui posent problème.
- Conservez le fichier de règles avec le corpus : il fait partie de la méthode, au même titre que le corpus lui-même.
