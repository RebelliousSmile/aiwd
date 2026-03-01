# Client : auto — AIDW (AI Documentation Writing)

## Contexte

Projet open source — système de production de documentation professionnelle avec LLM. Pipeline universel : `Markdown → ICML → InDesign → PDF`. Compatible tout LLM (Claude, GPT, Gemini, LLaMA…) et tout OS (Windows, macOS, Linux).

**Dépôt :** `generationPDF/` — héberge la méthode AIDW elle-même et les projets documentation produits avec elle (`cerascan/`, `auto/`, etc.)

**Particularité :** Ce client est récursif — il documente l'outil qui sert à produire les autres documentations.

## Audience Cible

- **Primaire :** Développeur ou rédacteur technique découvrant le dépôt sur GitHub (sceptique, 10 minutes pour décider)
- **Secondaire :** Professionnel cherchant à adopter AIDW comme méthode interne (veut voir des exemples concrets, workflow complet)

**Niveau technique :** À l'aise avec Git, Markdown, terminal, et LLMs. Pas forcément familier avec InDesign.

## Ton & Style

- **Tutoiement** dans les guides pratiques — communauté open source, pas corporate
- **Show, don't tell** — commandes avant explications, output attendu documenté
- Sceptique bienvenu : anticiper "pourquoi pas juste ChatGPT ?" dès l'intro
- Exemples avec vrais chemins (`scripts/build-icml.py --project "acme-corp/api-docs"`, jamais `<votre-projet>`)
- Honnêteté sur les limites : mentionner ce qui est hors périmètre, les prérequis réels

## Contraintes

- Agnostique LLM : aucun LLM imposé, exemples génériques
- Cross-platform : commandes Windows, macOS et Linux documentées (ou notées)
- Langue : README en anglais (vitrine GitHub), guide complet en français (référence)
- Pas de contenu inventé : documenter uniquement ce qui existe dans le dépôt
- Maintenir cohérence avec `CLAUDE.md` (source de vérité système)
