# Pipeline ICML/InDesign - Workflow PAO

## Vue d'ensemble

Le projet utilise **un pipeline unique** pour la production de documents :

```
Écriture → review-pipeline → chapitres/*.md (source de vérité)
     |
     ↓
build-icml.py → Pandoc + Lua filter → .icml (lié dans InDesign)
     |
     ↓
Mise en page InDesign → PDF HD final
```

**Pipeline ICML** : Le fichier `.icml` est importé dans InDesign comme fichier lié. Quand `build-icml.py` est relancé, InDesign propose de rafraîchir automatiquement — cycle édition Markdown → mise en page sans friction ni copier-coller.

---

## Prérequis

### Logiciels

| Logiciel | Version | Installation |
|----------|---------|--------------|
| **Pandoc** | 3.0+ | https://pandoc.org/installing.html |
| **Python** | 3.10+ | https://python.org |
| **PyYAML** | - | `pip install pyyaml` |
| **InDesign** | 2024+ | Creative Cloud (Windows 23H2+) |

### Vérification

```bash
# Vérifier Pandoc
pandoc --version

# Vérifier PyYAML
python -c "import yaml; print(yaml.__version__)"
```

---

## Usage

### build-icml.py - Conversion Markdown vers ICML

```bash
# Conversion standard (concatène tous les chapitres)
python scripts/build-icml.py --project "demonslayer/old-stories-new-enemies"

# Avec nom de sortie personnalisé
python scripts/build-icml.py --project "archipels/carnets_de_voyage" --output-name "carnets-v2"

# Un fichier ICML par chapitre (pas de concaténation)
python scripts/build-icml.py --project "zombiology/scenario-alpha" --no-concat

# Simulation (aucun fichier généré)
python scripts/build-icml.py --project "wot/scenario-cairhien" --dry-run

# Ouvrir le fichier après génération
python scripts/build-icml.py --project "ecryme/solo-ecryme" --open

```

**Options :**

| Option | Court | Description |
|--------|-------|-------------|
| `--project` | `-p` | Chemin du projet (obligatoire) |
| `--output-name` | `-o` | Nom du ICML sans extension |
| `--no-concat` | | Un ICML par chapitre |
| `--open` | | Ouvrir après génération |
| `--dry-run` | | Simulation sans génération |

**Sortie :** `<projet>/output/<nom>.icml`

---

## Workflow avec schémas ASCII

`build-icml.py` intègre automatiquement la conversion des blocs ASCII en images PNG. Si Pillow est installé, la détection et conversion se font dans la même commande :

```bash
# Commande unique — ASCII art détecté et converti automatiquement
python scripts/build-icml.py --project "ecryme/solo-ecryme"
# → Détecte les blocs ASCII (≥ 3% box-drawing chars)
# → Génère output/ascii-images/*.png (images liées dans l'ICML)
# → Génère output/<nom>.icml avec refs images absolues
```

**Prérequis :** `pip install Pillow` (sinon les blocs ASCII restent en texte brut dans l'ICML).

**Détection des blocs ASCII :** densité ≥ 3% de box-drawing chars parmi le total de chars du bloc.

**Sortie images :** `<projet>/output/ascii-images/` — images PNG liées dans l'ICML via chemin absolu.

---

## Syntaxe Markdown pour les encadrés

Les fenced divs Pandoc (`:::type Titre`) sont convertis en styles ICML nommés par le filtre Lua.

### Convention universelle

```markdown
:::type Titre de l'encadré
Contenu ici. Peut contenir **gras**, *italique*, listes.
:::
```

### Types d'encadrés → 2 styles génériques

**Tous** les types d'encadrés (:::regle, :::mecanisme, :::oracle, :::pnj, etc.) sont convertis en deux styles génériques par le filtre Lua :

| Élément | Style ICML | Description |
|---------|------------|-------------|
| Premier paragraphe du div | `EncartTitre` | Titre de l'encart (gras) |
| Paragraphes suivants | `EncartCorps` | Corps de l'encart |

Le maquettiste applique une mise en forme unique à `EncartTitre` et `EncartCorps` — valable pour tous les univers et tous les types d'encadrés.

### Éléments standards

| Élément Markdown | Style ICML | Description |
|------------------|------------|-------------|
| `# Titre` | `Header1` | Titre de chapitre |
| `## Titre` | `Header2` | Section |
| `### Titre` | `Header3` | Sous-section |
| `#### Titre` | `Header4` | Paragraphe titré |
| `##### Titre` | `Header5` | Micro-titre |
| Corps de texte | `Paragraph` | Corps standard |
| `- item` | `BulList` | Liste à puces |
| `1. item` | `NumList` | Liste numérotée |
| `> citation` | `ReadAloud` | Texte narratif / lu à voix haute |
| `---` | `Separator` | Séparateur `* * *` |
| Cellule tableau | `TablePar` | Corps de cellule |
| En-tête tableau | `TablePar > TableHeader` | En-tête (gras) |

---

## Référence styles — Livrable maquettiste

### Styles de paragraphe (15)

| # | Nom ICML | Rôle | Valeurs ICML par défaut |
|---|----------|------|------------------------|
| 1 | `Header1` | Titre de chapitre | 36pt, Bold |
| 2 | `Header2` | Section | 30pt, Bold |
| 3 | `Header3` | Sous-section | 24pt, Bold |
| 4 | `Header4` | Paragraphe titré | 18pt, Bold |
| 5 | `Header5` | Micro-titre | 14pt, Bold |
| 6 | `Paragraph` | Corps de texte | 10pt, Regular |
| 7 | `BulList` | Liste à puces | 10pt, indent 6pt |
| 8 | `NumList` | Liste numérotée | 10pt, indent 6pt |
| 9 | `TablePar` | Cellule tableau | 9pt, Regular |
| 10 | `TablePar > TableHeader` | En-tête tableau | 9pt, Bold |
| 11 | `EncartTitre` | Titre d'encart | 9pt, Bold, espace avant 6pt |
| 12 | `EncartCorps` | Corps d'encart | 9pt, Regular, espace après 3pt |
| 13 | `ReadAloud` | Citation narrative | 9pt, Italic, indent 8pt |
| 14 | `Separator` | Séparateur | 6pt, centré |
| 15 | `CodeBlock` | Bloc de code | 8pt, Courier New |

### Styles de caractère (3)

| # | Nom ICML | Rôle |
|---|----------|------|
| 1 | `Bold` | Gras inline |
| 2 | `Italic` | Italique inline |
| 3 | `Bold Italic` | Gras italique inline |

**Total : 18 styles nommés.** Tous sont injectés automatiquement dans chaque ICML généré avec des valeurs fonctionnelles — le maquettiste affine la mise en forme dans InDesign sans modifier les fichiers source.

### Procédure de prise en main (maquettiste)

1. **Générer les ICML** : `python scripts/build-icml.py --project "<univers>/<projet>" --no-concat`
2. **Créer le document InDesign** (gabarit, colonnes, marges selon le projet)
3. **Importer un ICML** : Fichier > Importer > sélectionner `output/chapitre01.icml` → placer dans un bloc texte
4. **Les 18 styles apparaissent** dans le panneau Styles de paragraphe avec leurs valeurs par défaut
5. **Affiner les styles** : clic droit sur chaque style > Modifier — la mise en forme se propage à tout le document
6. **Importer les autres chapitres** : répéter l'import pour chaque `.icml` — les styles définis s'appliquent automatiquement (même nom = même style)
7. **Rafraîchissement** : quand `build-icml.py` est relancé, InDesign propose "Mettre à jour les liens" → le texte se met à jour sans toucher à la mise en page

### Cycle de rafraîchissement (lien live)

Quand `build-icml.py` est relancé après modifications dans `chapitres/` :

1. InDesign détecte que le `.icml` lié a été modifié
2. Un message propose de rafraîchir les fichiers liés
3. Cliquer "Mettre à jour" → le texte se met à jour en conservant la mise en page

---

## Organisation du dossier output/

```
output/
├── *.icml                 ← racine — liens InDesign (emplacement immuable)
├── ascii-images/          ← PNG générés depuis blocs ASCII art
├── [assets]/              ← sous-dossiers pour assets spécialisés (deck/, fiches/…)
├── indesign/              ← sources maquette InDesign
│   ├── <nom>.indd         ← WIP courant (écrasé à chaque session)
│   ├── <nom>-vX.Y.indd    ← jalons archivés manuellement
│   └── <nom>.indb         ← book InDesign (si multi-documents)
└── releases/              ← exports PDF finaux
    ├── <nom>-v0.3.pdf
    └── <nom>-v1.0.pdf
```

### Règles de nommage

**ICML (racine)** — emplacement fixe, jamais déplacé ni renommé
- Concat : `<nom-projet>.icml`
- Par chapitre : `chapitre01.icml`, `annexes.icml`, etc.

**InDesign (`indesign/`)** — sources maquette
- Fichier courant : `<nom-projet>.indd` (sans numéro de version — WIP continu)
- Jalons : `<nom-projet>-vX.Y.indd` (copie manuelle avant envoi relecteur, impression…)
- Book : `<nom-projet>.indb`

**Releases (`releases/`)** — exports PDF
- Toujours versionné : `<nom-projet>-vX.Y.pdf`
- `v0.x` = brouillon/relecture, `v1.0+` = release publique
- Pas de `<nom-projet>.pdf` sans version — évite l'ambiguïté

### Gitignore par projet

```gitignore
# Pipeline ICML — générés, reconstruisibles
output/*.icml
output/ascii-images/

# InDesign — binaires, non diffables
output/indesign/

# output/releases/ est tracké (livrables de référence)
```

### Chemins InDesign (liens relatifs)

Quand le `.indd` est dans `output/indesign/`, les liens vers les ICML utilisent des chemins relatifs :
- `../chapitre01.icml` → `output/chapitre01.icml`
- `../ascii-images/chapitre02-ascii-01.png`

**Migration d'un projet existant** (`.indd` à la racine de `output/`) : déplacer le `.indd` dans `output/indesign/`, ouvrir InDesign → "Relier les liens manquants" en pointant vers `output/`. Opération unique.

---

## Configuration bank.yml

Ajouter la section `icml:` au `bank.yml` du projet si nécessaire :

```yaml
icml:
  chapitres-source: "chapitres/"
  chapitres-order:
    - "chapitres/chapitre01.md"
    - "chapitres/chapitre02.md"
  output: "output/mon-projet.icml"
```

Si `chapitres-order` n'est pas défini, les chapitres sont concaténés par ordre alphabétique.

---

## Validation

### Test du filtre Lua

```bash
# Test minimal
echo ":::secret Mon Secret" > test.md
echo "Contenu secret." >> test.md
echo ":::" >> test.md
pandoc test.md --lua-filter=scripts/pandoc/icml-filter.lua -f markdown+fenced_divs -t icml -o test.icml
# Vérifier: test.icml contient "SecretBoxTitle" et "SecretBoxBody"
```

### Test intégration

```bash
# Vérification sans génération
python scripts/build-icml.py --project "ecryme/solo-ecryme" --dry-run

# Génération complète
python scripts/build-icml.py --project "ecryme/solo-ecryme" --open
```

### Validation ICML

Ouvrir le `.icml` dans un éditeur texte et vérifier :
- Les `<ParagraphStyleRange>` contiennent les `AppliedParagraphStyle` nommés
- Les styles correspondent au mapping attendu

---

**Version :** 2.1
**Date :** 2026-02-20
**Changelog 2.1 :** Intégration ascii-to-images dans build-icml.py (commande unique). Remplacement police Minion Pro → Georgia via patch_icml_fonts(). Système de styles YAML (icml-styles.yml) — 18 styles injectés automatiquement. Simplification encadrés : tous types → EncartTitre/EncartCorps. Images liées via --resource-path Pandoc.
**Changelog 2.0 :** Pipeline unique ICML (suppression pipeline LaTeX/preview PDF). Ajout section cycle rafraîchissement InDesign.
**Changelog 1.0 :** Pipeline ICML initial (2026-02-11)
