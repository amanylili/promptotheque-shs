---
description: "Bien utiliser les prompts d'IA en sciences du langage : fonctionnement par phases, calculs vérifiables, RGPD, pseudonymisation et citation de l'IA."
---

# Bien utiliser les prompts

## Le fonctionnement par phases

Les prompts sont découpés en **phases**. Le symbole ⏸️ WAIT indique que l'assistant doit s'arrêter et attendre votre réponse. Les réponses sont stockées dans des **variables** en majuscules (`CORPUS`, `PARTITION`, `SEUIL`…), réutilisées dans les phases suivantes.

Si l'assistant enchaîne plusieurs phases sans attendre, rappelez-lui simplement : « Respecte les ⏸️ WAIT. »

## Les calculs : jamais « de tête »

Un modèle de langue **ne sait pas compter de façon fiable**. Il ne peut pas non plus faire un calcul hypergéométrique ou une AFC sans exécuter de code. Les prompts de lexicométrie imposent donc deux modes :

| Situation | Ce que fait l'assistant |
|---|---|
| L'assistant peut exécuter du code (analyse de fichiers, Code Interpreter, environnement Python…) | Il exécute le script sur vos fichiers et affiche les résultats réels. |
| L'assistant ne peut pas exécuter de code | Il fournit un script R ou Python commenté, que vous lancez vous-même. Il interprète ensuite les résultats que vous lui collez. |

Dans tous les cas, **vérifiez** les résultats avec un logiciel de référence (TXM, IRaMuTeQ) avant publication.

## Données personnelles et éthique de la recherche

!!! danger "Avant de coller un corpus dans un assistant en ligne"
    Les entretiens, les copies d'apprenants, les messages de forums ou les échanges de messagerie contiennent souvent des **données personnelles** au sens du RGPD.

    - Vérifiez ce que prévoient votre formulaire de consentement et votre plan de gestion des données.
    - Demandez l'avis du **délégué à la protection des données (DPO)** de votre établissement.
    - Privilégiez un **modèle installé localement** (Ollama, LM Studio…) ou une instance validée par votre établissement.
    - Si vous utilisez un service en ligne, désactivez l'usage de vos données pour l'entraînement quand c'est possible.

La **pseudonymisation** ne rend pas les données anonymes au sens du RGPD. Tant qu'une table de correspondance existe, les données restent personnelles. Conservez cette table à part, chiffrée.

## Citer l'usage de l'IA

Dans un mémoire, une thèse ou un article, indiquez dans la partie méthodologique :

- l'outil et le modèle utilisés (nom, version, date) ;
- les tâches confiées à l'IA (nettoyage, aide à l'interprétation…) ;
- les vérifications humaines réalisées.

Les prompts de cette promptothèque proposent, en fin de parcours, un paragraphe méthodologique prêt à adapter.

## Adapter un prompt

Les prompts sont sous licence libre. Vous pouvez les modifier : changer les seuils par défaut, ajouter vos conventions de transcription, traduire… Pensez à [proposer vos améliorations](contribuer.md).
