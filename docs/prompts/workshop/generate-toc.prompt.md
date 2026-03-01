---
name: generate-toc
description: Generate INDEX.md table of contents from a source document
argument-hint: Source file path (e.g., "BRAINSTORM.md", "extraction.txt", "synopsis.md")
version: 1.3
changelog:
  - version: 1.3
    date: 2026-02-14
    changes:
      - "Tags [INTRO] et [REF ChXX] dans les points clés pour distribuer PNJ, concepts et mécaniques entre chapitres"
      - "Nouvelle règle : distribution du contenu entre chapitres (anti-répétition)"
  - version: 1.2
    date: 2026-02-14
    changes:
      - "Step 7 : note de référence vers la relecture persona TOC (write-toc-chapter Step 5.5)"
  - version: 1.1
    date: 2026-02-14
    changes:
      - "Chargement de TOUTE la documentation projet depuis bank.yml (docs.projet + docs univers + rules-files)"
      - "Nouveau Step 2.5 : cross-reference documentation avant découpage"
      - "Points clés enrichis par les contraintes mécaniques et narratives des docs projet"
  - version: 1.0
    date: 2026-01-31
    changes:
      - "Version initiale"
---

# Generate TOC Structure

## Objectif

Analyser un document source et generer INDEX.md: une table des matieres complete avec synopsis et points cles pour chaque chapitre.

## Contexte

### Document Source (OBLIGATOIRE)
```markdown
@$ARGUMENTS
```

Formats acceptes: BRAINSTORM.md, extraction.txt, synopsis existant, notes de session, ou tout document structure decrivant l'intrigue.

### Configuration Projet
```yaml
@bank.yml
```

### Overview du Projet (SI DISPONIBLE)
```markdown
@overview.md
```

Le fichier overview.md contient le brief general du projet (pitch, intentions, ton). S'il existe, l'utiliser pour guider le decoupage et le ton des synopsis.

### Documentation Projet (depuis bank.yml)

Charger **tous** les fichiers declares dans bank.yml, sections `docs` et `rules-files` :

**Documentation univers** (section `docs` — clés directes) :
```markdown
@[docs.univers]       # ex: wot/.docs/UNIVERS.md
@[docs.terminologie]  # ex: wot/.docs/terminologie.md
@[docs.factions]      # si déclaré
@[docs.personnages]   # si déclaré
@[docs.histoire]      # si déclaré
@[docs.geographie]    # si déclaré
@[docs.magie]         # si déclaré
```

**Documentation projet** (section `docs.projet` — liste) :
```markdown
@[docs.projet[0]]     # ex: .docs/introduction.md
@[docs.projet[1]]     # ex: .docs/pj.md
@[docs.projet[2]]     # ex: .docs/document-rules.md
@[docs.projet[3]]     # ex: .docs/equipement.md
@[docs.projet[4]]     # ex: .docs/novels-details.md
@[docs.projet[5]]     # ex: .docs/scenarios-details.md
# ... tous les fichiers listés
```

**Règles de jeu** (section `rules-files`) :
```markdown
@[rules-files.systeme]  # ex: .rules-files/adrenaline-d100.md
# ... toutes les entrées
```

Ne charger que les fichiers effectivement déclarés dans bank.yml. Ne pas inventer de chemins.

## Regles

- INDEX.md dans le dossier `.toc/` du projet
- Chaque chapitre inclut synopsis ET points cles (grandes lignes)
- Contenu en francais, sans emojis
- Preserver les elements narratifs cles et personnages du source
- Synopsis: 2-3 phrases par chapitre
- Points cles: 3-7 puces par chapitre (puces simples, sans checkboxes)
- Fichiers toc-chapter<XX>.md OPTIONNELS (uniquement sur demande)
- Numerotation chapitres: format 2 chiffres (01, 02... 10, 11)
- Longueur INDEX.md: indicativement 100-300 lignes selon nombre de chapitres
- **Les points clés doivent refléter les contraintes des docs projet** : si document-rules.md définit des mécaniques spécifiques (compétences, créatures, systèmes), les chapitres de règles doivent les mentionner explicitement. Si pj.md définit des personnages, les chapitres narratifs doivent les référencer.
- **Ne pas inventer de contenu absent du source ET de la documentation.** Mais croiser source + docs pour enrichir les points clés (ex: le source mentionne "règles de magie" → document-rules.md précise "Pouvoir Unique, Souillure, Résonance" → les points clés nomment ces mécaniques).
- **Chaque chapitre doit inclure `Relecteur TOC:` et `Output-style:`** — assigner le(s) persona(s) relecteur(s) depuis overview.md (section "Relecture TOC") et l'output-style depuis bank.yml (section `output-style`). Laisser vide si aucun relecteur ne s'applique.
- **Distribution du contenu entre chapitres :** Les points clés doivent indiquer quel chapitre **introduit** chaque PNJ majeur, concept homebrew ou mécanique clé (tag `[INTRO]`). Les chapitres suivants qui réutilisent ces éléments les marquent `[REF ChXX]` pour que write-roleplaying sache utiliser le format abrégé.

## Etapes

1. Charger le document source, identifier la structure (actes, parties, chapitres, scenes)
2. Charger bank.yml pour le contexte projet (type, univers)
2.5. **Charger et cross-référencer la documentation projet:**
   - Lire tous les fichiers déclarés dans `docs` et `rules-files` de bank.yml
   - Identifier les **contraintes mécaniques** (systèmes de règles, compétences spéciales, créatures) depuis document-rules.md et rules-files
   - Identifier les **personnages définis** (PJ prétirés, PNJ clés) depuis pj.md
   - Identifier les **détails narratifs** (arcs de nouvelles, synopsis de scénarios) depuis novels-details.md, scenarios-details.md
   - Identifier la **terminologie obligatoire** depuis terminologie.md
   - Compiler une liste de "contraintes à distribuer" dans les chapitres appropriés
3. Extraire: themes, personnages, lieux, points narratifs
4. **Proposer le decoupage en chapitres pour validation:**
   - Lister les chapitres proposes avec titres
   - Demander: "Cette structure vous convient-elle? Voulez-vous modifier, fusionner ou diviser certains chapitres?"
   - Attendre confirmation avant de continuer
5. Creer le dossier `.toc/` si inexistant
6. Generer `.toc/INDEX.md` avec synopsis + points cles pour chaque chapitre, en croisant le source avec la documentation projet (les points clés doivent nommer les mécaniques, personnages et termes spécifiques issus des docs)
7. **Proposer les fichiers toc-chapter detailles:**
   - Demander: "Souhaitez-vous des fiches detaillees pour certains chapitres? (format toc-chapter<XX>.md)"
   - Si oui: generer les fiches demandees dans `.toc/`
   - Si non: terminer

   Note : si overview.md déclare une section "Relecture TOC" avec un ou plusieurs personas, chaque toc-chapter bénéficiera d'une relecture persona (sélectionnée par type de chapitre) lors de sa génération via write-toc-chapter. L'INDEX.md n'est pas soumis à cette relecture (grain trop large), mais les focus points doivent être gardés en tête lors du découpage des points clés.
8. Rapporter les fichiers crees

## Gestion des Erreurs

- **Source vide/illisible:** Signaler le probleme, demander clarification
- **Structure ambigue:** Proposer plusieurs interpretations possibles
- **Manque d'informations:** Lister les elements manquants, continuer avec ce qui est disponible

## Format de Sortie

### .toc/INDEX.md

```markdown
# Table des Matieres: [Nom Projet]

**Type:** [novel/scenario/roleplaying]
**Univers:** [univers]

## Vue d'ensemble
[resume global en 3-5 lignes]

## Personnages principaux
- **[nom]**: [role]

## Themes
- [theme 1]
- [theme 2]

## Structure
- **Acte I:** Chapitres 1-3 - [titre acte]
- **Acte II:** Chapitres 4-7 - [titre acte]
- **Acte III:** Chapitres 8-10 - [titre acte]

---

## Chapitres

### Chapitre 01: [Titre]

**Relecteur TOC :** [persona-id ou vide] (depuis overview.md section "Relecture TOC")
**Output-style :** [nom-fichier.md] (depuis bank.yml section output-style)

**Synopsis:** [2-3 phrases decrivant ce qui se passe]

**Points cles:**
- [INTRO] Premiere apparition de [PNJ] — fiche complete (description + statblock + reference rapide)
- [element narratif obligatoire]
- [revelation ou tournant]

---

### Chapitre 02: [Titre]

**Relecteur TOC :** [persona-id(s)]
**Output-style :** [nom-fichier.md]

**Synopsis:** [2-3 phrases]

**Points cles:**
- [REF Ch01] [PNJ] reapparait — format abrege (stats utiles + renvoi fiche Ch01)
- [INTRO] Premiere mention du [concept homebrew] — disclaimer "Creation originale"
- [point narratif]

---

[... autres chapitres ...]

---
**Source:** [fichier source]
**Genere:** [date]
```

Note: La section "Structure" (Actes/Parties) est optionnelle - l'inclure uniquement si le document source utilise une hierarchie.

### .toc/toc-chapter<XX>.md (OPTIONNEL)

Creer uniquement si l'utilisateur demande plus de details pour un chapitre specifique.

```markdown
# Chapitre [N]: [Titre]

## Synopsis
[2-3 phrases decrivant ce qui se passe]

## Points cles
- [element narratif obligatoire]
- [revelation ou tournant]
- [developpement personnage]

## Personnages
| Nom | Role dans ce chapitre |
|-----|----------------------|
| [perso] | [action/evolution] |

## Lieux (si pertinent)
- [lieu]: [description courte]

## Ambiance (si pertinent)
[ton, atmosphere, references visuelles]

## Connexions
- Precedent: [lien avec chapitre N-1, ou "Premier chapitre - introduction de l'histoire" si N=1]
- Suivant: [preparation chapitre N+1, ou "Dernier chapitre - conclusion" si final]

## Notes d'ecriture
[instructions specifiques, points d'attention]
```
