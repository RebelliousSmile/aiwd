# Output Style: Documentation Développeur

**Usage:** Documentation API, SDKs, guides d'intégration  
**Audience:** Développeurs intégrant/utilisant l'API  
**Ton:** Technique mais accessible, pratique

---

## Principes Généraux

1. **Code d'abord** — Exemples avant explications théoriques
2. **Quick Start prioritaire** — Utilisateur opérationnel en <5 min
3. **Cas d'usage réels** — Pas d'exemples "foo/bar" abstraits
4. **Troubleshooting proactif** — Anticiper erreurs courantes
5. **Références complètes** — Tous endpoints/méthodes documentés

---

## Structure Type

1. **Quick Start** : Code minimal fonctionnel en 5 lignes
2. **Installation** : Prérequis, dépendances, première config
3. **Concepts clés** : 3-5 concepts essentiels max
4. **Guides** : Cas d'usage courants pas-à-pas
5. **Référence API** : Exhaustif, généré si possible
6. **Troubleshooting** : Erreurs courantes et solutions

---

## Format Code

### Quick Start Exemple
```python
# Installation
pip install notre-sdk

# Hello World (3 lignes)
from notre_sdk import Client
client = Client(api_key="YOUR_KEY")
result = client.users.list()
```

### Exemples Réels
```python
# ❌ PAS ÇA (trop abstrait)
foo = api.call("bar", {"baz": 42})

# ✅ ÇA (cas réel)
# Récupérer les transactions du mois en cours
transactions = client.transactions.list(
    start_date="2026-02-01",
    end_date="2026-02-28",
    status="completed"
)
```

---

## Ton

- **Tutoyage acceptable** dans guides pratiques : "Tu peux maintenant..."
- **Vouvoiement** dans référence API formelle
- **Concis mais pas laconique** — Expliquer le "pourquoi" si non évident

---

**Version:** 1.0  
**Date:** 2026-02-28
