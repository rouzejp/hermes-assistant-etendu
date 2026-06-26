# Intégrations

> Connecteurs et services externes reliés à Hermes.

---

## Communication

| Canal | Usage | Technologie |
|-------|-------|-------------|
| **Telegram** | Canal principal — bot @hermeswsljpr_bot | Telegram Bot API |
| **Discord** | #jenkins (logs), #hermes-reports (briefings), #inter-hermes | Discord Bot |
| **Email** | 3 comptes IMAP (hermes, jeanpaul, rouzejp) | Himalaya CLI |
| **WhatsApp** | Notifications | WhatsApp API |
| **WebUI** | Dashboard Hermes (hermes.rouze.eu) | Hermes WebUI |

---

## Domotique

| Service | Accès | Usage |
|---------|-------|-------|
| **Home Assistant GTV** | ha.rouze.eu | Volets, capteurs, clim |
| **Home Assistant Sorède** | domotique.gite-puig-neulos.fr | Domotique résidence secondaire |

**Actions courantes :** ouverture/fermeture volets roulants, lecture capteurs, programmation chauffage.

---

## Stockage et données

| Service | Usage | Technologie |
|---------|-------|-------------|
| **Obsidian** | Base de connaissance, projets, veille | Markdown + Dataview |
| **Notion** | Listes partagées (courses, équipement Sorède) | Notion API + ntn CLI |
| **NAS Synology DS218play** | Stockage fichiers, backups | API REST DSM |
| **Karakeep** | Bookmarks et veille web | Karakeep API (MCP) |

---

## Publication

| Service | Usage | Technologie |
|---------|-------|-------------|
| **WordPress** | Blog principal | WordPress REST API |
| **FAL.ai** | Génération d'images (FLUX 2 Klein) | FAL API |
| **ComfyUI** | Génération d'images locale (Book4) | Comfy Desktop |

---

## Recherche et veille

| Service | Usage | Technologie |
|---------|-------|-------------|
| **SearX** | Moteur de recherche local (search.rouze.eu) | SearXNG |
| **Watchers** | Surveillance RSS/API | Hermes watchers |
| **Cron veille** | Collecte multi-sources toutes les 3h | Hermes cron |

---

## Infrastructure

| Service | Usage | Technologie |
|---------|-------|-------------|
| **GitHub** | Code, documentation publique | gh CLI |
| **Uptime Kuma** | Surveillance de services | Uptime Kuma (LXC) |
| **Ntfy** | Notifications push | Ntfy (LXC) |
| **Vaultwarden** | Gestion de mots de passe | Vaultwarden (LXC) |
| **Syncthing** | Sync coffre VPS ↔ Book4 | Syncthing |
| **WireGuard** | Tunnel VPS ↔ Homelab | WireGuard |
| **Caddy** | Reverse proxy + TLS | Caddy (Docker) |

---

## Plan de secours (mode hors-ligne)

En cas d'absence d'internet, le **Book4 Windows** prend le relais :

- **Hermes One** tourne en local
- **Ollama** avec Qwen 3.5:9b
- **Obsidian** en local (synced via Syncthing quand le réseau revient)
- Pas de dépendance VPS
