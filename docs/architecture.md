# Architecture

> Vue d'ensemble des composants, de leurs interactions et des flux de données.

---

## Vue d'ensemble

Le système repose sur **Hermes Agent** comme orchestrateur central. Tout passe par lui : les requêtes entrantes (Telegram, Discord, email, webhooks), les tâches planifiées (cron), les surveillances (watchers RSS/API), et les actions sortantes (domotique, blog, email, GitHub).

```
                    ┌──────────────────────┐
                    │    Canaux entrants    │
                    │  TG ─ Discord ─ Email │
                    │  WhatsApp ─ WebUI     │
                    └──────────┬───────────┘
                               ▼
                    ┌──────────────────────┐
                    │    Hermes Agent      │
                    │  (orchestrateur)     │
                    │                      │
                    │  ┌────┐ ┌────┐      │
                    │  │Cron│ │Watch│      │
                    │  └────┘ └────┘      │
                    └──────────┬───────────┘
                               │
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
     ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
     │  Obsidian    │  │  Memory      │  │  Kanban      │
     │  (coffre)    │  │  Kernel      │  │  Board       │
     │              │  │  (SQLite)    │  │  (SQLite)    │
     │  Notes       │  │  Faits       │  │  Tâches      │
     │  Projets     │  │  structurés  │  │  Workers     │
     │  Veille      │  │              │  │              │
     └──────────────┘  └──────────────┘  └──────────────┘
```

---

## Flux principal

### 1. Entrée — Réception d'une demande

- **Telegram** (canal principal) → message entrant → Hermes parse, identifie l'intention
- **Discord** → messages dans #jenkins, #hermes-reports
- **Email** → 3 comptes IMAP scrutés par cron/watchers
- **Webhooks** → événements externes (HA, services)

### 2. Traitement — Orchestration

Hermes détermine le mode d'exécution :

| Type de tâche | Mécanisme | Exemple |
|---------------|-----------|---------|
| Simple, < 2 outils | Action directe | Lire une note, répondre |
| Raisonnement lourd | `delegate_task` | Recherche + synthèse |
| Multi-étapes validables | Carte Kanban | Publication article blog |
| Récurrent | Cron | Briefing dominical |
| Surveillance | Watcher | Nouvel article RSS |

### 3. Mémoire — Contextualisation

Le **Memory Kernel** injecte automatiquement les faits pertinents avant chaque échange :
- Préférences de l'utilisateur
- Connaissances durables
- État des projets
- Contraintes techniques

Pas de RAG aveugle — les faits sont structurés, catégorisés, avec importance.

### 4. Sortie — Action et réponse

- Réponse directe dans le canal d'origine
- Création/mise à jour de notes Obsidian
- Appels API (HA, WordPress, Notion, GitHub)
- Notifications push (Telegram, Discord)

---

## Composants détaillés

### Hermes Agent

- **Profil unique** (`default`) — pas de dispatch multi-profil
- **Modèle par défaut** : DeepSeek V4 Flash (via ollama-cloud)
- **Modèle pro** : DeepSeek V4 (disponible manuellement)
- **Fallback local** : Qwen 3.5:9b (Book4 Windows, mode hors-ligne)

### Obsidian Vault

- **Structure** : dossiers numérotés (00-99), français
- **Conventions** : wikilinks, Dataview, templates
- **Sync** : Syncthing entre VPS et Book4 Windows
- **Dashboard** : page racine avec Dataview queries

### Memory Kernel

- **Base** : SQLite avec graphe de faits (sujet → prédicat → objet)
- **Catégories** : preference, project, knowledge, habit, constraint, tool, etc.
- **Injection** : automatique avant chaque échange Hermes
- **Dashboard** : Flask sur port 8080 (kernel.rouze.eu)

### Kanban Board

- **Stockage** : SQLite
- **Workers** : agents Hermes spawnés par le dispatcher
- **Cycle de vie** : show → work → heartbeat → block/complete
- **Handoff** : summary + metadata structurés

---

## Infrastructure

### VPS (serveur principal)

- **Hébergement** : VPS Linux (Debian/Ubuntu)
- **Services** : Hermes Agent, Caddy (reverse proxy), Syncthing, SearX
- **Réseau** : WireGuard vers homelab (10.0.0.0/24)

### Homelab (Proxmox)

| Machine | IP | Rôle |
|---------|-----|------|
| Alpha | 192.168.1.201 (WG 10.0.0.2) | Serveur principal homelab |
| Bravo | 192.168.1.204 | LXCs divers |
| Charly | 192.168.1.163 | Services additionnels |

**LXCs notables** : Termix, Wallos, Uptime Kuma, Vaultwarden, Ntfy, Cloudflared, Traefik, Guacamole, Karakeep, Collabora

### Book4 Windows (mode hors-ligne)

- Hermes One + Ollama (Qwen 3.5:9b)
- PYTHONUTF8=1
- Fonctionne sans dépendance VPS

---

## Sécurité

- **Authentification** : Caddy basic auth (jpr / catherine)
- **Réseau** : WireGuard tunnel VPS ↔ Alpha
- **Mots de passe** : jamais dans les notes ou la mémoire
- **Tokens** : stockés dans les configs Hermes, pas dans le coffre
