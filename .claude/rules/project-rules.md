# Resonant.ai - Project Rules

> Règles spécifiques pour le projet Resonant.ai

---

## 1. Phase Actuelle : Pre-Development

### Ce qu'on fait
- Documentation
- Planification
- Fundraising preparation
- Pitch materials

### Ce qu'on NE fait PAS encore
- Écriture de code production
- Setup d'infrastructure
- Commits vers un repo

---

## 2. Stack Technique (Locked)

Ces choix sont **finaux** et ne doivent pas être remis en question :

| Layer | Technology | Rationale |
|-------|------------|-----------|
| **Framework** | Next.js 16 | RSC, Turbopack, modern |
| **UI** | shadcn/ui + Tailwind 4 | DafnckStudio standard |
| **Backend** | Convex | Real-time native |
| **Auth** | Clerk | Standard DafnckStudio |
| **Payments** | Stripe | Standard DafnckStudio |
| **Workflow Editor** | React Flow | Best-in-class DAG |
| **Collaboration** | Yjs | CRDT leader |
| **AI** | Vercel AI SDK | Multi-provider abstraction |

**NE PAS proposer** : Supabase, Firebase, NextAuth, Automerge, autres.

---

## 3. Architecture Decisions (ADRs)

### ADR-001: Multi-AI Architecture
**Decision:** Support multiple AI providers from day one
**Context:** Single-provider lock-in is Poppy's main weakness
**Consequences:** More complex, but major competitive advantage

### ADR-002: DAG over Linear
**Decision:** Use directed acyclic graph (DAG) for workflows
**Context:** Linear workflows are limiting
**Consequences:** More powerful, requires React Flow expertise

### ADR-003: CRDT over OT
**Decision:** Use Yjs (CRDT) over Operational Transform
**Context:** CRDT is more robust for collaboration
**Consequences:** Better offline support, easier conflict resolution

### ADR-004: Token-based Pricing
**Decision:** Token-based billing, not credits
**Context:** Credits are opaque and frustrating (Poppy)
**Consequences:** Transparent, industry-standard, easier to understand

---

## 4. Design System Rules

### Typography
- **Primary font:** Inter
- **Monospace:** Space Mono
- **No other fonts**

### Icons
- **ONLY Lucide** (`lucide-react`)
- **NEVER** Heroicons, FontAwesome, etc.

### Colors
- Use Tailwind + shadcn/ui theme system
- Primary: Blue (AI/tech feel)
- Dark mode first

### Components
- Use shadcn/ui primitives
- Reference DentistryGPT pour patterns complexes
- Framer Motion pour animations

---

## 5. Naming Conventions

### Files
```
components/
├── workflow/
│   ├── WorkflowCanvas.tsx    # PascalCase for components
│   ├── use-workflow.ts       # kebab-case for hooks
│   └── workflow-types.ts     # kebab-case for utils
```

### Convex
```typescript
// Tables: camelCase pluriel
workflows, blocks, executions

// Queries/Mutations: camelCase verbe
getWorkflow, createWorkflow, updateBlock

// Index: by_field
by_userId, by_workspaceId
```

### Variables
```typescript
// React state
const [isExecuting, setIsExecuting] = useState(false);

// Handlers
const handleBlockAdd = () => {};
const onExecutionComplete = () => {};

// Boolean props
isLoading, hasError, canEdit
```

---

## 6. Code Quality Standards

### File Size Limits
- **Components:** 150 lines max
- **Files:** 300 lines max
- **Functions:** 40 lines max

### Required in Components
```typescript
// Always include
"use client"; // if needed
interface Props { }
export function ComponentName({ }: Props) { }
```

### TypeScript
- `strict: true` obligatoire
- No `any` sauf cas exceptionnels documentés
- Prefer `unknown` over `any`

---

## 7. Security Rules

### Authentication
- TOUTES les routes dashboard protégées via middleware Clerk
- Server-side auth check sur mutations Convex

### Data Access
- Row-level security sur toutes les tables
- Vérifier `userId` sur chaque query/mutation

### Secrets
- Jamais dans le code
- `.env.local` pour dev
- Vercel env vars pour prod

### AI Prompts
- Sanitize user input avant envoi aux LLMs
- Rate limiting obligatoire
- Log les prompts pour audit

---

## 8. Performance Rules

### React Flow
```typescript
// TOUJOURS mémoiser les custom nodes
const AIBlockNode = memo(({ data }) => { });

// Limiter les re-renders
const nodes = useMemo(() => calculateNodes(data), [data]);
```

### Convex
```typescript
// Utiliser indexes pour les queries fréquentes
.index("by_workspaceId_status", ["workspaceId", "status"])

// Pagination pour les listes longues
const workflows = await ctx.db.query("workflows")
  .withIndex("by_workspaceId")
  .paginate(paginationOpts);
```

### Images
- Toujours `next/image`
- WebP format
- Lazy loading par défaut

---

## 9. Testing Strategy

### Required Tests
| Type | Coverage | Tools |
|------|----------|-------|
| Unit | Utilities, hooks | Vitest |
| Integration | Convex mutations | Vitest + convex-test |
| E2E | Critical flows | Playwright |

### Critical Flows to Test
1. User signup → workspace creation
2. Create workflow → add blocks → execute
3. Real-time collaboration sync
4. Subscription upgrade flow
5. AI execution with fallback

---

## 10. Git Workflow (When Dev Starts)

### Branches
```
main        # Production
├── develop # Integration
    ├── feature/workflow-editor
    ├── feature/multi-ai
    └── fix/execution-timeout
```

### Commit Messages
```
feat(workflow): add block duplication
fix(ai): handle Claude rate limit
perf(canvas): memoize node rendering
docs(readme): update setup instructions
```

### PR Requirements
- Build passes
- Tests pass
- Code review approved
- No TypeScript errors

---

## 11. Documentation Requirements

### Code Comments
- JSDoc pour fonctions publiques
- Inline comments pour logique complexe uniquement
- Pas de commentaires évidents

### README
- Setup instructions
- Architecture overview
- Environment variables

### Changelog
- Maintenu pour chaque release
- Format Keep a Changelog

---

## 12. Deployment Rules (Future)

### Environments
| Env | URL | Branch |
|-----|-----|--------|
| **Preview** | pr-*.vercel.app | PR branches |
| **Staging** | staging.resonant.ai | develop |
| **Production** | resonant.ai | main |

### Deploy Checklist
- [ ] Build passes locally
- [ ] All tests pass
- [ ] No TypeScript errors
- [ ] Environment variables set
- [ ] Database migrations applied
- [ ] Feature flags configured

---

## 13. Third-Party Integration Rules

### AI Providers
- Vercel AI SDK UNIQUEMENT
- Direct SDK calls interdits
- Unified interface obligatoire

### Stripe
- Webhooks via API route
- Idempotency keys obligatoires
- Customer portal pour self-service

### Clerk
- Middleware pour protection routes
- Webhook pour sync users → Convex
- Organizations pour teams

---

## 14. Monitoring (Future)

### Required
- Error tracking (Sentry)
- Analytics (Vercel Analytics)
- AI usage metrics (custom)
- Performance monitoring (Vercel)

### Alerts
- Error rate > 1%
- AI provider failures
- Subscription webhook failures
- Database connection issues

---

*Last Updated: January 2026*
