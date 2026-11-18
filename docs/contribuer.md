---
description: "Contribuer à la promptothèque SHS : proposer un nouveau prompt de linguistique, de lexicométrie ou de didactique du FLE."
---

# Contribuer

Cette promptothèque est collaborative. Toute amélioration est bienvenue : nouveau prompt, correction, retour d'usage.

## Proposer un nouveau prompt

1. Créez un fichier `.md` dans le dossier de la rubrique concernée (`docs/lexicometrie/`, etc.).
2. Reprenez le modèle ci-dessous.
3. Ajoutez la page dans la section `nav` du fichier `mkdocs.yml`.
4. Ouvrez une *pull request*, ou envoyez le texte via une *issue*.

## Modèle de fiche

````markdown
# Titre du prompt

<div class="fiche" markdown>
**Rubrique :** … · **Niveau :** débutant / intermédiaire / avancé · **Outils associés :** …
</div>

!!! info "À quoi sert ce prompt ?"
    Deux ou trois phrases.

| Vous fournissez | Vous obtenez |
|---|---|
| … | … |

## Le prompt

```text
Tu es …

À chaque ligne contenant le symbole ⏸️ WAIT, arrête-toi et attends la réponse de l'utilisateur.

────────────────────────────────────────────

# Phase 1 — …

…

⏸️ WAIT

────────────────────────────────────────────

# Contraintes

- …
```

## Conseils d'utilisation

- …
````

## Règles de rédaction

- Un prompt = une tâche de recherche identifiable.
- Phases courtes, avec des variables en MAJUSCULES.
- Des choix proposés sous forme de tableau, avec une option par défaut.
- Aucun calcul « de tête » : exécution de code ou script fourni.
- Une section **Contraintes** qui rappelle l'éthique et les limites.
- Des phrases courtes et claires.

## Signaler un problème

Ouvrez une *issue* sur le dépôt GitHub. Décrivez le prompt, l'assistant utilisé et le comportement observé.
