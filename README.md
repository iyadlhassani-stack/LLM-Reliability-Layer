# LLM Reliability Layer

En utilisant ChatGPT au quotidien, j'ai remarqué un truc qui m'a intrigué :
le modèle invente des réponses à des questions qui n'ont pas de réponse.
Ce projet est né de ça — je voulais mesurer ce comportement de façon rigoureuse,
sans RAG, juste le modèle face à lui-même.

## Ce que j'ai exploré

Quand on demande à un LLM de répondre en JSON structuré,
trois choses peuvent foirer indépendamment :

- La structure (JSON invalide ou mal formé)
- Le comportement (hallucination, refus incorrect)
- La logique (réponse valide mais fausse)

Ce projet mesure les trois séparément, puis ajoute un retry loop
pour voir si le modèle se corrige tout seul.

## Ce que j'ai trouvé

La découverte la plus frappante : le modèle invente des réponses
à des questions intentionnellement sans réponse.
Un humain dirait "je ne sais pas". Le LLM, lui, construit une réponse convaincante.

Autre observation : un JSON valide ne veut pas dire une réponse correcte.
La fiabilité doit être mesurée à deux niveaux — structure ET contenu.

## Structure du projet

- Notebook 01 — baseline : mesure brute du comportement du modèle
- Notebook 02 — taxonomie des erreurs : pourquoi les échecs arrivent
- Notebook 03 — retry layer : est-ce que le modèle se corrige ?
- Notebook 04 — métriques de fiabilité : comparaison baseline vs pipeline avec retry

## Stack

Python · HuggingFace Transformers · PyTorch · JSON validation
