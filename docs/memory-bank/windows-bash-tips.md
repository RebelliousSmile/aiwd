# Windows + Bash : Pièges Courants

Notes pour éviter les problèmes récurrents lors de l'utilisation de bash sur Windows.

---

## Fichier `nul` Parasite

### Problème

Un fichier nommé `nul` apparaît à la racine du projet.

### Cause

Confusion entre conventions Windows et Unix :

| Environnement | Périphérique null | Exemple |
|---------------|-------------------|---------|
| Windows CMD | `nul` | `command > nul` |
| Bash/Unix | `/dev/null` | `command > /dev/null` |

Quand on exécute `command > nul` en bash, il crée un **fichier littéral** nommé `nul` au lieu de rediriger vers le néant.

### Contenu Typique

```
/usr/bin/bash: line 1: tree: command not found
```

Ou autre message d'erreur de la commande qui a échoué.

### Solution

1. **Supprimer le fichier :** `rm nul`
2. **Utiliser la bonne syntaxe bash :**

```bash
# Rediriger stdout
command > /dev/null

# Rediriger stderr
command 2> /dev/null

# Rediriger les deux
command > /dev/null 2>&1
```

### Prévention

Dans les scripts cross-platform, détecter l'environnement :

```bash
# Détection automatique
if [[ "$OSTYPE" == "msys" ]] || [[ "$OSTYPE" == "cygwin" ]]; then
    # Git Bash ou Cygwin sur Windows - utiliser /dev/null
    NULL_DEV="/dev/null"
else
    NULL_DEV="/dev/null"
fi

command > "$NULL_DEV" 2>&1
```

---

## Commande `tree` Non Disponible

### Problème

`tree: command not found`

### Cause

La commande `tree` n'est pas installée par défaut dans Git Bash.

### Alternatives

```bash
# Option 1 : Utiliser find
find . -type f -name "*.md" | head -20

# Option 2 : Utiliser ls récursif
ls -R

# Option 3 : Installer tree via pacman (si MSYS2)
pacman -S tree
```

---

## Chemins Windows vs Unix

### Problème

Les chemins Windows (`C:\Users\...`) peuvent causer des erreurs en bash.

### Solutions

```bash
# Échapper les backslashes
cd "C:\\Users\\fxgui\\Public"

# Ou utiliser des slashes normaux (bash les accepte)
cd "C:/Users/fxgui/Public"

# Ou utiliser le format MSYS
cd "/c/Users/fxgui/Public"
```

### Attention au Backslash Final

```bash
# INCORRECT - cause une erreur
ls "C:\Users\fxgui\"

# CORRECT
ls "C:\Users\fxgui"
ls "C:/Users/fxgui/"
```

---

## Commandes Windows vs Bash

| Action | Windows CMD | Bash |
|--------|-------------|------|
| Supprimer fichier | `del` | `rm` |
| Supprimer dossier | `rmdir /s` | `rm -r` |
| Renommer | `ren` | `mv` |
| Copier | `copy` | `cp` |
| Lister | `dir` | `ls` |
| Afficher fichier | `type` | `cat` |
| Null device | `nul` | `/dev/null` |

---

**Version:** 1.0
**Date:** 2026-02-01
