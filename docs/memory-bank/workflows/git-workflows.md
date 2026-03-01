# Git Multi-Dépôts - Workflows

## Architecture

```
PDFLateX/                           ← PAS de .git ici (racine)
├── .claude/                        ← Dépôt Git: Configuration
│   └── .git/
├── <univers>/
│   ├── <projet-1>/                 ← Dépôt Git: Projet 1
│   │   └── .git/
│   └── <projet-N>/                 ← Dépôt Git: Projet N
│       └── .git/
└── scripts/                        ← Pas de .git (scripts partagés)
```

## Principe: Isolation par Projet

1. **Configuration (.claude/)**: Dépôt partagé pour skills, agents, config itch.io
2. **Chaque projet**: Dépôt indépendant avec son propre historique
3. **Documentation univers**: Intégrée dans `<univers>/.docs/`

## Workflows

### 1. Modifications Configuration

```bash
cd .claude
git status
git add <fichiers-modifiés>
git commit -m "Update: <description>"
git push
```

### 2. Modifications Projet

```bash
cd <univers>/<nom-projet>
git status
git add chapitres/*.tex
git commit -m "Add chapter: <Titre>

- <détails>"
git push
```

## Commandes Utiles

### Vérifier Tous les Dépôts (PowerShell)

```powershell
Get-ChildItem -Recurse -Directory -Filter ".git" | ForEach-Object {
    $repo = $_.Parent.FullName
    Write-Host "`n=== $repo ===" -ForegroundColor Cyan
    git -C $repo status -s
}
```

### Vérifier Tous les Dépôts (Bash)

```bash
find . -name ".git" -type d | while read gitdir; do
    repo=$(dirname "$gitdir")
    echo -e "\n=== $repo ==="
    git -C "$repo" status -s
done
```

### Pull Toutes les Mises à Jour

```bash
find . -name ".git" -type d | while read gitdir; do
    repo=$(dirname "$gitdir")
    echo -e "\nPulling $repo..."
    git -C "$repo" pull
done
```

## Initialisation Nouveau Projet

```bash
cd <univers>/<nouveau-projet>
git init
git add .
git commit -m "Initial commit: <Nom Projet>

- Structure projet créée depuis template
- Main.tex configuré
- Chapitres vides initialisés"

# Optionnel: lier à GitHub
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin main
```

## Bonnes Pratiques

1. Toujours se placer dans le bon répertoire avant git add/commit/push
2. Vérifier avec `pwd` (Bash) ou `Get-Location` (PowerShell)
3. Messages de commit descriptifs
4. Ne jamais initialiser de .git à la racine PDFLateX/
