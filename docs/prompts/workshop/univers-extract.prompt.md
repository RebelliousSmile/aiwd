---
name: univers-extract
description: Extrait et organise les informations d'univers depuis des sources brutes vers des fichiers thématiques structurés.
---

# Univers Extract

> **TL;DR** : Sources brutes → Fichiers thématiques (250 lignes max). Valide les thèmes, extrait, synthétise si besoin, demande arbitrage si choix difficile. [6]

## Objectif

Transformer des fichiers sources bruts (extractions PDF, notes, textes) en fichiers thématiques structurés pour un univers, prêts à être utilisés par les prompts workshop.

## Entrées

- **Univers cible** : $1 (identifiant : archipels, wot, ecryme, zombiology, demonslayer, otherscape, mina)
- **Fichiers sources** : $2, $3, ... (chemins vers les fichiers à traiter)
- **Options** :
  - `--update` : Mode incrémental, enrichit les fichiers existants [2]
  - `--force` : Régénère tout même si fichiers existent

## Formats acceptés

- Fichiers texte : `.txt`, `.md`
- Fichiers LaTeX : `.tex`
- Encodage : UTF-8 recommandé (sinon, normaliser avec `scripts/normalize-text.py`)

## Paramètres

- **Taille max par fichier** : 250 lignes
- **Seuil thème custom** : 50 lignes minimum de contenu unique
- **Thèmes standard** : terminologie, histoire, geographie, factions, personnages
- **Thèmes optionnels** : magie, technologie, creatures, religions, economie (templates fournis)
- **Priorité de traitement** [5] : terminologie > factions > personnages > histoire > geographie > autres

## Sortie

Fichiers générés dans `<univers>/.docs/` :
- `terminologie.md` (obligatoire) - vocabulaire univers uniquement, pas de règles de jeu
- `[thematique].md` (selon contenu détecté : histoire, geographie, factions, personnages, etc.)

**Si exécuté au niveau projet** (`<univers>/<projet>/`), peut aussi générer :
- `<projet>/.docs/document-rules.md` - règles spécifiques introduites par ce document (distinctes des règles système)

---

## Étapes

### 0. Validation des sources

Avant toute extraction, vérifier pour chaque fichier source :

- [ ] Le fichier existe et est accessible
- [ ] Le fichier n'est pas vide
- [ ] L'encodage est lisible (UTF-8 ou convertible)

**Mode `--update` [2] :**
- [ ] Lister les fichiers `.docs/` existants
- [ ] Identifier les sources déjà traitées (via métadonnées)
- [ ] Ne traiter que les nouvelles sources

**Si problème détecté :**
```
Erreur source : [fichier]
- Problème : [fichier introuvable / vide / encodage illisible]
- Action : [corriger le chemin / fournir un autre fichier / lancer normalize-text.py]

Continuer avec les autres sources ? (oui/non)
```

**Rapport de validation :**
```
Sources validées : [N]/[total]
Volume total estimé : ~[X] lignes
Langues détectées : [français, anglais, ...]
Mode : [création / mise à jour]
Fichiers existants : [liste si --update]
```

### 1. Lecture et analyse des sources

Lire tous les fichiers sources validés. Noter :
- Volume total de texte
- Qualité (structuré vs brut)
- Langue principale

**Gestion multilingue :**
- Langue cible : français
- Termes spécifiques à l'univers : conserver l'original entre parenthèses à la première mention
- Ex: "Le Pouvoir Unique (*One Power*) permet de canaliser..."

### 2. Détection des thèmes

Analyser le contenu et identifier les thèmes présents.

**Pour chaque thème standard**, évaluer :
- Présent : oui/non
- Volume estimé : faible (< 50 lignes) / moyen (50-150) / important (> 150)
- Pertinence pour cet univers

**Thèmes custom :**
Créer un thème custom si :
- Le contenu ne rentre dans aucun thème standard
- Volume > 50 lignes
- Cohérence thématique claire

### 3. Validation des thèmes

Présenter à l'utilisateur :

```
Thèmes détectés dans les sources :

| Thème | Présent | Volume | Priorité | Recommandation |
|-------|---------|--------|----------|----------------|
| terminologie | oui | moyen | 1 | Obligatoire |
| factions | oui | important | 2 | Recommandé |
| personnages | oui | moyen | 3 | Recommandé |
| histoire | oui | important | 4 | Recommandé |
| geographie | oui | faible | 5 | Optionnel |
| [custom] | oui | moyen | 6 | Proposé |

Valides-tu cette liste ? (oui / modifier)
```

### 4. Extraction par thème

Pour chaque thème validé (dans l'ordre de priorité [5]), extraire les informations pertinentes.

**Priorité d'extraction (garder) :**
1. Noms propres, termes uniques à l'univers
2. Relations entre entités (alliances, conflits, hiérarchies)
3. Dates et événements structurants
4. Règles ou lois de l'univers (magie, physique, social)
5. Nuances et exceptions importantes

**Déprioritiser (couper si nécessaire) :**
1. Descriptions redondantes
2. Détails de couleur sans impact narratif
3. Exemples multiples du même concept
4. Texte d'ambiance générique
5. Méta-informations (numéros de page, références éditoriales)

**Règle des doublons inter-fichiers [4] :**

| Information | Fichier principal | Mention secondaire |
|-------------|-------------------|-------------------|
| Fondation d'une faction | histoire.md | factions.md (date seulement) |
| Lieu de résidence d'un PNJ | personnages.md | geographie.md (liste habitants) |
| Événement impliquant une faction | histoire.md | factions.md (conséquences) |
| Terme technique | terminologie.md | Jamais dupliqué |

**Principe** : L'information complète va dans UN fichier, les autres fichiers font référence ou résument.

### 5. Détection des contradictions [1]

Pendant l'extraction, si deux sources donnent des informations contradictoires :

```
Contradiction détectée :

Source A ([fichier]) : "[information A]"
Source B ([fichier]) : "[information B]"

Sujet : [ex: date de fondation de la Guilde]
Fichier cible : [factions.md]

Quelle source fait autorité ? (A/B/fusionner/ignorer)
```

### 6. Aperçu avant synthèse

Afficher un résumé intermédiaire :

```
Extraction brute terminée :

| Fichier | Lignes | Statut | Priorité |
|---------|--------|--------|----------|
| terminologie.md | 120 | OK | 1 |
| factions.md | 280 | DÉPASSE (+30) | 2 |
| histoire.md | 310 | DÉPASSE (+60) | 4 |

Fichiers à synthétiser : factions.md, histoire.md
Ordre de traitement : factions.md (priorité 2), puis histoire.md (priorité 4)

Continuer vers la synthèse ? (oui / voir détails [fichier])
```

### 7. Vérification taille et synthèse

Pour chaque fichier > 250 lignes (dans l'ordre de priorité) :

#### Synthèse automatique

Appliquer si les critères suivants sont remplis :
- Réduction possible de > 20% par fusion de redondances
- Aucune entité nommée majeure supprimée
- Pas de perte de relations clés

**Actions automatiques :**
- Fusionner descriptions redondantes
- Réduire exemples multiples à 1-2
- Supprimer méta-informations
- Déplacer doublons vers fichier principal (cf. règle étape 4)

#### Arbitrage humain

Déclencher si :
- Synthèse automatique insuffisante (réduction < 20%)
- Plus de 3 entités nommées seraient supprimées
- Relations clés seraient perdues

**Format de la demande d'arbitrage :**

```
Le fichier [thème].md dépasse 250 lignes (actuellement [N] lignes).
Synthèse automatique : -[X] lignes (insuffisant, besoin -[Y] total)

Je dois choisir quoi retirer. Options :

1. [Option A] - Perte : [entités/relations affectées]
2. [Option B] - Perte : [entités/relations affectées]
3. [Option C] - Perte : [entités/relations affectées]

Que préfères-tu ? (1/2/3/autre suggestion)
```

### 8. Génération des fichiers

#### terminologie.md (template)

**IMPORTANT :** Ce fichier contient uniquement le vocabulaire propre à l'univers fictif.
Les règles de jeu (attributs, dés, mécaniques) vont dans `rules-files/`.

```markdown
# Terminologie: [Nom Univers]

## Termes Principaux

| Terme | Définition |
|-------|------------|
| [terme1] | [définition courte] |

## Noms Propres

### Lieux

| Nom | Type | Notes |
|-----|------|-------|
| [Lieu1] | Ville | [description courte] |

### Personnages

| Nom | Titre/Fonction | Notes |
|-----|----------------|-------|
| [Perso1] | [titre] | [description courte] |

### Organisations

| Nom | Description |
|-----|-------------|
| [Orga1] | [rôle] |

## Termes à Ne Pas Utiliser

| Incorrect | Correct | Raison |
|-----------|---------|--------|
| [terme faux] | [terme juste] | [explication] |

---
**Lignes:** [N]/250
**Màj:** [date]
```

#### histoire.md (template) [3]

```markdown
# Histoire: [Nom Univers]

## Chronologie

| Période/Date | Événement | Impact |
|--------------|-----------|--------|
| [date] | [événement] | [conséquences] |

## Ères / Périodes

### [Nom Période 1]

**Dates:** [début] - [fin]
**Caractéristiques:** [description]

[Paragraphe résumé]

### [Nom Période 2]

...

## Événements Majeurs

### [Événement 1]

**Date:** [date]
**Acteurs:** [qui]
**Cause:** [pourquoi]
**Conséquences:** [impact]

[Paragraphe détail]

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

#### factions.md (template) [3]

```markdown
# Factions: [Nom Univers]

## Vue d'Ensemble des Relations

```
[Faction A] --alliance--> [Faction B]
[Faction A] --conflit--> [Faction C]
[Faction B] --neutre--> [Faction C]
```

## Factions Majeures

### [Faction 1]

**Type:** [gouvernement / guilde / ordre / culte / ...]
**Siège:** [lieu]
**Chef:** [nom] (voir personnages.md)
**Objectifs:** [buts]
**Ressources:** [forces, richesses, influence]

**Relations:**
- Allié de : [factions]
- Ennemi de : [factions]
- Neutre avec : [factions]

[Paragraphe description]

### [Faction 2]

...

## Factions Mineures

| Nom | Type | Influence | Notes |
|-----|------|-----------|-------|
| [Faction] | [type] | [faible/moyenne] | [description courte] |

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

#### geographie.md (template) [3]

```markdown
# Géographie: [Nom Univers]

## Carte Mentale

```
[Région Nord]
    └── [Ville A]
    └── [Ville B]
[Région Sud]
    └── [Ville C]
[Mer / Frontière]
```

## Régions

### [Région 1]

**Climat:** [description]
**Terrain:** [description]
**Ressources:** [liste]
**Population:** [estimation]

**Lieux notables:**
- [Lieu A] : [description courte]
- [Lieu B] : [description courte]

### [Région 2]

...

## Lieux Importants

### [Lieu 1]

**Type:** [ville / forteresse / ruine / ...]
**Région:** [région]
**Population:** [si applicable]
**Particularité:** [ce qui le rend unique]

[Paragraphe description]

## Distances et Voyages

| De | À | Distance | Durée |
|----|---|----------|-------|
| [A] | [B] | [km/lieues] | [jours] |

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

#### personnages.md (template) [3]

```markdown
# Personnages: [Nom Univers]

## Personnages Majeurs

### [Personnage 1]

**Nom complet:** [nom]
**Titre/Fonction:** [titre]
**Affiliation:** [faction] (voir factions.md)
**Lieu:** [résidence]

**Description:** [apparence, traits marquants]
**Personnalité:** [traits de caractère]
**Objectifs:** [motivations]
**Secrets:** [informations cachées - pour le MJ]

**Relations:**
- [Perso B] : [relation]
- [Faction X] : [relation]

### [Personnage 2]

...

## Personnages Secondaires

| Nom | Fonction | Lieu | Notes |
|-----|----------|------|-------|
| [Perso] | [rôle] | [lieu] | [trait distinctif] |

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

#### magie.md (template - thème optionnel)

```markdown
# Magie: [Nom Univers]

## Vue d'Ensemble

**Nom du système:** [ex: Le Pouvoir Unique, l'Art, la Trame]
**Source:** [d'où vient la magie]
**Qui peut l'utiliser:** [conditions, restrictions]
**Perception sociale:** [acceptée / crainte / interdite]

[Paragraphe de présentation]

## Fonctionnement

### Principes Fondamentaux

- **[Principe 1]:** [explication]
- **[Principe 2]:** [explication]

### Limitations

| Limitation | Description | Conséquence |
|------------|-------------|-------------|
| [limite 1] | [description] | [ce qui arrive si dépassée] |

### Coût / Prix

**Utilisation:** [fatigue / composants / sacrifice / ...]
**Abus:** [conséquences de la surexploitation]

## Types / Écoles / Traditions

### [Type 1]

**Spécialité:** [domaine]
**Pratiquants:** [qui]
**Techniques:** [exemples]

### [Type 2]

...

## Objets Magiques

| Nom | Type | Effet | Rareté |
|-----|------|-------|--------|
| [objet] | [catégorie] | [ce qu'il fait] | [commun/rare/unique] |

## Dangers et Tabous

- **[Danger 1]:** [description]
- **[Tabou 1]:** [ce qui est interdit et pourquoi]

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

#### technologie.md (template - thème optionnel)

```markdown
# Technologie: [Nom Univers]

## Niveau Technologique

**Ère équivalente:** [ex: médiéval, Renaissance, victorien, futuriste]
**Particularités:** [ce qui diffère de notre histoire]

## Domaines Clés

### [Domaine 1 - ex: Transport]

**Niveau:** [primitif / développé / avancé]
**Technologies principales:**
- [techno 1] : [description]
- [techno 2] : [description]

**Limitations:** [ce qui n'existe pas encore]

### [Domaine 2 - ex: Communication]

...

### [Domaine 3 - ex: Armement]

...

## Inventions Notables

| Invention | Inventeur | Date | Impact |
|-----------|-----------|------|--------|
| [invention] | [qui] | [quand] | [changement apporté] |

## Énergie et Ressources

**Sources d'énergie:** [vapeur / magie / électricité / ...]
**Ressources rares:** [matériaux précieux, leur usage]

## Restrictions et Contrôle

- **Qui contrôle:** [guildes / état / libre]
- **Technologies interdites:** [liste]
- **Accès:** [élite / masse / régional]

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

#### creatures.md (template - thème optionnel)

```markdown
# Créatures: [Nom Univers]

## Classification

| Catégorie | Description | Exemples |
|-----------|-------------|----------|
| [catégorie 1] | [définition] | [créatures] |
| [catégorie 2] | [définition] | [créatures] |

## Créatures Majeures

### [Créature 1]

**Type:** [catégorie]
**Habitat:** [où la trouver]
**Comportement:** [hostile / neutre / allié]
**Dangerosité:** [faible / moyenne / élevée / mortelle]

**Description:** [apparence]
**Capacités:** [pouvoirs, attaques]
**Faiblesses:** [points faibles]

**Rôle narratif:** [pourquoi elle existe dans l'univers]

### [Créature 2]

...

## Créatures Mineures

| Nom | Type | Habitat | Danger | Notes |
|-----|------|---------|--------|-------|
| [créature] | [type] | [lieu] | [niveau] | [trait distinctif] |

## Créatures Intelligentes (non-humaines)

### [Peuple 1]

**Nom:** [nom propre et nom commun]
**Origine:** [histoire]
**Territoire:** [où ils vivent]
**Culture:** [traits culturels]
**Relations avec humains:** [alliés / neutres / hostiles]

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

#### religions.md (template - thème optionnel)

```markdown
# Religions: [Nom Univers]

## Panorama Religieux

**Religion dominante:** [nom]
**Pluralisme:** [monothéisme / polythéisme / mixte]
**Place dans la société:** [centrale / marginale]

## Religions Majeures

### [Religion 1]

**Nom:** [nom officiel]
**Type:** [monothéiste / polythéiste / animiste / ...]
**Divinité(s):** [noms des dieux]
**Symbole:** [emblème]
**Clergé:** [structure, titres]

**Croyances:**
- [dogme 1]
- [dogme 2]

**Pratiques:**
- [rituel 1]
- [rituel 2]

**Lieux saints:** [temples, sites]
**Fêtes:** [célébrations importantes]

**Influence:** [politique / sociale / militaire]

### [Religion 2]

...

## Cultes Mineurs / Sectes

| Nom | Type | Influence | Légalité | Notes |
|-----|------|-----------|----------|-------|
| [culte] | [nature] | [faible/locale] | [légal/interdit] | [particularité] |

## Conflits Religieux

- **[Conflit 1]:** [description]
- **[Schisme]:** [division et raison]

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

#### economie.md (template - thème optionnel)

```markdown
# Économie: [Nom Univers]

## Vue d'Ensemble

**Système:** [féodal / capitaliste / mercantile / ...]
**Monnaie:** [noms, valeurs relatives]
**Richesse:** [comment on devient riche]

## Monnaies

| Nom | Valeur | Matériau | Usage |
|-----|--------|----------|-------|
| [pièce 1] | 1 | [or/argent/cuivre] | [courant/élite] |
| [pièce 2] | 10 [pièce 1] | [matériau] | [usage] |

**Équivalences utiles:**
- Repas simple : [X pièces]
- Nuit d'auberge : [X pièces]
- Cheval : [X pièces]
- Épée : [X pièces]

## Commerce

### Routes Commerciales

| Route | De → À | Marchandises | Contrôle |
|-------|--------|--------------|----------|
| [route] | [origine] → [destination] | [produits] | [qui contrôle] |

### Marchandises Principales

| Produit | Origine | Valeur | Notes |
|---------|---------|--------|-------|
| [produit] | [région] | [haute/moyenne/basse] | [particularité] |

## Guildes et Monopoles

| Guilde | Secteur | Siège | Influence |
|--------|---------|-------|-----------|
| [guilde] | [domaine] | [ville] | [forte/moyenne] |

## Classes Sociales

| Classe | Revenus | Population | Pouvoir |
|--------|---------|------------|---------|
| [noblesse] | [description] | [%] | [élevé] |
| [marchands] | [description] | [%] | [moyen] |
| [paysans] | [description] | [%] | [faible] |

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

### 9. Mise à jour des métadonnées

Si mode `--update`, mettre à jour les métadonnées de chaque fichier :

```markdown
---
**Lignes:** [N]/250
**Sources:** [anciens + nouveaux fichiers]
**Màj:** [date]
**Historique:**
- [date1] : Création depuis [sources]
- [date2] : Ajout depuis [nouvelles sources]
```

### 10. Rapport final

```
Extraction terminée pour [univers].

Fichiers générés/mis à jour :
- terminologie.md ([N] lignes) ✓
- factions.md ([N] lignes) ✓
- histoire.md ([N] lignes) ✓
- ...

Total : [X] fichiers, [Y] lignes
Mode : [création / mise à jour]
Arbitrages effectués : [N]
Contradictions résolues : [N]
Sources traitées : [liste]

Prochaine étape : Mettre à jour bank.yml des projets pour référencer ces fichiers et renomme les fichiers source avec le suffixe `.processed`.
```

---

## Exemple concret

### Entrée
```
@univers-extract.prompt.md archipels sources/setting.txt sources/factions.md
```

### terminologie.md généré (extrait)

```markdown
# Terminologie: Archipels

## Termes Principaux

| Terme | Définition |
|-------|------------|
| Îles Connues | Archipel cartographié et fréquenté par les navigateurs |
| Sargasses | Eaux mortes où s'accumulent les algues, piège pour navires |
| Marée d'Argent | Phénomène lunaire qui révèle les passes secrètes |

## Termes à Ne Pas Utiliser

| Incorrect | Correct | Raison |
|-----------|---------|--------|
| Océan | La Mer | Un seul océan, jamais nommé "océan" |
| Capitaine | Maître de bord | Titre officiel dans les Îles |
```

### factions.md généré (extrait)

```markdown
# Factions: Archipels

## Vue d'Ensemble des Relations

```
Ordre des Navigateurs --alliance--> Guilde des Cartographes
Ordre des Navigateurs --tension--> Marchands Libres
Marchands Libres --conflit--> Confrérie des Contrebandiers
```

## Factions Majeures

### Ordre des Navigateurs

**Type:** Ordre professionnel
**Siège:** Vendrest
**Chef:** Maître Elric (voir personnages.md)
**Objectifs:** Contrôler la navigation, cartographier, former
**Ressources:** Savoir, influence politique, flotte modeste

**Relations:**
- Allié de : Guilde des Cartographes
- En tension avec : Marchands Libres (concurrence)
- Neutre avec : Autorités portuaires

L'Ordre des Navigateurs détient le monopole de la certification des
capitaines dans les Îles Connues. Sans leur sceau, aucun navire ne peut
légalement quitter un port majeur.
```

---

## Notes

- Si un thème < 30 lignes, proposer fusion avec INDEX.md ou thème proche
- Sources en anglais traduites ; termes uniques conservés entre parenthèses
- En cas de doute sur une coupe, toujours demander arbitrage
- Mode `--update` préserve le contenu existant et ajoute les nouvelles informations [2]
