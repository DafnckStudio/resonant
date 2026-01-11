# Resonant.ai - Product Requirements Document (PRD)

> **Document Version:** 1.0
> **Date:** January 2026
> **Status:** Pre-Development Specification
> **Author:** Product Team

---

## 1. Product Overview

### 1.1 Product Name
**Resonant.ai** - Visual AI Content Workflow Platform

### 1.2 Product Vision
Create the definitive platform for AI-assisted content creation that combines multi-AI orchestration, visual workflow building, and real-time collaboration.

### 1.3 Target Release
- **Alpha:** Q2 2026
- **Beta:** Q3 2026
- **Public Launch:** Q4 2026

### 1.4 Success Metrics (V1)

| Metric | Target | Timeframe |
|--------|--------|-----------|
| Registered Users | 10,000 | 6 months post-launch |
| Daily Active Users | 2,000 | 6 months post-launch |
| Paid Conversion Rate | 5% | Ongoing |
| User Retention (D30) | 40% | Ongoing |
| NPS Score | >50 | Quarterly |
| Workflow Completion Rate | >80% | Ongoing |

---

## 2. User Personas

### 2.1 Primary Persona: Content Creator Pro ("Alex")

```
┌─────────────────────────────────────────────────────────────┐
│ PERSONA: Alex - YouTube Content Creator                      │
├─────────────────────────────────────────────────────────────┤
│ Demographics:                                                │
│ • Age: 28, Location: Austin, TX                             │
│ • Income: $120K/year from content                           │
│ • Platforms: YouTube (500K subs), TikTok (200K)             │
│                                                              │
│ Goals:                                                       │
│ • Scale from 2 videos/week to 5 videos/week                 │
│ • Maintain quality while increasing output                   │
│ • Reduce time spent on research and scripting               │
│                                                              │
│ Pain Points:                                                 │
│ • Spends 40% of time on research (wants to create)          │
│ • Inconsistent video performance                             │
│ • Creative burnout from constant production                  │
│                                                              │
│ Current Tools: ChatGPT, VidIQ, Google Docs, Notion          │
│ Budget: $200-500/month for tools                            │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 Secondary Persona: Agency Growth ("Sarah")

```
┌─────────────────────────────────────────────────────────────┐
│ PERSONA: Sarah - Agency Owner                                │
├─────────────────────────────────────────────────────────────┤
│ Demographics:                                                │
│ • Age: 35, Location: New York, NY                           │
│ • Company: 12-person marketing agency                       │
│ • Clients: 25 active retainers                              │
│                                                              │
│ Goals:                                                       │
│ • Scale content production without hiring                    │
│ • Standardize quality across team                           │
│ • Reduce client onboarding time                             │
│                                                              │
│ Pain Points:                                                 │
│ • Quality varies by team member                              │
│ • Can't onboard new clients fast enough                     │
│ • No visibility into team productivity                       │
│                                                              │
│ Current Tools: Jasper, Asana, Slack, Google Workspace       │
│ Budget: $1,000-3,000/month for content tools                │
└─────────────────────────────────────────────────────────────┘
```

### 2.3 Tertiary Persona: Enterprise Content Lead ("Michael")

```
┌─────────────────────────────────────────────────────────────┐
│ PERSONA: Michael - Enterprise Marketing Director            │
├─────────────────────────────────────────────────────────────┤
│ Demographics:                                                │
│ • Age: 42, Location: San Francisco, CA                      │
│ • Company: Fortune 500 SaaS company                         │
│ • Team: 50-person content organization                      │
│                                                              │
│ Goals:                                                       │
│ • Ensure brand consistency across all content               │
│ • Meet compliance requirements for AI content               │
│ • Reduce content production costs by 30%                    │
│                                                              │
│ Pain Points:                                                 │
│ • Brand guidelines constantly violated                       │
│ • No audit trail for AI-generated content                   │
│ • Siloed tools across teams                                 │
│                                                              │
│ Current Tools: Adobe Suite, Sprinklr, internal tools        │
│ Budget: $10,000+/month, enterprise contracts                │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Feature Requirements

### 3.1 Core Features (MVP)

#### F1: Multi-AI Router Engine

**Priority:** P0 (Critical)
**Complexity:** High

**Description:**
Intelligent routing system that selects the optimal AI provider for each task based on task type, quality requirements, and cost constraints.

**User Stories:**
- US-001: As a user, I want the system to automatically choose the best AI for my task so I get optimal results
- US-002: As a user, I want to see which AI processed my request for transparency
- US-003: As a user, I want to override the AI selection when needed

**Acceptance Criteria:**
- [ ] Supports Claude, GPT-4, and Gemini
- [ ] Routing based on task type (writing, research, analysis)
- [ ] Fallback to secondary provider if primary fails
- [ ] Response time <3 seconds for routing decision
- [ ] User can force specific provider
- [ ] Cost tracking per provider

**Technical Notes:**
```typescript
interface AIRouter {
  route(task: Task): Promise<AIProvider>;
  execute(task: Task, provider?: AIProvider): Promise<Result>;
  getProviderStats(): ProviderStats[];
}

type AIProvider = 'claude' | 'gpt4' | 'gemini' | 'opensource';

interface RoutingDecision {
  provider: AIProvider;
  reason: string;
  estimatedCost: number;
  estimatedQuality: number;
}
```

---

#### F2: Visual DAG Workflow Builder

**Priority:** P0 (Critical)
**Complexity:** High

**Description:**
React Flow-based visual editor for creating, editing, and managing content workflows as directed acyclic graphs.

**User Stories:**
- US-004: As a user, I want to drag and drop blocks to create workflows visually
- US-005: As a user, I want to connect blocks to define data flow
- US-006: As a user, I want to see real-time execution progress on the graph
- US-007: As a user, I want to save and reuse my workflows

**Acceptance Criteria:**
- [ ] Drag-and-drop block placement
- [ ] Visual connection lines between blocks
- [ ] Real-time validation (no cycles, required inputs)
- [ ] Zoom/pan controls
- [ ] Mini-map for large workflows
- [ ] Undo/redo support
- [ ] Auto-layout option
- [ ] Mobile-responsive (view-only)

**UI Mockup Reference:**
```
┌─────────────────────────────────────────────────────────────┐
│ [Toolbar: Save | Run | Share | Settings]          [Zoom: -+]│
├───────────┬─────────────────────────────────────────────────┤
│ BLOCKS    │                                                 │
│           │    ┌──────────┐     ┌──────────┐               │
│ □ Research│    │ Research │────▶│ Outline  │               │
│ □ Outline │    │   ✓      │     │   ⏳     │               │
│ □ Write   │    └──────────┘     └────┬─────┘               │
│ □ SEO     │                          │                      │
│ □ Review  │                          ▼                      │
│           │                    ┌──────────┐                 │
│ TEMPLATES │                    │  Write   │                 │
│           │                    │   ○      │                 │
│ ▸ YouTube │                    └──────────┘                 │
│ ▸ Blog    │                                                 │
│ ▸ Social  │                                                 │
└───────────┴─────────────────────────────────────────────────┘
```

---

#### F3: Composable Block System

**Priority:** P0 (Critical)
**Complexity:** Medium

**Description:**
Modular, reusable blocks that can be combined to create workflows. Each block has defined inputs, outputs, and configuration.

**User Stories:**
- US-008: As a user, I want to configure each block's behavior
- US-009: As a user, I want to see what data flows in and out of each block
- US-010: As a user, I want to create custom blocks from templates

**Block Types (V1):**

| Block Type | Input | Output | Config Options |
|------------|-------|--------|----------------|
| **Research** | Topic, keywords | Research notes | Sources, depth |
| **Outline** | Research, format | Structured outline | Style, length |
| **Write** | Outline, tone | Draft content | Word count, style |
| **SEO** | Content, keywords | Optimized content | Density, structure |
| **Review** | Content, guidelines | Feedback | Criteria |
| **Hooks** | Content | Hook variations | Count, style |
| **Thumbnail** | Topic, style | Image prompts | Platform |

**Acceptance Criteria:**
- [ ] Each block has clear input/output schema
- [ ] Blocks validate inputs before execution
- [ ] Blocks can be configured via side panel
- [ ] Block execution is atomic (success or fail)
- [ ] Blocks can be saved as favorites
- [ ] Custom blocks can reference built-in blocks

---

#### F4: Real-time Collaboration

**Priority:** P1 (High)
**Complexity:** High

**Description:**
Figma-like multiplayer editing with presence awareness, cursor sharing, and conflict-free editing using CRDT (Yjs).

**User Stories:**
- US-011: As a team member, I want to see who else is viewing the workflow
- US-012: As a team member, I want to edit simultaneously without conflicts
- US-013: As a team member, I want to leave comments on specific blocks

**Acceptance Criteria:**
- [ ] Show avatar presence of active users
- [ ] Real-time cursor position (optional toggle)
- [ ] Conflict-free simultaneous editing
- [ ] Comments attached to blocks
- [ ] Notification for mentions
- [ ] Activity feed in sidebar
- [ ] Offline mode with sync on reconnect

**Technical Implementation:**
```typescript
// Using Yjs for CRDT
import * as Y from 'yjs';
import { WebsocketProvider } from 'y-websocket';

const ydoc = new Y.Doc();
const provider = new WebsocketProvider(
  'wss://sync.resonant.ai',
  `workflow-${workflowId}`,
  ydoc
);

const yWorkflow = ydoc.getMap('workflow');
const yBlocks = ydoc.getArray('blocks');
const yConnections = ydoc.getArray('connections');
```

---

#### F5: Version Control System

**Priority:** P1 (High)
**Complexity:** Medium

**Description:**
Git-like version control for workflows with commits, branches, and rollback capabilities.

**User Stories:**
- US-014: As a user, I want to save versions of my workflow
- US-015: As a user, I want to compare versions side-by-side
- US-016: As a user, I want to rollback to a previous version
- US-017: As a user, I want to branch a workflow for experimentation

**Acceptance Criteria:**
- [ ] Auto-save creates checkpoints every 5 minutes
- [ ] Manual save with commit message
- [ ] Version history list with timestamps
- [ ] Diff view between versions
- [ ] One-click rollback
- [ ] Branch creation from any version
- [ ] Merge branches (with conflict resolution)

---

#### F6: Template Library

**Priority:** P1 (High)
**Complexity:** Low

**Description:**
Pre-built workflow templates for common content types that users can clone and customize.

**User Stories:**
- US-018: As a new user, I want to start from a template instead of blank
- US-019: As a user, I want to save my workflows as templates
- US-020: As a user, I want to share templates with my team

**V1 Templates:**
1. YouTube Video Script
2. Blog Post (SEO-optimized)
3. Twitter Thread
4. LinkedIn Post
5. Newsletter
6. Product Description
7. Email Sequence
8. Video Hook Generator

**Acceptance Criteria:**
- [ ] Template gallery with preview
- [ ] One-click clone to workspace
- [ ] Templates include documentation
- [ ] User can save personal templates
- [ ] Team templates (Team plan+)
- [ ] Public templates (with approval)

---

### 3.2 Supporting Features (V1)

#### F7: User Authentication & Accounts

**Priority:** P0 (Critical)
**Complexity:** Low

**Provider:** Clerk

**Features:**
- [ ] Email/password signup
- [ ] Google OAuth
- [ ] GitHub OAuth
- [ ] Email verification
- [ ] Password reset
- [ ] User profile management
- [ ] Team invitations

---

#### F8: Subscription & Billing

**Priority:** P0 (Critical)
**Complexity:** Medium

**Provider:** Stripe

**Features:**
- [ ] Plan selection (Free, Creator, Pro, Team)
- [ ] Credit card payment
- [ ] Usage metering (tokens)
- [ ] Overage billing
- [ ] Invoices and receipts
- [ ] Plan upgrade/downgrade
- [ ] Cancel subscription
- [ ] Customer portal

---

#### F9: Usage Dashboard

**Priority:** P1 (High)
**Complexity:** Low

**Features:**
- [ ] Token usage graph (daily/weekly/monthly)
- [ ] Usage by AI provider
- [ ] Usage by workflow
- [ ] Remaining quota display
- [ ] Usage alerts (80%, 100%)
- [ ] Export usage data

---

#### F10: Workflow Analytics

**Priority:** P2 (Medium)
**Complexity:** Medium

**Features:**
- [ ] Workflow execution time
- [ ] Block-level performance
- [ ] Success/failure rates
- [ ] Cost per workflow
- [ ] Comparison between workflow versions
- [ ] A/B test results (future)

---

### 3.3 Future Features (Post-V1)

| Feature | Priority | Target Release |
|---------|----------|----------------|
| Skills Marketplace | P1 | V1.2 |
| API Access | P1 | V1.2 |
| Autonomous Agents | P2 | V1.5 |
| Brand Voice Training | P2 | V1.5 |
| Mobile App | P2 | V2.0 |
| Integrations (Notion, Airtable) | P1 | V1.3 |
| Enterprise SSO | P1 | V1.4 |
| Audit Logs | P1 | V1.4 |
| White-Label | P3 | V2.0 |

---

## 4. Technical Requirements

### 4.1 Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                      FRONTEND (Next.js)                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │ React    │  │ React    │  │ Yjs      │  │ Clerk    │    │
│  │ Flow     │  │ 19       │  │ CRDT     │  │ Auth     │    │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘    │
│       └─────────────┴─────────────┴─────────────┘           │
│                           │                                  │
├───────────────────────────┼──────────────────────────────────┤
│                      BACKEND (Convex)                        │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │ Real-time│  │ Functions│  │ File     │  │ Auth     │    │
│  │ Sync     │  │ (Queries │  │ Storage  │  │ Webhooks │    │
│  │          │  │ Mutations│  │          │  │          │    │
│  │          │  │ Actions) │  │          │  │          │    │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘    │
│       └─────────────┴─────────────┴─────────────┘           │
│                           │                                  │
├───────────────────────────┼──────────────────────────────────┤
│                    AI PROVIDERS                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │ Anthropic│  │ OpenAI   │  │ Google   │  │ Local    │    │
│  │ (Claude) │  │ (GPT-4)  │  │ (Gemini) │  │ (Llama)  │    │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘    │
├───────────────────────────┴──────────────────────────────────┤
│                    EXTERNAL SERVICES                         │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                   │
│  │ Clerk    │  │ Stripe   │  │ Resend   │                   │
│  │ (Auth)   │  │ (Payment)│  │ (Email)  │                   │
│  └──────────┘  └──────────┘  └──────────┘                   │
└─────────────────────────────────────────────────────────────┘
```

### 4.2 Technology Stack

| Layer | Technology | Rationale |
|-------|------------|-----------|
| **Frontend** | Next.js 16 | App Router, RSC, performance |
| **UI Library** | shadcn/ui | Accessible, customizable |
| **Styling** | Tailwind CSS 4 | Utility-first, fast |
| **Workflow Editor** | React Flow | Battle-tested DAG library |
| **Real-time** | Yjs + y-websocket | CRDT, proven at scale |
| **State** | Zustand / Convex | Simple, reactive |
| **Backend** | Convex | Real-time, serverless |
| **Auth** | Clerk | Complete auth solution |
| **Payments** | Stripe | Industry standard |
| **Email** | Resend | Developer-friendly |
| **AI SDKs** | Vercel AI SDK | Unified interface |
| **Analytics** | PostHog | Product analytics |
| **Monitoring** | Sentry | Error tracking |

### 4.3 Performance Requirements

| Metric | Target | Measurement |
|--------|--------|-------------|
| **Page Load (LCP)** | <2.5s | Core Web Vitals |
| **Time to Interactive** | <3.5s | Lighthouse |
| **API Response** | <200ms | P95 |
| **AI Response (start)** | <2s | P95 |
| **Real-time Sync** | <100ms | P95 |
| **Uptime** | 99.9% | Monthly |

### 4.4 Security Requirements

| Requirement | Implementation |
|-------------|----------------|
| **Authentication** | Clerk with MFA option |
| **Authorization** | Role-based (Owner, Admin, Member, Viewer) |
| **Data Encryption** | TLS 1.3 in transit, AES-256 at rest |
| **API Security** | Rate limiting, API keys |
| **GDPR Compliance** | Data export, deletion |
| **SOC 2** | Target for V1.5 |

---

## 5. User Flows

### 5.1 New User Onboarding

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Landing │───▶│  Sign Up │───▶│ Onboard  │───▶│ Template │
│   Page   │    │  (Clerk) │    │  Survey  │    │  Select  │
└──────────┘    └──────────┘    └──────────┘    └────┬─────┘
                                                      │
┌──────────┐    ┌──────────┐    ┌──────────┐         │
│   Use    │◀───│  First   │◀───│ Template │◀────────┘
│  Daily   │    │  Success │    │   Run    │
└──────────┘    └──────────┘    └──────────┘
```

**Onboarding Survey Questions:**
1. What type of content do you create? (Video, Blog, Social, Other)
2. How often do you publish? (Daily, Weekly, Monthly)
3. What's your biggest content challenge? (Ideas, Writing, SEO, Consistency)
4. What tools do you currently use? (ChatGPT, Jasper, Other)

**Template Selection:**
Based on survey, recommend top 3 templates.

### 5.2 Workflow Creation Flow

```
Start
  │
  ▼
┌─────────────────┐
│ New Workflow    │──▶ From Template OR Blank
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Add First Block │──▶ Drag from sidebar OR Quick Add
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Configure Block │──▶ Set parameters in side panel
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Add More Blocks │──▶ Connect with lines
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Validate Flow   │──▶ Check for errors (auto)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Run Workflow    │──▶ Execute all blocks
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Review Output   │──▶ Edit, approve, export
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Save & Iterate  │──▶ Version saved automatically
└─────────────────┘
```

### 5.3 Workflow Execution Flow

```
User clicks "Run"
        │
        ▼
┌────────────────────┐
│ Validate Workflow  │──▶ Check: All inputs filled?
│                    │    Check: No cycles?
└─────────┬──────────┘    Check: All blocks valid?
          │
          ▼
┌────────────────────┐
│ Queue Execution    │──▶ Create execution record
└─────────┬──────────┘    Set status: "running"
          │
          ▼
┌────────────────────┐
│ Execute Blocks     │──▶ Topological sort
│ (Sequential)       │    Execute in order
└─────────┬──────────┘    Update UI in real-time
          │
          ├──────────────────────────┐
          │                          │
          ▼                          ▼
┌─────────────────┐        ┌─────────────────┐
│ Block Success   │        │ Block Failure   │
│ • Update status │        │ • Log error     │
│ • Pass output   │        │ • Option: Retry │
│ • Continue      │        │ • Option: Skip  │
└─────────────────┘        │ • Option: Stop  │
                           └─────────────────┘
          │
          ▼
┌────────────────────┐
│ Execution Complete │──▶ All outputs available
│                    │    Usage logged
└─────────┬──────────┘    Analytics updated
          │
          ▼
┌────────────────────┐
│ Show Results       │──▶ Output panel
│                    │    Export options
└────────────────────┘
```

---

## 6. Data Model

### 6.1 Core Entities

```typescript
// convex/schema.ts

// Users
users: defineTable({
  clerkId: v.string(),
  email: v.string(),
  name: v.optional(v.string()),
  avatarUrl: v.optional(v.string()),
  plan: v.union(v.literal("free"), v.literal("creator"), v.literal("pro"), v.literal("team"), v.literal("enterprise")),
  stripeCustomerId: v.optional(v.string()),
  tokenBalance: v.number(),
  tokenUsedThisMonth: v.number(),
  createdAt: v.number(),
  updatedAt: v.number(),
})
  .index("by_clerkId", ["clerkId"])
  .index("by_email", ["email"])

// Workspaces (for team collaboration)
workspaces: defineTable({
  name: v.string(),
  ownerId: v.id("users"),
  plan: v.string(),
  createdAt: v.number(),
})
  .index("by_owner", ["ownerId"])

// Workspace Members
workspaceMembers: defineTable({
  workspaceId: v.id("workspaces"),
  userId: v.id("users"),
  role: v.union(v.literal("owner"), v.literal("admin"), v.literal("member"), v.literal("viewer")),
  joinedAt: v.number(),
})
  .index("by_workspace", ["workspaceId"])
  .index("by_user", ["userId"])

// Workflows
workflows: defineTable({
  workspaceId: v.id("workspaces"),
  name: v.string(),
  description: v.optional(v.string()),
  blocks: v.array(v.object({
    id: v.string(),
    type: v.string(),
    position: v.object({ x: v.number(), y: v.number() }),
    config: v.any(),
  })),
  connections: v.array(v.object({
    id: v.string(),
    source: v.string(),
    target: v.string(),
    sourceHandle: v.optional(v.string()),
    targetHandle: v.optional(v.string()),
  })),
  isTemplate: v.boolean(),
  isPublic: v.boolean(),
  version: v.number(),
  createdBy: v.id("users"),
  createdAt: v.number(),
  updatedAt: v.number(),
})
  .index("by_workspace", ["workspaceId"])
  .index("by_creator", ["createdBy"])
  .index("by_template", ["isTemplate", "isPublic"])

// Workflow Versions (for version control)
workflowVersions: defineTable({
  workflowId: v.id("workflows"),
  version: v.number(),
  blocks: v.array(v.any()),
  connections: v.array(v.any()),
  message: v.optional(v.string()),
  createdBy: v.id("users"),
  createdAt: v.number(),
})
  .index("by_workflow", ["workflowId"])

// Executions
executions: defineTable({
  workflowId: v.id("workflows"),
  userId: v.id("users"),
  status: v.union(v.literal("pending"), v.literal("running"), v.literal("completed"), v.literal("failed")),
  inputs: v.any(),
  outputs: v.optional(v.any()),
  blockResults: v.array(v.object({
    blockId: v.string(),
    status: v.string(),
    output: v.optional(v.any()),
    error: v.optional(v.string()),
    startedAt: v.number(),
    completedAt: v.optional(v.number()),
    tokensUsed: v.number(),
    provider: v.optional(v.string()),
  })),
  totalTokens: v.number(),
  totalCost: v.number(),
  startedAt: v.number(),
  completedAt: v.optional(v.number()),
})
  .index("by_workflow", ["workflowId"])
  .index("by_user", ["userId"])

// Usage Tracking
usageRecords: defineTable({
  userId: v.id("users"),
  workspaceId: v.optional(v.id("workspaces")),
  executionId: v.optional(v.id("executions")),
  provider: v.string(),
  tokensInput: v.number(),
  tokensOutput: v.number(),
  cost: v.number(),
  createdAt: v.number(),
})
  .index("by_user", ["userId"])
  .index("by_workspace", ["workspaceId"])
  .index("by_date", ["createdAt"])
```

---

## 7. Release Plan

### 7.1 Alpha Release (Q2 2026)

**Goal:** Validate core product with 50-100 beta users

**Features:**
- [ ] Basic authentication (Clerk)
- [ ] Workflow editor (React Flow)
- [ ] 5 block types (Research, Outline, Write, SEO, Hooks)
- [ ] Single AI provider (Claude)
- [ ] Manual saving
- [ ] 3 templates

**Success Criteria:**
- 50 users complete at least 1 workflow
- Average session length >10 minutes
- Qualitative feedback positive

### 7.2 Beta Release (Q3 2026)

**Goal:** Product-market fit validation with 500-1000 users

**Features:**
- [ ] Multi-AI routing
- [ ] All V1 block types
- [ ] Version control
- [ ] Real-time collaboration
- [ ] Subscription billing (Stripe)
- [ ] 8 templates
- [ ] Usage dashboard

**Success Criteria:**
- 500 active users
- 5% paid conversion
- NPS >40

### 7.3 Public Launch (Q4 2026)

**Goal:** Scale to 10,000 users

**Features:**
- [ ] All V1 features complete
- [ ] Mobile-responsive
- [ ] Team features
- [ ] Template marketplace (user submissions)
- [ ] API (beta)
- [ ] Integrations (Notion, Airtable)

**Success Criteria:**
- 10,000 registered users
- 500 paid subscribers
- $40K MRR

---

## 8. Open Questions

| # | Question | Owner | Due Date |
|---|----------|-------|----------|
| 1 | Should we support offline mode in V1? | Product | TBD |
| 2 | How do we handle AI provider outages? | Engineering | TBD |
| 3 | What's the free tier token limit? | Product | TBD |
| 4 | Do we build mobile app or responsive web? | Product | TBD |
| 5 | How do we moderate public templates? | Product | TBD |

---

## 9. Appendix

### 9.1 Competitive Feature Matrix

| Feature | Resonant | Poppy | Jasper | Copy.ai |
|---------|----------|-------|--------|---------|
| Multi-AI Providers | ✅ | ❌ | ❌ | ❌ |
| Visual Workflows | ✅ | ❌ | ❌ | ❌ |
| Real-time Collab | ✅ | ❌ | ✅ | ❌ |
| Version Control | ✅ | ❌ | ❌ | ❌ |
| Custom Templates | ✅ | ✅ | ✅ | ✅ |
| API Access | ✅ | Limited | ✅ | ✅ |
| Team Features | ✅ | Limited | ✅ | ✅ |
| Marketplace | ✅ (V1.2) | ❌ | ❌ | ❌ |

### 9.2 Glossary

| Term | Definition |
|------|------------|
| **Block** | A single unit of work in a workflow (e.g., "Research", "Write") |
| **DAG** | Directed Acyclic Graph - data structure for workflows |
| **CRDT** | Conflict-free Replicated Data Type - enables real-time collaboration |
| **Token** | Unit of AI usage (roughly 4 characters) |
| **Workflow** | A complete content creation process made of connected blocks |

---

*Document maintained by Product Team. Last updated: January 2026*
