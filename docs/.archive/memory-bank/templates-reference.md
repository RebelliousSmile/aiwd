# Templates LaTeX - Référence Complète

## Archipels (v1.1)

**Palette:**
- `archipelsblue` : Bleu marine (océan)
- `archipelsbeige` : Beige parchemin
- `archipelsred` : Rouge accent
- `archipelsgold` : Or détails

**Commandes:**
```latex
\citationarchipels{texte}{auteur}              % Citations maritimes
\begin{statsbox}{Nom Lieu}...\end{statsbox}    % Stats (lettrine "S")
\begin{quotebox}[Titre]...\end{quotebox}       % Encadré ambiance (lettrine "Q")
\begin{rulebox}[Titre]...\end{rulebox}         % Règles de jeu (lettrine "R")
\begin{twocoltext}...\end{twocoltext}          % Texte sur 2 colonnes
\colbreak                                      % Saut de colonne
\talent{nom}{prérequis}{description}           % Talents personnage
\equipement{nom}{prix}{description}            % Équipement
\pnj{nom}{description}                         % PNJ simple
\checkbox                                      % Case à cocher
```

**Documentation:** `archipels/.templates/README.md`

---

## La Roue du Temps (v1.0)

**Palette:**
- `wotgold` : Or (Aes Sedai)
- `wotblue` : Bleu profond (Ajah Bleue)
- `wotred` : Rouge foncé (Ajah Rouge)
- `wotgreen` : Vert (Ajah Verte)
- `wotparchment` : Beige parchemin
- `wotdarkblue` : Bleu marine foncé

**Commandes raccourcies:**
```latex
\saidin                                     % → \textit{saidin}
\saidar                                     % → \textit{saidar}
\angreal, \sangreal, \terangreal            % Objets de Pouvoir
\cuendillar                                 % Coeur-de-pierre
\taveren                                    % ta'veren
\telaran                                    % Tel'aran'rhiod
\motitalique{terme}                         % Auto-italique personnalisé
```

**Environnements:**
```latex
\citationwot{texte}{auteur}                 % Citations WoT
\begin{quotebox}...\end{quotebox}           % Encadré ambiance
\begin{pouvoirbox}{titre}...\end{pouvoirbox}% Tissages du Pouvoir
\begin{nationbox}{nom}...\end{nationbox}    % Nations/peuples
\begin{rulebox}...\end{rulebox}             % Règles de jeu
```

**Conventions STRICTES:**
- Termes Vieille Langue en italique: `\saidin`, `\saidar`
- Orthographe officielle: "Aes Sedai" (invariable)
- Majuscules conceptuelles: Le Pouvoir, Le Ténébreux, La Lumière

**Documentation:** `wot/.templates/README.md`

---

## Ecryme (v1.0)

**Style:** Steampunk gothique mécanique

**Palette:**
- `ecrymecopper` : Cuivre terni
- `ecrymeiron` : Fer sombre
- `ecrymeshadow` : Gris ombre profonde
- `ecrymeparchment` : Parchemin vieilli
- `ecrymegold` : Or terni
- `ecrymerust` : Rouille

**Commandes:**
```latex
\terme{mot}                                     % Termes techniques en italique
\citationecryme{texte}                          % Citations Ecryme
\gothicfont                                     % Police gothique
```

**Environnements:**
```latex
\begin{quotebox}...\end{quotebox}               % Encadré ambiance
\begin{mecanismebox}{titre}...\end{mecanismebox}% Mécanismes techniques
\begin{secretbox}...\end{secretbox}             % Secrets/mystères
\begin{rulebox}...\end{rulebox}                 % Règles de jeu
```

**Documentation:** `ecryme/.templates/README.md`
