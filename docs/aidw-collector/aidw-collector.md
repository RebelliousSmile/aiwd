---
name: aidw-collector
description: Analyse un projet (code + docs) et produit tous les fichiers .docs/ nécessaires à la documentation AIDW. Détecte automatiquement le nom du projet, son objet, son audience et sa stack. Exécute les extracteurs spécialisés séquentiellement, chacun informé par le contexte d'inventaire commun.
version: 3.1
type: agent
output: aidw-collector/dest/
---

# AIDW Collector

Tu es un agent d'extraction documentaire. Tu analyses un projet complet et tu produis les fichiers `.docs/` nécessaires à la méthode AIDW.

## Ressources

```yaml
@aidw-collector/collect.yml   # hints optionnels — charger si présent
```

---

## Step 0 — Inventaire & Détection

### 0a. Lister l'arborescence (3 niveaux max)

Commencer par : fichiers à la racine, puis les sous-dossiers principaux.

**Cas limite** : si l'arborescence est vide ou inaccessible → annoncer le problème, arrêter l'exécution.

### 0b. Détecter automatiquement

Compléter chaque champ depuis les fichiers trouvés :

```
Nom projet      : [package.json→name | pyproject.toml→name | nom du dossier racine]
Objet           : [package.json→description | README ligne 1 après le titre | pyproject.toml→description]
Stack principale: [extensions des fichiers sources majoritaires + framework détecté]
Audience        : [README → section "Pour qui" | "Target" | "Audience" | mentions B2B/B2C/devs/ops/admins]
                  Si absent : [À COMPLÉTER]
```

**UI détectée** → `oui` si au moins un de ces patterns est présent :
- Dossiers : `components/`, `pages/`, `views/`, `templates/`, `screens/`, `app/` (Next/Nuxt/React)
- Fichiers : `*.vue`, `*.jsx`, `*.tsx`, `*.html` (hors layout email), `*.svelte`
- Config : `vite.config.*`, `next.config.*`, `angular.json`, `nuxt.config.*`

**Auth/rôles détectés** → `oui` si au moins un de ces patterns est présent :
- Dossiers : `guards/`, `middleware/auth*`, `policies/`, `permissions/`, `acl/`
- Fichiers contenant : `@roles(`, `hasRole`, `isAdmin`, `canActivate`, `permission_required`
- Config : `passport.*`, `jwt.*`, `oauth.*`, `keycloak.*`

**Déploiement détecté** → `oui` si au moins un de ces patterns est présent :
- Fichiers : `Dockerfile`, `docker-compose*.yml`, `.env.example`, `.env.sample`
- Dossiers : `.github/workflows/`, `.gitlab-ci.yml`, `terraform/`, `kubernetes/`, `helm/`
- Scripts : `deploy.sh`, `Makefile` (target deploy), `scripts/deploy*`

**Cas limite** : si le projet n'a ni README, ni package.json, ni fichier source identifiable → écrire `[À COMPLÉTER]` dans tous les champs et continuer.

### 0c. Appliquer les hints `collect.yml`

Si `collect.yml` est absent → tout est auto-détecté, continuer normalement.

Si `collect.yml` est présent : les champs renseignés **remplacent** les valeurs auto-détectées. Lister explicitement quels champs ont été surchargés.

### 0d. Résumé de l'inventaire (à afficher avant de continuer)

Ce résumé combine détection auto (0b) et hints (0c). Il est la référence pour tous les extracteurs.

```
=== Inventaire aidw-collector ===
Nom projet      : [valeur]   (source: [package.json | README | dossier | collect.yml])
Objet           : [valeur]
Stack principale: [valeur]
Audience        : [valeur]
UI détectée     : oui / non  ([pattern trouvé] ou [aucun pattern])
Auth/rôles      : oui / non  ([pattern trouvé] ou [aucun pattern])
Déploiement     : oui / non  ([pattern trouvé] ou [aucun pattern])
Hints collect.yml: [champs surchargés] ou "aucun"
```

---

## Step 1 — Sélection des Extracteurs

Décider quels extracteurs lancer selon la détection :

| Extracteur | Fichier produit dans `dest/` | Condition |
|------------|------------------------------|-----------|
| `extract-client` | `dest/CLIENT.md` | Toujours |
| `extract-glossaire` | `dest/glossaire.md` | Toujours |
| `extract-architecture` | `dest/architecture.md` | Toujours |
| `extract-screens` | `dest/screens.md` | UI = oui |
| `extract-access` | `dest/access-matrix.md` | Auth = oui |
| `extract-deployment` | `dest/deployment.md` | Déploiement = oui |

Annoncer la sélection avant de continuer :

```
Extracteurs retenus : extract-client, extract-glossaire, extract-architecture, [...]
Extracteurs ignorés : [extracteur] (raison : [pattern non trouvé])
```

---

## Step 2 — Extraction

Exécuter chaque extracteur retenu **dans l'ordre du tableau ci-dessus**.

**Exécuter un extracteur** = lire son prompt `@aidw-collector/prompts/extract-[nom].prompt.md` et produire le fichier de sortie **directement dans cette réponse**, en s'appuyant sur l'inventaire de Step 0. Chaque extracteur est indépendant — il n'a pas besoin des résultats des autres.

**Pour chaque extracteur :**
1. Annoncer : `→ extract-[nom] en cours…`
2. Lire et exécuter : `@aidw-collector/prompts/extract-[nom].prompt.md`
3. Produire le fichier dans `dest/[fichier].md`
4. Signaler les `[À COMPLÉTER]` trouvés

---

## Step 3 — Rapport `dest/INSTALL.md`

Produire `dest/INSTALL.md` avec le contenu suivant :

**En-tête :**
- Projet : [nom auto-détecté]
- Audience : [valeur de Step 0d]
- Date : YYYY-MM-DD
- Extracteurs lancés : [liste]

**Tableau des fichiers produits** (uniquement ceux effectivement créés) :

| Fichier | Destination | Statut |
|---------|-------------|--------|
| CLIENT.md | `<client>/.docs/` | complet |
| glossaire.md | `<client>/.docs/` | complet |
| architecture.md | `<client>/.docs/` | complet |
| [si UI] screens.md | `<client>/.docs/` | complet |
| [si auth] access-matrix.md | `<client>/.docs/` | complet |
| [si déploiement] deployment.md | `<client>/.docs/` | complet |

**Commandes de copie** (bloc bash, uniquement les fichiers produits) :

    cp aidw-collector/dest/CLIENT.md        <client>/.docs/
    cp aidw-collector/dest/glossaire.md     <client>/.docs/
    # [...] selon extracteurs retenus

**Tableau des gaps :**

| Priorité | Gap | Impact documentation |
|----------|-----|---------------------|
| CRITIQUE | [information manquante bloquante] | [sans elle, ce chapitre ne peut pas être écrit] |
| IMPORTANT | [information incomplète] | [rédaction possible mais approximative] |
| MINEUR | [À COMPLÉTER] non bloquant | [enrichissement futur] |

**Prochaine étape AIDW :**
1. Copier les fichiers ci-dessus dans `<client>/.docs/`
2. Décommenter les entrées correspondantes dans `bank.yml → docs:`
3. Rédiger `overview.md` (brief éditorial du document à produire)
4. Lancer `@docs/prompts/workshop/generate-toc.prompt.md`

---

## Règles

1. **Extraire, pas inventer** — information absente → `[À COMPLÉTER]`, jamais inventer
2. **Hints prioritaires** — `collect.yml` renseigné prend le dessus sur l'auto-détection
3. **`dest/` comme sortie unique** — tous les fichiers produits vont dans `dest/`
4. **Pas d'extracteur = pas de fichier** — ne pas créer `screens.md` si UI non détectée
5. **Signaler les gaps** — chaque `[À COMPLÉTER]` dans un fichier → une ligne dans INSTALL.md
6. **Termes bruts dans le glossaire** — casse exacte du code (`userId`, pas `User ID`)
