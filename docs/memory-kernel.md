# Memory Kernel

> Graphe de faits persistants — la mémoire structurée d'Hermes.

---

## Principe

Le Memory Kernel est un **graphe de faits** stocké dans SQLite. Chaque fait est un triplet :

```
sujet → prédicat → objet
```

Avec des métadonnées : catégorie, importance (0-1), timestamp, source.

Contrairement à un RAG vectoriel (qui retrouve des documents par similarité), le Memory Kernel stocke des **faits atomiques et structurés** — des affirmations précises, pas des blocs de texte.

---

## Catégories de faits

| Catégorie | Contenu | Exemple |
|-----------|---------|---------|
| `preference` | Préférences utilisateur | "jean-paul préfère solutions simples" |
| `project` | État des projets | "jean-paul travaille sur soc-autonome" |
| `knowledge` | Connaissances durables | "jean-paul connaît real-time-computing" |
| `habit` | Habitudes et routines | "jean-paul utilise fail2ban-webhook" |
| `constraint` | Contraintes techniques | "jean-paul a contrainte pas-de-saas" |
| `tool` | Outils et configurations | "jean-paul utilise ollama-cloud" |
| `decision` | Décisions d'architecture | "jean-paul a décidé event-driven-inbox" |
| `identity` | Identité et rôles | "jean-paul est rouzejp sur GitHub" |
| `relationship` | Relations | "catherine est conjointe de jean-paul" |
| `persona` | Traits de personnalité | "jean-paul attend action rapide" |
| `values` | Valeurs et principes | "jean-paul valorise zéro-coût" |
| `belief` | Croyances | "jean-paul croit best-part-is-no-part" |

---

## Injection automatique

Avant chaque échange avec l'utilisateur, les faits pertinents du Memory Kernel sont automatiquement injectés dans le contexte de l'agent. Cela permet :

- De se souvenir des préférences sans les redemander
- D'adapter le ton et le style
- De connaître l'état des projets en cours
- D'éviter les répétitions et les erreurs déjà corrigées

---

## Dashboard

Un dashboard Flask (port 8080, kernel.rouze.eu) permet de visualiser et interroger le graphe :

- **Recherche** par mot-clé
- **Probe** : tous les faits sur un sujet
- **Liste** par catégorie
- **Stats** : nombre de faits, répartition par catégorie

---

## Commandes

```bash
# Recherche textuelle
memory_kernel search "préfère"

# Tous les faits sur un sujet
memory_kernel probe jean-paul

# Liste par catégorie
memory_kernel list preference

# Statistiques
memory_kernel stats

# Ajout manuel
memory_kernel add jean-paul préfère solutions-simples 0.7
```

---

## Bonnes pratiques

- **Ne pas** sauvegarder de secrets, mots de passe, ou tokens
- **Ne pas** sauvegarder d'état temporaire (logs, résultats de tâches)
- **Préférer** les faits durables et réutilisables
- **Corriger** un fait erroné plutôt qu'en ajouter un contradictoire
- **Importance** : 0.8+ pour les règles absolues, 0.5 pour les préférences, 0.3 pour les connaissances anecdotiques
