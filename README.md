<p align="center">
  <img src="https://user-images.githubusercontent.com/10284570/173569848-c624317f-42b1-45a6-ab09-f0ea3c247648.png" alt="n8n Logo" />
</p>

<div align="center">

<img src="https://raw.githubusercontent.com/n8n-io/n8n/master/assets/n8n-logo.png" width="180" alt="n8n" />

# FlowMind — AI Delivery Intelligence

<div align="center">

[![n8n](https://img.shields.io/badge/n8n-Orchestration-FF6D5A?logo=n8n&logoColor=white)](https://n8n.io)
[![Google Gemini](https://img.shields.io/badge/LLM-Google%20Gemini-4285F4?logo=google&logoColor=white)](https://ai.google.dev)
[![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-4169E1?logo=postgresql&logoColor=white)](https://postgresql.org)
[![Jira](https://img.shields.io/badge/Jira-Integration-0052CC?logo=jira&logoColor=white)](https://atlassian.com/jira)
[![GitHub](https://img.shields.io/badge/GitHub-Integration-181717?logo=github&logoColor=white)](https://github.com)
[![GitLab](https://img.shields.io/badge/GitLab-Integration-FC6D26?logo=gitlab&logoColor=white)](https://gitlab.com)
[![Slack](https://img.shields.io/badge/Slack-Notifications-4A154B?logo=slack&logoColor=white)](https://slack.com)
[![Multi-tenant](https://img.shields.io/badge/Multi--tenant-Ready-A855F7)]()
[![Nodes](https://img.shields.io/badge/Nodes-117-22C55E)]()
[![Status](https://img.shields.io/badge/Status-Production%20Ready-22C55E)]()

**Système d'orchestration décisionnelle IA pour la prévention d'échec de sprint logiciel**

*Détecte · Analyse · Décide · Agit — sans attendre*

</div>

---

## Image Workflow

<p align="center">
  <img src="https://github.com/Tchatchoua14/flowmind-ai-delivery-intelligence/blob/1a49db1013f603641ae706b12469ecfbaf8f144e/FlowMind%20%20Ai%20Delivery%20Intelligence.png" alt="n8n Logo" />
</p>

---

## Table des matières

- [Vue d'ensemble](#vue-densemble)
- [Architecture cognitive](#architecture-cognitive)
- [Sources de données](#sources-de-données)
- [Pipeline décisionnel](#pipeline-décisionnel)
  - [Étape 1 — Collecte et normalisation](#étape-1--collecte-et-normalisation)
  - [Étape 2 — Enrichissement et corrélation](#étape-2--enrichissement-et-corrélation)
  - [Étape 3 — Scoring de risque](#étape-3--scoring-de-risque)
  - [Étape 4 — Agents IA spécialisés](#étape-4--agents-ia-spécialisés)
  - [Étape 5 — Synthèse décisionnelle](#étape-5--synthèse-décisionnelle)
  - [Étape 6 — Actions autonomes](#étape-6--actions-autonomes)
  - [Étape 7 — Notifications intelligentes](#étape-7--notifications-intelligentes)
- [Modèle de scoring](#modèle-de-scoring)
- [Modèle de données](#modèle-de-données)
- [Gestion des erreurs](#gestion-des-erreurs)
- [Configuration multi-tenant](#configuration-multi-tenant)
- [Installation](#installation)
- [Roadmap](#roadmap)

---

## Vue d'ensemble

**FlowMind** est un système d'orchestration décisionnelle construit sur **n8n**, propulsé par **Google Gemini**, qui surveille en continu l'état de vos sprints logiciels sur 5 sources de données et déclenche des actions correctives autonomes avant que les retards ne deviennent irréversibles.

Le système couvre l'intégralité du cycle décisionnel :

- **Collecte multi-source** — Jira, GitHub, GitLab, Linear, Azure DevOps normalisés dans un modèle unifié
- **Enrichissement** — corrélation automatique tâches ↔ PR/MR, détection de dépendances cachées
- **Scoring** — calcul d'un `sprint_health_score` (0–100) et d'un `risk_score` par élément
- **Analyse IA** — 4 agents Gemini spécialisés raisonnent en parallèle sur le contexte enrichi
- **Synthèse** — un agent orchestrateur produit la décision finale avec owner, deadline et impact
- **Action** — le système agit directement : commente une PR, réassigne un ticket Jira, crée un événement Calendar, ping un reviewer sur Slack
- **Traçabilité** — toutes les décisions, recommandations et erreurs sont persistées dans PostgreSQL

> FlowMind ne recommande pas. Il décide et agit. La supervision humaine reste la finalité — mais le chemin jusqu'à l'humain est automatisé.

---

## Diagramme 1 — Vue d'ensemble de l'architecture (les 7 phases)

<p align="center">
  <img src="https://github.com/Tchatchoua14/flowmind-ai-delivery-intelligence/blob/3ab9abf6efafea49edec254e17999e5779d23135/flowmind_architecture_overview.svg" alt="n8n Logo" />
</p>

## Architecture cognitive

```
┌─────────────────────────────────────────────────────────────────┐
│                    SOURCES DE DONNÉES                           │
│  Jira  ·  GitHub  ·  GitLab  ·  Linear  ·  Azure DevOps        │
└─────────────────────────┬───────────────────────────────────────┘
                          │ Polling + normalisation
                   ┌──────▼──────┐
                   │ MODÈLE      │  Format unifié
                   │ UNIFIÉ      │  task / pull_request / merge_request
                   └──────┬──────┘
                          │
                   ┌──────▼──────┐
                   │ ENRICHISSE- │  Link Tasks ↔ PRs
                   │ MENT        │  Détection blocages, CI, reviews
                   └──────┬──────┘
                          │
                   ┌──────▼──────┐
                   │  RISK       │  Score 0–100 par élément
                   │  ENGINE     │  Sprint Health Score global
                   └──────┬──────┘
                          │
         ┌────────────────┼────────────────┐
         │                │                │
   ┌─────▼──────┐  ┌──────▼──────┐  ┌─────▼──────┐
   │ Guardian   │  │  Friction   │  │ Knowledge  │
   │ of         │  │  Killer     │  │ Gap        │
   │ Deadlines  │  │             │  │ Detector   │
   └─────┬──────┘  └──────┬──────┘  └─────┬──────┘
         └────────────────┼────────────────┘
                          │ Merge - Agent Outputs
                   ┌──────▼──────┐
                   │  DECISION   │  Decision Synthesizer
                   │  SYNTHESIZER│  Gemini — JSON structuré
                   └──────┬──────┘
                          │
         ┌────────────────┼────────────────┐
         │                │                │
   ┌─────▼──────┐  ┌──────▼──────┐  ┌─────▼──────┐
   │ GitHub     │  │  Jira       │  │  Calendar  │
   │ Comment PR │  │  Reassign   │  │  War Room  │
   └────────────┘  └─────────────┘  └────────────┘
         │                │
   ┌─────▼──────┐  ┌──────▼──────┐
   │  Slack     │  │  Gmail      │
   │  Ping      │  │  Alert      │
   └────────────┘  └─────────────┘
```

### Stack technique

| Composant | Rôle | Détail |
|-----------|------|--------|
| **n8n** | Orchestration | 117 nœuds, 104 connexions |
| **Google Gemini** | LLM décisionnel | 4 agents en parallèle |
| **PostgreSQL** | Persistance | 6 tables métier |
| **Jira** | Source tâches | Sprint issues, assignation |
| **GitHub** | Source PR | Pull requests, commentaires |
| **GitLab** | Source MR | Merge requests, CI/CD |
| **Linear** | Source tâches | Issues, cycles |
| **Azure DevOps** | Source work items | WIQL, détails |
| **Slack** | Notifications | 10 nœuds dédiés |
| **Gmail** | Alertes email | 5 nœuds dédiés |
| **Google Calendar** | War room | Création automatique d'événements |

---

## Sources de données

FlowMind collecte et normalise en parallèle 5 sources hétérogènes dans un **modèle unifié** :

```javascript
{
  source: "jira" | "github" | "gitlab" | "linear" | "azure_devops",
  type: "task" | "pull_request" | "merge_request",
  external_id: "PROJ-123" | "GH-PR-42" | "GL-MR-7",
  title: "...",
  status: "...",
  assignee: "...",
  priority: "Critique" | "Haute" | "Moyenne" | "Basse",
  sprint: "Sprint 14",
  blocked: true | false,
  blocker_type: "Validation" | "Technique" | "Fonctionnel" | "Dépendance" | "Disponibilité",
  review_status: "Review demandée" | "Aucun reviewer" | "Draft",
  ci_status: "failed" | "passed" | "",
  age_hours: 32,
  source_url: "https://...",
  normalized_at: "2025-05-13T..."
}
```

### Logique de détection par source

| Source | Blocage détecté | CI/CD | Review |
|--------|----------------|-------|--------|
| Jira | Statut, labels, summary | — | — |
| GitHub | `age_hours >= 24` + review manquante | `ci_status = failed` | `requested_reviewers` |
| GitLab | Review absente ou lente | `head_pipeline.status` | `reviewers[]` |
| Linear | Labels, description, statut | — | — |
| Azure DevOps | Statut work item | — | — |

---

## Pipeline décisionnel

### Étape 1 — Collecte et normalisation

Déclenchement par **Schedule Trigger** (intervalle configurable).

Chaque source dispose de son pipeline indépendant :

```
[Source] → Code - Normalize → IF - Valide ? → Merge - Unified Delivery Data
                                     └─ FALSE → Set Error → Postgres Audit Log
```

**Validation stricte** : `external_id !== ""` ET `title !== ""`. Tout élément invalide est journalisé dans `audit_logs` avec type `VALIDATION_ERROR` et n'est jamais injecté dans le pipeline.

**Azure DevOps** suit un flux en deux appels HTTP : WIQL pour récupérer les IDs, puis détail par batch — avec gestion d'erreur dédiée sur chaque appel.

---

### Étape 2 — Enrichissement et corrélation

Le nœud `Code - Link Tasks With PRs` corrèle automatiquement les tâches et les PR/MR via 3 patterns de détection :

```javascript
const patterns = [
  /([A-Z]+-\d+)/,       // JIRA-123
  /(US[-_ ]?\d+)/i,     // US-42
  /(TASK[-_ ]?\d+)/i    // TASK-7
];
```

Chaque tâche se voit enrichie de :

```javascript
linked_pr_count: 2,
linked_prs: [...],
has_blocked_pr: true,
has_waiting_review: true,
has_failed_ci: false,
pr_blocker_summary: "GH-PR-42 - Review en attente | GL-MR-7 - CI/CD échouée"
```

Les PR orphelines (sans tâche liée) sont conservées comme **signaux faibles** dans le pipeline.

---

### Étape 3 — Scoring de risque

#### Score individuel (0–100+)

Le nœud `Code - Detect Delivery Risks` calcule un `risk_score` pour chaque élément :

| Signal détecté | Points |
|----------------|--------|
| Élément bloqué (`blocked = true`) | +30 |
| CI/CD échouée (tâche avec PR liée) | +25 |
| PR/MR liée bloquée | +25 |
| Deadline dépassée | +25 |
| Review en attente | +20 |
| PR ouverte depuis ≥ 24h | +20 |
| Aucun reviewer assigné | +20 |
| CI/CD échouée (directe) | +25 |
| Aucun responsable | +15 |
| Inactivité ≥ 3 jours | +10 |
| Tâche ouverte ≥ 5 jours | +10 |
| Priorité haute (Haute) | +15 |
| Priorité critique | +20 |

**Seuil de filtrage** : seuls les éléments avec `risk_score >= 30` entrent dans le pipeline décisionnel.

| Niveau | Score |
|--------|-------|
| Faible | < 30 |
| Moyen | 30–54 |
| Élevé | 55–79 |
| Critique | ≥ 80 |

#### Sprint Health Score (0–100)

Le nœud `Code - Sprint Health Score` agrège tous les risques :

```javascript
sprintHealthScore = 100
  - (criticalRisks.length × 18)
  - (highRisks.length × 10)
  - (mediumRisks.length × 5)
  - (totalRiskScore >= 300 ? 20 : totalRiskScore >= 200 ? 12 : 6)
```

| Sprint Health | Niveau global |
|---------------|---------------|
| ≥ 80 | Faible |
| 60–79 | Moyen |
| 40–59 | Élevé |
| < 40 | Critique |

---

### Étape 4 — Agents IA spécialisés

Le nœud `Code - Build Anti-Failure Context` prépare un objet de contexte structuré contenant :
- `project_context` : sprint_health_score, global_risk_level, estimated_failure_probability
- `risk_summary` : compteurs par niveau, PR risks, overdue, no-owner
- `top_risks` : top 10 risques classés par score
- `grouped_insights` : risques par assignee, type, source

Ce contexte est envoyé **en parallèle** aux 3 agents Gemini :

---

#### Agent 1 — Guardian of Deadlines

> Prédit le risque d'échec de sprint et recommande des arbitrages opérationnels.

**Entrée** : contexte anti-échec complet

**Sortie JSON structurée** :
```json
{
  "agent": "Guardian of Deadlines",
  "sprint_failure_probability": 72,
  "delivery_forecast": "Livraison compromise vendredi",
  "global_risk_level": "Élevé",
  "main_failure_causes": [...],
  "scope_decisions": [
    {
      "decision": "Sortir US-14 du scope",
      "target": "US-14",
      "owner": "PM",
      "reason": "Non critique, consomme 2 jours de dev",
      "expected_impact": "Libère la capacité pour les US bloquantes",
      "urgency": "Aujourd'hui"
    }
  ],
  "critical_actions": [...],
  "tasks_to_deprioritize": ["US-14", "TASK-22"],
  "tasks_to_protect": ["PROJ-47", "PROJ-51"],
  "confidence_score": 85,
  "critical_decision": true
}
```

---

#### Agent 2 — Friction Killer

> Analyse les temps morts de validation : PR stagnantes, reviews absentes, CI échouées.

**Règles strictes** :
- Ne recommande jamais un merge automatique
- Ne dit jamais "Safe to Merge"
- Suggère uniquement : `"Low Risk Review"` / `"Needs Human Review"` / `"Blocked Review"` / `"CI Failed"`

**Sortie JSON structurée** :
```json
{
  "agent": "Friction Killer",
  "friction_level": "Élevé",
  "blocked_reviews": [...],
  "slow_reviews": [...],
  "prs_without_reviewer": [...],
  "ci_failed_items": [...],
  "recommended_review_actions": [
    {
      "target": "GH-PR-42",
      "action": "Réassigner le review à @dev-senior",
      "owner": "Lead Tech",
      "reason": "Aucun reviewer depuis 28h, bloque PROJ-47",
      "suggested_label": "Blocked Review",
      "urgency": "Immédiate"
    }
  ],
  "review_acceleration_summary": "...",
  "confidence_score": 78,
  "critical_decision": false
}
```

---

#### Agent 3 — Knowledge Gap Detector

> Identifie les zones de manque de compétence et les dépendances à une seule personne.

**Règles strictes** :
- N'invente jamais d'ancien cas similaire
- Ne cite pas de documentation inexistante
- Dit clairement ce qu'il faut demander ou créer

**Sortie JSON structurée** :
```json
{
  "agent": "Knowledge Gap Detector",
  "knowledge_gap_level": "Moyen",
  "knowledge_risks": [...],
  "senior_needed_topics": ["Base de données PostgreSQL", "Architecture microservices"],
  "documentation_needed": [...],
  "recommended_knowledge_actions": [
    {
      "target": "PROJ-53",
      "action": "Organiser un pair programming avec @senior-dev",
      "owner": "Tech Lead",
      "reason": "Ticket bloqué depuis 4 jours, assignee junior sans expérience DB",
      "urgency": "Avant demain 14h"
    }
  ],
  "confidence_score": 70,
  "critical_decision": false
}
```

---

### Étape 5 — Synthèse décisionnelle

Les 3 sorties agents sont mergées dans `Merge - Agent Outputs`, puis envoyées au **Decision Synthesizer**.

```
Guardian output (valid) ─┐
Friction output (valid) ──┼─→ Merge - Agent Outputs → AI Agent - Decision Synthesizer
Knowledge output (valid) ─┘
```

**Validation de chaque sortie agent** : chaque agent dispose de son propre nœud `Code - Validate [Agent] Output` qui parse le JSON, vérifie les champs requis, et route vers `Set - [Agent] AI Error → Postgres - Insert AI Audit Log` en cas d'échec — garantissant qu'aucun JSON invalide n'atteint le Synthesizer.

#### Agent 4 — Decision Synthesizer

> Transforme les 3 analyses en une décision unique, actionnelle, distribuable.

**Sortie JSON structurée** :
```json
{
  "final_summary": "Sprint en danger critique. 3 PR bloquées, 2 tickets sans owner.",
  "global_risk_level": "Critique",
  "sprint_failure_probability": 78,
  "critical_decision": true,
  "decisions_today": [
    {
      "decision": "Réassigner GH-PR-42 immédiatement",
      "owner": "Lead Tech",
      "deadline": "Aujourd'hui 16h",
      "reason": "Bloque PROJ-47, livraison vendredi compromise",
      "expected_impact": "Débloque 2 US critiques"
    }
  ],
  "review_actions": [...],
  "scope_actions": [...],
  "slack_message": "🚨 FlowMind — Sprint critique...",
  "confidence_score": 88
}
```

**Routage par score** (`Switch - Decision Score`) :

| Score | Branche | Actions déclenchées |
|-------|---------|---------------------|
| ≥ 80 | Critique | War Room Calendar + Slack critique + Gmail + Jira update + PR comment |
| ≥ 50 | Suivi / Rappel | Ping reviewer Slack + Gmail rappel |

---

### Étape 6 — Actions autonomes

FlowMind ne s'arrête pas à la notification. Dès la décision validée, il **agit directement** :

#### Commentaire automatique sur PR GitHub

```
Code - Detect Delivery Risks
  └─→ GitHub - Comment PR  (createComment)
        Body : "⚠️ CI échouée — vérifier le build avant de merger"
        Journalisation : Postgres - Github Insert Action Log
```

Après la synthèse décisionnelle :

```
AI Agent - Decision Synthesizer
  ├─→ GitHub - Comment PR1  (filtre sur CI failed / blocked PR)
  ├─→ Jira - Update Issue / Assign  (réassignation automatique)
  ├─→ Google Calendar - Create Event  (War Room 15 min)
  └─→ Set - Critical Decision Notification1
        ├─→ Gmail - Ping Reviewer
        └─→ Slack - Ping Reviewer
              "@reviewer La PR GH-PR-42 attend depuis 28h."
```

**Chaque action est journalisée** dans `action_logs` avec le contexte complet.

---

### Étape 7 — Notifications intelligentes

Le système distingue deux niveaux de notification :

#### Décision critique (`critical_decision = true`)

```
IF - Critical Decision ?
  └─ TRUE → Set - Critical Decision Notification
               ├─→ Gmail - Send Critical Decision
               └─→ Slack - Send Critical Decision
```

Contenu : `global_risk_level`, `sprint_failure_probability`, `final_summary`, `decisions_today`, `review_actions`, `scope_actions`.

#### Rapport quotidien (`critical_decision = false`)

```
IF - Critical Decision ?
  └─ FALSE → Set - Daily Delivery Report Notification
                ├─→ Slack - Send Daily Delivery Report
                └─→ Gmail - Send Daily Delivery Report
```

**Retry automatique sur les actions GitHub** : 4 nœuds `Wait - Github Retry Delay` (2 min) permettent de rejouer les actions en cas d'erreur API GitHub temporaire.

---

## Modèle de scoring

### Algorithme de `estimated_failure_probability`

```javascript
// Base selon sprint health score
if (sprintHealthScore >= 80) base = 15;
else if (sprintHealthScore >= 60) base = 35;
else if (sprintHealthScore >= 40) base = 60;
else base = 80;

// Ajustements par type de risque
probability = base
  + (criticalRisks.length × 5)
  + (highRisks.length × 3)
  + (prRisks.length × 2);

// Plafond à 95%
probability = Math.min(probability, 95);
```

---

## Modèle de données

### `delivery_risks` — Risques détectés

```sql
CREATE TABLE delivery_risks (
  id                  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  alert_key           TEXT UNIQUE NOT NULL,
  source              TEXT,
  type                TEXT,
  external_id         TEXT,
  task_id             TEXT,
  title               TEXT,
  status              TEXT,
  assignee            TEXT,
  priority            TEXT,
  sprint              TEXT,
  risk_score          INTEGER,
  risk_level          TEXT,
  risk_type           TEXT,
  reasons_detected    TEXT,
  decision_required   TEXT,
  blocker_type        TEXT,
  pr_blocker_summary  TEXT,
  linked_pr_count     INTEGER,
  linked_prs          JSONB,
  source_url          TEXT,
  sprint_health_score INTEGER,
  global_risk_level   TEXT,
  total_risks         INTEGER,
  critical_risks      INTEGER,
  high_risks          INTEGER,
  medium_risks        INTEGER,
  executive_summary   TEXT,
  status_alert        TEXT DEFAULT 'Ouverte',
  detected_at         TIMESTAMPTZ,
  updated_at          TIMESTAMPTZ DEFAULT NOW(),
  raw_context         JSONB
);

-- ON CONFLICT (alert_key) DO UPDATE — pas de doublon, seulement une mise à jour
```

### `agent_recommendations` — Décisions IA

```sql
CREATE TABLE agent_recommendations (
  id                       UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  agent_name               TEXT,
  recommendation_type      TEXT,
  global_risk_level        TEXT,
  sprint_failure_probability INTEGER,
  summary                  TEXT,
  recommendation           JSONB,
  confidence_score         INTEGER,
  critical_decision        BOOLEAN,
  created_at               TIMESTAMPTZ DEFAULT NOW()
);
```

### `delivery_decisions` — Décisions actionnées

```sql
CREATE TABLE delivery_decisions (
  id                         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  decision_title             TEXT,
  decision_owner             TEXT,
  decision_deadline          TEXT,
  decision_reason            TEXT,
  expected_impact            TEXT,
  global_risk_level          TEXT,
  sprint_failure_probability INTEGER,
  critical_decision          BOOLEAN,
  slack_message              TEXT,
  raw_decision               JSONB,
  status                     TEXT DEFAULT 'pending',
  created_at                 TIMESTAMPTZ DEFAULT NOW(),
  updated_at                 TIMESTAMPTZ DEFAULT NOW()
);
```

### `audit_logs` — Traçabilité complète

```sql
CREATE TABLE audit_logs (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  job_id      TEXT,
  level       TEXT,             -- info | warn | error
  source      TEXT,
  error_type  TEXT,             -- VALIDATION_ERROR | AI_OUTPUT_VALIDATION
  message     TEXT,
  details     JSONB,
  created_at  TIMESTAMPTZ DEFAULT NOW()
);
```

### `action_logs` — Journal des actions autonomes

```sql
CREATE TABLE action_logs (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  action_type TEXT,
  target      TEXT,
  result      JSONB,
  created_at  TIMESTAMPTZ DEFAULT NOW()
);
```

---

## Gestion des erreurs

### Matrice complète par couche

| Zone | Erreur possible | Branche n8n | Action |
|------|-----------------|-------------|--------|
| Validation Jira | `external_id` ou `title` absent | `IF - Jira Issue Valide ? → FALSE` | `Set Error → Postgres Audit Log` |
| Validation GitHub PR | Même condition | `IF - GitHub PR Valide ? → FALSE` | `Set Error → Postgres Audit Log` |
| Validation GitLab MR | Même condition | `IF - GitLab MR Valide ? → FALSE` | `Set Error → Postgres Audit Log` |
| Validation Linear | Même condition | `IF - Linear Issue Valide ? → FALSE` | `Set Error → Postgres Audit Log` |
| Validation Azure DevOps | Même condition | `IF - Azure DevOps Work Item Valide ? → FALSE` | `Set Error → Postgres Audit Log` |
| API Azure DevOps | HTTP 4xx/5xx | `Set - HTTP Azure API Error` | `Slack - Notify Ops API Error` |
| Guardian output | JSON non parseable | `IF - Guardian Output Valid ? → FALSE` | `Set - Guardian AI Error → Postgres AI Audit Log` |
| Friction output | JSON invalide | `IF - Friction Output Valid ? → FALSE` | `Set - Friction AI Error → Postgres AI Audit Log` |
| Decision Synthesizer | JSON invalide | `IF - Final Decision Output Valid ? → FALSE` | `Set - Final Decision AI Error → Postgres AI Audit Log` |
| GitHub API | Erreur createComment | `Wait - Github Retry Delay (×4, 2 min)` | Retry automatique 4 passes |
| Actions critiques | Échec général | `Set Error → Audit Log` | Notification Ops via Slack |

### Règles de retry GitHub

```
Action GitHub échouée
  └─→ Wait - Github Retry Delay (2 min)
        └─→ Re-tentative
              └─→ Wait - Github Retry Delay1 (2 min)
                    └─→ Re-tentative (×4 total)
```

---

## Configuration multi-tenant

Le workflow démarre par le nœud `Set - Client Context` qui fournit les credentials dynamiques :

```json
{
  "client_id": "client_001",
  "jira_url": "https://client001.atlassian.net",
  "jira_token": "{{ $json.jira_token }}",
  "github_token": "{{ $json.github_token }}",
  "gitlab_token": "{{ $json.gitlab_token }}",
  "azure_token": "{{ $json.azure_token }}",
  "slack_channel": "#general",
  "email_to": "pm@client001.com",
  "scoring_thresholds": {
    "critical": 80,
    "high": 55,
    "medium": 30
  }
}
```

**Principe multi-tenant** : remplacer ce nœud par une requête Postgres sur une table `workspaces` contenant les credentials chiffrés par client. Le `client_id` devient le `tenant_id` propagé sur tout le pipeline.

**À ne jamais faire** :
```
❌  credentials en dur dans un nœud
❌  email hardcodé (rickystephane@gmail.com → remplacer par $json.email_to)
❌  channel Slack fixe (#general → remplacer par $json.slack_channel)
```

---

## Installation

### Prérequis

- n8n ≥ 1.40
- PostgreSQL ≥ 15
- Node.js ≥ 20
- Credentials : Google Gemini API, Jira, GitHub, GitLab, Linear, Slack, Gmail

### 1. Importer le workflow

Dans n8n → **Settings > Import from file** → `FlowMind_Delivery_Intelligence.json`

### 2. Configurer les credentials

| Credential | Nodes concernés |
|-----------|-----------------|
| `Postgres account` | Tous les nœuds Postgres (23 nœuds) |
| `Google Gemini (PaLM) Api account` | 4 nœuds LLM + Embeddings |
| `Slack account` | 10 nœuds Slack |
| `Jira account` | Jira - Get Sprint Issues, Jira - Update Issue |
| `GitHub account` | GitHub - Get Pull Requests, GitHub - Comment PR |
| `GitLab account` | GitLab - Get Merge Requests |
| `Linear account` | Linear - Get Issues |
| `Gmail account` | 5 nœuds Gmail |
| `Google Calendar account` | 2 nœuds Calendar |

### 3. Initialiser la base de données

```sql
-- Tables requises
CREATE TABLE delivery_risks (...);       -- voir modèle de données
CREATE TABLE agent_recommendations (...);
CREATE TABLE delivery_decisions (...);
CREATE TABLE audit_logs (...);
CREATE TABLE action_logs (...);

-- Index recommandés
CREATE INDEX idx_delivery_risks_level ON delivery_risks(risk_level);
CREATE INDEX idx_delivery_risks_sprint ON delivery_risks(sprint);
CREATE INDEX idx_audit_logs_level ON audit_logs(level);
CREATE INDEX idx_audit_logs_created ON audit_logs(created_at DESC);
```

### 4. Configurer les sources Jira / GitHub

Dans les nœuds de collecte, adapter :

```
Jira - Get Sprint Issues
  JQL : project = "VOTRE-PROJET" AND sprint in openSprints()
        AND statusCategory != Done
        ORDER BY priority DESC, updated DESC

GitHub - Get Pull Requests
  Owner  : votre-organisation
  Repo   : votre-repo
```

### 5. Activer le workflow

Settings > Activate — le Schedule Trigger démarre la collecte selon l'intervalle configuré.

---

## Roadmap

| Priorité | Fonctionnalité | Statut |
|----------|----------------|--------|
| 🔴 P0 | Remplacer polling par webhooks Jira/GitHub temps réel | À faire |
| 🔴 P0 | Multi-tenant via table `workspaces` Postgres | À faire |
| 🔴 P0 | Formatter les messages Slack en Markdown lisible | À faire |
| 🟠 P1 | Vector Memory (pgvector) pour Knowledge Bridge historique | À faire |
| 🟠 P1 | Dashboard Angular — supervision et approbation 1 clic | À faire |
| 🟠 P1 | Circuit breaker sur les 3 agents Gemini parallèles | À faire |
| 🟡 P2 | Corriger pipeline Azure DevOps (workItemsDetails manquant) | À faire |
| 🟡 P2 | Auth JWT + RBAC (PM / Dev / CTO) | À faire |
| 🟢 P3 | Neo4j — dépendances humaines et chemins critiques | À faire |
| 🟢 P3 | Redis Streams — event bus temps réel | À faire |

---

## Chiffres clés

| Métrique | Valeur |
|----------|--------|
| Nœuds n8n | 117 |
| Connexions | 104 |
| Sources de données | 5 |
| Agents IA Gemini | 4 |
| Nœuds Postgres | 23 |
| Tables métier | 6 |
| Nœuds Slack | 10 |
| Nœuds Gmail | 5 |
| Nœuds d'action autonome | 6 (PR comment, Jira assign, Calendar war room, Slack ping, Gmail ping) |
| Nœuds de validation erreur | 14 (IF + Set Error + Audit Log par source) |
| Score de risque max observable | 225 (tous signaux cumulés) |

---

<div align="center">

Construit avec [n8n](https://n8n.io) · [Google Gemini](https://ai.google.dev) · [PostgreSQL](https://postgresql.org) · [Jira](https://atlassian.com) · [GitHub](https://github.com) · [Slack](https://slack.com)

**FlowMind — parce que les sprints échouent en silence, pas en fanfare.**

</div>
