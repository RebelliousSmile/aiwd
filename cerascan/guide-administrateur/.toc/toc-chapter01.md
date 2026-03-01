# Chapitre 01: Comment Utiliser ce Guide

**Type:** technical-doc (introduction guide)  
**Output-style:** user-friendly.md  
**Persona review:** clarity-expert.md  
**Pages prévues:** 2-3  
**Statut:** À écrire

---

## Synopsis

Mode d'emploi de ce guide : objectif (manuel opérationnel administration), comment lire (lecture linéaire vs consultation rapide), conventions utilisées. Workflow global en 4 étapes (créer partenaire → importer CSV → générer QR → livrer PDFs). Contacts support technique (prestataire externe) et fonctionnel. Procédure feedback sur le guide. Ressources complémentaires (autres guides CERASCAN, templates, formations).

---

## Points Clés

- Objectif guide : Manuel opérationnel administration multi-tenant (pas présentation CERASCAN, vous connaissez déjà)
- Comment lire : Lecture complète (3h formation) ou consultation rapide par besoin (index, recherche)
- Conventions document : 💡 Astuce, ⚠️ Attention, ✅ Bonne pratique, 📋 Exemple concret
- Workflow global 4 étapes : Créer partenaire (10 min, Ch4) → Importer catalogue (5 min, Ch5) → Générer QR (2 min, Ch5) → Livrer PDFs (3 min)
- Contacts support technique : Prestataire externe (serveur, DB, bugs code), email/téléphone, délai <4h urgent
- Feedback guide : Signaler erreurs, procédures floues, suggestions amélioration (doc-cerascan@efficeram.fr)
- Ressources : Guide Partenaire (showrooms), Guide Commercial (vendeuses), templates CSV (Annexe A), formations disponibles

---

## Section 1: À Propos de ce Guide

**Objectif:** Clarifier l'objectif et le périmètre du document.

**Instructions paragraphes:**
- **Titre section :** "Ce guide est fait pour vous"
- Phrase ouverture : "Ce guide est votre manuel opérationnel pour administrer CERASCAN au quotidien."
- **Objectif document :** Manuel opérationnel administration plateforme multi-tenant (création partenaires, import catalogues, génération QR codes, troubleshooting)
- **Pour qui :** Vous, administrateur CERASCAN (gérez tous les partenaires, responsable opérations)
- **Ce que vous trouverez :**
  - Procédures détaillées pas-à-pas (captures écran, exemples)
  - Troubleshooting problèmes courants (diagnostics, solutions)
  - Référence rapide (tables, checklists, glossaire)
- **Ce que vous ne trouverez PAS :**
  - Présentation CERASCAN (vous connaissez le projet, vous l'avez créé)
  - Configuration serveur, déploiement infrastructure (délégué prestataire technique)
  - Code source, architecture développeur (hors périmètre)
  - Personas détaillés utilisateurs (couverts par autres guides)

**Encadré récapitulatif :**

```
┌────────────────────────────────────────────────────────┐
│ 📋 EN BREF                                             │
├────────────────────────────────────────────────────────┤
│ Objectif : Manuel opérationnel administration         │
│ Audience : Vous, administrateur CERASCAN              │
│ Focus : Gestion partenaires, catalogues, QR codes     │
│ Longueur : 53-65 pages, lecture complète 3h           │
│ Format : PDF interactif (liens, index, glossaire)     │
└────────────────────────────────────────────────────────┘
```

**Ton:** Direct, opérationnel. Pas de vulgarisation (vous connaissez CERASCAN).

---

## Section 2: Comment Lire ce Guide

**Objectif:** Expliquer les 2 modes de lecture et les conventions utilisées.

### 2.1 Modes de Lecture

**Instructions:**
- **Lecture linéaire (formation initiale) :**
  - Durée : 3h lecture complète
  - Parcours : Chapitres 1-7 dans l'ordre
  - Recommandé : Formation 4-5h sur 3-4 jours (lecture + pratique interface)
  - Résultat : Autonomie 90% opérations quotidiennes garantie

- **Consultation rapide (besoin ponctuel) :**
  - Utiliser sommaire interactif (liens cliquables)
  - Recherche par mot-clé (Ctrl+F / Cmd+F)
  - Index alphabétique (Annexe D)
  - Chapitre 7 "Référence Rapide" (accès ultra-rapide info)

**Tableau navigation rapide :**

| Besoin | Chapitre | Section |
|--------|----------|---------|
| Créer nouveau partenaire | Chapitre 4 | Section "Créer Partenaire" |
| Importer catalogue CSV | Chapitre 5 | Section "Import CSV" |
| Générer QR codes | Chapitre 5 | Section "Génération QR Batch" |
| Problème import (erreurs) | Chapitre 5 | Section "Diagnostics Erreurs" |
| Application lente | Chapitre 6 | Section "Maintenance Courante" |
| Chercher info technique | Chapitre 7 | Tables + Glossaire |

### 2.2 Conventions Utilisées

**Instructions:**
- Lister les 4 types d'encadrés avec exemples visuels

**Encadrés :**

```
💡 ASTUCE
Raccourcis clavier, optimisations, astuces productivité.
Exemple : "Utilisez Ctrl+K pour recherche globale rapide"
```

```
⚠️ ATTENTION
Opérations sensibles, risques, précautions à prendre.
Exemple : "Suppression partenaire irréversible après 30 jours"
```

```
✅ BONNE PRATIQUE
Recommandations, conseils métier, workflows optimaux.
Exemple : "Sauvegarder CSV originaux (traçabilité imports)"
```

```
📋 EXEMPLE CONCRET
Cas d'usage réel fil rouge "Carrelage Dupont".
Exemple : "Dupont veut ajouter 50 nouveaux produits..."
```

### 2.3 Navigation Document

**Instructions:**
- **Sommaire interactif :** Cliquer titres chapitres/sections (liens hypertexte)
- **Renvois internes :** "(voir Chapitre 5, Section X)" = liens cliquables
- **Index alphabétique :** Annexe D, tous termes techniques + opérations
- **Glossaire :** Annexe E, définitions multi-tenant, ExecutionContext, etc.
- **Recherche PDF :** Ctrl+F (Windows) / Cmd+F (Mac) pour chercher mot-clé

**Ton:** Pratique, opérationnel. Mettre en avant consultation rapide (usage quotidien réel).

---

## Section 3: Workflow Global

**Objectif:** Vue d'ensemble 4 étapes du workflow administrateur.

**Instructions paragraphes:**
- Introduction : "Voici le workflow typique pour lancer un nouveau partenaire showroom"
- Schéma visuel 4 étapes avec timings
- Renvois vers chapitres détaillés

**Schéma à inclure (version ASCII art) :**

```
WORKFLOW ADMINISTRATEUR : LANCER NOUVEAU PARTENAIRE

┌─────────────────────────────────────────────────────────────┐
│  ÉTAPE 1 : CRÉER PARTENAIRE                                │
│  ⏱️ 10 minutes                           → Chapitre 4       │
├─────────────────────────────────────────────────────────────┤
│  • Nouveau showroom demande abonnement CERASCAN            │
│  • Vous créez partenaire dans interface admin              │
│  • Configuration identité (nom, slug, coordonnées)         │
│  • Personnalisation marque blanche (logo, couleurs)        │
│  • Création compte admin partenaire (email, rôle)          │
│  ✅ Résultat : Partenaire actif, espace dédié créé         │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│  ÉTAPE 2 : IMPORTER CATALOGUE                              │
│  ⏱️ 5 minutes (+ 2 min traitement serveur) → Chapitre 5    │
├─────────────────────────────────────────────────────────────┤
│  • Partenaire vous envoie fichier CSV catalogue (500 prod.)│
│  • Vous uploadez CSV via interface admin                   │
│  • Validation temps réel (colonnes, doublons, encodage)    │
│  • Import lancé, barre progression                         │
│  • Email confirmation (succès ou erreurs détectées)        │
│  ✅ Résultat : Catalogue 500 produits dans base partenaire │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│  ÉTAPE 3 : GÉNÉRER QR CODES                                │
│  ⏱️ 2 minutes (+ 2 min génération serveur)  → Chapitre 5   │
├─────────────────────────────────────────────────────────────┤
│  • Sélection produits (tous ou filtrage par catégorie)     │
│  • Configuration paramètres (taille QR, format PNG)        │
│  • Génération batch 500 QR codes                           │
│  • Création PDFs impression (grilles 10x10, 100 codes/PDF) │
│  • Téléchargement fichiers (5 PDFs)                        │
│  ✅ Résultat : 5 PDFs prêts impression (500 QR codes)      │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│  ÉTAPE 4 : LIVRER AU PARTENAIRE                            │
│  ⏱️ 3 minutes                            → Chapitre 4       │
├─────────────────────────────────────────────────────────────┤
│  • Télécharger PDFs QR codes (fichier ZIP)                 │
│  • Envoyer email partenaire (PDFs joints + instructions)   │
│  • Fournir accès interface partenaire (URL, login)         │
│  • Partenaire imprime, plastifie, pose QR en showroom      │
│  ⏰ Délai partenaire : 48h (impression + pose)              │
│  ✅ Résultat : Showroom opérationnel CERASCAN              │
└─────────────────────────────────────────────────────────────┘

RÉCAPITULATIF TIMING
• Votre travail : 20 minutes (10 + 5 + 2 + 3)
• Temps serveur : 4 minutes (import 2 min + QR 2 min)
• Délai partenaire : 48h (impression/pose côté showroom)
```

**Renvois détaillés :**
- Étape 1 détaillée : Chapitre 4, Section "Créer Nouveau Partenaire"
- Étape 2 détaillée : Chapitre 5, Section "Import Catalogue CSV"
- Étape 3 détaillée : Chapitre 5, Section "Génération QR Codes Batch"
- Étape 4 détaillée : Chapitre 4, Section "Onboarding Partenaire"

**Ton:** Visuel, schéma clair, timings précis. Montrer que c'est rapide (20 min).

---

## Section 4: Contacts et Support

**Objectif:** Clarifier qui contacter selon type de problème.

### 4.1 Support Technique (Prestataire Externe)

**Instructions:**
- **Quand contacter prestataire :**
  - Erreur serveur, crash application
  - Problème base de données (corruptions, migrations)
  - Configuration infrastructure (Nginx, PM2, Node.js)
  - Bug applicatif (erreurs JavaScript, code source)
  - Performance dégradée (serveur lent, timeout)

**Coordonnées support technique :**

```
┌────────────────────────────────────────────────────────┐
│ 🔧 SUPPORT TECHNIQUE (Prestataire)                    │
├────────────────────────────────────────────────────────┤
│ 📧 Email    : support-technique@prestataire.fr        │
│ 📞 Téléphone: +33 X XX XX XX XX (urgent uniquement)   │
│ ⏱️ Horaires : Lun-Ven 9h-18h                          │
│ ⚡ Délai    : <4h (urgent), <24h (standard)           │
└────────────────────────────────────────────────────────┘
```

**Template email support (à inclure) :**
- Modèle complet email avec sections numérotées
- Description problème, étapes reproduction, captures écran, logs, contexte, urgence

### 4.2 Questions Fonctionnelles

**Instructions:**
- Questions partenaires : Template CSV, procédures mises à jour, renvois Guide Partenaire/Commercial
- Contact interne EFFICERAM (email, Slack)

### 4.3 Feedback sur ce Guide

**Instructions:**
- Types feedback appréciés (erreurs, procédures floues, suggestions)
- Email doc-cerascan@efficeram.fr avec template objet
- Versionnage : v1.0 (2026-02-28), mises à jour trimestrielles, changelog Annexe D

---

## Section 5: Ressources Complémentaires

**Objectif:** Lister autres documents et ressources disponibles.

### 5.1 Autres Guides CERASCAN

**Instructions:**
- **Guide Partenaire** (20-25 pages) : Pour propriétaires showrooms
- **Guide Commercial** (5-10 pages) : Pour commerciaux showrooms
- **Documentation Technique** : Réservée prestataire (hors accès)

### 5.2 Templates et Fichiers Types

**Instructions:**
- Template CSV catalogue (Annexe A)
- Template email onboarding partenaire (ressources en ligne)
- Schémas placement QR showroom (Annexe B)

### 5.3 Formations Disponibles

**Instructions:**
- Formation initiale admin (4-5h, présentiel/visio)
- Webinaires mensuels nouveautés (1x/mois)
- Support vidéo procédures (YouTube interne)

---

## Notes d'Écriture

**Ton :** Direct, opérationnel, pas de pédagogie (vous connaissez CERASCAN)  
**Terminologie :** Rôles uniquement (le partenaire, le commercial, le prestataire)  
**Visuels obligatoires :** Encadré "EN BREF", tableau navigation, schéma workflow 4 étapes, blocs contacts  
**Longueur cible :** 2-3 pages (~1000-1200 mots)

---

**Version :** 2.0 (refonte complète)  
**Date :** 2026-02-28
