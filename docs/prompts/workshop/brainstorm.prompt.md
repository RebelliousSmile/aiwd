---
name: brainstorm
description: Iterer sur l'overview du projet, challenger les concepts, proposer des alternatives jusqu'a generate-toc
argument-hint: Chemin vers le projet (ex: "acme-corp/api-v2")
---

# Brainstorm

## Goal

Accompagner l'utilisateur dans l'élaboration et le raffinement de son concept de document, en challengeant les idées, proposant des alternatives, et itérant jusqu'à ce que l'overview soit suffisamment solide pour passer à la génération de la table des matières.

**Durée typique:** 3-8 itérations avant d'atteindre les critères de sortie.

**Boucle:**
```
Analyser -> Challenger -> Proposer -> Valider -> Mettre à jour -> Répéter
```

## Context

### Configuration Projet

```yaml
@$ARGUMENTS/bank.yml
```

**Si bank.yml est absent ou invalide:**
```
Le projet n'a pas de fichier bank.yml valide.
Veuillez d'abord initialiser le projet avec :
@docs/prompts/workshop/init-project.prompt.md $ARGUMENTS
```

### Chargement des Ressources

Lire bank.yml et charger **tous** les fichiers déclarés :

1. **Overview:** Le fichier référencé dans `overview`
2. **Client:** `docs.client` → CLIENT.md (ton, audience, contraintes)
3. **Glossaire:** `docs.glossaire` → termes métier à respecter
4. **Documentation projet:** Tous les fichiers dans `docs.projet`
5. **Type de document:** Noter `document.type` pour adapter les questions (défaut: "technical-doc")

Ne pas inventer de chemins. Suivre uniquement les liens présents dans bank.yml.

## Rules

- Sortie en français
- Mode conversationnel : poser des questions, ne pas imposer
- Challenger les idées sans les rejeter
- Proposer des alternatives, pas des remplacements
- **Maximum 2-3 questions par itération** (ne pas submerger l'utilisateur)
- Mettre à jour l'overview à chaque itération validée
- Ne jamais inventer de contenu sans validation utilisateur
- MOINS C'EST PLUS : un périmètre clair vaut mieux que dix fonctionnalités floues
- Utiliser CLIENT.md et glossaire.md pour vérifier la cohérence terminologique
- Si un sujet manque de documentation, suggérer `@docs/prompts/workshop/research.prompt.md "<sujet>"`

## Étapes

### 0. Vérifier l'Existence de l'Overview

**Si l'overview n'existe pas:**

```markdown
Je vois que le projet n'a pas encore d'overview.

Pour commencer, décris-moi le document à créer en quelques phrases :
- Quel est l'objet du document ?
- À qui s'adresse-t-il ?
- Quels sont les 2-3 sujets principaux à couvrir ?
```

Après les premières réponses, créer un overview minimal avec cette structure :

```markdown
# [Titre du document]

## Pitch
[2-3 phrases décrivant l'objet et la valeur du document]

## Audience Cible
[Profil du lecteur : rôle, niveau technique, contexte d'utilisation]

## Périmètre
[Ce qui est couvert / ce qui est hors périmètre]

## Use Cases Principaux
[Liste des 3-5 cas d'usage que le document doit couvrir]

## Contraintes
[Contraintes connues : format, longueur, terminologie imposée, audience multilingue...]

## Livrables
[Format de sortie attendu : guide, spec, API ref, SOP...]
```

**Si l'overview existe:** Passer à l'étape 1.

### 1. Analyser l'Overview Existant

Lire le fichier overview (depuis bank.yml) et identifier :

**Éléments présents :**
- Objet du document (pitch)
- Audience cible et niveau technique
- Périmètre fonctionnel (inclus/exclus)
- Use cases couverts
- Contraintes identifiées
- Terminologie alignée avec le glossaire client

**Éléments manquants ou flous :**
- Audience non définie ou trop large
- Périmètre sans frontières claires
- Use cases implicites mais non énoncés
- Contradictions avec CLIENT.md (ton, terminologie)
- Termes non définis dans le glossaire

### 2. Challenger le Concept

Poser des questions pour tester la solidité du document à construire. Utiliser CLIENT.md et glossaire.md pour vérifier la cohérence.

**Format standard de question:**
```
Je remarque que [observation].
- Option A: [alternative 1]
- Option B: [alternative 2]
- Option C: conserver tel quel

Qu'en penses-tu ?
```

**Questions de fond :**
- Pourquoi ce document doit-il être créé maintenant ?
- Qu'est-ce qui le rend utile par rapport à ce qui existe déjà ?
- Quel est le résultat attendu pour l'audience après lecture ?

**Questions de périmètre :**
- Quels sujets sont dans le périmètre ? Lesquels en sont explicitement exclus ?
- Le scope est-il réaliste pour le format et la longueur visés ?
- Y a-t-il des dépendances (autres docs, versions, APIs) à clarifier ?

**Questions d'audience :**
- Le lecteur est-il débutant, intermédiaire, ou expert ?
- Lit-il le document de A à Z ou cherche-t-il une information précise ?
- Quelle action doit-il pouvoir effectuer après lecture ?

**Questions de cohérence :**
- Les termes utilisés correspondent-ils au glossaire client ?
- Le ton est-il aligné avec CLIENT.md ?
- Y a-t-il des contradictions avec d'autres documents existants ?

**Questions spécifiques au type :**

| Type | Questions supplémentaires |
|------|--------------------------|
| `technical-doc` | Architecture concernée ? Prérequis techniques ? Environnements couverts ? |
| `user-guide` | Parcours utilisateur ? Cas d'erreur à documenter ? Captures écran nécessaires ? |
| `api-doc` | Endpoints prioritaires ? Authentification ? Exemples de requêtes/réponses ? |
| `process-doc` | Acteurs impliqués ? Déclencheurs ? Exceptions et cas limites ? |

**Techniques de questionnement :**

- **"Et si...?"** — Variations de périmètre (Et si on excluait ce cas d'usage ?)
- **"Pourquoi pas...?"** — Challenger les choix implicites (Pourquoi ce format ?)
- **"5 Pourquoi"** — Creuser jusqu'à la racine (Pourquoi couvrir X ? → Pourquoi Y ? → ...)
- **"Conséquences?"** — Explorer les ramifications (Que se passe-t-il si le lecteur ne trouve pas l'info ?)
- **"Avocat du diable"** — Défendre le point de vue de l'audience pour tester la clarté
- **Inversion** — Inverser un choix pour voir ce que ça révèle (Doc courte généraliste vs. longue exhaustive)

**Si lacune documentaire identifiée :**
```
Ce sujet ([sujet]) n'est pas assez documenté pour avancer.
Je suggère : @docs/prompts/workshop/research.prompt.md "[sujet]"
```

**Si contradiction avec CLIENT.md ou le glossaire :**
```
Je note que [terme/concept de l'overview] semble contredire [élément de CLIENT.md / glossaire].
- Est-ce intentionnel (divergence voulue pour ce projet) ?
- Faut-il mettre à jour le glossaire ?
- Faut-il modifier l'overview ?
```

### 3. Proposer des Alternatives

Pour chaque zone floue ou problème identifié, proposer 2-3 alternatives :

```markdown
## [Sujet]

**Situation actuelle:** [description]
**Problème potentiel:** [ce qui pourrait ne pas fonctionner]

**Option A - [Nom]:** [Description]
  - Avantage: [point fort]
  - Inconvénient: [point faible]

**Option B - [Nom]:** [Description]
  - Avantage: [point fort]
  - Inconvénient: [point faible]

**Option C - Conserver l'actuel:**
  - Avantage: [pourquoi ça peut marcher]
  - Risque: [ce qu'il faut surveiller]
```

### 4. Itérer et Mettre à Jour

Après chaque réponse de l'utilisateur :

1. Intégrer les décisions dans une version mise à jour de l'overview
2. Identifier les nouvelles questions soulevées (max 2-3)
3. Proposer la mise à jour et demander validation :

```markdown
Je vais mettre à jour l'overview avec :

**Ajouts:** [élément 1], [élément 2]
**Modifications:** [avant] -> [après]
**Suppressions:** [élément retiré]

Confirmes-tu ?
```

4. Écrire seulement après validation

**Si l'utilisateur rejette la mise à jour :**
```
D'accord, je ne modifie pas l'overview.
Peux-tu préciser ce qui ne convient pas ? Je reformule ma proposition.
```

**Si l'utilisateur veut pivoter/recommencer :**
```
Tu veux repartir sur une nouvelle direction. Avant de modifier l'overview actuel :
- Veux-tu sauvegarder une copie de la version actuelle ?
- Quel est le nouveau concept de départ ?
```

### 5. Gérer les Projets Multi-Sections

**Pour les suites documentaires** (`document.type: technical-doc` avec plusieurs modules) :
- Créer/mettre à jour `.docs/modules-breakdown.md`
- Contenu : synopsis de chaque module, dépendances entre modules, ordre de lecture recommandé

**Pour les guides multi-parcours** (`document.type: user-guide` avec plusieurs profils d'audience) :
- Créer/mettre à jour `.docs/audience-map.md`
- Contenu : description de chaque profil, sections recommandées, chemins de lecture

**Structure type :**
```markdown
# [Modules|Audiences] : [Nom du projet]

## Vue d'Ensemble
[Description de l'ensemble, logique de découpage, ordre recommandé]

## [Module/Profil 1]
**Audience:** [qui lit cette section]
**Synopsis:** [2-3 phrases]
**Prérequis:** [ce que le lecteur doit déjà savoir]
**Liens:** [connexions avec les autres modules]

## [Module/Profil 2]
...

## Dépendances Transversales
- [Concept A] : [utilisé dans les sections X, Y, Z — définir en section X]
- [Concept B] : ...
```

### 6. Vérifier la Complétude

Avant de passer à generate-toc, vérifier que l'overview contient :

**Obligatoire (tous types) :**
- [ ] Objet du document en 2-3 phrases (pitch)
- [ ] Audience cible définie (rôle, niveau technique)
- [ ] Périmètre fonctionnel (inclus + exclus explicitement)
- [ ] Use cases principaux listés (3-5 minimum)
- [ ] Contraintes identifiées (format, longueur, terminologie)
- [ ] Alignement terminologique avec glossaire.md vérifié

**Selon document.type :**

| Type | Éléments supplémentaires |
|------|-------------------------|
| `technical-doc` | Prérequis techniques, environnements, architecture concernée |
| `user-guide` | Parcours utilisateur, cas d'erreur, captures écran prévues |
| `api-doc` | Liste des endpoints, format auth, exemples requêtes/réponses |
| `process-doc` | Acteurs, déclencheurs, exceptions et cas limites, KPIs |

**Recommandé :**
- [ ] Ordre de lecture suggéré
- [ ] Documents connexes identifiés
- [ ] Points de complexité ou risques documentaires signalés

**Critère de sortie :**
Proposer generate-toc quand :
- Tous les éléments obligatoires sont présents
- L'utilisateur n'a plus de questions majeures
- Les 2-3 dernières itérations n'ont apporté que des ajustements mineurs

### 7. Valider et Transitionner

Quand l'overview est complet :

```markdown
## Résumé du Document

[Pitch final en 3-5 phrases]

## Éléments Validés

- Pitch: OK | Audience: OK | Périmètre: OK | Use cases: OK | Contraintes: OK

## Prêt pour la Suite

L'overview est suffisamment solide.

Prochaine étape : `@docs/prompts/workshop/generate-toc.prompt.md $ARGUMENTS`
```

## Exemple de Dialogue

```
CLAUDE: Je lis l'overview : le guide couvre l'API REST d'authentification.
        Mais l'audience n'est pas définie.
        - Option A: Développeurs backend intégrant l'API (niveau intermédiaire)
        - Option B: Développeurs fullstack incluant les aspects frontend (OAuth flows)
        - Option C: Laisser ouvert (les deux profils)
        Qu'en penses-tu ?

USER:   Option A, on cible le backend uniquement.

CLAUDE: Je vais mettre à jour l'overview avec :
        **Ajouts:** Audience = développeurs backend, niveau intermédiaire
        **Suppressions:** mention "OAuth flows frontend"
        Confirmes-tu ?

USER:   Oui.

CLAUDE: [Met à jour l'overview]
        Nouvelle question : le guide doit-il couvrir l'authentification par clé API
        en plus de JWT, ou uniquement JWT ?
        - Option A: JWT uniquement (plus simple, cohérent avec le glossaire client)
        - Option B: JWT + clé API (plus exhaustif, mais périmètre plus large)
        - Option C: JWT principal + section courte sur clé API (en annexe)

USER:   Option A, JWT uniquement pour cette version.

CLAUDE: Je vais mettre à jour l'overview avec :
        **Ajouts:** Périmètre = authentification JWT uniquement ; clé API hors périmètre v1
        Confirmes-tu ?

USER:   Oui.

CLAUDE: [Met à jour l'overview]
        L'overview couvre maintenant : pitch, audience backend, périmètre JWT,
        use cases (login, refresh, logout, token introspection).
        Tous les éléments obligatoires sont présents.

        Prochaine étape : @docs/prompts/workshop/generate-toc.prompt.md acme-corp/api-auth-guide
```
