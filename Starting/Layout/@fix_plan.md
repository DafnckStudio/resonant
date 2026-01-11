# Resonant.ai - Layout Documentation Plan

> Documentation complète des layouts de l'application Resonant.ai
> Style inspiré de Relevance AI + Poppy AI

---

## Tâches Ralph

### Phase 1: Structure Globale

- [ ] Créer 00-overview.md : Vue d'ensemble de l'architecture UI (app shell, navigation patterns, breakpoints, design system tokens)
- [ ] Créer 01-app-shell.md : Layout principal avec sidebar, header, content area + diagrammes ASCII art
- [ ] Créer 02-navigation-system.md : Système de navigation (sidebar, breadcrumbs, tabs, command palette)
- [ ] Créer 03-responsive-strategy.md : Stratégie responsive (mobile, tablet, desktop, wide) avec breakpoints

### Phase 2: Dashboard & Home

- [ ] Créer 10-dashboard-home.md : Page d'accueil dashboard avec stats cards, widgets, activity feed
- [ ] Créer 11-dashboard-widgets.md : Catalogue des widgets dashboard (recent workflows, quick actions, usage stats)
- [ ] Créer 12-dashboard-analytics.md : Layout des analytics dashboard (charts, graphs, metrics)

### Phase 3: Workflows Management

- [ ] Créer 20-workflows-list.md : Liste des workflows avec grille/liste toggle, filtres, search
- [ ] Créer 21-workflow-card.md : Design du workflow card (thumbnail, status, actions, metadata)
- [ ] Créer 22-workflow-details.md : Page détails workflow (overview, executions, versions, settings)

### Phase 4: Workflow Editor (ReactFlow)

- [ ] Créer 30-editor-layout.md : Layout complet de l'éditeur ReactFlow (canvas, panels, toolbar)
- [ ] Créer 31-editor-toolbar.md : Barre d'outils éditeur (actions, zoom, mode switches)
- [ ] Créer 32-block-palette.md : Palette des blocs avec catégories, search, drag & drop
- [ ] Créer 33-properties-panel.md : Panel de propriétés des nœuds (config, inputs, outputs)
- [ ] Créer 34-execution-panel.md : Panel d'exécution (run, logs, streaming output)
- [ ] Créer 35-collaboration-overlay.md : Overlay collaboration (cursors, presence, comments)
- [ ] Créer 36-version-panel.md : Panel historique versions (timeline, diff, restore)

### Phase 5: Block System

- [ ] Créer 40-block-types.md : Catalogue visuel de tous les types de blocs (input, AI, transform, output, logic)
- [ ] Créer 41-block-anatomy.md : Anatomie d'un bloc (handles, header, body, status indicators)
- [ ] Créer 42-block-connections.md : Design des connexions (edges, data flow visualization)

### Phase 6: Templates & Marketplace

- [ ] Créer 50-templates-gallery.md : Galerie des templates (grid, filters, categories, featured)
- [ ] Créer 51-template-detail.md : Page détail template (preview, description, reviews, use button)
- [ ] Créer 52-marketplace-browse.md : Navigation marketplace (search, categories, trending, seller pages)
- [ ] Créer 53-marketplace-seller.md : Dashboard vendeur (listings, analytics, payouts)

### Phase 7: Settings Pages

- [ ] Créer 60-settings-layout.md : Layout settings avec sidebar tabs
- [ ] Créer 61-profile-settings.md : Page profil utilisateur (avatar, info, preferences)
- [ ] Créer 62-workspace-settings.md : Settings workspace (general, members, permissions)
- [ ] Créer 63-billing-settings.md : Page billing (plan, usage, invoices, payment methods)
- [ ] Créer 64-ai-providers.md : Configuration AI providers (API keys, models, budgets)
- [ ] Créer 65-integrations.md : Page intégrations (webhooks, API keys, connected apps)

### Phase 8: Execution & Monitoring

- [ ] Créer 70-executions-list.md : Liste des exécutions (filtres, status, logs access)
- [ ] Créer 71-execution-detail.md : Détail exécution (timeline, block outputs, costs)
- [ ] Créer 72-execution-logs.md : Visualisation logs (streaming, filters, export)

### Phase 9: Collaboration Features

- [ ] Créer 80-team-management.md : Gestion d'équipe (members, roles, invitations)
- [ ] Créer 81-sharing-dialog.md : Dialog de partage (permissions, public links, embed)
- [ ] Créer 82-comments-system.md : Système de commentaires (threads, mentions, resolve)
- [ ] Créer 83-notifications-panel.md : Panel notifications (feed, settings, mark read)

### Phase 10: Modals & Dialogs

- [ ] Créer 90-modal-system.md : Système de modales (sizes, animations, stacking)
- [ ] Créer 91-create-workflow.md : Modal création workflow (templates selection, blank start)
- [ ] Créer 92-command-palette.md : Command palette (search, actions, shortcuts)
- [ ] Créer 93-onboarding-flow.md : Flow onboarding (steps, progress, skip options)

### Phase 11: Components Library

- [ ] Créer A0-design-tokens.md : Design tokens (colors, spacing, typography, shadows)
- [ ] Créer A1-buttons-inputs.md : Buttons et inputs (variants, states, sizes)
- [ ] Créer A2-cards-containers.md : Cards et containers (variants, sections)
- [ ] Créer A3-tables-lists.md : Tables et listes (sorting, filtering, pagination)
- [ ] Créer A4-feedback-states.md : États feedback (loading, empty, error, success)

### Phase 12: Diagrams & Flows

- [ ] Créer B0-user-flows.md : Diagrammes des parcours utilisateurs principaux
- [ ] Créer B1-information-architecture.md : Architecture de l'information (sitemap, hierarchy)
- [ ] Créer B2-component-hierarchy.md : Hiérarchie des composants React

## Validation

- [ ] Build passes (documentation Markdown valide)
- [ ] Tous les fichiers créés avec diagrammes ASCII art
- [ ] Liens croisés entre les documents
- [ ] Table des matières globale dans 00-overview.md
