# Resonant.ai - Project CLAUDE.md

> **Version:** 1.0
> **Created:** January 2026
> **Status:** Pre-Development (Fundraising Phase)
> **Category:** Client Project

---

## Project Overview

**Resonant.ai** est une plateforme de création de contenu IA de nouvelle génération, positionnée comme le "Figma for AI Content Workflows". Le projet combine orchestration multi-AI, workflows visuels DAG, et collaboration temps réel.

```
┌─────────────────────────────────────────────────────────────┐
│                    RESONANT.AI PLATFORM                      │
├─────────────────────────────────────────────────────────────┤
│  Multi-AI Router  │  Visual DAG Workflows  │  Real-time     │
│  (Claude, GPT-4,  │  (React Flow)          │  Collaboration │
│  Gemini, Llama)   │                        │  (Yjs CRDT)    │
├─────────────────────────────────────────────────────────────┤
│        Convex Backend  │  Clerk Auth  │  Stripe Billing     │
└─────────────────────────────────────────────────────────────┘
```

---

## Project Structure

```
/home/hacker/VibeCoding/clients/resonant/
├── CLAUDE.md              # This file
├── Infos.md               # Original Poppy AI audit (source)
├── .claude/
│   ├── docs/              # Technical documentation
│   │   └── architecture.md
│   ├── rules/             # Project-specific rules
│   │   └── project-rules.md
│   └── step.json          # Progress tracking
└── Starting/              # Pre-launch documentation
    ├── 01-MARKET-RESEARCH.md
    ├── 02-BUSINESS-POTENTIAL.md
    ├── 03-VISION.md
    ├── 04-PRD.md
    ├── 05-FEATURES-STACK.md
    └── 06-POPPY-VS-RESONANT.md
```

---

## Current Phase

| Phase | Status | Description |
|-------|--------|-------------|
| **Discovery** | ✅ Complete | Poppy AI audit, market research |
| **Documentation** | ✅ Complete | PRD, Vision, Stack, Comparisons |
| **Planning** | 🔄 In Progress | Architecture decisions |
| **Development** | ⏳ Not Started | Awaiting funding |
| **Fundraising** | 🎯 Target | $2M Pre-Seed |

---

## Git Configuration

| Setting | Value |
|---------|-------|
| **Git Email** | `studio@dafnck.com` |
| **Repository** | TBD (à créer après validation) |
| **Branch Strategy** | `main` → `develop` → `feature/*` |

**Note:** Projet client DafnckStudio, utiliser l'email studio.

---

## Technical Stack (Planned)

### Frontend
| Technology | Version | Usage |
|------------|---------|-------|
| **Next.js** | 16.x | App Router, RSC, Turbopack |
| **React** | 19.x | UI framework |
| **React Flow** | 12.x | DAG workflow editor |
| **Tailwind CSS** | 4.x | Styling |
| **shadcn/ui** | latest | UI components |
| **Framer Motion** | latest | Animations |
| **Yjs** | latest | CRDT collaboration |

### Backend
| Technology | Version | Usage |
|------------|---------|-------|
| **Convex** | latest | Real-time database |
| **Clerk** | latest | Authentication |
| **Stripe** | latest | Payments & subscriptions |
| **Resend** | latest | Transactional emails |

### AI Providers
| Provider | Model | Usage |
|----------|-------|-------|
| **Anthropic** | Claude 3.5 | Complex content, nuance |
| **OpenAI** | GPT-4 | Creative, brainstorming |
| **Google** | Gemini 1.5 | Long context, research |
| **Meta** | Llama 3 | Bulk tasks, cost-efficient |

### Infrastructure
| Service | Usage |
|---------|-------|
| **Vercel** | Hosting, Edge functions |
| **Convex** | Backend as a Service |
| **Upstash** | Rate limiting (Redis) |

---

## Obligations Claude Code

### À chaque session
1. Lire `.claude/step.json` pour connaître l'état du projet
2. Lire les documents dans `Starting/` pour le contexte business
3. Consulter les agents pertinents selon la tâche

### Phase actuelle (Pre-Dev)
- Focus sur documentation et planification
- Pas de code en production
- Préparation pitch deck et fundraising

### Quand le développement commencera
1. Créer la structure Next.js 16
2. Setup Convex + Clerk + Stripe
3. Implémenter selon le PRD (`04-PRD.md`)
4. Suivre les features par tiers (`05-FEATURES-STACK.md`)

---

## Agents Pertinents pour Resonant

### Architecture & Planning
| Agent | Usage | Fichier |
|-------|-------|---------|
| **architect-reviewer** | Review des décisions d'architecture | `architect-reviewer.md` |
| **product-manager** | PRD, roadmap, prioritization | `product-manager.md` |
| **business-analyst** | Business model, metrics | `business-analyst.md` |

### Frontend Development
| Agent | Usage | Fichier |
|-------|-------|---------|
| **nextjs-developer** | Next.js 16, App Router, RSC | `nextjs-developer.md` |
| **react-specialist** | React 19, hooks, patterns | `react-specialist.md` |
| **typescript-pro** | Types stricts, generics | `typescript-pro.md` |
| **shadcn-ui-expert** | Composants UI, design system | `shadcn-ui-expert.md` |
| **frontend-developer** | Responsive, performance | `frontend-developer.md` |

### Backend Development
| Agent | Usage | Fichier |
|-------|-------|---------|
| **convex-expert** | Schema, queries, mutations, real-time | `convex-expert.md` |
| **clerk-expert** | Auth, webhooks, middleware | `clerk-expert.md` |
| **stripe-expert** | Paiements, subscriptions, webhooks | `stripe-expert.md` |
| **backend-developer** | API design, architecture | `backend-developer.md` |

### AI & Agents
| Agent | Usage | Fichier |
|-------|-------|---------|
| **ai-engineer** | Multi-AI routing, LLM integration | `ai-engineer.md` |
| **llm-architect** | Prompt engineering, model selection | `llm-architect.md` |
| **prompt-engineer** | Optimisation prompts | `prompt-engineer.md` |

### Collaboration Features
| Agent | Usage | Fichier |
|-------|-------|---------|
| **websocket-engineer** | Real-time, Yjs integration | `websocket-engineer.md` |

### Quality & Security
| Agent | Usage | Fichier |
|-------|-------|---------|
| **security-auditor** | Security review, OWASP | `security-auditor.md` |
| **code-reviewer** | Code quality, best practices | `code-reviewer.md` |
| **test-automator** | Tests, coverage | `test-automator.md` |
| **performance-engineer** | Optimization, metrics | `performance-engineer.md` |

---

## Plugins Prioritaires pour Resonant

### Core Development
| Plugin | Usage |
|--------|-------|
| `full-stack-orchestration` | Coordination multi-domaines |
| `frontend-design` | UI/UX patterns |
| `javascript-typescript` | TypeScript avancé |
| `database-design` | Schema Convex |

### AI Features
| Plugin | Usage |
|--------|-------|
| `llm-application-dev` | Integration AI providers |
| `ai-sdk-agents` | Agent orchestration |

### Quality
| Plugin | Usage |
|--------|-------|
| `code-review-ai` | Review automatique |
| `security-scanning` | Security checks |
| `unit-testing` | Tests unitaires |
| `tdd-workflows` | Test-driven development |

### DevOps
| Plugin | Usage |
|--------|-------|
| `deployment-validation` | Pre-deploy checks |
| `cicd-automation` | CI/CD pipelines |

---

## Key Documents Reference

### Pour comprendre le projet
| Document | Contenu |
|----------|---------|
| `Infos.md` | Audit complet de Poppy AI (source data) |
| `Starting/03-VISION.md` | Mission, vision, pillars |
| `Starting/06-POPPY-VS-RESONANT.md` | Competitive positioning |

### Pour le business
| Document | Contenu |
|----------|---------|
| `Starting/01-MARKET-RESEARCH.md` | TAM/SAM/SOM, personas |
| `Starting/02-BUSINESS-POTENTIAL.md` | Investment thesis, financials |

### Pour le développement
| Document | Contenu |
|----------|---------|
| `Starting/04-PRD.md` | User stories, specs techniques |
| `Starting/05-FEATURES-STACK.md` | Features par tiers, stack complet |

---

## Pricing Model

| Tier | Price | Tokens | Features |
|------|-------|--------|----------|
| **Free** | $0 | 1,000/mo | 3 workflows, basic |
| **Creator** | $49/mo | 50,000/mo | Unlimited workflows, all AI |
| **Pro** | $99/mo | 200,000/mo | Team (3), API access |
| **Team** | $299/mo | 1,000,000/mo | Team (10), white-label |
| **Enterprise** | Custom | Unlimited | SSO, SLA, dedicated |

---

## Development Phases (When Ready)

### Phase 1: Foundation (Weeks 1-4)
- [ ] Setup Next.js 16 + Convex + Clerk
- [ ] Basic UI with shadcn/ui
- [ ] User authentication flow
- [ ] Database schema implementation

### Phase 2: Core Editor (Weeks 5-8)
- [ ] React Flow integration
- [ ] Block system (Input, AI, Output, Condition)
- [ ] Single AI provider (Claude)
- [ ] Basic workflow execution

### Phase 3: Multi-AI (Weeks 9-12)
- [ ] Add GPT-4, Gemini, Llama
- [ ] Smart routing logic
- [ ] Cost optimization
- [ ] Provider fallbacks

### Phase 4: Collaboration (Weeks 13-16)
- [ ] Yjs integration
- [ ] Real-time cursors
- [ ] Comments system
- [ ] Version control

### Phase 5: Monetization (Weeks 17-20)
- [ ] Stripe integration
- [ ] Subscription management
- [ ] Usage tracking
- [ ] Billing portal

---

## Competitors to Monitor

| Competitor | Watch For |
|------------|-----------|
| **Poppy AI** | Feature updates, pricing changes |
| **Jasper** | Enterprise moves, AI additions |
| **Copy.ai** | Workflow features |
| **Notion AI** | Integration expansion |
| **ChatGPT** | Custom GPTs evolution |

---

## Success Metrics (Post-Launch)

| Metric | Target Y1 | Target Y3 |
|--------|-----------|-----------|
| **ARR** | $500K | $15M |
| **Paid Users** | 500 | 13,000 |
| **MRR** | $42K | $1.25M |
| **Churn** | <5% | <3% |
| **NPS** | >50 | >60 |

---

## Notes for Development

### React Flow Best Practices
```typescript
// Custom node type for AI blocks
const AIBlockNode = memo(({ data, selected }) => {
  return (
    <div className={cn(
      "rounded-lg border-2 p-4",
      selected && "border-primary"
    )}>
      {/* Node content */}
    </div>
  );
});
```

### Convex Real-time Pattern
```typescript
// Subscribe to workflow changes
const workflow = useQuery(api.workflows.get, { id: workflowId });

// Optimistic mutations
const updateBlock = useMutation(api.workflows.updateBlock)
  .withOptimisticUpdate((localStore, args) => {
    // Immediate UI update
  });
```

### AI Provider Abstraction
```typescript
// Unified interface via Vercel AI SDK
import { generateText } from 'ai';
import { anthropic } from '@ai-sdk/anthropic';
import { openai } from '@ai-sdk/openai';

const providers = {
  claude: anthropic('claude-3-5-sonnet-20241022'),
  gpt4: openai('gpt-4-turbo'),
  // ...
};
```

---

## Quick Reference

### Commands (when dev starts)
```bash
# Development
cd /home/hacker/VibeCoding/clients/resonant
pnpm dev

# Build
pnpm build

# Convex
npx convex dev

# Type check
pnpm typecheck
```

### Key Decisions Made
1. **Multi-AI** over single provider → Flexibility, cost optimization
2. **React Flow** over custom canvas → Proven library, ecosystem
3. **Convex** over Supabase → Real-time native, serverless
4. **Yjs** over Automerge → Better React integration, smaller bundle
5. **Token-based** over credits → Industry standard, transparency

---

## Contact & Resources

| Resource | Location |
|----------|----------|
| **Documentation** | `/Starting/*.md` |
| **Source Audit** | `/Infos.md` |
| **Global Rules** | `/home/hacker/.claude/CLAUDE.md` |
| **Agents** | `/home/hacker/.claude/agents/*.md` |
| **Reference Project** | `/home/hacker/VibeCoding/clients/DentistryGPT/` |

---

*Last Updated: January 2026*
*Phase: Pre-Development / Fundraising*
