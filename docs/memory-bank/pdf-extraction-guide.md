# Guide d'extraction PDF — Optimisation de tokens

## Types de PDF

| Type | Détection | Coût tokens |
|------|-----------|-------------|
| **Texte** (vectoriel) | Texte sélectionnable, fichier léger | Faible |
| **Image** (scanné/rendu) | Texte non sélectionnable, fichier lourd (>5 MB/50 pages) | Élevé (~10-50x le texte) |
| **Mixte** | Pages texte + pages images | Traiter chaque section selon son type |

## Choix du modèle

| Contenu | Modèle | Pourquoi |
|---------|--------|----------|
| Texte narratif, règles en prose, glossaire | **Sonnet** | Économie ~60-70%, qualité équivalente |
| Tableaux de stats denses, pourcentages | **Opus** | Fiabilité des chiffres, pas de seconde passe |
| Noms propres obscurs, néologismes | **Opus** | Préserve les termes inhabituels |
| Classification / triage (Phase A) | **Opus** | Meilleur jugement éditorial |
| Gros PDF (>100 pages) | **Opus Phase A** + **Sonnet Phase B/C** | Compromis coût/qualité |

> **Règle** : Si une seconde passe de vérification est nécessaire (re-lire les mêmes images + comparer), elle annule l'économie de Sonnet (~50-80% du coût de la première passe).

## Stratégies d'optimisation

1. **Cibler les pages** — Lire la TOC d'abord (1 page), extraire seulement les sections pertinentes
2. **Lire par blocs thématiques** — Regrouper par chapitre plutôt que séquentiellement (Read limité à 20 pages/appel)
3. **Extraction sélective** — Document court (<50 p.) → exhaustive. Document long → ciblée
4. **Ne pas re-lire** — Pages déjà dans le contexte de la session

## Retours d'expérience

- **Heavy Metal L1-L2** (lore + équipement, PDF images 48p) : Sonnet aurait suffi
- **Heavy Metal L3** (24 fiches URC avec stats denses, PDF images 48p) : Opus nécessaire
- **Voidheart Symphony** (306p images, prose PbtA) : Sonnet recommandé, Opus Phase A uniquement

---

*Référencé depuis `extract.prompt.md` (étape A.1b). Basé sur l'extraction Heavy Metal (février 2026).*
