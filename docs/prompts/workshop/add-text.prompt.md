---
name: add-text
description: Ajout de texte narratif dans un chapitre existant (respect output-style, causalité, atmosphère)
argument-hint: <fichier-cible> "sujet ou thème" [--section "<titre-section>"] [--length short|medium|full]
version: 1.0
changelog:
  - version: 1.0
    date: 2026-02-20
    changes:
      - "Migration depuis ecryme/solo-ecryme/.claude/templates/add-text.md"
      - "Reformaté format workshop standard"
      - "Suppression références obsolètes (sommaire.md, first-draft/)"
---

# Add Text: Ajout de texte narratif

## Goal

Ajouter un ou plusieurs paragraphes de texte narratif dans un chapitre existant, en respectant l'output-style du projet, la règle de causalité et le fil narratif du fichier cible.

## Arguments **[3]**

```bash
# Usage de base — Claude détermine l'emplacement optimal
add-text.prompt.md chapitres/chapitre02.md "La Vapeur : exemple d'introduction atmosphérique"

# Avec section cible précise
add-text.prompt.md chapitres/chapitre02.md "Soupape face Vertige" --section "De la Soupape et de ses deux visages"

# Avec longueur souhaitée
add-text.prompt.md chapitres/chapitre03.md "Exemple ballade Bienvenue dans ma demeure" --length full
```

## Context Resources **[8]**

**MUST load all (in order):**

1. **Bank:** `@bank.yml` (document type, univers)
2. **TOC-Chapter:** `@.toc/toc-chapter<XX>.md` (correspondant au fichier cible — récupérer output-style spécifique)
3. **Output-style:** Charger le fichier indiqué dans toc-chapter (ex: `@ecryme/.output-styles/solo-ecryme.md`)
4. **Terminologie:** `@ecryme/.docs/terminologie.md`
5. **Document-rules:** `@.docs/document-rules.md` (mécaniques officielles, valeurs exactes)
6. **Target file:** `@$ARGUMENTS[0]` (chapitre cible — lire **en entier** avant d'insérer)
7. **Sujet:** `$ARGUMENTS[1]` (thème ou sujet à développer)
8. **Section cible:** `$ARGUMENTS[--section]` (optionnel — si absent, déterminer par règle de causalité)

**Logique output-style:**
- Extraire du toc-chapter la ligne `**Output-style:** <filename>.md`
- Charger ce fichier depuis `<univers>/.output-styles/<filename>.md`
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
- ✅ SUIVRE l'output-style (atmosphère, structure phrases, formules titres)
- ✅ FONDER le contenu sur document-rules.md et terminologie.md — ne jamais inventer
- ❌ NEVER insérer du contenu redondant avec ce qui existe déjà dans le chapitre
- ❌ NEVER modifier le texte existant (sauf insertions explicites)
- ❌ NEVER utiliser des personas (c'est comment.prompt.md)

**Style obligatoire (cf. output-style) :**
1. Prose XIXe soutenue — pas de liste pour les explications
2. Atmosphère avant mécanique — contexte narratif avant la règle
3. Métaphores mécaniques présentes naturellement
4. Sensorialité écrymienne — au moins 2 sens distincts dans les passages descriptifs
5. Typographie française (guillemets «», tirets —, accents, majuscules accentuées)

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

[texte narratif ici]

---
**Position :** Après la ligne 47 (fin de la section "De la Soupape"), avant "De la Jauge Confident"
**Justification :** La Soupape est présentée avant ses effets sur les jauges — insertion respecte la causalité.
```
