---
name: add-text
description: Ajout de contenu dans un chapitre existant (respect output-style, causalité, structure)
argument-hint: <fichier-cible> "sujet ou thème" [--section "<titre-section>"] [--length short|medium|full]
version: 1.1
changelog:
  - version: 1.1
    date: 2026-03-01
    changes:
      - "Généralisation : suppression références Écryme hardcodées"
      - "Output-style chargé depuis bank.yml (client-agnostic)"
      - "Rules : suppression style Écryme-specific"
  - version: 1.0
    date: 2026-02-20
    changes:
      - "Migration depuis ecryme/solo-ecryme/.claude/templates/add-text.md"
      - "Reformaté format workshop standard"
      - "Suppression références obsolètes (sommaire.md, first-draft/)"
---

# Add Text: Ajout de contenu dans un chapitre

## Goal

Ajouter un ou plusieurs paragraphes de contenu dans un chapitre existant, en respectant l'output-style du projet, la règle de causalité et la structure du fichier cible.

## Arguments **[3]**

```bash
# Usage de base — Claude détermine l'emplacement optimal
add-text.prompt.md chapitres/chapitre02.md "Introduction à l'authentification OAuth"

# Avec section cible précise
add-text.prompt.md chapitres/chapitre02.md "Gestion des erreurs 401" --section "Codes d'erreur API"

# Avec longueur souhaitée
add-text.prompt.md chapitres/chapitre03.md "Exemple d'intégration SDK Python" --length full
```

## Context Resources **[7]**

**MUST load all (in order):**

1. **Bank:** `@bank.yml` (document type, client, output-style)
2. **TOC-Chapter:** `@.toc/toc-chapter<XX>.md` (correspondant au fichier cible — récupérer output-style spécifique)
3. **Output-style:** Charger le fichier indiqué dans toc-chapter ; fallback : `output-style.global` de bank.yml
4. **Glossaire:** `@<client>/.docs/glossaire.md` (termes métier)
5. **Document-rules:** `@.docs/document-rules.md` (optionnel — si présent)
6. **Target file:** `@$ARGUMENTS[0]` (chapitre cible — lire **en entier** avant d'insérer)
7. **Sujet:** `$ARGUMENTS[1]` (thème ou sujet à développer)

**Logique output-style:**
- Extraire du toc-chapter la ligne `**Output-style:** <filename>.md`
- Charger ce fichier depuis `<client>/.output-styles/<filename>.md`
- Fallback: utiliser `output-style.global` de bank.yml

## Règle de causalité (placement)

Quand `--section` n'est pas précisé, déterminer l'emplacement optimal :

1. **Lire le fichier cible en entier** — cartographier la structure existante (titres, sous-sections, lacunes)
2. **Identifier les sections complètes vs à développer** — repérer les transitions et la progression narrative
3. **Appliquer la règle de causalité :**
   - Concepts fondamentaux avant leurs applications
   - Causes avant conséquences
   - Règles générales avant exceptions
   - Introductions avant développements
4. **Insérer à l'endroit qui respecte cette progression** — ne jamais introduire un concept après son application

## Rules **[10]**

**CRITICAL:**
- ✅ LIRE le fichier cible en entier avant d'insérer quoi que ce soit
- ✅ RESPECTER le ton et le registre déjà présents dans le fichier cible
- ✅ SUIVRE l'output-style (ton, structure des phrases, formules de titres)
- ✅ FONDER le contenu sur glossaire.md et document-rules.md — ne jamais inventer
- ✅ RESPECTER la terminologie définie dans glossaire.md
- ❌ NEVER insérer du contenu redondant avec ce qui existe déjà dans le chapitre
- ❌ NEVER modifier le texte existant (sauf insertions explicites demandées)
- ❌ NEVER utiliser des personas (c'est comment.prompt.md)

## Longueur

| `--length` | Volume | Usage |
|------------|--------|-------|
| `short` | 1 paragraphe (3-5 phrases) | Transition, touche atmosphérique |
| `medium` | 2-3 paragraphes | Concept mécanique + contexte narratif |
| `full` | Section complète | Développement thématique complet avec exemple |

*Par défaut (sans `--length`) : `medium`.*

## Output

Produire :
1. **Texte insérable** — prêt à copier-coller, au format markdown du fichier cible
2. **Position d'insertion** — ligne exacte (après la ligne N, avant la section X)
3. **Justification** — 1 phrase expliquant le choix de placement (règle de causalité)

```markdown
## Texte à insérer

[contenu ici]

---
**Position :** Après la ligne 47 (fin de la section "Authentification"), avant "Gestion des tokens"
**Justification :** Le concept d'OAuth est présenté avant son implémentation — insertion respecte la causalité.
```
