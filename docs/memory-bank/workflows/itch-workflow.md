# Publication itch.io - Guide Complet

## Concept

**1 page "Game Assets" par univers** sur itch.io, avec plusieurs PDFs par page.

```
https://tnnka.itch.io/archipels-assets
  ├── carnets-de-voyage → carnets_de_voyage.pdf
  ├── eveil-des-ombres → eveil_des_ombres.pdf
  └── tournoi-nemesis → tournoi-nemesis.pdf
```

---

## Workflow Recommandé (Manuel)

Butler n'est pas adapté pour avoir plusieurs boutons Download séparés. **L'upload manuel reste la solution optimale** pour 3-5 PDFs par univers.

### 1. Compiler les PDFs

```bash
python scripts/build-pdf.py --project "archipels/carnets_de_voyage" --clean
python scripts/build-pdf.py --project "archipels/eveil_des_ombres" --clean
```

### 2. Préparer pour upload

```bash
python scripts/prepare-for-itch.py --universe "archipels"
```

Crée un dossier `Desktop/itch-upload/` avec les PDFs renommés.

### 3. Upload manuel sur itch.io

1. Allez sur https://itch.io/dashboard
2. Éditez le projet (ex: `archipels-assets`)
3. Section **Uploads** → **Upload files**
4. Pour chaque fichier :
   - Nom d'affichage : "Carnets de Voyage v1.2"
   - Décochez "This file will be played in browser"
5. **Save & View page**

**Résultat :** Chaque PDF a son propre bouton "Télécharger"

---

## Setup Initial (une seule fois)

### 1. Créer les pages sur itch.io

Sur https://itch.io/game/new :

- **Title** : Archipels - Assets JdR
- **URL** : `archipels-assets`
- **Kind** : Physical game → Tabletop RPG
- **Classification** : Assets
- **Pricing** : Pay what you want (minimum $0)
- **Tags** : ttrpg, fantasy, archipels

### 2. Installer Butler

```bash
python scripts/install-butler.py
scripts/butler/butler login
```

---

## Scripts Disponibles

| Script | Usage |
|--------|-------|
| `build-pdf.py` | Compile un projet |
| `prepare-for-itch.py` | Prépare les PDFs pour upload manuel |
| `publish-one.py` | Publie via Butler (1 bouton par page) |
| `publish-all.py` | Publie tous les projets via Butler |
| `itch-checklist.py` | Vérifie l'état des projets |

---

## Configuration: itch-projects.json

Fichier de config dans `docs/templates/itch-projects.json` :

```json
{
  "username": "tnnka",
  "projects": [
    {
      "path": "archipels/tournoi-nemesis",
      "title": "Tournoi Nemesis",
      "itch_project": "tournoi-nemesis",
      "collection": "archipels",
      "status": "ready"
    }
  ]
}
```

---

## Avantages Upload Manuel

- **Boutons séparés** - Un bouton par PDF
- **Noms personnalisés** - Contrôle total
- **Ordre d'affichage** - Drag & drop
- **Simplicité** - Pas de config complexe

---

## Commandes Butler (Alternative)

```bash
# Status d'un projet
scripts/butler/butler status tnnka/archipels-assets

# Upload direct
scripts/butler/butler push \
  archipels/tournoi-nemesis/output/tournoi-nemesis.pdf \
  tnnka/archipels-assets:tournoi-nemesis \
  --userversion v1.0
```

---

## Workflow Final

```bash
# 1. Compiler
python scripts/build-pdf.py --project "archipels/tournoi-nemesis" --clean

# 2. Préparer
python scripts/prepare-for-itch.py --universe archipels

# 3. Upload manuel (2-3 min)
# → https://itch.io/dashboard

# 4. Tag git (optionnel)
cd archipels/tournoi-nemesis
git tag -a v1.0 -m "Release v1.0"
git push origin v1.0
```
