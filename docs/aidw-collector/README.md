# AIDW Collector

Kit à copier dans un projet cible pour extraire toute l'information nécessaire à la documentation AIDW.

**Point d'entrée unique :** `aidw-collector.md`

## Ce que ça produit

`aidw-collector` détecte automatiquement le projet, sélectionne les extracteurs pertinents et les lance séquentiellement :

| Fichier produit | Contenu | Condition |
|----------------|---------|-----------|
| `CLIENT.md` | Contexte, audience, ton, contraintes | Toujours |
| `glossaire.md` | Vocabulaire technique extrait du code | Toujours |
| `architecture.md` | Stack, modules, flux de données | Toujours |
| `screens-[module].md` | Inventaire des écrans (champs, actions, messages) — 1 fichier par module | Si UI détectée |
| `userflows-[groupe].md` | Parcours utilisateurs — tâches cross-écrans, erreurs — 1 fichier par groupe | Si UI détectée |
| `access-matrix.md` | Matrice rôles × fonctionnalités | Si auth/rôles détectés |
| `deployment.md` | Environnements, variables, CI/CD, procédure | Si config déploiement détectée |
| `requirements.md` | Spécifications fonctionnelles, features, use cases, règles métier | Si specs détectées |
| `[theme-custom].md` | Domaines fonctionnels majeurs hors standards (multi-tenant, QR system…) | Si défini dans collect.yml |
| `INSTALL.md` | Commandes de copie + tableau des gaps | Toujours |

> **Règle de split :** screens et userflows sont automatiquement découpés en plusieurs fichiers si l'application a plus de 20 écrans ou 8 flux. Chaque fichier reste sous 250 lignes.

## Architecture interne

```
docs/aidw-collector/
├── aidw-collector.md          ← orchestrateur (POINT D'ENTRÉE)
├── collect.yml                ← hints optionnels (surcharge auto-détection + thèmes custom)
├── README.md                  ← ce fichier
├── dest/                      ← fichiers produits (output)
├── templates/                 ← squelettes avec [À COMPLÉTER]
│   ├── CLIENT.md
│   ├── glossaire.md
│   ├── architecture.md
│   ├── screens.md
│   ├── userflows.md
│   ├── access-matrix.md
│   ├── deployment.md
│   └── requirements.md
└── prompts/
    ├── extract-client.prompt.md
    ├── extract-glossaire.prompt.md
    ├── extract-architecture.prompt.md
    ├── extract-screens.prompt.md
    ├── extract-userflows.prompt.md
    ├── extract-access.prompt.md
    ├── extract-deployment.prompt.md
    └── extract-requirements.prompt.md
```

Chaque extracteur charge son template depuis `templates/` et remplace les `[À COMPLÉTER]` avec les données extraites du projet.

## Utilisation

### 1. Copier ce dossier dans le projet cible

```bash
cp -r docs/aidw-collector/ /chemin/vers/mon-projet/
```

### 2. (Optionnel) Renseigner `collect.yml`

Uniquement pour forcer ou corriger l'auto-détection. Laisser vide pour une détection entièrement automatique.

### 3. Lancer l'agent

Avec n'importe quel LLM (Claude, GPT, Gemini…) ayant accès aux fichiers du projet :

```
@aidw-collector/aidw-collector.md
```

L'agent analyse le projet, sélectionne les extracteurs, les lance en séquence, puis produit les fichiers dans `aidw-collector/dest/`.

### 4. Déployer dans generationPDF

Suivre les instructions dans `aidw-collector/dest/INSTALL.md` (généré automatiquement).

```bash
# Exemple pour cerascan :
cp aidw-collector/dest/CLIENT.md        generationPDF/cerascan/.docs/
cp aidw-collector/dest/glossaire.md     generationPDF/cerascan/.docs/
cp aidw-collector/dest/architecture.md  generationPDF/cerascan/.docs/
cp aidw-collector/dest/screens.md       generationPDF/cerascan/.docs/
cp aidw-collector/dest/userflows.md     generationPDF/cerascan/.docs/
cp aidw-collector/dest/access-matrix.md generationPDF/cerascan/.docs/
cp aidw-collector/dest/deployment.md    generationPDF/cerascan/.docs/
```

Puis décommenter les entrées correspondantes dans `bank.yml → docs:`.

### 5. Prochaine étape AIDW

Rédiger `overview.md` (brief éditorial du document à produire),  
puis lancer `@docs/prompts/workshop/generate-toc.prompt.md`.

## Prérequis

- Un LLM avec accès aux fichiers du projet (Claude, GPT-4, Gemini…)
- Aucun Python, aucun Pandoc requis à cette étape

