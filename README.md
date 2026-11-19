# Promptothèque en sciences humaines et sociales

Des prompts prêts à l'emploi pour les **sciences du langage**, à utiliser avec un assistant d'IA générative.

🌐 **Site :** https://amanylili.github.io/promptotheque-shs/

## Rubriques

| Rubrique | Prompts |
|---|---|
| 📚 Linguistique de corpus et lexicométrie | Constituer · Nettoyer · Segmenter · Pseudonymiser · Annoter · Concordances (KWIC) · Concordancier · Requêtes CQL/SQL · Fréquences · Spécificités (Lafon) · Segments répétés · Motifs · Cooccurrences · AFC · ACP · Classification (Reinert, CAH, k-means) · Statistiques (Brezina) · Entités nommées · Alignement · Apprentissage profond |
| 🗣️ Sociolinguistique | Variable sociolinguistique · Entretien (Labov) · Enquête rapide et anonyme · Analyse quantitative de la variation · Récit oral (Labov et Waletzky) · Insécurité linguistique · Guide d'entretien · Transcription de l'oral · Alternance codique |
| 🎓 Didactique du FLE | Séquence sur corpus · Adapter un texte au CECRL · Corriger une production écrite |
| 🔎 Recherche documentaire | Recherche Google avancée |
| 🧠 Visualisation | Carte mentale (Mermaid) |

Chaque prompt est **interactif** : l'assistant avance par phases et s'arrête à chaque ⏸️ WAIT pour attendre votre réponse.

## Principes

- **Aucun chiffre inventé** : les calculs passent par du code exécuté ou par un script fourni.
- **Le chercheur décide** de chaque choix méthodologique.
- **Données protégées** : RGPD, pseudonymisation, avis du DPO.

## Structure du dépôt

```
docs/                       contenu du site (Markdown)
  linguistique-de-corpus/
  lexicometrie/
  sociolinguistique/
  didactique-fle/
  recherche-documentaire/
  visualisation/
mkdocs.yml                  configuration du site
.github/workflows/deploy.yml  publication automatique sur GitHub Pages
```

## Prévisualiser en local

```bash
pip install mkdocs-material
mkdocs serve
```

Puis ouvrez http://127.0.0.1:8000

## Contribuer

Voir [docs/contribuer.md](docs/contribuer.md).

## Licence

Contenus sous licence [Creative Commons BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.fr) : réutilisation et modification libres, avec mention de la source.

Inspiré de la démarche de la promptothèque du ministère de la Transition écologique.
