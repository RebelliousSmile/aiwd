# Rules Entity Templates

Templates pour formater les entités de jeu de manière cohérente.

## Fichiers

| Template | Usage |
|----------|-------|
| `player-character.template.md` | Personnages joueurs |
| `npc.template.md` | Personnages non-joueurs |
| `obstacle.template.md` | Obstacles et menaces |
| `asset.template.md` | Objets et équipement |

## Utilisation

Ces templates sont référencés par le prompt `rules-keeper`. Lors de la restructuration d'un fichier de règles, le LLM adapte ces templates aux mécaniques spécifiques du système.

### Personnalisation

1. Copier le template de base
2. Adapter les placeholders au système de jeu
3. Ajouter/retirer des champs selon les mécaniques

### Exemple: Adaptation pour Blades in the Dark

**player-character.template.md** devient:
```
**[Name]** - [Playbook]

Actions: [action]: [dots]...
Special Abilities: [ability1], [ability2]...
Trauma: [trauma1], [trauma2]...
Load: [current]/[max]
```
