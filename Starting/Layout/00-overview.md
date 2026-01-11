# Resonant.ai - Layout Overview

> Vue d'ensemble de l'architecture UI complète de Resonant.ai

---

## Table des Matières

### Structure Globale
- [00-overview.md](./00-overview.md) - Ce document
- [01-app-shell.md](./01-app-shell.md) - Layout principal
- [02-navigation-system.md](./02-navigation-system.md) - Système de navigation
- [03-responsive-strategy.md](./03-responsive-strategy.md) - Stratégie responsive

### Dashboard & Home
- [10-dashboard-home.md](./10-dashboard-home.md) - Page d'accueil
- [11-dashboard-widgets.md](./11-dashboard-widgets.md) - Widgets dashboard
- [12-dashboard-analytics.md](./12-dashboard-analytics.md) - Analytics dashboard

### Workflows Management
- [20-workflows-list.md](./20-workflows-list.md) - Liste des workflows
- [21-workflow-card.md](./21-workflow-card.md) - Workflow card design
- [22-workflow-details.md](./22-workflow-details.md) - Page détails

### Workflow Editor (ReactFlow)
- [30-editor-layout.md](./30-editor-layout.md) - Layout éditeur complet
- [31-editor-toolbar.md](./31-editor-toolbar.md) - Barre d'outils
- [32-block-palette.md](./32-block-palette.md) - Palette des blocs
- [33-properties-panel.md](./33-properties-panel.md) - Panel propriétés
- [34-execution-panel.md](./34-execution-panel.md) - Panel exécution
- [35-collaboration-overlay.md](./35-collaboration-overlay.md) - Overlay collab
- [36-version-panel.md](./36-version-panel.md) - Panel versions

### Block System
- [40-block-types.md](./40-block-types.md) - Catalogue des blocs
- [41-block-anatomy.md](./41-block-anatomy.md) - Anatomie d'un bloc
- [42-block-connections.md](./42-block-connections.md) - Connexions

### Templates & Marketplace
- [50-templates-gallery.md](./50-templates-gallery.md) - Galerie templates
- [51-template-detail.md](./51-template-detail.md) - Page détail
- [52-marketplace-browse.md](./52-marketplace-browse.md) - Navigation marketplace
- [53-marketplace-seller.md](./53-marketplace-seller.md) - Dashboard vendeur

### Settings Pages
- [60-settings-layout.md](./60-settings-layout.md) - Layout settings
- [61-profile-settings.md](./61-profile-settings.md) - Profil utilisateur
- [62-workspace-settings.md](./62-workspace-settings.md) - Settings workspace
- [63-billing-settings.md](./63-billing-settings.md) - Billing
- [64-ai-providers.md](./64-ai-providers.md) - AI providers
- [65-integrations.md](./65-integrations.md) - Intégrations

### Execution & Monitoring
- [70-executions-list.md](./70-executions-list.md) - Liste exécutions
- [71-execution-detail.md](./71-execution-detail.md) - Détail exécution
- [72-execution-logs.md](./72-execution-logs.md) - Logs

### Collaboration Features
- [80-team-management.md](./80-team-management.md) - Gestion équipe
- [81-sharing-dialog.md](./81-sharing-dialog.md) - Dialog partage
- [82-comments-system.md](./82-comments-system.md) - Commentaires
- [83-notifications-panel.md](./83-notifications-panel.md) - Notifications

### Modals & Dialogs
- [90-modal-system.md](./90-modal-system.md) - Système modales
- [91-create-workflow.md](./91-create-workflow.md) - Création workflow
- [92-command-palette.md](./92-command-palette.md) - Command palette
- [93-onboarding-flow.md](./93-onboarding-flow.md) - Onboarding

### Components Library
- [A0-design-tokens.md](./A0-design-tokens.md) - Design tokens
- [A1-buttons-inputs.md](./A1-buttons-inputs.md) - Buttons & inputs
- [A2-cards-containers.md](./A2-cards-containers.md) - Cards & containers
- [A3-tables-lists.md](./A3-tables-lists.md) - Tables & listes
- [A4-feedback-states.md](./A4-feedback-states.md) - États feedback

### Diagrams & Flows
- [B0-user-flows.md](./B0-user-flows.md) - Parcours utilisateurs
- [B1-information-architecture.md](./B1-information-architecture.md) - Architecture info
- [B2-component-hierarchy.md](./B2-component-hierarchy.md) - Hiérarchie composants

---

## Architecture UI Globale

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           RESONANT.AI APPLICATION                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         APP SHELL                                    │   │
│  │  ┌──────────┬───────────────────────────────────────────────────┐   │   │
│  │  │          │                    HEADER BAR                      │   │   │
│  │  │          ├───────────────────────────────────────────────────┤   │   │
│  │  │          │                                                    │   │   │
│  │  │  SIDEBAR │              MAIN CONTENT AREA                    │   │   │
│  │  │          │                                                    │   │   │
│  │  │  - Nav   │   ┌─────────────────────────────────────────┐     │   │   │
│  │  │  - Work  │   │                                         │     │   │   │
│  │  │  - User  │   │          PAGE CONTENT                   │     │   │   │
│  │  │          │   │                                         │     │   │   │
│  │  │          │   │   (Dashboard / Editor / Settings...)    │     │   │   │
│  │  │          │   │                                         │     │   │   │
│  │  │          │   └─────────────────────────────────────────┘     │   │   │
│  │  │          │                                                    │   │   │
│  │  └──────────┴───────────────────────────────────────────────────┘   │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      OVERLAY LAYER                                   │   │
│  │  - Modals          - Command Palette      - Notifications           │   │
│  │  - Dialogs         - Toasts               - Collaboration Cursors   │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Design System Tokens

### Breakpoints

| Name | Min Width | Target Devices |
|------|-----------|----------------|
| `xs` | 0px | Mobile portrait |
| `sm` | 640px | Mobile landscape |
| `md` | 768px | Tablet portrait |
| `lg` | 1024px | Tablet landscape / Small desktop |
| `xl` | 1280px | Desktop |
| `2xl` | 1536px | Large desktop |

### Spacing Scale

```
4px   →  space-1   (0.25rem)
8px   →  space-2   (0.5rem)
12px  →  space-3   (0.75rem)
16px  →  space-4   (1rem)
20px  →  space-5   (1.25rem)
24px  →  space-6   (1.5rem)
32px  →  space-8   (2rem)
40px  →  space-10  (2.5rem)
48px  →  space-12  (3rem)
64px  →  space-16  (4rem)
```

### Color Palette

```
┌─────────────────────────────────────────────────────────────────┐
│ BRAND                                                            │
├─────────────────────────────────────────────────────────────────┤
│ Primary:      #6366F1 (Indigo-500)    - Actions, links          │
│ Primary Dark: #4F46E5 (Indigo-600)    - Hover states            │
│ Accent:       #8B5CF6 (Violet-500)    - AI features             │
├─────────────────────────────────────────────────────────────────┤
│ SEMANTIC                                                         │
├─────────────────────────────────────────────────────────────────┤
│ Success:      #22C55E (Green-500)     - Success states          │
│ Warning:      #F59E0B (Amber-500)     - Warnings                │
│ Error:        #EF4444 (Red-500)       - Errors                  │
│ Info:         #3B82F6 (Blue-500)      - Information             │
├─────────────────────────────────────────────────────────────────┤
│ NEUTRAL (Light Mode)                                             │
├─────────────────────────────────────────────────────────────────┤
│ Background:   #FFFFFF                 - Main background          │
│ Surface:      #F9FAFB (Gray-50)       - Cards, panels           │
│ Border:       #E5E7EB (Gray-200)      - Borders                 │
│ Text:         #111827 (Gray-900)      - Primary text            │
│ Text Muted:   #6B7280 (Gray-500)      - Secondary text          │
├─────────────────────────────────────────────────────────────────┤
│ NEUTRAL (Dark Mode)                                              │
├─────────────────────────────────────────────────────────────────┤
│ Background:   #0F172A (Slate-900)     - Main background          │
│ Surface:      #1E293B (Slate-800)     - Cards, panels           │
│ Border:       #334155 (Slate-700)     - Borders                 │
│ Text:         #F8FAFC (Slate-50)      - Primary text            │
│ Text Muted:   #94A3B8 (Slate-400)     - Secondary text          │
└─────────────────────────────────────────────────────────────────┘
```

### Typography

```
┌─────────────────────────────────────────────────────────────────┐
│ FONT FAMILIES                                                    │
├─────────────────────────────────────────────────────────────────┤
│ Sans:  Inter, system-ui, sans-serif     - UI text               │
│ Mono:  Space Mono, monospace            - Code, IDs             │
├─────────────────────────────────────────────────────────────────┤
│ TYPE SCALE                                                       │
├─────────────────────────────────────────────────────────────────┤
│ text-xs:    12px / 16px    - Captions, badges                   │
│ text-sm:    14px / 20px    - Secondary text, labels             │
│ text-base:  16px / 24px    - Body text                          │
│ text-lg:    18px / 28px    - Emphasis                           │
│ text-xl:    20px / 28px    - Section titles                     │
│ text-2xl:   24px / 32px    - Page subtitles                     │
│ text-3xl:   30px / 36px    - Page titles                        │
│ text-4xl:   36px / 40px    - Hero titles                        │
├─────────────────────────────────────────────────────────────────┤
│ FONT WEIGHTS                                                     │
├─────────────────────────────────────────────────────────────────┤
│ Normal:     400            - Body text                          │
│ Medium:     500            - Labels, emphasis                   │
│ Semibold:   600            - Headings, buttons                  │
│ Bold:       700            - Strong emphasis                    │
└─────────────────────────────────────────────────────────────────┘
```

### Shadows

```
shadow-sm:    0 1px 2px rgba(0,0,0,0.05)
shadow:       0 1px 3px rgba(0,0,0,0.1), 0 1px 2px rgba(0,0,0,0.06)
shadow-md:    0 4px 6px rgba(0,0,0,0.1), 0 2px 4px rgba(0,0,0,0.06)
shadow-lg:    0 10px 15px rgba(0,0,0,0.1), 0 4px 6px rgba(0,0,0,0.05)
shadow-xl:    0 20px 25px rgba(0,0,0,0.1), 0 10px 10px rgba(0,0,0,0.04)
```

### Border Radius

```
rounded-sm:   2px
rounded:      4px
rounded-md:   6px
rounded-lg:   8px
rounded-xl:   12px
rounded-2xl:  16px
rounded-full: 9999px
```

---

## Layout Dimensions

### Sidebar

```
┌──────────────────┐
│                  │
│  Collapsed: 64px │
│  Expanded: 256px │
│                  │
│  Transition:     │
│  200ms ease      │
│                  │
└──────────────────┘
```

### Header

```
┌─────────────────────────────────────────────────────────────────┐
│                         Height: 56px                             │
│  Logo area: 256px (matches expanded sidebar)                     │
│  Search: flex-1 (min 200px, max 600px)                          │
│  Actions: auto                                                   │
└─────────────────────────────────────────────────────────────────┘
```

### Content Area

```
┌─────────────────────────────────────────────────────────────────┐
│  Padding: 24px (desktop) / 16px (mobile)                         │
│  Max-width: 1440px (centered)                                    │
│  Min-height: calc(100vh - 56px)                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Page Types

### 1. Dashboard Pages
Standard layout with sidebar + header + content grid

### 2. Editor Pages
Full-width canvas with floating panels, no standard layout

### 3. Settings Pages
Two-column layout with settings navigation + content

### 4. Marketing Pages
Full-width, no sidebar, marketing-focused layout

---

## Navigation Patterns

```
┌─────────────────────────────────────────────────────────────────┐
│                     NAVIGATION HIERARCHY                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Level 1: Sidebar Navigation                                     │
│  ├── Dashboard                                                   │
│  ├── Workflows                                                   │
│  ├── Templates                                                   │
│  ├── Executions                                                  │
│  ├── Marketplace                                                 │
│  └── Settings                                                    │
│                                                                  │
│  Level 2: Page Tabs / Sub-navigation                            │
│  └── e.g., Settings → Profile | Workspace | Billing | AI        │
│                                                                  │
│  Level 3: Breadcrumbs                                           │
│  └── e.g., Workflows > My First Workflow > Settings             │
│                                                                  │
│  Level 4: In-page anchors                                       │
│  └── e.g., Scroll to section                                    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Z-Index Scale

```
z-0:      0       - Base layer
z-10:     10      - Elevated cards
z-20:     20      - Sticky elements
z-30:     30      - Dropdowns
z-40:     40      - Fixed sidebar
z-50:     50      - Modal backdrop
z-60:     60      - Modal content
z-70:     70      - Toasts
z-80:     80      - Command palette
z-90:     90      - Tooltips
z-100:    100     - Maximum (popovers)
```

---

## Animation Tokens

```
┌─────────────────────────────────────────────────────────────────┐
│ DURATIONS                                                        │
├─────────────────────────────────────────────────────────────────┤
│ Fast:       150ms    - Hovers, toggles                          │
│ Normal:     200ms    - Standard transitions                      │
│ Slow:       300ms    - Panel slides, modals                     │
│ Slower:     500ms    - Page transitions                         │
├─────────────────────────────────────────────────────────────────┤
│ EASINGS                                                          │
├─────────────────────────────────────────────────────────────────┤
│ ease-out:        cubic-bezier(0, 0, 0.2, 1)     - Entrances    │
│ ease-in:         cubic-bezier(0.4, 0, 1, 1)     - Exits        │
│ ease-in-out:     cubic-bezier(0.4, 0, 0.2, 1)   - Standard     │
│ spring:          cubic-bezier(0.34, 1.56, 0.64, 1) - Bouncy    │
└─────────────────────────────────────────────────────────────────┘
```

---

## Accessibility Standards

- **Contrast**: WCAG AA minimum (4.5:1 for text, 3:1 for large text)
- **Focus**: Visible focus rings on all interactive elements
- **Keyboard**: Full keyboard navigation support
- **Screen readers**: Proper ARIA labels and landmarks
- **Reduced motion**: Respect `prefers-reduced-motion`

---

## Next Steps

Proceed to [01-app-shell.md](./01-app-shell.md) for the detailed app shell layout.
