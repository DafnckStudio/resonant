# Resonant.ai - Features & Technical Stack

> **Document Version:** 1.0
> **Date:** January 2026
> **Status:** Technical Specification

---

## Part 1: Feature Tiers

### Tier Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    RESONANT.AI FEATURE MAP                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  TIER 1 (MVP)           TIER 2 (V1.2)        TIER 3 (V2+)  │
│  ═══════════           ═══════════          ═══════════    │
│  • Multi-AI Router     • Marketplace        • AI Agents    │
│  • DAG Workflows       • API Access         • Brand AI     │
│  • Composable Blocks   • Integrations       • Enterprise   │
│  • Real-time Collab    • Analytics Pro      • White-label  │
│  • Version Control     • Team Management    • Mobile App   │
│  • Templates           • Custom Blocks      • Predictive   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Tier 1: Core Platform (MVP)

### 1.1 Multi-AI Router Engine

**Description:** Intelligent system that routes each AI task to the optimal provider based on task type, quality requirements, and cost.

**Components:**

```typescript
// AI Provider Configuration
const providers = {
  claude: {
    name: 'Claude 3.5 Sonnet',
    vendor: 'Anthropic',
    strengths: ['nuanced writing', 'long-form', 'analysis'],
    costPer1MTokens: 15,
    maxTokens: 200000,
  },
  gpt4: {
    name: 'GPT-4 Turbo',
    vendor: 'OpenAI',
    strengths: ['creative', 'coding', 'general'],
    costPer1MTokens: 30,
    maxTokens: 128000,
  },
  gemini: {
    name: 'Gemini 1.5 Pro',
    vendor: 'Google',
    strengths: ['research', 'multimodal', 'long context'],
    costPer1MTokens: 7,
    maxTokens: 1000000,
  },
  llama: {
    name: 'Llama 3.2',
    vendor: 'Meta (Self-hosted)',
    strengths: ['bulk tasks', 'cost-effective'],
    costPer1MTokens: 2,
    maxTokens: 128000,
  },
};

// Routing Logic
interface RoutingDecision {
  provider: AIProvider;
  reason: string;
  estimatedCost: number;
  confidenceScore: number;
}

function routeTask(task: AITask): RoutingDecision {
  // Factors: task type, quality requirement, cost constraint, context length
}
```

**Features:**
- [ ] Automatic provider selection based on task
- [ ] Manual override option
- [ ] Fallback chain if primary fails
- [ ] Cost tracking per provider
- [ ] Quality scoring system
- [ ] A/B testing between providers

**UI Elements:**
- Provider badge on each AI response
- Cost breakdown in execution details
- Provider preferences in settings

---

### 1.2 Visual DAG Workflow Builder

**Description:** React Flow-powered visual editor for creating content workflows as directed acyclic graphs.

**Architecture:**

```
┌─────────────────────────────────────────────────────────────┐
│                     WORKFLOW EDITOR                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────┐                      ┌──────────────────────┐  │
│  │ SIDEBAR │                      │      CANVAS          │  │
│  │         │                      │                      │  │
│  │ Blocks  │                      │    [Block A]         │  │
│  │ ────────│                      │        │             │  │
│  │ □ Input │                      │        ▼             │  │
│  │ □ AI    │     Drag & Drop      │    [Block B]         │  │
│  │ □ Logic │    ─────────────▶    │        │             │  │
│  │ □ Output│                      │    ┌───┴───┐         │  │
│  │         │                      │    ▼       ▼         │  │
│  │ Templates                      │  [B1]    [B2]        │  │
│  │ ─────────                      │                      │  │
│  │ ▸ YouTube                      │                      │  │
│  │ ▸ Blog   │                      └──────────────────────┘  │
│  └─────────┘                                                 │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                   CONFIG PANEL                        │   │
│  │  Block: Research  │ Provider: Auto  │ Tokens: ~500   │   │
│  │  Topic: [______]  │ Sources: [Web]  │                 │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

**Features:**
- [ ] Drag-and-drop block placement
- [ ] Visual connection lines
- [ ] Zoom/pan controls
- [ ] Mini-map navigation
- [ ] Auto-layout algorithm
- [ ] Undo/redo (50 steps)
- [ ] Keyboard shortcuts
- [ ] Real-time validation
- [ ] Execution visualization

**React Flow Integration:**

```typescript
import ReactFlow, {
  Node,
  Edge,
  Controls,
  Background,
  MiniMap,
  useNodesState,
  useEdgesState,
} from 'reactflow';

const WorkflowEditor = () => {
  const [nodes, setNodes, onNodesChange] = useNodesState(initialNodes);
  const [edges, setEdges, onEdgesChange] = useEdgesState(initialEdges);

  return (
    <ReactFlow
      nodes={nodes}
      edges={edges}
      onNodesChange={onNodesChange}
      onEdgesChange={onEdgesChange}
      nodeTypes={customNodeTypes}
      edgeTypes={customEdgeTypes}
      fitView
    >
      <Background />
      <Controls />
      <MiniMap />
    </ReactFlow>
  );
};
```

---

### 1.3 Composable Block System

**Description:** Modular blocks that can be configured and combined to create any content workflow.

**Block Categories:**

| Category | Blocks | Purpose |
|----------|--------|---------|
| **Input** | Topic, Keywords, Reference, Upload | Start data |
| **AI Processing** | Research, Write, Summarize, Translate | AI tasks |
| **Transform** | Format, Split, Merge, Filter | Data manipulation |
| **Output** | Preview, Export, Publish | End results |
| **Logic** | Condition, Loop, A/B Test | Flow control |

**Block Architecture:**

```typescript
interface Block {
  id: string;
  type: BlockType;
  name: string;
  description: string;
  category: 'input' | 'ai' | 'transform' | 'output' | 'logic';
  inputs: InputPort[];
  outputs: OutputPort[];
  config: BlockConfig;
  execute: (inputs: any, config: BlockConfig) => Promise<any>;
}

interface InputPort {
  id: string;
  name: string;
  type: DataType;
  required: boolean;
}

interface OutputPort {
  id: string;
  name: string;
  type: DataType;
}

type DataType = 'text' | 'number' | 'boolean' | 'array' | 'object' | 'file';
```

**V1 Blocks:**

| Block | Inputs | Outputs | Config |
|-------|--------|---------|--------|
| **Research** | topic, keywords | notes, sources | depth, sources |
| **Outline** | content, format | outline | style, sections |
| **Write** | outline, tone | draft | length, style |
| **SEO Optimize** | content, keywords | optimized | density |
| **Hooks** | content | hooks[] | count, style |
| **Summarize** | content | summary | length |
| **Translate** | content, targetLang | translated | - |
| **Format** | content, format | formatted | template |
| **Review** | content, criteria | feedback | checklist |

---

### 1.4 Real-time Collaboration

**Description:** Figma-like multiplayer editing with presence awareness and conflict-free editing.

**Technical Implementation:**

```typescript
// Using Yjs for CRDT
import * as Y from 'yjs';
import { WebsocketProvider } from 'y-websocket';
import { Awareness } from 'y-protocols/awareness';

// Document setup
const ydoc = new Y.Doc();
const provider = new WebsocketProvider(
  process.env.YJS_SERVER_URL,
  `workflow-${workflowId}`,
  ydoc
);

// Shared data structures
const yBlocks = ydoc.getArray<Y.Map<any>>('blocks');
const yConnections = ydoc.getArray<Y.Map<any>>('connections');
const yComments = ydoc.getArray<Y.Map<any>>('comments');

// Awareness (presence)
const awareness = provider.awareness;
awareness.setLocalState({
  user: currentUser,
  cursor: { x: 0, y: 0 },
  selection: null,
});

// React hook
function useCollaborators() {
  const [collaborators, setCollaborators] = useState([]);

  useEffect(() => {
    awareness.on('change', () => {
      const states = Array.from(awareness.getStates().values());
      setCollaborators(states.filter(s => s.user?.id !== currentUser.id));
    });
  }, []);

  return collaborators;
}
```

**Features:**
- [ ] Real-time cursor positions
- [ ] Avatar presence indicators
- [ ] Simultaneous editing
- [ ] Comments on blocks
- [ ] @mentions with notifications
- [ ] Activity feed
- [ ] Offline mode with sync

---

### 1.5 Version Control

**Description:** Git-like version history with commits, branches, and rollback.

**Features:**
- [ ] Auto-save checkpoints (every 5 min)
- [ ] Manual save with message
- [ ] Version history timeline
- [ ] Visual diff between versions
- [ ] One-click rollback
- [ ] Branch creation
- [ ] Merge with conflict resolution

**Data Model:**

```typescript
interface WorkflowVersion {
  id: string;
  workflowId: string;
  version: number;
  parentVersion: number | null;
  branch: string; // 'main' or custom
  blocks: Block[];
  connections: Connection[];
  message: string;
  createdBy: string;
  createdAt: Date;
}

// Version operations
async function saveVersion(workflowId: string, message: string): Promise<WorkflowVersion>;
async function rollback(workflowId: string, versionId: string): Promise<void>;
async function createBranch(workflowId: string, branchName: string): Promise<string>;
async function mergeBranch(workflowId: string, fromBranch: string, toBranch: string): Promise<void>;
```

---

### 1.6 Template Library

**Description:** Pre-built workflows for common content types.

**V1 Templates:**

| Template | Blocks | Use Case |
|----------|--------|----------|
| **YouTube Script** | Research → Outline → Hook → Script → CTA | Video creators |
| **SEO Blog Post** | Keywords → Research → Outline → Write → SEO | Bloggers |
| **Twitter Thread** | Topic → Points → Expand → Format | Social marketers |
| **LinkedIn Post** | Topic → Hook → Story → CTA | B2B marketers |
| **Newsletter** | Curate → Summarize → Format → Subject | Newsletter writers |
| **Product Description** | Features → Benefits → Copy → SEO | E-commerce |
| **Email Sequence** | Goal → Emails × 5 → Subject Lines | Email marketers |
| **Podcast Show Notes** | Transcript → Summary → Timestamps → SEO | Podcasters |

---

## Tier 2: Growth Features (V1.2+)

### 2.1 Skills Marketplace

**Description:** Community-driven marketplace for sharing and monetizing custom blocks and workflows.

**Seller Features:**
- [ ] Publish skills with documentation
- [ ] Set pricing (free, one-time, subscription)
- [ ] Analytics dashboard
- [ ] Version management
- [ ] Reviews and ratings

**Buyer Features:**
- [ ] Browse/search skills
- [ ] Preview before purchase
- [ ] One-click install
- [ ] Reviews and ratings
- [ ] Support requests

**Revenue Model:**
- 70/30 split (creator/platform)
- Premium placement fees
- Subscription bundles

### 2.2 API Access

**Description:** REST and GraphQL API for programmatic access.

**Endpoints:**

```
POST   /api/v1/workflows              Create workflow
GET    /api/v1/workflows              List workflows
GET    /api/v1/workflows/:id          Get workflow
PUT    /api/v1/workflows/:id          Update workflow
DELETE /api/v1/workflows/:id          Delete workflow
POST   /api/v1/workflows/:id/execute  Execute workflow
GET    /api/v1/executions/:id         Get execution result
GET    /api/v1/usage                  Get usage stats
```

**Authentication:**
- API keys (revocable)
- OAuth 2.0 for third-party apps
- Rate limiting per plan

### 2.3 Integrations

**Priority Integrations:**

| Integration | Type | Use Case |
|-------------|------|----------|
| **Notion** | Export | Save outputs to Notion |
| **Airtable** | Sync | Database for content calendar |
| **Google Docs** | Export | Collaborative editing |
| **Zapier** | Trigger | Automate workflows |
| **Slack** | Notify | Team notifications |
| **WordPress** | Publish | Direct publishing |
| **HubSpot** | CRM | Lead tracking |

### 2.4 Advanced Analytics

**Metrics:**
- [ ] Workflow performance scores
- [ ] AI provider comparison
- [ ] Cost optimization suggestions
- [ ] Team productivity metrics
- [ ] Content performance tracking
- [ ] A/B test results

---

## Tier 3: Enterprise & Advanced (V2+)

### 3.1 Autonomous Agents

**Description:** AI agents that can execute complex tasks autonomously.

**Agent Types:**

| Agent | Function |
|-------|----------|
| **Research Agent** | Continuous web monitoring for topics |
| **Content Agent** | Scheduled content generation |
| **Optimization Agent** | Automatic content improvement |
| **Publishing Agent** | Cross-platform distribution |
| **Analytics Agent** | Performance monitoring and alerts |

### 3.2 Brand Voice Training

**Description:** Fine-tune AI outputs to match brand guidelines.

**Features:**
- [ ] Brand voice analyzer
- [ ] Style guide integration
- [ ] Custom vocabulary
- [ ] Tone calibration
- [ ] Consistency scoring

### 3.3 Enterprise Features

**Security:**
- [ ] SSO (SAML, OIDC)
- [ ] Role-based access control
- [ ] Audit logs
- [ ] Data residency options
- [ ] SOC 2 compliance

**Management:**
- [ ] Admin dashboard
- [ ] User provisioning
- [ ] Usage policies
- [ ] Cost allocation
- [ ] Billing management

---

## Part 2: Technical Stack

### Stack Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    RESONANT.AI STACK                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  FRONTEND                    BACKEND                         │
│  ════════                    ═══════                        │
│  Next.js 16                  Convex                         │
│  React 19                    Real-time DB                   │
│  React Flow                  Serverless Functions           │
│  Tailwind CSS 4                                             │
│  shadcn/ui                   AI LAYER                       │
│  Framer Motion               ═════════                      │
│  Yjs (CRDT)                  Vercel AI SDK                  │
│                              Anthropic SDK                   │
│  STATE                       OpenAI SDK                      │
│  ═════                       Google AI SDK                   │
│  Zustand                                                     │
│  React Query                 SERVICES                        │
│  Convex Client               ════════                       │
│                              Clerk (Auth)                    │
│  TOOLING                     Stripe (Payments)              │
│  ═══════                     Resend (Email)                 │
│  TypeScript 5.x              PostHog (Analytics)            │
│  Biome (Lint/Format)         Sentry (Monitoring)            │
│  Vitest (Testing)                                           │
│  Playwright (E2E)                                           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

### Frontend Stack

#### Next.js 16

**Why:** Latest App Router, React Server Components, Turbopack, excellent DX.

**Configuration:**

```typescript
// next.config.ts
import type { NextConfig } from 'next';

const config: NextConfig = {
  experimental: {
    turbo: {},
    reactCompiler: true,
  },
  images: {
    remotePatterns: [
      { hostname: '*.convex.cloud' },
      { hostname: '*.clerk.dev' },
    ],
  },
};

export default config;
```

**Key Features Used:**
- App Router with layouts
- React Server Components
- Server Actions
- Streaming with Suspense
- Image optimization
- Metadata API

#### React Flow

**Why:** Best-in-class library for node-based UIs, active development, great docs.

**Custom Node Types:**

```typescript
// components/workflow/nodes/AIBlockNode.tsx
import { Handle, Position, NodeProps } from 'reactflow';

export function AIBlockNode({ data, selected }: NodeProps<AIBlockData>) {
  return (
    <div className={cn(
      "rounded-lg border bg-card p-4 shadow-sm",
      selected && "ring-2 ring-primary"
    )}>
      <Handle type="target" position={Position.Top} />

      <div className="flex items-center gap-2">
        <BlockIcon type={data.blockType} />
        <span className="font-medium">{data.label}</span>
      </div>

      <div className="mt-2 text-sm text-muted-foreground">
        {data.description}
      </div>

      <Handle type="source" position={Position.Bottom} />
    </div>
  );
}
```

#### UI Components (shadcn/ui)

**Components Used:**

```bash
# Core
button card input label form toast dialog dropdown-menu

# Layout
sidebar sheet separator scroll-area tabs

# Data
table badge avatar skeleton

# Workflow specific
tooltip popover command
```

#### Yjs (Real-time Collaboration)

**Setup:**

```typescript
// lib/collaboration.ts
import * as Y from 'yjs';
import { WebsocketProvider } from 'y-websocket';
import { bind } from 'valtio-yjs';
import { store } from './store';

export function initCollaboration(workflowId: string) {
  const ydoc = new Y.Doc();

  const provider = new WebsocketProvider(
    process.env.NEXT_PUBLIC_YJS_URL!,
    `workflow:${workflowId}`,
    ydoc,
    { connect: true }
  );

  // Bind Yjs to Zustand/Valtio store
  const yBlocks = ydoc.getArray('blocks');
  bind(store.workflow.blocks, yBlocks);

  return { ydoc, provider };
}
```

---

### Backend Stack

#### Convex

**Why:** Real-time by default, serverless, excellent TypeScript support, built-in file storage.

**Schema:**

```typescript
// convex/schema.ts
import { defineSchema, defineTable } from 'convex/server';
import { v } from 'convex/values';

export default defineSchema({
  users: defineTable({
    clerkId: v.string(),
    email: v.string(),
    name: v.optional(v.string()),
    avatarUrl: v.optional(v.string()),
    plan: v.union(
      v.literal('free'),
      v.literal('creator'),
      v.literal('pro'),
      v.literal('team'),
      v.literal('enterprise')
    ),
    stripeCustomerId: v.optional(v.string()),
    tokenBalance: v.number(),
    createdAt: v.number(),
  })
    .index('by_clerkId', ['clerkId'])
    .index('by_email', ['email']),

  workspaces: defineTable({
    name: v.string(),
    ownerId: v.id('users'),
    plan: v.string(),
    settings: v.optional(v.any()),
    createdAt: v.number(),
  })
    .index('by_owner', ['ownerId']),

  workflows: defineTable({
    workspaceId: v.id('workspaces'),
    name: v.string(),
    description: v.optional(v.string()),
    blocks: v.array(v.any()),
    connections: v.array(v.any()),
    isTemplate: v.boolean(),
    isPublic: v.boolean(),
    createdBy: v.id('users'),
    createdAt: v.number(),
    updatedAt: v.number(),
  })
    .index('by_workspace', ['workspaceId'])
    .index('by_templates', ['isTemplate', 'isPublic']),

  executions: defineTable({
    workflowId: v.id('workflows'),
    userId: v.id('users'),
    status: v.union(
      v.literal('pending'),
      v.literal('running'),
      v.literal('completed'),
      v.literal('failed')
    ),
    inputs: v.any(),
    outputs: v.optional(v.any()),
    blockResults: v.array(v.any()),
    totalTokens: v.number(),
    totalCost: v.number(),
    startedAt: v.number(),
    completedAt: v.optional(v.number()),
  })
    .index('by_workflow', ['workflowId'])
    .index('by_user', ['userId']),
});
```

**Functions:**

```typescript
// convex/workflows.ts
import { mutation, query, action } from './_generated/server';
import { v } from 'convex/values';

export const create = mutation({
  args: {
    workspaceId: v.id('workspaces'),
    name: v.string(),
    templateId: v.optional(v.id('workflows')),
  },
  handler: async (ctx, args) => {
    const identity = await ctx.auth.getUserIdentity();
    if (!identity) throw new Error('Unauthorized');

    const user = await ctx.db
      .query('users')
      .withIndex('by_clerkId', (q) => q.eq('clerkId', identity.subject))
      .unique();

    let blocks = [];
    let connections = [];

    if (args.templateId) {
      const template = await ctx.db.get(args.templateId);
      if (template) {
        blocks = template.blocks;
        connections = template.connections;
      }
    }

    return await ctx.db.insert('workflows', {
      workspaceId: args.workspaceId,
      name: args.name,
      blocks,
      connections,
      isTemplate: false,
      isPublic: false,
      createdBy: user!._id,
      createdAt: Date.now(),
      updatedAt: Date.now(),
    });
  },
});

export const execute = action({
  args: {
    workflowId: v.id('workflows'),
    inputs: v.any(),
  },
  handler: async (ctx, args) => {
    // 1. Get workflow
    const workflow = await ctx.runQuery(internal.workflows.get, {
      id: args.workflowId,
    });

    // 2. Create execution record
    const executionId = await ctx.runMutation(internal.executions.create, {
      workflowId: args.workflowId,
      inputs: args.inputs,
    });

    // 3. Execute blocks in topological order
    const results = await executeBlocks(workflow.blocks, workflow.connections, args.inputs);

    // 4. Update execution with results
    await ctx.runMutation(internal.executions.complete, {
      executionId,
      results,
    });

    return { executionId, results };
  },
});
```

---

### AI Layer

#### Vercel AI SDK

**Why:** Unified interface for multiple providers, streaming support, excellent TypeScript types.

**Configuration:**

```typescript
// lib/ai/providers.ts
import { createAnthropic } from '@ai-sdk/anthropic';
import { createOpenAI } from '@ai-sdk/openai';
import { createGoogleGenerativeAI } from '@ai-sdk/google';

export const anthropic = createAnthropic({
  apiKey: process.env.ANTHROPIC_API_KEY,
});

export const openai = createOpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

export const google = createGoogleGenerativeAI({
  apiKey: process.env.GOOGLE_AI_API_KEY,
});

export const models = {
  claude: anthropic('claude-3-5-sonnet-20241022'),
  gpt4: openai('gpt-4-turbo'),
  gemini: google('gemini-1.5-pro'),
};
```

**Usage:**

```typescript
// lib/ai/router.ts
import { generateText, streamText } from 'ai';
import { models } from './providers';

interface TaskRequest {
  type: 'research' | 'write' | 'summarize' | 'translate';
  prompt: string;
  options?: {
    forceProvider?: keyof typeof models;
    maxTokens?: number;
    temperature?: number;
  };
}

export async function routeAndExecute(task: TaskRequest) {
  const provider = task.options?.forceProvider || selectProvider(task.type);
  const model = models[provider];

  const result = await generateText({
    model,
    prompt: task.prompt,
    maxTokens: task.options?.maxTokens || 4096,
    temperature: task.options?.temperature || 0.7,
  });

  return {
    text: result.text,
    provider,
    usage: result.usage,
  };
}

function selectProvider(taskType: string): keyof typeof models {
  const routing: Record<string, keyof typeof models> = {
    research: 'gemini',      // Best for research, long context
    write: 'claude',         // Best for nuanced writing
    summarize: 'gpt4',       // Good balance
    translate: 'gpt4',       // Good multilingual support
  };
  return routing[taskType] || 'claude';
}
```

---

### External Services

#### Clerk (Authentication)

```typescript
// middleware.ts
import { clerkMiddleware, createRouteMatcher } from '@clerk/nextjs/server';

const isPublicRoute = createRouteMatcher([
  '/',
  '/sign-in(.*)',
  '/sign-up(.*)',
  '/api/webhooks(.*)',
]);

export default clerkMiddleware(async (auth, req) => {
  if (!isPublicRoute(req)) {
    await auth.protect();
  }
});
```

#### Stripe (Payments)

```typescript
// convex/stripe.ts
import Stripe from 'stripe';

const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!);

export const createCheckoutSession = action({
  args: { priceId: v.string() },
  handler: async (ctx, args) => {
    const identity = await ctx.auth.getUserIdentity();
    const user = await getUser(ctx, identity!.subject);

    const session = await stripe.checkout.sessions.create({
      customer: user.stripeCustomerId,
      line_items: [{ price: args.priceId, quantity: 1 }],
      mode: 'subscription',
      success_url: `${process.env.APP_URL}/dashboard?success=true`,
      cancel_url: `${process.env.APP_URL}/pricing?canceled=true`,
    });

    return session.url;
  },
});
```

#### Resend (Email)

```typescript
// lib/email.ts
import { Resend } from 'resend';

const resend = new Resend(process.env.RESEND_API_KEY);

export async function sendWelcomeEmail(email: string, name: string) {
  await resend.emails.send({
    from: 'Resonant.ai <hello@resonant.ai>',
    to: email,
    subject: 'Welcome to Resonant.ai!',
    react: WelcomeEmailTemplate({ name }),
  });
}
```

---

### Development Tools

```json
// package.json (key dependencies)
{
  "dependencies": {
    "next": "^16.0.0",
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "reactflow": "^11.11.0",
    "@clerk/nextjs": "^6.0.0",
    "convex": "^1.17.0",
    "ai": "^4.0.0",
    "@ai-sdk/anthropic": "^1.0.0",
    "@ai-sdk/openai": "^1.0.0",
    "@ai-sdk/google": "^1.0.0",
    "stripe": "^17.0.0",
    "yjs": "^13.6.0",
    "y-websocket": "^2.0.0",
    "zustand": "^5.0.0",
    "zod": "^3.23.0",
    "resend": "^4.0.0"
  },
  "devDependencies": {
    "typescript": "^5.7.0",
    "@biomejs/biome": "^1.9.0",
    "vitest": "^2.1.0",
    "@playwright/test": "^1.49.0"
  }
}
```

---

### Infrastructure

```
┌─────────────────────────────────────────────────────────────┐
│                    DEPLOYMENT ARCHITECTURE                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────┐          ┌─────────────┐                   │
│  │   Vercel    │          │   Convex    │                   │
│  │  (Frontend) │ ◀──────▶ │  (Backend)  │                   │
│  │             │          │             │                   │
│  │  - Next.js  │          │  - Database │                   │
│  │  - Edge     │          │  - Functions│                   │
│  │  - CDN      │          │  - Files    │                   │
│  └─────────────┘          └─────────────┘                   │
│         │                        │                          │
│         │                        │                          │
│         ▼                        ▼                          │
│  ┌─────────────┐          ┌─────────────┐                   │
│  │    Clerk    │          │   AI APIs   │                   │
│  │   (Auth)    │          │             │                   │
│  └─────────────┘          │  - Claude   │                   │
│                           │  - GPT-4    │                   │
│  ┌─────────────┐          │  - Gemini   │                   │
│  │   Stripe    │          └─────────────┘                   │
│  │ (Payments)  │                                            │
│  └─────────────┘          ┌─────────────┐                   │
│                           │    Yjs      │                   │
│  ┌─────────────┐          │  (Collab)   │                   │
│  │   Resend    │          └─────────────┘                   │
│  │  (Email)    │                                            │
│  └─────────────┘                                            │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

*Document maintained by Engineering Team. Last updated: January 2026*
