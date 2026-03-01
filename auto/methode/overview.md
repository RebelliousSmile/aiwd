# Méthode generationPDF — Overview

## Contexte

`generationPDF` est un système de génération de documentation professionnelle multi-clients. Il orchestre un pipeline Markdown → ICML → InDesign → PDF avec des prompts Claude structurés.

Ce document — **Méthode generationPDF** — documente le système lui-même : comment il fonctionne, comment l'utiliser, et pourquoi il est construit ainsi.

## Objectif du Document

Permettre à un nouvel utilisateur (ou à l'équipe après un long break) de :

1. Comprendre la philosophie du pipeline
2. Démarrer un nouveau projet documentation en moins de 30 minutes
3. Utiliser les prompts workshop efficacement
4. Maintenir la cohérence du système au fil du temps

## Audience

Utilisateurs du dépôt `generationPDF` — rédacteurs techniques, auteurs, équipe projet.

## Portée

- Architecture du pipeline (Markdown → ICML → InDesign → PDF)
- Conventions et structure (`<client>/<projet>/`, bank.yml, output-styles)
- Prompts workshop (rôles, enchaînements, options)
- Agents autonomes (writing-pipeline, coherence-checker)
- Workflows type (nouveau projet, extraction PDF, review-pipeline)
- Maintenance et évolution du système
