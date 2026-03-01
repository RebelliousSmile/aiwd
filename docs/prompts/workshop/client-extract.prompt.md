---
name: client-extract
description: Extrait et organise les informations client/projet depuis des sources brutes vers des fichiers thématiques structurés pour documentation technique.
---

# Client Extract

> **TL;DR** : Sources brutes → Fichiers thématiques (250 lignes max). Valide les thèmes, extrait, synthétise si besoin, demande arbitrage si choix difficile.

## Objectif

Transformer des fichiers sources bruts (PDFs, captures, notes, documentation existante) en fichiers thématiques structurés pour un client, prêts à être utilisés par les prompts workshop de documentation technique.

## Entrées

- **Client cible** : $1 (identifiant : ex: cerascan, acme-corp, client-x)
- **Dossier sources** : $2 (chemin vers `<client>/.docs/sources/`)
- **Options** :
  - `--update` : Mode incrémental, enrichit les fichiers existants
  - `--force` : Régénère tout même si fichiers existent
  - `--project <nom>` : Extraction au niveau projet (génère aussi dans `<client>/<projet>/.docs/`)

## Formats acceptés

- Documents : `.pdf`, `.docx`, `.txt`, `.md`
- Images : `.png`, `.jpg` (captures d'écran UI)
- Code : `.json`, `.yaml`, `.xml` (configs existantes)
- Encodage : UTF-8 recommandé (sinon, normaliser avec `scripts/normalize-text.py`)

## Paramètres

- **Taille max par fichier** : 250 lignes
- **Seuil thème custom** : 50 lignes minimum de contenu unique
- **Thèmes standard** : CLIENT.md, glossaire.md, architecture.md, requirements.md, users.md, workflows.md
- **Thèmes optionnels** : api.md, security.md, integration.md, deployment.md, monitoring.md, data-model.md
- **Priorité de traitement** : CLIENT.md > glossaire.md > architecture.md > requirements.md > users.md > workflows.md > autres

## Sortie

Fichiers générés dans `<client>/.docs/` :
- `CLIENT.md` (obligatoire) - contexte client, audience, contraintes
- `glossaire.md` (obligatoire) - termes métier et vocabulaire technique
- `[thematique].md` (selon contenu détecté : architecture, requirements, users, workflows, etc.)

**Si exécuté au niveau projet** (`<client>/<projet>/`) avec `--project` :
- `<projet>/.docs/context.md` - contexte spécifique du projet
- `<projet>/.docs/document-rules.md` - règles de rédaction spécifiques

---

## Étapes

### 0. Validation des sources

Avant toute extraction, vérifier pour chaque fichier source :

- [ ] Le dossier `sources/` existe
- [ ] Les fichiers sont accessibles et non vides
- [ ] L'encodage est lisible (UTF-8 ou convertible)
- [ ] Les PDFs sont extractibles (pas de scan image sans OCR)

**Mode `--update` :**
- [ ] Lister les fichiers `.docs/` existants
- [ ] Identifier les sources déjà traitées (via métadonnées)
- [ ] Ne traiter que les nouvelles sources

**Si problème détecté :**
```
Erreur source : [fichier]
- Problème : [fichier introuvable / vide / encodage illisible / PDF scanné]
- Action : [corriger le chemin / fournir autre fichier / lancer OCR / normalize-text.py]

Continuer avec les autres sources ? (oui/non)
```

**Rapport de validation :**
```
Sources validées : [N]/[total]
Volume total estimé : ~[X] lignes
Langues détectées : [français, anglais, ...]
Mode : [création / mise à jour]
Fichiers existants : [liste si --update]
Types détectés : [PDFs: N, captures: M, notes: P, code: Q]
```

### 1. Lecture et analyse des sources

Lire tous les fichiers sources validés. Noter :
- Volume total de texte
- Qualité (structuré vs brut)
- Langue principale
- Type de contenu dominant (specs, UI, processus, code)

**Gestion multilingue :**
- Langue cible : français
- Termes techniques anglais : conserver l'original entre parenthèses à la première mention
- Ex: "Le composant de traitement (*processor*) gère..."

**Gestion captures d'écran :**
- Analyser les images UI pour extraire :
  - Noms d'écrans/pages
  - Libellés de boutons/menus
  - Workflows visuels
  - Messages d'erreur visibles
- Stocker descriptions textuelles dans les fichiers thématiques
- Référencer les captures : `![Écran Authentification](sources/captures/login.png)`

### 2. Détection des thèmes

Analyser le contenu et identifier les thèmes présents.

**Pour chaque thème standard**, évaluer :
- Présent : oui/non
- Volume estimé : faible (< 50 lignes) / moyen (50-150) / important (> 150)
- Pertinence pour ce client

**Thèmes custom :**
Créer un thème custom si :
- Le contenu ne rentre dans aucun thème standard
- Volume > 50 lignes
- Cohérence thématique claire

**Exemples thèmes custom :**
- `payments.md` - système de paiement spécifique
- `reporting.md` - outils de reporting métier
- `compliance.md` - contraintes réglementaires (RGPD, etc.)
- `migration.md` - stratégie de migration depuis ancien système

### 3. Validation des thèmes

Présenter à l'utilisateur :

```
Thèmes détectés dans les sources :

| Thème | Présent | Volume | Priorité | Recommandation |
|-------|---------|--------|----------|----------------|
| CLIENT.md | oui | moyen | 1 | Obligatoire |
| glossaire.md | oui | important | 2 | Obligatoire |
| architecture.md | oui | important | 3 | Recommandé |
| requirements.md | oui | important | 4 | Recommandé |
| users.md | oui | moyen | 5 | Recommandé |
| workflows.md | oui | faible | 6 | Optionnel |
| [payments.md] | oui | moyen | 7 | Proposé (custom) |

Valides-tu cette liste ? (oui / modifier)
```

### 4. Extraction par thème

Pour chaque thème validé (dans l'ordre de priorité), extraire les informations pertinentes.

**Priorité d'extraction (garder) :**
1. Termes métier uniques au domaine
2. Contraintes techniques et métier
3. Relations entre composants/acteurs
4. Workflows et processus métier
5. Règles de gestion et exceptions
6. Endpoints, APIs, formats de données

**Déprioritiser (couper si nécessaire) :**
1. Descriptions marketing génériques
2. Historique de versions anciennes
3. Exemples multiples du même concept
4. Détails d'implémentation interne non pertinents
5. Méta-informations éditoriales

**Règle des doublons inter-fichiers :**

| Information | Fichier principal | Mention secondaire |
|-------------|-------------------|-------------------|
| Architecture composant | architecture.md | requirements.md (référence) |
| Workflow métier | workflows.md | users.md (résumé) |
| Endpoint API | api.md | architecture.md (diagramme) |
| Terme technique | glossaire.md | Jamais dupliqué |
| Contrainte sécurité | security.md | requirements.md (mention) |

**Principe** : L'information complète va dans UN fichier, les autres fichiers font référence ou résument.

### 5. Détection des contradictions

Pendant l'extraction, si deux sources donnent des informations contradictoires :

```
Contradiction détectée :

Source A ([fichier]) : "[information A]"
Source B ([fichier]) : "[information B]"

Sujet : [ex: nombre max de connexions simultanées]
Fichier cible : [architecture.md]

Quelle source fait autorité ? (A/B/fusionner/ignorer)
```

**Si documentation existante vs captures récentes :**
- Privilégier les captures (état actuel) vs doc obsolète
- Signaler l'obsolescence dans CLIENT.md

### 6. Aperçu avant synthèse

Afficher un résumé intermédiaire :

```
Extraction brute terminée :

| Fichier | Lignes | Statut | Priorité |
|---------|--------|--------|----------|
| CLIENT.md | 95 | OK | 1 |
| glossaire.md | 180 | OK | 2 |
| architecture.md | 310 | DÉPASSE (+60) | 3 |
| requirements.md | 280 | DÉPASSE (+30) | 4 |

Fichiers à synthétiser : architecture.md, requirements.md
Ordre de traitement : architecture.md (priorité 3), puis requirements.md (priorité 4)

Continuer vers la synthèse ? (oui / voir détails [fichier])
```

### 7. Vérification taille et synthèse

Pour chaque fichier > 250 lignes (dans l'ordre de priorité) :

#### Synthèse automatique

Appliquer si les critères suivants sont remplis :
- Réduction possible de > 20% par fusion de redondances
- Aucun composant majeur supprimé
- Pas de perte de workflows clés

**Actions automatiques :**
- Fusionner descriptions redondantes de composants
- Réduire exemples multiples à 1-2
- Supprimer méta-informations
- Déplacer doublons vers fichier principal (cf. règle étape 4)
- Condenser tableaux (fusionner colonnes similaires)

#### Arbitrage humain

Déclencher si :
- Synthèse automatique insuffisante (réduction < 20%)
- Plus de 3 composants/workflows seraient supprimés
- Relations clés seraient perdues

**Format de la demande d'arbitrage :**

```
Le fichier [thème].md dépasse 250 lignes (actuellement [N] lignes).
Synthèse automatique : -[X] lignes (insuffisant, besoin -[Y] total)

Je dois choisir quoi retirer. Options :

1. [Option A] - Perte : [composants/workflows affectés]
2. [Option B] - Perte : [composants/workflows affectés]
3. [Option C] - Perte : [composants/workflows affectés]

Que préfères-tu ? (1/2/3/autre suggestion)
```

**Alternative : Split en sous-fichiers**
Si arbitrage impossible, proposer split :
```
architecture.md (250 lignes)
architecture-backend.md (150 lignes)
architecture-frontend.md (120 lignes)
```

### 8. Génération des fichiers

#### CLIENT.md (template)

**IMPORTANT :** Ce fichier contient le contexte métier, audience cible, et contraintes projet.

```markdown
# Client: [Nom Client]

## Contexte Métier

**Secteur:** [industrie, domaine]
**Activité:** [description activité principale]

[Paragraphe contexte]

## Audience Cible

### Utilisateurs Finaux

| Type | Profil | Besoins |
|------|--------|---------|
| [Type 1] | [description] | [attentes principales] |
| [Type 2] | [description] | [attentes principales] |

### Parties Prenantes

- **Décideurs:** [rôles]
- **Équipes techniques:** [rôles]
- **Support:** [rôles]

## Contraintes Projet

### Techniques
- [Contrainte 1]
- [Contrainte 2]

### Métier
- [Contrainte 1]
- [Contrainte 2]

### Réglementaires
- [RGPD, normes, certifications]

## Objectifs Documentation

**Document principal:** [Type : guide utilisateur / specs techniques / API / ...]
**Livrables attendus:** [formats, volumes]
**Délais:** [si applicable]

## Tone & Style

**Ton attendu:** [formel / accessible / technique / ...]
**Niveau technique:** [débutant / intermédiaire / expert]
**Exemples à privilégier:** [concrets / abstraits / screenshots / code]

---
**Lignes:** [N]/250
**Màj:** [date]
**Sources:** [liste fichiers]
```

#### glossaire.md (template)

```markdown
# Glossaire: [Nom Client]

## Termes Métier

| Terme | Définition | Anglais |
|-------|------------|---------|
| [terme1] | [définition] | [term1] |

## Termes Techniques

| Terme | Définition | Contexte |
|-------|------------|----------|
| [terme1] | [définition] | [où utilisé] |

## Acronymes

| Acronyme | Signification | Définition |
|----------|---------------|------------|
| API | Application Programming Interface | [définition] |

## Composants Système

| Nom | Type | Description |
|-----|------|-------------|
| [composant1] | [service/module/API] | [rôle] |

## Termes à Éviter

| Incorrect | Correct | Raison |
|-----------|---------|--------|
| [terme faux] | [terme juste] | [explication] |

---
**Lignes:** [N]/250
**Màj:** [date]
```

#### architecture.md (template)

```markdown
# Architecture: [Nom Client/Produit]

## Vue d'Ensemble

```
[Diagramme architecture en ASCII art ou Mermaid]

[Frontend] <--> [API Gateway] <--> [Backend Services]
                                   [Database]
```

**Stack Technique:**
- Frontend: [technologies]
- Backend: [technologies]
- Base de données: [type, version]
- Infrastructure: [cloud, on-premise]

## Composants Principaux

### [Composant 1]

**Type:** [service / module / API / ...]
**Responsabilités:** [rôle]
**Technologies:** [stack]
**Dépendances:** [autres composants]

[Description détaillée]

### [Composant 2]

...

## Flux de Données

### [Flux 1]

```
[User] → [Frontend] → [API] → [Service A] → [DB]
                              ↓
                         [Service B] (async)
```

**Étapes:**
1. [Étape 1]
2. [Étape 2]

## Intégrations Externes

| Système | Type | Usage | Criticité |
|---------|------|-------|-----------|
| [Système 1] | API REST | [usage] | Haute |

## Contraintes Techniques

- [Contrainte 1]
- [Contrainte 2]

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

#### requirements.md (template)

```markdown
# Spécifications: [Nom Projet]

## Vue d'Ensemble Fonctionnelle

[Paragraphe résumé]

## Features Principales

### [Feature 1]

**Priorité:** [Haute / Moyenne / Basse]
**Statut:** [Implémenté / En cours / Planifié]

**Description:**
[Description fonctionnelle]

**Use Cases:**
1. [UC 1]
2. [UC 2]

**Règles de Gestion:**
- [Règle 1]
- [Règle 2]

### [Feature 2]

...

## Use Cases Détaillés

### UC-01: [Nom Use Case]

**Acteur:** [utilisateur type]
**Pré-conditions:** [état initial]
**Post-conditions:** [état final]

**Scénario nominal:**
1. [Action 1]
2. [Action 2]
3. [Action 3]

**Scénarios alternatifs:**
- **Alt 1:** [cas erreur]
- **Alt 2:** [cas particulier]

## Règles Métier

| ID | Règle | Composants Concernés |
|----|-------|---------------------|
| RG-01 | [règle] | [composants] |

## Contraintes Non-Fonctionnelles

- **Performance:** [ex: temps réponse < 2s]
- **Scalabilité:** [ex: 10000 users concurrents]
- **Disponibilité:** [ex: 99.9% uptime]
- **Sécurité:** [ex: chiffrement TLS 1.3]

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

#### users.md (template)

```markdown
# Utilisateurs: [Nom Client/Produit]

## Personas

### Persona 1: [Nom Persona]

**Profil:**
- Âge: [tranche]
- Fonction: [rôle]
- Compétences techniques: [niveau]

**Contexte d'usage:**
[Où, quand, comment utilise le système]

**Objectifs:**
1. [Objectif 1]
2. [Objectif 2]

**Frustrations:**
- [Frustration 1]
- [Frustration 2]

**Attentes documentation:**
- [Attente 1]
- [Attente 2]

### Persona 2: [Nom Persona]

...

## Parcours Utilisateurs

### Parcours 1: [Nom]

**Persona concerné:** [persona]
**Fréquence:** [quotidien / hebdomadaire / occasionnel]

**Étapes:**
1. [Étape 1] → [écran/action]
2. [Étape 2] → [écran/action]
3. [Étape 3] → [écran/action]

**Points de friction:**
- [Friction 1]

### Parcours 2: [Nom]

...

## Droits et Permissions

| Rôle | Permissions | Écrans Accessibles |
|------|-------------|--------------------|
| [Rôle 1] | [permissions] | [écrans] |

---
**Lignes:** [N]/250
**Sources:** [fichiers, captures]
```

#### workflows.md (template)

```markdown
# Workflows: [Nom Client]

## Processus Métier

### Workflow 1: [Nom Processus]

**Acteurs:** [rôles impliqués]
**Déclencheur:** [événement déclencheur]
**Fréquence:** [quotidien / hebdomadaire / ...]

**Étapes:**

```
[Acteur 1] → [Action 1] → [Système]
                          ↓
                    [Validation]
                          ↓
[Acteur 2] ← [Notification]
```

1. [Étape 1] - [description]
2. [Étape 2] - [description]
3. [Étape 3] - [description]

**Décisions:**
- **Si [condition]** → [action A]
- **Sinon** → [action B]

**Exceptions:**
- [Exception 1] : [action]
- [Exception 2] : [action]

### Workflow 2: [Nom Processus]

...

## Standard Operating Procedures (SOP)

### SOP-01: [Nom Procédure]

**Objectif:** [but de la procédure]
**Responsable:** [rôle]

**Instructions:**
1. [Instruction 1]
2. [Instruction 2]
3. [Instruction 3]

**Vérifications:**
- [ ] [Check 1]
- [ ] [Check 2]

### SOP-02: [Nom Procédure]

...

---
**Lignes:** [N]/250
**Sources:** [fichiers]
```

### 9. Génération INDEX.md

Créer un fichier index dans `<client>/.docs/INDEX.md` :

```markdown
# Documentation: [Nom Client]

## Fichiers Générés

| Fichier | Lignes | Màj | Description |
|---------|--------|-----|-------------|
| CLIENT.md | [N] | [date] | Contexte client |
| glossaire.md | [N] | [date] | Termes métier |
| architecture.md | [N] | [date] | Architecture technique |
| ... | ... | ... | ... |

## Sources Traitées

| Fichier Source | Type | Traité le | Thèmes extraits |
|----------------|------|-----------|-----------------|
| [fichier1.pdf] | PDF | [date] | CLIENT, glossaire, architecture |
| [capture1.png] | Image | [date] | users, workflows |
| ... | ... | ... | ... |

## Statistiques

- **Volume total:** [N] lignes
- **Thèmes:** [X] standard, [Y] custom
- **Sources:** [Z] fichiers
- **Dernière mise à jour:** [date]

---
**Généré par:** client-extract.prompt.md
**Mode:** [création / mise à jour]
```

### 10. Rapport final

```
═══════════════════════════════════════════════════
 CLIENT EXTRACT: [Nom Client] - TERMINÉ
═══════════════════════════════════════════════════

Fichiers générés dans <client>/.docs/ :

✓ CLIENT.md (95 lignes)
✓ glossaire.md (180 lignes)
✓ architecture.md (250 lignes) - synthétisé
✓ requirements.md (240 lignes) - synthétisé
✓ users.md (120 lignes)
✓ workflows.md (85 lignes)
✓ INDEX.md (45 lignes)

Total: 1015 lignes réparties en 7 fichiers

Sources traitées:
- documentation-existante.pdf (150 pages)
- captures-ui/ (24 images)
- notes-reunion.md (3500 mots)

Contradictions résolues: 2
Arbitrages demandés: 1 (architecture.md)
Thèmes custom créés: 0

═══════════════════════════════════════════════════

Prochaines étapes suggérées:

1. Vérifier <client>/.docs/ (surtout CLIENT.md et glossaire.md)
2. Créer premier projet:
   mkdir <client>/<nom-projet>
   cp docs/templates/bank.yml <client>/<nom-projet>/

3. Lancer brainstorm:
   @docs/prompts/workshop/brainstorm.prompt.md <client>/<nom-projet>
```

---

## Validation Finale

Avant de terminer, vérifier :

- [ ] Tous les fichiers respectent 250 lignes max
- [ ] CLIENT.md et glossaire.md existent (obligatoires)
- [ ] Pas de doublons entre fichiers
- [ ] INDEX.md créé avec métadonnées
- [ ] Références croisées cohérentes (ex: "(voir architecture.md)")
- [ ] Encodage UTF-8 pour tous les fichiers
- [ ] Métadonnées en pied de page (lignes, màj, sources)

---

## Troubleshooting

### PDF non extractible

**Problème:** Le PDF est un scan image sans couche texte

**Solution:**
```bash
# Utiliser OCR
python scripts/ocr-to-latex.py sources/document.pdf
```

### Captures d'écran nombreuses

**Problème:** 50+ captures, extraction manuelle fastidieuse

**Solution:**
- Grouper par workflow/écran
- Extraire seulement les écrans principaux
- Référencer les autres : "Voir aussi: sources/captures/admin-*.png"

### Sources contradictoires sans autorité claire

**Problème:** Documentation dit X, capture montre Y, note dit Z

**Solution:**
1. Privilégier l'état actuel : captures > notes récentes > doc ancienne
2. Documenter l'incertitude : "[Information contradictoire, à vérifier]"
3. Créer task de vérification dans CLIENT.md

### Vocabulaire technique massif

**Problème:** glossaire.md dépasse 250 lignes

**Solution:**
```markdown
# Split en sous-fichiers
glossaire.md (termes A-M, 250 lignes)
glossaire-suite.md (termes N-Z, 180 lignes)

# OU grouper par domaine
glossaire-metier.md (250 lignes)
glossaire-technique.md (200 lignes)
```

---

**Version:** 1.0
**Date:** 2026-02-28
**Adapté de:** univers-extract.prompt.md
