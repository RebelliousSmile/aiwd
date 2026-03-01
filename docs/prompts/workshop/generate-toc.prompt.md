---
name: generate-toc
description: Génère INDEX.md (table des matières) depuis un document source (overview, brief, brainstorm)
argument-hint: <client>/<projet> (chemin du projet, ex. acme-corp/guide-api)
version: 2.0
changelog:
  - version: 2.0
    date: 2026-03-01
    changes:
      - "Migration doc-tech : suppression JdR (univers, PNJ, terminologie, factions, lieux)"
      - "Argument : <client>/<projet> au lieu d'un fichier source"
      - "Context : CLIENT.md + glossaire.md + docs.projet depuis bank.yml"
      - "Step 2.5 : cross-reference doc-tech (glossaire, audience, composants)"
      - "INDEX.md : Type doc-tech, Client (pas Univers), pas de Personnages principaux"
      - "toc-chapter template : Composants/Acteurs + Interfaces (pas Personnages/Lieux)"
      - "Tags [INTRO]/[REF] adaptés doc-tech (concepts, composants, termes métier)"
  - version: 1.3
    date: 2026-02-14
    changes:
      - "Tags [INTRO] et [REF ChXX] dans les points clés"
      - "Règle : distribution du contenu entre chapitres (anti-répétition)"
  - version: 1.2
    date: 2026-02-14
    changes:
      - "Step 7 : note relecture persona TOC"
  - version: 1.1
    date: 2026-02-14
    changes:
      - "Chargement documentation projet depuis bank.yml"
      - "Step 2.5 : cross-reference documentation"
  - version: 1.0
    date: 2026-01-31
    changes:
      - "Version initiale"
---

# Generate TOC Structure

## Objectif

Analyser le document source du projet et générer `INDEX.md` : une table des matières complète avec synopsis et points clés pour chaque chapitre/section.

## Contexte

### Configuration Projet (OBLIGATOIRE)
```yaml
@bank.yml
```
*Extraire : `document.client`, `document.type`, `document.name`, `docs.*`, `toc.*`*

### Document Source (OBLIGATOIRE)
```markdown
@overview.md
```
*Le brief initial du projet. Si absent, charger le fichier indiqué dans `$ARGUMENTS` ou dans `bank.yml`.*

### Documentation Client (depuis bank.yml)

```markdown
@<client>/.docs/CLIENT.md       # Infos client, ton, audience cible
@<client>/.docs/glossaire.md    # Termes métier obligatoires
@<client>/.docs/brand-guidelines.md  # Si déclaré dans bank.yml
```

### Documentation Projet (depuis bank.yml section `docs.projet`)

```markdown
@[docs.projet[0]]   # ex: .docs/introduction.md
@[docs.projet[1]]   # ex: .docs/document-rules.md
@[docs.projet[2]]   # ex: .docs/architecture.md
# ... tous les fichiers listés dans docs.projet
```

Ne charger que les fichiers effectivement déclarés dans bank.yml. Ne pas inventer de chemins.

## Règles

- `INDEX.md` dans le dossier `.toc/` du projet
- Chaque chapitre inclut synopsis ET points clés (grandes lignes)
- Contenu en français, sans emojis
- Synopsis : 2-3 phrases par chapitre
- Points clés : 3-7 puces par chapitre (puces simples, sans checkboxes)
- Fichiers `toc-chapter<XX>.md` OPTIONNELS (uniquement sur demande)
- Numérotation chapitres : format 2 chiffres (01, 02… 10, 11)
- Longueur `INDEX.md` : indicativement 100-300 lignes selon nombre de chapitres
- **Les points clés doivent refléter les contraintes des docs projet** : si `document-rules.md` définit des procédures spécifiques ou des composants, les chapitres concernés doivent les mentionner explicitement.
- **Ne pas inventer de contenu absent du source ET de la documentation.** Croiser source + docs pour enrichir les points clés (ex: le source mentionne "configuration réseau" → `document-rules.md` précise "VPN, pare-feu, certificats" → les points clés nomment ces éléments).
- **Chaque chapitre doit inclure `Relecteur TOC:` et `Output-style:`** — assigner le(s) persona(s) relecteur(s) depuis `overview.md` (section "Relecture TOC") et l'output-style depuis `bank.yml` (section `output-style`).
- **Distribution du contenu entre chapitres :** Les points clés doivent indiquer quel chapitre **introduit** chaque composant, concept ou terme métier clé (tag `[INTRO]`). Les chapitres suivants qui réutilisent ces éléments les marquent `[REF ChXX]` pour que `write-technical`/`write-user-guide` sache utiliser le format abrégé (référence au lieu de ré-explication).

## Étapes

1. Charger `bank.yml` — extraire `document.client`, `document.type`, `document.name`
2. Charger `overview.md` (brief principal) et la documentation client/projet
2.5. **Charger et cross-référencer la documentation projet :**
   - Lire tous les fichiers déclarés dans `docs.client` et `docs.projet` de bank.yml
   - Identifier les **termes métier obligatoires** depuis `glossaire.md`
   - Identifier les **contraintes d'audience** depuis `CLIENT.md` (niveau, besoins, attentes)
   - Identifier les **contraintes spécifiques** depuis `document-rules.md` (si présent) : composants, procédures, règles de nommage
   - Identifier les **éléments de marque** depuis `brand-guidelines.md` (si présent) : ton, valeurs, interdits
   - Compiler une liste de « contraintes à distribuer » dans les chapitres appropriés
3. Extraire depuis le document source : structure, sections, concepts clés, procédures, composants
4. **Proposer le découpage en chapitres pour validation :**
   - Lister les chapitres proposés avec titres
   - Demander : « Cette structure vous convient-elle ? Voulez-vous modifier, fusionner ou diviser certains chapitres ? »
   - Attendre confirmation avant de continuer
5. Créer le dossier `.toc/` si inexistant
6. Générer `.toc/INDEX.md` avec synopsis + points clés pour chaque chapitre, en croisant le source avec la documentation projet (les points clés doivent nommer les composants, procédures et termes spécifiques issus des docs)
7. **Proposer les fiches toc-chapter détaillées :**
   - Demander : « Souhaitez-vous des fiches détaillées pour certains chapitres ? (format `toc-chapter<XX>.md`) »
   - Si oui : générer les fiches demandées dans `.toc/`
   - Si non : terminer

   Note : si `overview.md` déclare une section "Relecture TOC" avec des personas, chaque `toc-chapter` bénéficiera d'une relecture persona lors de sa génération via `write-toc-chapter`. L'`INDEX.md` n'est pas soumis à cette relecture (grain trop large).
8. Rapporter les fichiers créés

## Gestion des Erreurs

- **Source vide/illisible :** Signaler le problème, demander clarification
- **Structure ambiguë :** Proposer plusieurs interprétations possibles
- **Manque d'informations :** Lister les éléments manquants, continuer avec ce qui est disponible

## Format de Sortie

### `.toc/INDEX.md`

```markdown
# Table des Matières : [Nom Projet]

**Type :** [technical-doc|user-guide|api-doc|process-doc]
**Client :** [client]
**Document :** [document.name depuis bank.yml]

## Vue d'ensemble
[résumé global en 3-5 lignes]

## Thèmes & Objectifs
- [objectif 1]
- [objectif 2]

## Structure
- **Partie I :** Chapitres 1-3 — [titre partie]  *(optionnel si hiérarchie présente)*
- **Partie II :** Chapitres 4-7 — [titre partie]

---

## Chapitres

### Chapitre 01 : [Titre]

**Relecteur TOC :** [persona-id ou vide]
**Output-style :** [nom-fichier.md]

**Synopsis :** [2-3 phrases décrivant l'objectif et le contenu]

**Points clés :**
- [INTRO] Premier usage de [terme métier] — définition complète (depuis glossaire.md)
- [composant ou concept principal]
- [procédure ou étape décrite]

---

### Chapitre 02 : [Titre]

**Relecteur TOC :** [persona-id(s)]
**Output-style :** [nom-fichier.md]

**Synopsis :** [2-3 phrases]

**Points clés :**
- [REF Ch01] [terme] réutilisé — format abrégé (renvoi Ch01)
- [INTRO] Première occurrence de [composant] — documentation complète
- [point technique ou procédural]

---

[... autres chapitres ...]

---
**Source :** [fichier source]
**Généré :** [date]
```

Note : La section "Structure" (Parties) est optionnelle — l'inclure uniquement si le document source utilise une hiérarchie en parties.

### `.toc/toc-chapter<XX>.md` (OPTIONNEL)

Créer uniquement si l'utilisateur demande plus de détails pour un chapitre spécifique.

```markdown
# Chapitre [N] : [Titre]

## Synopsis
[2-3 phrases décrivant l'objectif et le contenu]

## Points clés
- [concept ou composant principal]
- [procédure ou étape clé]
- [résultat attendu ou livrables]

## Composants / Acteurs (si pertinent)
| Élément | Rôle dans ce chapitre |
|---------|----------------------|
| [composant] | [usage/comportement] |

## Interfaces / Systèmes (si pertinent)
- [système ou interface] : [description courte]

## Ambiance & Ton (si pertinent)
[ton attendu, niveau technique, style d'écriture]

## Connexions
- Précédent : [lien avec chapitre N-1, ou "Premier chapitre" si N=1]
- Suivant : [préparation chapitre N+1, ou "Dernier chapitre" si final]

## Notes d'écriture
[instructions spécifiques, points d'attention, contraintes particulières]
```
