# Kanban Workflow

> Système de workers, dispatch et cycle de vie des tâches.

---

## Principe

Hermes orchestre le travail via un **système de Kanban** :

- **Carte kanban** = tâche atomique (titre, description, assignataire, dépendances)
- **Worker kanban** = agent Hermes spawné pour exécuter une carte
- **Dispatcher** = processus qui surveille le tableau, réclame les tâches prêtes et lance les workers

```
┌─────────────────────────────────────────────────┐
│              Kanban Board (SQLite)              │
│  ┌────────┐  ┌────────┐  ┌────────┐            │
│  │  Todo  │→ │ Ready  │→ │Running │  → Done    │
│  └────────┘  └────────┘  └────────┘            │
│                   │   dispatch                  │
└───────────────────┼─────────────────────────────┘
                    ▼
          ┌──────────────────┐
          │  Kanban Worker   │
          │  (Hermes Agent)  │
          │  spawn + outils  │
          └──────────────────┘
```

---

## Composants

| Composant | Rôle |
|-----------|------|
| **Board** | Tableau persistant SQLite des tâches |
| **Dispatcher** | Ordonnanceur : claim → spawn → supervise |
| **Worker** | Agent Hermes exécutant une carte |
| **Orchestrator** | Agent qui décompose un objectif en cartes |

---

## Cycle de vie d'un worker

Chaque worker suit 6 étapes — son contrat avec le système :

1. **`kanban_show()`** — S'orienter : lire le body, le comment thread, les runs précédents
2. **`cd $HERMES_KANBAN_WORKSPACE`** — Travailler dans le workspace dédié
3. **`kanban_heartbeat(note=...)`** — Signaler sa vitalité (tâches > quelques minutes)
4. **Travailler** — Utiliser les outils Hermes (terminal, file, write, search, etc.)
5. **`kanban_block(reason=...)`** — Bloquer sur une décision humaine si ambiguïté
6. **`kanban_complete(summary=..., metadata=...)`** — Terminer avec handoff structuré

> ⚠️ Les workers n'ont **pas de mémoire persistante** entre les runs. Tout le contexte nécessaire doit passer par le body de la tâche, les commentaires, et le handoff.

---

## Handoff structuré

```python
kanban_complete(
    summary="1-3 phrases lisibles nommant les artéfacts concrets",
    metadata={
        "changed_files": ["/root/obsidian-jenkins/.../note.md"],
        "notes_created": 2,
        "folder_routed": "20_Concepts/...",
        "decisions": [
            "classé sous X plutôt que Y",
            "wikilinks ajoutés vers [[Note1]] et [[Note2]]"
        ],
    },
)
```

### Champs metadata importants

- `changed_files` — chemins absolus des fichiers modifiés ou créés
- `notes_created` — nombre de notes créées
- `folder_routed` — dossier de destination (si routage thématique)
- `decisions` — choix importants faits pendant la tâche

---

## Guide de délégation

| Mécanisme | Quand l'utiliser | Exemple |
|-----------|------------------|---------|
| **Action directe** | Tâche simple, unique, < 2 appels outil | Créer une note, lire un fichier |
| `delegate_task` | Raisonnement lourd, parallélisme | Rechercher 3 sujets + synthèse |
| **Carte kanban** | Tâche devant survivre à un redémarrage | Workflow publication article |
| **Cron** | Récurrent, planifié | Briefing dominical, récap coûts |

### Règles empiriques

- 1-2 échanges → action directe
- Recherche + synthèse → `delegate_task`
- Plusieurs étapes validables séparément → kanban avec dépendances
- Tâche > 5 minutes → kanban ou cron (résiste aux interruptions)
- **Ne pas** créer une carte kanban pour une action directe (sauf si trace nécessaire)

---

## Exemple : article de blog en pipeline

```
Tâche A (recherche)
  └── collecter les notes existantes sur le sujet
      │
Tâche B (rédaction) ← dépend de A
  └── rédiger le brouillon à partir des notes collectées
      │
Tâche C (fact-check) ← dépend de B
  └── vérifier les affirmations, sourcer les citations
      │
Tâche D (finalisation) ← dépend de C
  └── corriger, générer image, publier WordPress
```

Chaque étape est une carte avec `kanban_create(title=..., parents=[...])`. Le dispatcher ne lance pas B avant que A soit terminée.
