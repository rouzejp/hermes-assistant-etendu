# Structure du coffre Obsidian

> Conventions, organisation et règles de nommage du vault Obsidian.

---

## Arborescence

```
/root/obsidian-jenkins/
├── 00_Boite_Reception/     ← Captures web en attente de tri
│   └── _inbox.md           ← Index des entrées
├── 02_Notes_Permanentes/   ← Notes thématiques durables
├── 11_Racine/              ← Dashboard, AGENTS.md, log, grille JD
├── 12_Systeme/             ← Modèles, config, documentation système
│   └── 01_Modeles/         ← Templates de notes
├── 13_Archives/            ← Projets finis, notes inactives
├── 14_Excalidraw/          ← Schémas, diagrammes
├── 20_Concepts/            ← Domaines de connaissance
│   ├── Histoire/
│   ├── Immobilier/
│   ├── Reseaux-sociaux/    (Facebook/, Bluesky/, Instagram/)
│   ├── Scepticisme/        (dossiers de recherches/)
│   ├── Sciences/           (ia/, Techno/)
│   ├── Societe/            (naturisme/, wokisme/)
│   ├── Vehicules/
│   ├── Securite/
│   └── OSINT/              (Personnes/, Sites/, Organisations/, Rumeurs/)
├── 21_Reflexions/          ← Pistes émergentes, notes de réflexion
├── 30_Projets/             ← Projets actifs
├── 31_Second_Cerveau/      ← Bookmarks, captures rapides
├── 40_Homelab/             ← Serveurs, réseau, services
├── 50_Usine_Contenu/       ← Pipeline idéation → publication
├── 51_Blog/                ← Articles blog, cogitations
├── 60_Bibliotheque/        ← Lectures, fiches livres
├── 61_Entites/             ← Personnes, outils, projets documentés
├── 62_Comparaisons/        ← Tableaux comparatifs
├── 70_Raw/                 ← Données brutes, articles sources
├── 71_Queries/             ← Veille, recherches temporaires
├── 72_Rapports/            ← Analyses, synthèses
├── 84_Radio/               ← Radioamateur, SDR, trafic
├── Archives/               ← Ancienne structure (transition)
├── data/                   ← Données externes (hors vault)
├── db/                     ← Bases de données (hors vault)
├── scripts/                ← Scripts système (hors vault)
└── YOLO/                   ← Bac à sable (hors vault)
```

---

## Conventions de nommage

- **Dossiers en français** uniquement (sauf exceptions établies : `31_Second_Cerveau`, `Excalidraw`)
- **Pas de doublons bilingues** — `31_Second_Cerveau` OK, pas `30_Second_Brain`
- **Fichiers auto-descriptifs** — pas de `note1.md`
- **Dates** au format `YYYY-MM-DD` dans les noms de fichiers de veille

---

## Préfixes de commandes

| Préfixe | Usage | Routage |
|---------|-------|---------|
| `note:` | Brouillon/article | Dossier thématique selon le sujet |
| `lu:` | Fiche livre lu | `60_Bibliotheque/` (date du jour) |
| `alire:` | Fiche livre à lire | `60_Bibliotheque/` |
| `fact:` | Demande de fact-checking | Note dans dossier approprié |
| `RAG:` | Réponse sourcée depuis une source | Dans la conversation uniquement |
| `vu:` | Élément regardé (liste TV) | Mentionné dans le fil de discussion |

---

## Conventions de format

- **Wikilinks** `[[note]]` pour la navigation interne
- **Callouts** `>` avec emoji pour avertissements et notes
- **Tables** pour les références structurées
- **Code blocks** pour commandes shell et exemples
- **Titres hiérarchiques** `##`, `###`, `####`
- **Séparateurs** `---` entre les grandes sections
- **Dataview** pour les vues dynamiques (dashboard, index)

---

## Workflows courants

### Capture web
1. Extraire le contenu de l'URL
2. Résumer les points clés
3. Déposer dans `00_Boite_Reception/` avec titre et date
4. L'utilisateur trie via l'inbox

### Fiche de lecture
1. `lu:` → date = aujourd'hui, auteur recherché si non fourni
2. Créer la fiche dans `60_Bibliotheque/`
3. Structure : titre, auteur, date, résumé, citation(s), notation

### Article de blog
1. Ébauche → `51_Blog/` ou via `note:`
2. Fact-check des affirmations
3. Relecture et corrections
4. Publication WordPress (catégorie selon le dossier, image à la une)

---

## AGENTS.md

Le fichier [`AGENTS.md`](../AGENTS.md) à la racine du coffre est le point d'entrée pour tout agent Hermes travaillant dans ce vault. Il documente :

- L'architecture des workers Kanban
- Les conventions du coffre
- Les patterns de tâches
- Le guide de délégation
- Les workflows types
