# Review Pipeline - Guidelines Feedback Progression

## Problème Identifié

**UX issue :** L'agent review-pipeline est "silencieux" pendant l'exécution. L'utilisateur ne voit pas :
- Itération actuelle (1/3 ? 2/3 ?)
- Personas en cours d'analyse
- Scores intermédiaires
- Actions déclenchées (doctor, tone-finder)
- Estimation durée restante

**Impact :** Frustration utilisateur ("c'est long et je ne sais pas où ça en est")

---

## Solution : Feedback Temps Réel

### Format Recommandé

```
=== REVIEW PIPELINE CHAPITRE 02 ===
Target: 18/20 | Threshold: 14/20
Personas: 4 (olivier-larue, mj-medfan, mj-zombiology, fan-wot)
Estimation: ~6min (3 itérations max × 2min)

────────────────────────────────────────

→ Itération 1/3
  Pool actif: 4 personas
  Analyse en cours... (~2min)
  
  ✅ Résultats:
    - olivier-larue : 12/20 (CRITICAL: Contexte pratique manquant)
    - mj-medfan : 15/20 (statblocks sans référence vanilla)
    - mj-zombiology : 11/20 (CRITICAL: Souillure sans mécanique SM)
    - fan-wot : 10/20 (DEALBREAKER: Résonance sans disclaimer)
  
  → Consensus: 12.5/20 (target 18/20, Δ -5.5)
  
  Actions déclenchées:
    [A] doctor.prompt.md (5 corrections 🔴🟡)
    [B] tone-finder.prompt.md (2 patterns)
    [C] SKIP persona-trainer (consensus >10/20)

────────────────────────────────────────

→ Itération 2/3
  Pool actif: 4 personas (aucune retirée)
  Analyse en cours... (~2min)
  
  ✅ Résultats:
    - olivier-larue : 16/20 (+4) ✅
    - mj-medfan : 17/20 (+2) ✅
    - mj-zombiology : 15/20 (+4) ✅
    - fan-wot : 14/20 (+4) ✅
  
  → Consensus: 15.8/20 (target 18/20, Δ -2.2)
  
  Actions déclenchées:
    [A] doctor.prompt.md (2 corrections 🟡)
    [B] SKIP tone-finder (patterns résolus)
    [C] SKIP persona-trainer

────────────────────────────────────────

→ Itération 3/3
  Pool actif: 4 personas
  Analyse en cours... (~2min)
  
  ✅ Résultats:
    - olivier-larue : 18/20 (+2) ✅ TARGET
    - mj-medfan : 19/20 (+2) ✅ TARGET
    - mj-zombiology : 18/20 (+3) ✅ TARGET
    - fan-wot : 18/20 (+4) ✅ TARGET
  
  → Consensus: 18.3/20 ✅ TARGET ATTEINT
  
  Pool vide: 4/4 personas ≥18/20 → BREAK

────────────────────────────────────────

✅ REVIEW TERMINÉE
  Durée: ~6min
  Consensus final: 18.3/20
  Itérations: 3
  Progression: +5.8 points
  
  Rapport: .docs/comments/chapitre02-personas-quatuor-v2.md
```

---

## Implémentation

### Dans review-pipeline.md

**Step 2.1 (avant loop) :**
```python
print("=== REVIEW PIPELINE CHAPITRE {chapter} ===")
print(f"Target: {target}/20 | Threshold: {threshold}/20")
print(f"Personas: {len(pool)} ({', '.join(pool)})")
print(f"Estimation: ~{len(pool)*2}min ({max_iterations} itérations max × 2min)")
```

**Step 2.2 (début itération) :**
```python
print(f"\n→ Itération {iteration}/{max_iterations}")
print(f"  Pool actif: {len(pool_actif)} personas ({', '.join(pool_actif)})")
print(f"  Analyse en cours... (~2min)")
```

**Step 2.3 (après comment.prompt.md) :**
```python
print(f"\n  ✅ Résultats:")
for persona, score in scores.items():
    delta = score - scores_prev.get(persona, 0)
    status = "✅ TARGET" if score >= target else ""
    print(f"    - {persona}: {score}/20 (+{delta}) {status}")
    
print(f"\n  → Consensus: {consensus}/20 (target {target}/20, Δ {consensus-target})")
```

**Step 2.5 (actions) :**
```python
print(f"\n  Actions déclenchées:")
if doctor_needed:
    print(f"    [A] doctor.prompt.md ({corrections_count} corrections)")
if tone_finder_needed:
    print(f"    [B] tone-finder.prompt.md ({patterns_count} patterns)")
else:
    print(f"    [B] SKIP tone-finder")
if persona_trainer_needed:
    print(f"    [C] persona-trainer.prompt.md")
else:
    print(f"    [C] SKIP persona-trainer")
```

---

## Métriques Timing

**Durées moyennes observées :**
- 1 persona : ~30 secondes (comment.prompt.md)
- 4 personas : ~2 minutes (itération complète)
- doctor.prompt.md : ~30 secondes
- tone-finder.prompt.md : ~1 minute

**Formule estimation :**
```
Durée totale ≈ max_iterations × (nb_personas × 30s + actions × 30s)
            ≈ 3 × (4×30s + 60s) = 3 × 180s = 9min max
            ≈ 6min moyenne (2 itérations typiques)
```

---

## Notes Utilisateur

Feedback session 2026-02-13 :
> "c'est juste que je n'ai pas le déroulé à mesure je ne sais pas quand ca va finir et où ca en est"

**Action:** Implémenter feedback temps réel dans prochaine version review-pipeline v3.6+
