---
name: pbta-simplifie
description: Système PbtA simplifié pour CYOA - 2d6, profils, traits évolutifs
---

# Système PbtA Simplifié pour CYOA

## Vue d'Ensemble

Système inspiré d'Apocalypse World, allégé pour livres Choose Your Own Adventure:
- **Jets:** 2d6 uniquement
- **Résultats:** 6- (échec), 7-9 (partiel), 10+ (succès)
- **Pas de stats fixes:** Profils de départ + traits acquis
- **Évolution:** Traits collectés pendant l'aventure

## Création de Personnage

### Profils de Départ

Le joueur choisit UN profil au §0, qui détermine son approche initiale.

**Format profil:**
```markdown
## [NOM PROFIL]

**Concept:** [Description en une phrase]

**Traits de départ:**
□ [Trait 1]: [Description et effet]
□ [Trait 2]: [Description et effet]
□ [Trait 3]: [Description et effet]

**Équipement:**
□ [Objet 1]
□ [Objet 2]

**Allez au §1**
```

**Exemples de profils:**

### LE COMBATTANT
**Concept:** Guerrier expérimenté au combat

**Traits de départ:**
□ **Lame Affûtée**: +1 quand tu attaques au corps-à-corps
□ **Résistant**: Ignore la 1ère blessure d'un combat
□ **Intimidant**: +1 quand tu menaces quelqu'un

**Équipement:**
□ Épée longue
□ Armure légère

### L'EXPLORATEUR
**Concept:** Aventurier rusé et observateur

**Traits de départ:**
□ **Œil Vif**: +1 quand tu cherches des indices
□ **Agile**: +1 quand tu évites un danger
□ **Débrouillard**: Trouve toujours une solution de fortune

**Équipement:**
□ Arc court
□ Kit d'exploration

### LE DIPLOMATE
**Concept:** Négociateur charismatique

**Traits de départ:**
□ **Persuasif**: +1 quand tu parlementes
□ **Réseau**: Tu connais toujours quelqu'un qui peut aider
□ **Lecture des Gens**: Détecte les mensonges facilement

**Équipement:**
□ Vêtements nobles
□ Bourse de pièces d'or

## Système de Jets (2d6)

### Quand Lancer les Dés ?

Quand tu fais une **action risquée** avec incertitude:
- Combat
- Négociation tendue
- Exploration dangereuse
- Utilisation de compétences sous pression

**Ne lance PAS les dés** quand:
- Action triviale sans risque
- Résultat narrativement déterminé
- Tu as un trait qui rend l'action automatique

### Résultats des Jets

**Lance 2d6:**

- **6-** : **Échec** - Conséquences négatives
  - Tu rates complètement
  - La situation empire
  - Tu perds quelque chose (PV, objet, temps)
  - → Section "échec"

- **7-9** : **Réussite partielle** - Succès avec un coût
  - Tu réussis MAIS...
  - Tu dois choisir entre options difficiles
  - Tu perds quelque chose de mineur
  - → Section "partiel"

- **10+** : **Succès complet** - Tu obtiens ce que tu veux
  - Réussite totale
  - Avantage bonus possible
  - → Section "succès"

### Modificateurs aux Jets

**+1 par trait applicable** (maximum +3 par jet)

**Exemple:**
```
Tu tentes d'escalader le mur (action risquée).
Tu possèdes les traits:
□ Agile (+1)
□ Grimpeur Expérimenté (+1)

Lance 2d6 + 2 (tes deux traits)
```

**Blessures:**
- 1-2 blessures: -1 aux jets
- 3-4 blessures: -2 aux jets
- 5+ blessures: Mort

## Format des Sections avec Jets

### Template Standard

```markdown
§[NUM] - [TITRE]

[Description situation - 2-4 phrases]
[Ambiance et détails sensoriels]

Tu dois [action risquée].

**Lance 2d6** [+ modificateurs si traits applicables]

• **10+** (Succès complet): [Résultat positif] → §[X]
• **7-9** (Partiel): [Succès avec coût/choix] → §[Y]
• **6-** (Échec): [Conséquence négative] → §[Z]
```

### Exemple Concret

```markdown
§42 - LE MUR D'ESCALADE

Tu te retrouves face à un mur de pierre de 5 mètres.
Les prises sont glissantes, humides. En contrebas,
le gouffre s'ouvre, menaçant.

Tu tentes l'escalade.

**Lance 2d6** [+1 si Agile, +1 si Grimpeur, +1 si Équipement d'escalade]

• **10+**: Tu escalades avec aisance et rapidité → §50
• **7-9**: Tu réussis mais perds 1 objet dans la montée → §51
• **6-**: Tu glisses et chutes, 2 blessures → §52
```

### Section avec Choix Partiel

```markdown
§51 - ESCALADE DIFFICILE

Tu parviens en haut du mur, essoufflé. Durant la montée,
un de tes objets a glissé de ta ceinture.

**Choisis ce que tu perds:**
• Corde (barre objet) → §53
• Rations (barre objet) → §53
• Bourse d'or (barre objet) → §53
```

## Système de Traits

### Traits de Départ

Définis par le profil choisi au §0 (3 traits typiquement).

### Acquisition de Nouveaux Traits

Les traits s'acquièrent durant l'aventure:

**Méthodes:**
- Récompense après succès majeur
- Entraînement auprès d'un mentor (section spéciale)
- Découverte d'objet magique/technologique
- Révélation sur soi-même

**Format acquisition:**
```markdown
§[X] - NOUVELLE COMPÉTENCE ACQUISE

[Narration événement]

**Nouveau trait acquis:**
□ **[Nom Trait]**: [Description effet]

Coche cette case dans ta liste de traits.

→ Continue au §[Y]
```

**Exemple:**
```markdown
§75 - L'ENSEIGNEMENT DU MAÎTRE

Le vieux guerrier t'enseigne sa technique secrète.

**Nouveau trait acquis:**
□ **Contre-Attaque**: Quand un ennemi te rate, +1 à ta riposte immédiate

Coche ce trait sur ta feuille de personnage.

→ Tu quittes le dojo → §80
```

### Limite de Traits

**Maximum 6-8 traits** par personnage (équilibrage).

Si nouveau trait acquis avec maximum atteint:
```markdown
Tu as atteint le maximum de traits. Choisis-en un à remplacer:
• Abandonner [Trait ancien] pour gagner [Trait nouveau]
• Refuser le nouveau trait et garder l'ancien
```

## Feuille de Personnage

### Template

```markdown
# FEUILLE DE PERSONNAGE

**Profil:** _______________

**TRAITS:**
□ _________________ (+1 à _________)
□ _________________ (+1 à _________)
□ _________________ (+1 à _________)
□ _________________ (effet: _______)
□ _________________ (effet: _______)
□ _________________ (effet: _______)

**BLESSURES:** □□□□□ (5 max, mort si toutes cochées)

**ÉQUIPEMENT:**
□ _________________
□ _________________
□ _________________
□ _________________
□ _________________

**NOTES:**
_______________________________
_______________________________
```

## Gestion des Blessures

**Subir des blessures:**
```
Coche [X] cases BLESSURES: □□□□□

Effets:
- 1-2 cases: -1 aux jets
- 3-4 cases: -2 aux jets
- 5 cases: Mort → §MORT
```

**Soigner:**
```
Repos complet (section spéciale): Décoche 2 cases
Potion/soin: Décoche X cases selon puissance
```

## Exemples de Traits

### Traits de Combat
- **Lame Affûtée**: +1 attaque mêlée
- **Archer Précis**: +1 attaque distance
- **Parade Experte**: +1 défense
- **Frappe Puissante**: +1 dégât (1 blessure → 2)
- **Résistant**: Ignore 1ère blessure/combat

### Traits d'Exploration
- **Œil Vif**: +1 chercher indices
- **Agile**: +1 éviter dangers
- **Grimpeur**: +1 escalade
- **Discret**: +1 furtivité
- **Survie**: Trouve nourriture/eau facilement

### Traits Sociaux
- **Persuasif**: +1 négociation
- **Intimidant**: +1 menaces
- **Empathique**: Détecte émotions
- **Réseau**: Connais toujours quelqu'un
- **Menteur Doué**: +1 bluff

### Traits Spéciaux
- **Chanceux**: Relance 1 jet/aventure
- **Débrouillard**: Improvise solutions
- **Sixième Sens**: Pressent dangers
- **Mémoire Parfaite**: Retiens tout
- **Main Leste**: +1 pickpocket/serrures

## Intégration avec Univers

Ce système est **agnostique** et s'adapte à tout univers:

**Fantasy:**
- Traits: Magie élémentaire, Bénédiction divine, etc.

**SF:**
- Traits: Pilotage expert, Hacking, Cybernétiques, etc.

**Contemporain:**
- Traits: Conduite, Informatique, Combat urbain, etc.

**Horreur:**
- Traits: Résistance mentale, Occultisme, etc.

**Adapter via:**
- Noms de traits thématiques
- Descriptions flavor texte
- Équipement adapté

## Conseils d'Écriture CYOA avec ce Système

### Clarté des Jets

Toujours préciser:
- Quand lancer (action risquée claire)
- Modificateurs applicables (traits)
- 3 résultats distincts (6-/7-9/10+)

### Équilibrage

- **40% sections** nécessitent jets
- **60% sections** narration/choix purs

### Variété

Alterner types de jets:
- Combat (physical)
- Social (persuasion)
- Exploration (découverte)
- Mystère (déduction)

### Récompenses Traits

**Fréquence:**
- 1 nouveau trait toutes les 15-20 sections
- Ou après boss majeur
- Ou découverte importante

## Compatibilité avec Autres Styles

Ce système peut se combiner avec:
- `cyoa-standard.md` → Structure sections
- `roue-du-temps-narratif.md` → Flavor texte WoT
- Autres univers → Adapter noms traits/équipement

Les skills `narrative-writer` et `scenario-writer` détecteront automatiquement ce système et l'intégreront.
