# Philosophie

> Principes directeurs et décisions de design du système.

---

## 1. The best part is no part

**Principe :** tout composant qu'on n'ajoute pas est un composant qui ne cassera jamais, qui n'aura jamais de dette technique, qui ne consommera jamais d'attention.

**Conséquences :**
- Remplacer un plugin buggé plutôt que le workarounder
- Supprimer un service inutilisé plutôt que le maintenir
- Préférer une solution simple et zéro coût à une architecture complexe
- Ne pas créer de carte kanban pour une action directe
- Ne pas ajouter de dépendance si une commande shell suffit

---

## 2. Event-driven, pas poll-driven

**Principe :** réagir aux événements plutôt que scruter périodiquement.

**Conséquences :**
- Webhooks > polling API
- Watchers RSS > cron de vérification
- IMAP IDLE > scrutation boîte mail
- Les cron jobs sont l'exception, pas la règle

---

## 3. Traçabilité sans bureaucratie

**Principe :** chaque action importante laisse une trace structurée, mais on n'ajoute pas de friction aux actions simples.

**Conséquences :**
- Les workers kanban produisent un handoff structuré (summary + metadata)
- Les actions directes (1-2 outils) ne génèrent pas de carte
- Les décisions importantes sont enregistrées dans le Memory Kernel
- Les logs sont dans Discord #jenkins, pas dans des fichiers éparpillés

---

## 4. Local d'abord

**Principe :** le système doit pouvoir fonctionner sans accès internet.

**Conséquences :**
- Modèle local (Ollama/Qwen) disponible en fallback
- Obsidian vault synced localement via Syncthing
- Pas de dépendance critique à un service cloud
- Le Book4 Windows est un nœud autonome, pas un terminal

---

## 5. Zéro coût

**Principe :** privilégier les solutions gratuites, auto-hébergées, ou à coût marginal.

**Conséquences :**
- Auto-hébergement maximum (VPS, homelab Proxmox)
- Pas de SaaS superflus
- FAL.ai pour la génération d'images (pay-per-use, pas d'abonnement)
- SearX local plutôt qu'API Google/Bing payante

---

## 6. Mémoire structurée, pas RAG aveugle

**Principe :** la mémoire est un graphe de faits, pas un sac de documents.

**Conséquences :**
- Memory Kernel : faits structurés (sujet → prédicat → objet) avec catégorie et importance
- Pas de RAG sur l'ensemble du vault à chaque requête
- Les faits durables sont injectés automatiquement
- Les connaissances temporaires restent dans les notes Obsidian

---

## 7. Un seul point d'entrée

**Principe :** Hermes Agent est l'unique orchestrateur. Pas de scripts éparpillés, pas de cron système parallèle, pas de bots concurrents.

**Conséquences :**
- Tout passe par Hermes (cron, watchers, webhooks, réponses)
- Un seul profil Hermes (pas de dispatch multi-profil)
- Les tâches complexes sont décomposées en cartes kanban, pas en scripts shell

---

## Décisions d'architecture notables

| Décision | Justification |
|----------|---------------|
| Profil Hermes unique | Pas de besoin de dispatch multi-profil dans ce contexte |
| Syncthing plutôt que git pour le vault | Sync bidirectionnelle temps réel, pas de conflits de merge |
| Memory Kernel plutôt que RAG | Faits structurés plus efficaces que similarité vectorielle pour des préférences et contraintes |
| Kanban plutôt que files d'attente | Traçabilité, dépendances, handoff structuré |
| Telegram comme canal principal | API simple, notifications push, bot permanent |
