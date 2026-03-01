---
name: brainstorm
description: Iterer sur l'overview du projet, challenger les concepts, proposer des alternatives jusqu'a generate-toc
argument-hint: Chemin vers le projet (ex: "zombiology/quonleurcoupelatete")
---

# Brainstorm

## Goal

Accompagner l'utilisateur dans l'elaboration et le raffinement de son concept de projet, en challengeant les idees, proposant des alternatives, et iterant jusqu'a ce que l'overview soit suffisamment solide pour passer a la generation de la table des matieres.

**Duree typique:** 3-8 iterations avant d'atteindre les criteres de sortie.

**Boucle:**
```
Analyser -> Challenger -> Proposer -> Valider -> Mettre a jour -> Repeter
```

## Context

### Configuration Projet

```yaml
@$ARGUMENTS/bank.yml
```

**Si bank.yml est absent ou invalide:**
```
Le projet n'a pas de fichier bank.yml valide.
Veuillez d'abord creer la configuration du projet avec le template:
@docs/templates/bank.yml
```

### Chargement des Ressources

Lire bank.yml et charger **tous** les fichiers declares:

1. **Overview:** Le fichier reference dans `overview`
2. **Documentation:** Tous les fichiers references dans la section `docs`
3. **Type de document:** Noter `document.type` pour adapter les questions (defaut: "scenario")

Ne pas inventer de chemins. Suivre uniquement les liens presents dans bank.yml.

## Rules

- Sortie en francais
- Mode conversationnel: poser des questions, ne pas imposer
- Challenger les idees sans les rejeter
- Proposer des alternatives, pas des remplacements
- **Maximum 2-3 questions par iteration** (ne pas submerger l'utilisateur)
- Mettre a jour l'overview a chaque iteration validee
- Ne jamais inventer de contenu sans validation utilisateur
- MOINS C'EST PLUS: un concept clair vaut mieux que dix idees floues
- Utiliser la documentation univers pour verifier la coherence
- Si un sujet manque de documentation, suggerer `@research.prompt.md "<sujet>"`

## Etapes

### 0. Verifier l'Existence de l'Overview

**Si l'overview n'existe pas:**

```markdown
Je vois que le projet n'a pas encore d'overview.

Pour commencer, decris-moi ton concept en quelques phrases:
- De quoi parle ce projet?
- Quel est le genre/ton vise?
- Qui sont les protagonistes?
```

Apres les premieres reponses, creer un overview minimal avec cette structure:

```markdown
# [Titre du projet]

## Pitch
[2-3 phrases de concept]

## Structure
[A definir]

## Protagonistes
[Liste initiale]

## Ton
[Atmosphere visee]
```

**Si l'overview existe:** Passer a l'etape 1.

### 1. Analyser l'Overview Existant

Lire le fichier overview (depuis bank.yml) et identifier:

**Elements presents:**
- Concept central (pitch)
- Structure narrative (actes, seances, chapitres)
- Personnages principaux
- Themes
- Ton et atmosphere

**Elements manquants ou flous:**
- Zones d'ombre narratives
- Motivations non explicites
- Conflits non resolus
- Questions sans reponse

### 2. Challenger le Concept

Poser des questions pour tester la solidite du concept. Utiliser la documentation univers pour verifier la coherence.

**Format standard de question:**
```
Je remarque que [observation].
- Option A: [alternative 1]
- Option B: [alternative 2]
- Option C: conserver tel quel

Qu'en penses-tu?
```

**Questions de fond:**
- Pourquoi cette histoire doit-elle etre racontee?
- Qu'est-ce qui la rend unique?
- Quel est l'enjeu central pour les protagonistes?

**Questions de coherence:**
- Les motivations des antagonistes sont-elles credibles?
- Les obstacles sont-ils a la hauteur des enjeux?
- La progression est-elle logique?

**Questions pratiques:**
- Le scope est-il realiste pour le format vise?
- Les ressources documentaires sont-elles suffisantes?
- Y a-t-il des trous dans l'univers a combler?

**Questions specifiques au type:**

| Type | Questions supplementaires |
|------|--------------------------|
| scenario | Role des PJ? Liberte d'action? Points de decision? |
| novel | Point de vue narratif? Arcs emotionnels? |
| roleplaying | Regles concernees? Exemples de jeu? |

**Techniques de questionnement:**

- **"Et si...?"** - Variations radicales (Et si l'antagoniste avait raison?)
- **"Pourquoi pas...?"** - Challenger les choix implicites (Pourquoi ce lieu?)
- **"5 Pourquoi"** - Creuser jusqu'a la racine (Pourquoi X? -> Pourquoi Y? -> ...)
- **"Consequences?"** - Explorer les ramifications (Que se passe-t-il si echec?)
- **"Avocat du diable"** - Defendre le point de vue oppose pour tester la solidite
- **Inversion** - Inverser un element pour voir ce que ca revele

**Si lacune documentaire identifiee:**
```
Ce sujet ([sujet]) n'est pas assez documente pour avancer.
Je suggere: @research.prompt.md "[sujet]"
```

**Si contradiction avec la documentation:**
```
Je note que [element de l'overview] semble contredire [element de la doc].
- Est-ce intentionnel (divergence voulue)?
- Faut-il mettre a jour la documentation?
- Faut-il modifier l'overview?
```

### 3. Proposer des Alternatives

Pour chaque zone floue ou probleme identifie, proposer 2-3 alternatives:

```markdown
## [Sujet]

**Situation actuelle:** [description]
**Probleme potentiel:** [ce qui pourrait ne pas fonctionner]

**Option A - [Nom]:** [Description]
  - Avantage: [point fort]
  - Inconvenient: [point faible]

**Option B - [Nom]:** [Description]
  - Avantage: [point fort]
  - Inconvenient: [point faible]

**Option C - Conserver l'actuel:**
  - Avantage: [pourquoi ca peut marcher]
  - Risque: [ce qu'il faut surveiller]
```

### 4. Iterer et Mettre a Jour

Apres chaque reponse de l'utilisateur:

1. Integrer les decisions dans une version mise a jour de l'overview
2. Identifier les nouvelles questions soulevees (max 2-3)
3. Proposer la mise a jour et demander validation:

```markdown
Je vais mettre a jour l'overview avec:

**Ajouts:** [element 1], [element 2]
**Modifications:** [avant] -> [apres]
**Suppressions:** [element retire]

Confirmes-tu?
```

4. Ecrire seulement apres validation

**Si l'utilisateur rejette la mise a jour:**
```
D'accord, je ne modifie pas l'overview.
Peux-tu preciser ce qui ne convient pas? Je reformule ma proposition.
```

**Si l'utilisateur veut pivoter/recommencer:**
```
Tu veux repartir sur une nouvelle direction. Avant d'effacer l'overview actuel:
- Veux-tu sauvegarder une copie de la version actuelle?
- Quel est le nouveau concept de depart?
```

### 5. Gerer les Fichiers de Details

**Pour les campagnes multi-scenarios** (`document.type: scenario` avec plusieurs scenarios):
- Creer/mettre a jour `.docs/scenarios-details.md`
- Contenu: synopsis de chaque scenario, liens narratifs, arcs transversaux

**Pour les recueils multi-nouvelles** (`document.type: novel` avec plusieurs nouvelles):
- Creer/mettre a jour `.docs/novels-details.md`
- Contenu: synopsis de chaque nouvelle, themes communs, ordre de lecture

**Structure type:**
```markdown
# [Scenarios|Nouvelles]: [Nom du projet]

## Vue d'Ensemble
[Description de l'ensemble, themes communs, progression]

## [Titre 1]
**Position:** [1er, 2eme, ...]
**Synopsis:** [2-3 phrases]
**Liens:** [connexions avec les autres]
**Enjeux specifiques:** [ce qui est propre a celui-ci]

## [Titre 2]
...

## Arcs Transversaux
- [Arc 1]: [description, progression a travers les episodes]
- [Arc 2]: ...
```

### 6. Verifier la Completude

Avant de passer a generate-toc, verifier que l'overview contient:

**Obligatoire (tous types):**
- [ ] Concept clair en 2-3 phrases (pitch)
- [ ] Structure narrative (debut, milieu, fin)
- [ ] Protagonistes identifies avec motivations
- [ ] Antagonistes/obstacles identifies
- [ ] Enjeux clairs
- [ ] Ton et atmosphere definis

**Selon document.type:**

| Type | Elements supplementaires |
|------|-------------------------|
| scenario | Role des PJ, liberte d'action, points de decision |
| novel | Point de vue, arcs emotionnels, denouement |
| roleplaying | Regles applicables, exemples, conseils MJ |

**Recommande:**
- [ ] Themes explores
- [ ] Arcs de personnages esquisses
- [ ] Points de bascule identifies

**Critere de sortie:**
Proposer generate-toc quand:
- Tous les elements obligatoires sont presents
- L'utilisateur n'a plus de questions majeures
- Les 2-3 dernieres iterations n'ont apporte que des ajustements mineurs

### 7. Valider et Transitionner

Quand l'overview est complet:

```markdown
## Resume du Concept

[Pitch final en 3-5 phrases]

## Elements Valides

- Concept: OK | Structure: OK | Personnages: OK | Enjeux: OK | Ton: OK

## Pret pour la Suite

L'overview est suffisamment solide.

Prochaine etape: `@generate-toc.prompt.md $ARGUMENTS`
```

## Exemple de Dialogue

```
CLAUDE: Je remarque que le scenario se deroule pendant la Revolution mais
        la documentation sur le vaudou haitien est absente.
        Le lien avec Saint-Domingue necessite ces informations.
        Je suggere: @research.prompt.md "vaudou haitien ceremonie Bois-Caiman"

USER:   Bonne idee, fais la recherche.

[... apres research ...]

CLAUDE: La recherche a enrichi la documentation. Je peux maintenant challenger:
        Le scenario mentionne "les PJ decouvrent la verite" mais cette
        verite n'est pas definie.
        - Option A: La verite est un complot politique lie aux Jacobins
        - Option B: La verite est une malediction vaudou ramenee de Saint-Domingue
        - Option C: Laisser volontairement flou pour l'instant
        Qu'en penses-tu?

USER:   Option B, ca melange bien les deux contextes historiques.

CLAUDE: Je vais mettre a jour l'overview avec:
        **Ajouts:** La verite = malediction vaudou originaire de Saint-Domingue
        Confirmes-tu?

USER:   Oui.

CLAUDE: [Met a jour l'overview]
        Nouvelle question: qui a ramene cette malediction en France?
```
