# Hermes Assistant Étendu

> Un assistant personnel orchestré par [Hermes Agent](https://hermes-agent.nousresearch.com), piloté depuis un coffre [Obsidian](https://obsidian.md), connecté à un homelab, des services cloud, et des canaux de communication quotidiens.

**Philosophie :** *the best part is no part* — chaque composant superflu est un composant qui cassera un jour. On ajoute avec parcimonie, on supprime avec enthousiasme.

---

## Vision

Transformer un agent LLM générique en **assistant personnel durable** :

- **Orchestré** par Hermes Agent (point d'entrée unique, dispatch de tâches, cron, watchers)
- **Mémorisant** via un graphe de faits structurés (Memory Kernel) — pas de RAG aveugle
- **Connecté** à la vie réelle : domotique, email, réseaux sociaux, blog, NAS, serveurs
- **Traçable** via un système Kanban — chaque tâche a son cycle de vie, ses dépendances, son handoff
- **Documenté** dans Obsidian — le coffre est à la fois la base de connaissance et le tableau de bord

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Hermes Agent                           │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌───────────┐  │
│  │  Cron    │  │ Watchers │  │ Kanban   │  │ Webhook   │  │
│  │  jobs    │  │ (RSS/API)│  │ Workers  │  │ listeners │  │
│  └──────────┘  └──────────┘  └──────────┘  └───────────┘  │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              Memory Kernel (SQLite)                  │  │
│  │  Graphe de faits persistants — préférences,         │  │
│  │  projets, connaissances, contraintes, habitudes     │  │
│  └──────────────────────────────────────────────────────┘  │
└──────────────────────┬──────────────────────────────────────┘
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
┌──────────────┐ ┌──────────┐ ┌──────────────┐
│   Obsidian   │ │ Services │ │ Canaux       │
│   (coffre)   │ │ externes │ │ (TG, Discord,│
│              │ │          │ │  email, etc.)│
│  Notes       │ │ HA       │ │              │
│  Projets     │ │ NAS      │ │ Telegram     │
│  Veille      │ │ Blog WP  │ │ Discord      │
│  Dashboard   │ │ Notion   │ │ Email        │
│              │ │ GitHub   │ │ WhatsApp     │
└──────────────┘ └──────────┘ └──────────────┘
```

---

## Composants clés

| Composant | Rôle | Technologie |
|-----------|------|-------------|
| **Hermes Agent** | Orchestrateur, dispatch, cron, watchers | Python, Nous Research |
| **Obsidian Vault** | Base de connaissance, projets, veille | Markdown, Dataview |
| **Memory Kernel** | Mémoire persistante structurée | SQLite + graphe de faits |
| **Kanban Board** | Pipeline de tâches traçables | SQLite + workers Hermes |
| **Home Assistant** | Domotique (volets, capteurs, clim) | HA + API REST |
| **Blog WordPress** | Publication d'articles | WordPress REST API |
| **Notion** | Listes partagées (courses, équipement) | Notion API + ntn CLI |
| **Karakeep** | Bookmarks et veille web | Karakeep API |
| **Syncthing** | Sync coffre VPS ↔ laptop | Syncthing |

---

## Canaux de communication

- **Telegram** — canal principal (bot @hermeswsljpr_bot)
- **Discord** — #jenkins (logs), #hermes-reports (briefings), #inter-hermes
- **Email** — 3 comptes IMAP (hermes, jeanpaul, rouzejp)
- **WhatsApp** — notifications
- **WebUI** — dashboard Hermes (hermes.rouze.eu)

---

## Principes directeurs

1. **The best part is no part** — supprimer plutôt qu'ajouter
2. **Event-driven** — réagir aux changements, pas poller
3. **Traçabilité** — chaque action a un handoff structuré
4. **Local d'abord** — fonctionner sans internet quand nécessaire
5. **Zéro coût** — solutions simples, pas de SaaS superflus

Voir [`docs/philosophy.md`](docs/philosophy.md) pour le détail.

---

## Documentation

| Document | Contenu |
|----------|---------|
| [`docs/architecture.md`](docs/architecture.md) | Architecture détaillée, composants, flux |
| [`docs/obsidian-vault.md`](docs/obsidian-vault.md) | Structure du coffre, conventions, nommage |
| [`docs/kanban-workflow.md`](docs/kanban-workflow.md) | Workers, dispatch, cycle de vie des tâches |
| [`docs/integrations.md`](docs/integrations.md) | Connecteurs et services externes |
| [`docs/philosophy.md`](docs/philosophy.md) | Principes et décisions de design |
| [`docs/memory-kernel.md`](docs/memory-kernel.md) | Graphe de faits persistants |

---

## Licence

MIT — voir [LICENSE](LICENSE).
