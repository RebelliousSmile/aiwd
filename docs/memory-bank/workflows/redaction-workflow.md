# Workflow: Rédaction Originale LaTeX

## Vue d'Ensemble

Ce workflow couvre la création de textes narratifs originaux directement en LaTeX, en respectant les styles et conventions d'univers.

**Types de rédaction:**
- Chapitres narratifs (nouvelles, romans)
- Descriptions de lieux
- Fiches PNJ (Personnages Non-Joueurs)
- Scénarios de jeu de rôle
- Règles et mécaniques de jeu

**Objectif:** Créativité narrative + respect canon et style univers + qualité LaTeX professionnelle

---

## Étapes du Workflow

### 1. Préparation et Contexte

**1.1. Définir le projet**

**Si nouveau projet:**
```
"Crée un nouveau projet '<nom>' dans l'univers <univers> (type: rédaction originale)"
→ Invoque agent project-manager
```

**Exemple:**
```
"Crée un nouveau projet 'Nouvelles de Houlemorte' dans l'univers Archipels"
"Crée un nouveau projet 'Chroniques de Tar Valon' dans l'univers La Roue du Temps"
```

**Structure créée:**
```
<univers>/<nom-projet>/
├── .git/
├── README.md (type: rédaction originale)
├── main.tex (template univers)
├── chapitres/
└── notes/  # Notes de préparation, plans
```

**1.2. Charger documentation tenant**

**Selon type de rédaction:**

**Narration (nouvelle, roman):**
```
@<univers>/.docs/UNIVERS.md
@<univers>/.docs/terminologie.md
@<univers>/.docs/style-guide.md
@<univers>/.docs/lieux.md (si applicable)
@<univers>/.docs/personnages.md (si applicable)
@docs/output-styles/latex-<univers>.md
```

**Règles JdR:**
```
@<univers>/.docs/UNIVERS.md
@<univers>/.docs/terminologie.md
@<univers>/.docs/regles-jdr.md (si existant)
@docs/output-styles/manuel-jdr-prose.md
```

**1.3. Définir le brief de rédaction**

**Éléments à clarifier avec l'utilisateur:**

| Élément | Exemples | Importance |
|---------|----------|------------|
| Sujet | Ville de Houlemorte, voyage en mer, rencontre PNJ | Critique |
| Longueur | 2 pages, 1 chapitre, 5000 mots | Critique |
| Ton | Épique, sombre, mystérieux, léger | Important |
| Point de vue | 1ère personne, 3ème personne | Important |
| Contraintes | Intégrer personnage X, se dérouler à lieu Y | Optionnel |

**Exemple de brief:**
```
Utilisateur: "Écris le chapitre 2 sur la ville de Houlemorte, environ 3 pages,
ton aventure maritime, point de vue 3ème personne, doit introduire Maître Elric"
```

---

### 2. Planification Narrative

**2.1. Créer plan/structure**

**Pour chapitre narratif:**
```
1. Accroche (1-2 paragraphes)
2. Développement (3-5 sections)
3. Climax/Tournant (1 section)
4. Conclusion/Ouverture (1-2 paragraphes)
```

**Pour description de lieu:**
```
1. Vue d'ensemble
2. Quartiers/Zones principales
3. Lieux notables (encadrés)
4. PNJ clés (encadrés)
5. Légendes et rumeurs
```

**Pour fiche PNJ:**
```
1. Description physique
2. Personnalité et motivations
3. Historique
4. Statistiques (encadré)
5. Liens avec autres PNJ/lieux
```

**Pour règles JdR:**
```
1. Concept de la règle
2. Mécaniques (étape par étape)
3. Exemples d'application
4. Tables récapitulatives
5. Cas particuliers
```

**2.2. Définir éléments clés**

**Checklist:**
- [ ] Personnages impliqués (noms, rôles)
- [ ] Lieux concernés (noms, descriptions courtes)
- [ ] Termes spécifiques à utiliser (terminologie univers)
- [ ] Citations/proverbes potentiels
- [ ] Encadrés nécessaires (stats, citations, règles)

---

### 3. Rédaction Assistée

**3.1. Invoquer skill narrative-writer**

**Commande utilisateur:**
```
"Écris le chapitre 2 sur la ville de Houlemorte"
"Rédige une fiche PNJ pour Capitaine Vérane"
"Crée un scénario d'introduction pour Archipels"
```

**La skill effectue automatiquement:**

1. **Chargement contexte:**
   - Documentation tenant <univers>
   - Style guide
   - Terminologie
   - Output-style LaTeX

2. **Respect style narratif:**
   - Ton défini dans style-guide.md
   - Longueur phrases adaptée
   - Vocabulaire spécifique univers
   - Structure narrative cohérente

3. **Application terminologie:**
   - Détection automatique termes à formater
   - Italiques, petites caps, majuscules selon conventions
   - Utilisation commandes template (`\citationarchipels{}`, `\textsc{}`, etc.)

4. **Génération encadrés:**
   - `\begin{statsbox}` pour PNJ/lieux
   - `\begin{quotebox}` pour citations
   - `\begin{mecanismebox}` pour règles (Ecryme)
   - `\begin{pouvoirbox}` pour pouvoirs (WoT)

5. **Structuration LaTeX:**
   - `\section{}`, `\subsection{}` pour titres
   - Paragraphes séparés (double saut de ligne)
   - Commandes spécifiques univers

**3.2. Résultat attendu**

**Fichier .tex généré:**
```latex
% chapitres/chapitre2.tex
% Rédaction originale: La ville de Houlemorte

\section{Houlemorte}

\citationarchipels{Houlemorte ne dort jamais, car la mer ne dort jamais.}
{Kalen, chroniqueur itinérant}

\vspace{0.5cm}

\textsc{Houlemorte} s'étend sur trois îles reliées par d'imposants ponts de
pierre grise, battus par les embruns depuis des siècles. La cité portuaire
principale des Îles Connues ne connaît ni le silence ni le repos : à toute
heure du jour et de la nuit, le Grand Port grouille d'une activité frénétique.

\subsection{Le Grand Port}

Les quais de Houlemorte s'étendent sur près de deux lieues, bordés de
navires aux pavillons multicolores. Voiliers marchands, caravelles
d'exploration, frégates de guerre : tous se côtoient dans un ballet maritime
incessant. L'odeur du sel se mêle à celle du poisson frais, des épices
exotiques et du goudron chaud.

[... suite du chapitre ...]

\subsection{Lieux Notables}

\begin{statsbox}{La Tour des Navigateurs}
  \textbf{Type:} Siège de l'\textit{Ordre des Navigateurs}\\
  \textbf{Architecture:} Tour de pierre bleue de 50 mètres\\
  \textbf{Accès:} Réservé aux membres de l'Ordre

  \vspace{0.3cm}

  La Tour des Navigateurs domine le Grand Port de sa silhouette imposante.
  C'est là que les maîtres navigateurs gardent jalousement leurs cartes des
  routes maritimes les plus lucratives.
\end{statsbox}

\subsection{Personnages Clés}

\begin{statsbox}{Maître Elric}
  \textbf{Rôle:} Maître cartographe de la \textit{Guilde des Cartographes}\\
  \textbf{Âge:} 67 ans\\
  \textbf{Compétences:} Cartographie experte, Connaissance des routes secrètes\\
  \textbf{Motivation:} Cartographier l'intégralité des Îles Connues

  \vspace{0.3cm}

  Maître Elric est un homme âgé aux mains tachées d'encre bleue, les doigts
  déformés par des décennies passées à dessiner des cartes. Ses yeux gris
  scrutent les interlocuteurs avec une intensité dérangeante, comme s'il
  cherchait à cartographier leur âme. Il possède la plus grande collection
  privée de cartes marines des Îles Connues.
\end{statsbox}
```

---

### 4. Révision et Enrichissement

**4.1. Relecture narrative**

**Checklist qualité:**
- [ ] Cohérence narrative (pas de contradictions)
- [ ] Personnages crédibles (motivations claires, actions logiques)
- [ ] Descriptions riches mais pas surchargées
- [ ] Dialogues naturels
- [ ] Rythme varié (alternance tension/calme)
- [ ] Pas de répétitions (vocabulaire varié)

**4.2. Vérification respect canon**

**Par univers:**

**Archipels:**
- [ ] Vocabulaire nautique correct
- [ ] Ambiance maritime présente (sons, odeurs, vues)
- [ ] Lieux cohérents avec géographie établie
- [ ] Organisations respectées (Ordre des Navigateurs, Guilde des Cartographes)

**La Roue du Temps:**
- [ ] Terminologie stricte (italiques pour Vieille Langue)
- [ ] Majuscules respectées (La Roue, Le Dessin, Le Pouvoir)
- [ ] Canon respecté (personnages, lieux, événements)
- [ ] Ton épique de Robert Jordan
- [ ] "Aes Sedai" invariable

**Ecryme:**
- [ ] Atmosphère sombre et steampunk
- [ ] Vocabulaire technique (rouages, engrenages, vapeur)
- [ ] Dualité lumière/ténèbres
- [ ] Termes avec `\terme{}` correctement utilisés

**4.3. Enrichissement interactif**

**Demander à l'utilisateur:**
```
"Veux-tu que j'ajoute/modifie ?"
- Plus de descriptions sensorielles (odeurs, sons, textures)
- Dialogue supplémentaire pour personnage X
- Encadré pour lieu Y
- Citation d'ouverture différente
```

**Exemple:**
```
Claude: "Le chapitre sur Houlemorte est terminé (3 pages). Veux-tu que j'ajoute:
- Un encadré sur le Quartier des Cartographes ?
- Un dialogue entre Maître Elric et un capitaine marchand ?
- Plus de descriptions du marché aux épices ?"

Utilisateur: "Oui, ajoute le dialogue avec le capitaine marchand"

Claude: [Génère section supplémentaire avec dialogue]
```

---

### 5. Validation et Compilation

**5.1. Validation syntaxe LaTeX**

**Commande:**
```
"Valide le fichier chapitres/chapitre2.tex"
```

**Skill latex-validator vérifie:**
- Syntaxe LaTeX correcte
- Commandes template valides
- Encodage UTF-8
- Encadrés bien formés

**5.2. Compilation test**

```bash
cd <univers>/<projet>/
pdflatex main.tex
pdflatex main.tex
```

**Checklist visuelle PDF:**
- [ ] Mise en page élégante
- [ ] Couleurs template appliquées
- [ ] Encadrés bien formés
- [ ] Pas de débordements texte
- [ ] Citations bien formatées
- [ ] Titres hiérarchisés correctement

**5.3. Corrections finales**

**Erreurs courantes:**
- Paragraphes trop longs (> 10 lignes) → Couper
- Encadrés trop denses → Aérer ou raccourcir
- Titres ambigus → Reformuler
- Répétitions de mots → Varier vocabulaire

---

### 6. Intégration et Finalisation

**6.1. Inclure dans main.tex**

```latex
\documentclass[a5paper,11pt]{book}

\usepackage{../.templates/archipels}

\begin{document}

\tableofcontents

\include{chapitres/chapitre1}
\include{chapitres/chapitre2}  % Nouveau chapitre rédigé

\end{document}
```

**6.2. Recompiler document complet**

```bash
pdflatex main.tex
pdflatex main.tex
```

**6.3. Commit Git**

```bash
cd <univers>/<projet>/
git add chapitres/chapitre2.tex
git commit -m "Rédaction chapitre 2: Houlemorte

- Description complète de la cité
- Ajout PNJ Maître Elric
- Encadrés Tour des Navigateurs et Quartier des Cartographes
- 3 pages, ton aventure maritime

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"
git push
```

---

## Cas Particuliers

### Rédaction Nouvelle Courte (5-10 pages)

**Structure recommandée:**
```latex
\section{Titre de la Nouvelle}

% Accroche
[1-2 paragraphes introductifs captivants]

\subsection{Partie I: Contexte}
[Introduction personnages et situation]

\subsection{Partie II: Développement}
[Péripéties, tensions]

\subsection{Partie III: Climax}
[Point culminant]

\subsection{Partie IV: Résolution}
[Conclusion, ouverture]
```

### Rédaction Fiche Scénario JdR

**Structure recommandée:**
```latex
\section{Nom du Scénario}

\subsection{Résumé}
[2-3 phrases décrivant l'intrigue]

\subsection{Contexte}
[Situation de départ, PNJ impliqués]

\begin{statsbox}{PNJ Principal}
  Statistiques et description
\end{statsbox}

\subsection{Actes}

\subsubsection{Acte I: Découverte}
[Scènes initiales]

\subsubsection{Acte II: Investigation}
[Développement]

\subsubsection{Acte III: Confrontation}
[Climax]

\subsection{Dénouements Possibles}
[Issues alternatives selon actions joueurs]
```

### Rédaction Règles de Jeu

**Structure recommandée:**
```latex
\section{Nom de la Règle}

\subsection{Concept}
[Explication générale, objectif de la règle]

\subsection{Mécaniques}

\begin{enumerate}
  \item Étape 1: [Description]
  \item Étape 2: [Description]
  \item Étape 3: [Description]
\end{enumerate}

\subsection{Exemple}

[Exemple concret d'application]

\subsection{Table Récapitulative}

\begin{center}
\begin{tabular}{|l|c|c|}
\hline
\textbf{Situation} & \textbf{Modificateur} & \textbf{Difficulté} \\
\hline
Facile & +2 & 10 \\
Normale & 0 & 15 \\
Difficile & -2 & 20 \\
\hline
\end{tabular}
\end{center}
```

---

## Optimisations et Bonnes Pratiques

### Créativité dans le Cadre

**Équilibre canon vs liberté créative:**

| Univers | Canon strict | Libertés possibles |
|---------|--------------|-------------------|
| **Archipels** | Géographie établie, organisations | Nouveaux lieux secondaires, PNJ originaux, légendes |
| **La Roue du Temps** | Terminologie, personnages principaux | Nouvelles dans cadre établi, PNJ secondaires, périodes peu documentées |
| **Ecryme** | Ambiance steampunk-gothique | Grande liberté créative (univers original) |

### Itération et Feedback

**Workflow itératif:**
```
1. Rédaction brouillon (skill narrative-writer)
2. Relecture utilisateur
3. Ajustements demandés
4. Validation finale
5. Compilation et commit
```

**Exemple:**
```
Claude: [Génère chapitre 2 sur Houlemorte]

Utilisateur: "Bien, mais j'aimerais plus de détails sur le marché aux épices"

Claude: [Ajoute section détaillée marché aux épices]

Utilisateur: "Parfait, valide"

Claude: [Compile et commit]
```

### Réutilisation d'Éléments

**Créer bibliothèque de snippets:**

```
<univers>/<projet>/snippets/
├── lieux/
│   ├── houlemorte.tex
│   └── vendrest.tex
├── pnj/
│   ├── verane.tex
│   └── elric.tex
└── citations/
    └── proverbes_marins.tex
```

**Inclusion dans nouveaux chapitres:**
```latex
\input{snippets/pnj/elric.tex}  % Encadré Maître Elric réutilisable
```

---

## Checklist Complète Rédaction

**Avant de commencer:**
- [ ] Projet créé ou existant identifié
- [ ] Univers cible défini
- [ ] Documentation tenant chargée
- [ ] Brief de rédaction clair (sujet, longueur, ton)

**Pendant rédaction:**
- [ ] Plan narrative défini
- [ ] Éléments clés listés (personnages, lieux, termes)
- [ ] Style guide respecté
- [ ] Terminologie appliquée correctement
- [ ] Encadrés utilisés judicieusement

**Après rédaction:**
- [ ] Relecture narrative (cohérence, qualité)
- [ ] Vérification canon
- [ ] Validation syntaxe LaTeX
- [ ] Compilation test réussie
- [ ] PDF visuellement satisfaisant
- [ ] Chapitre inclus dans main.tex
- [ ] Commit Git effectué

**Qualité finale:**
- [ ] Aucune erreur de compilation
- [ ] Récit captivant et cohérent
- [ ] Style univers respecté
- [ ] Terminologie exacte
- [ ] Mise en page professionnelle
- [ ] Lisibilité optimale

---

## Support et Résolution Problèmes

### Problème: Manque d'Inspiration

**Solution:**
- Consulter `lieux.md`, `personnages.md` pour idées
- Lire passages style-guide.md pour exemples
- Demander suggestions: "Propose 3 idées de scénarios Archipels"

### Problème: Ton Narratif Incorrect

**Solution:**
```
"Réécris le chapitre 2 avec un ton plus [épique/sombre/léger]"
→ Skill narrative-writer ajuste style
```

### Problème: Terminologie Oubliée

**Solution:**
```
"Vérifie que la terminologie Archipels est respectée dans chapitre2.tex"
→ Analyse et corrections automatiques
```

### Problème: Encadré Mal Formaté

**Solution:**
```
"Valide le fichier chapitres/chapitre2.tex"
→ Latex-validator identifie erreurs encadrés
```

---

**Version:** 1.0
**Dernière mise à jour:** 2025-11-08
**Maintenu par:** Claude Code + Utilisateur
