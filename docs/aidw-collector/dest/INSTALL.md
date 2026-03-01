# Installation — Résultat collector AIDW sur generationPDF

**Projet analysé :** `generationPDF` (dépôt AIDW)  
**Date extraction :** 2026-03-01  
**Collecté par :** `docs/aidw-collector/prompts/client-extract.prompt.md` v2.0

---

## Fichiers produits

| Fichier | Destination | Statut |
|---------|-------------|--------|
| `CLIENT.md` | `auto/.docs/CLIENT.md` | complet |
| `glossaire.md` | `auto/.docs/glossaire.md` | complet |
| `architecture.md` | `auto/.docs/architecture.md` | complet |
| `deployment.md` | `auto/.docs/deployment.md` | complet |
| `overview.md` | `auto/methode/overview.md` | déjà existant — comparer |
| `bank.yml` | `auto/methode/bank.yml` | déjà existant — comparer |
| `screens.md` | N/A | **non produit** (pas d'UI) |
| `access-matrix.md` | N/A | **non produit** (pas d'auth/rôles) |

## Commandes de déploiement

```powershell
# Copier les nouveaux fichiers dans auto/.docs/
Copy-Item docs\aidw-collector\dest\architecture.md auto\.docs\
Copy-Item docs\aidw-collector\dest\deployment.md   auto\.docs\

# CLIENT.md et glossaire.md existent déjà — comparer avant d'écraser
# Compare-Object (Get-Content auto\.docs\CLIENT.md) (Get-Content docs\aidw-collector\dest\CLIENT.md)

# overview.md et bank.yml existent déjà dans auto/methode/ — comparer avant d'écraser
# Compare-Object (Get-Content auto\methode\overview.md) (Get-Content docs\aidw-collector\dest\overview.md)
```

## Décommenter dans bank.yml

```yaml
docs:
  client: "auto/.docs/CLIENT.md"
  glossaire: "auto/.docs/glossaire.md"
  architecture: "auto/.docs/architecture.md"   # ← nouveau
  deployment: "auto/.docs/deployment.md"       # ← nouveau
```

## Prochaine étape AIDW

```
@docs/prompts/workshop/write-toc-chapter.prompt.md 02
```

---

## Gaps Identifiés

| Priorité | Gap | Impact |
|----------|-----|--------|
| IMPORTANT | Pas de `requirements.txt` | Onboarding difficile — installer les dépendances Python manuellement |
| IMPORTANT | Pas de CI/CD actif | Pas de validation automatique du pipeline |
| MINEUR | `screens.md` / `access-matrix.md` non applicables | Aucun — projet sans UI ni auth |
| MINEUR | Pas de `.env.example` | Aucun — pas de variables secrètes requises |
| INFO | `coherence-extract.py` non documenté dans scripts/README.md | Risque d'oubli dans le guide Ch06 |

## Observations Collector

- **Architecture :** Tooling local pur, pas de serveur — l'approche "fichiers texte + LLM + scripts CLI" est entièrement cohérente avec la méthode documentée
- **Termes métier :** Vocabulaire très stable et cohérent dans tout le dépôt
- **Gaps documentaires identifiés :** `requirements.txt` absent est un vrai obstacle à l'onboarding (à corriger avant Ch02)
- **Qualité du matériau source :** CLAUDE.md, overview.md et bank.yml sont des sources de haute qualité — extraction facile
