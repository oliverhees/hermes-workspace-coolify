# Hermes Workspace (outsourc-e) — Coolify Deployment

Gehärtetes Coolify-Compose-Template für **`outsourc-e/hermes-workspace`** — die
Orchestrierungs-UI mit **Conductor** (Mission-Dispatch + Dekomposition) und
**Swarm Mode** (persistente rollenbasierte Worker). Zum **Testen**, ob die
eingebaute Multi-Agent-Orchestrierung Olivers „Mira dirigiert das Team"-Vision
besser abbildet als das aktuelle nesquena-Setup.

## Was deployt wird

| Container | Image | Port | Zweck |
|-----------|-------|------|-------|
| `hermes-agent` | `nousresearch/hermes-agent:latest` | 8642 (Gateway) + 9119 (Dashboard) | Backend |
| `hermes-workspace` | `ghcr.io/outsourc-e/hermes-workspace:latest` | 3000 | Orchestrierungs-UI |

Geteiltes Volume `hermes-agent-data` (config/sessions/skills/profiles), kein Postgres.

## Installation (Coolify)

1. **+ Add Resource → Docker Compose Empty** → Inhalt von `docker-compose.yaml` einfügen
2. **Environment Variables:** mindestens einen LLM-Key setzen (z.B. `ANTHROPIC_API_KEY`).
   `SERVICE_PASSWORD_64_HERMESAPI` + `SERVICE_PASSWORD_WORKSPACE` generiert Coolify automatisch.
3. Domain vergeben (oder Coolify-Auto-Domain) → **Deploy**
4. Öffnen → mit dem Workspace-Passwort (`SERVICE_PASSWORD_WORKSPACE` im Env-Tab) einloggen

## Was testen

- **Conductor** (Mission-Dispatch): „erstelle Instagram-Karussell" → zerlegt + verteilt an Worker
- **Swarm Mode**: rollenbasierte Worker aus `swarm.yaml` (Roster)
- **Operations-Dashboard**: Multi-Agent-Übersicht

## ⚠️ Wichtige Hinweise

- **Eigenständige Instanz:** Eigenes `hermes-agent-data`-Volume — **nicht** dein bestehendes
  nesquena-Backend. Profile (Nina) sind hier zunächst **nicht** vorhanden. Zum Orchestrierungs-Test
  legst du hier ein paar Worker-Profile an (oder Nina neu).
- **Keine Extensions:** outsourc-e hat **kein** Extension-System wie nesquena — deine
  `mcp-manager`/`agent-importer` laufen hier nicht. MCP-Server verwaltest du über die eingebaute
  MCP-Presets-Oberfläche bzw. config.yaml.
- **Profil-Presets** (Sage/Trader/Builder…) sind outsourc-e-Demo-Personas, kein neutrales Framework.
- **`:latest`** für den Test — für Produktion auf feste Version pinnen.

## Entscheidungsgrundlage

Wenn Conductor + Swarm hier Olivers Vision (Mira → Nina/Content/Social, Worker untereinander)
spürbar besser/komfortabler abbilden als die manuelle Hermes-Kanban-Orchestrierung in nesquena —
und der Verlust der Extensions verschmerzbar/neu baubar ist — lohnt der Umstieg. Beide UIs teilen
dasselbe hermes-agent-Backend; ein späterer Wechsel verliert keine Profile.

---
*Erstellt mit Alice für Lokyy OS / KIMIBOCA. Vgl. ADR `lokyy-os-agent-stack` in Loki Brain.*
