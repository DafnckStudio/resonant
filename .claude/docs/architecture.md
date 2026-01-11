# Resonant.ai - Architecture Document

> Document technique détaillant l'architecture de la plateforme

---

## High-Level Architecture

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                              CLIENT (Next.js 16)                              │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐              │
│  │   Landing       │  │   Dashboard     │  │   Editor        │              │
│  │   (Marketing)   │  │   (App Shell)   │  │   (React Flow)  │              │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘              │
│                                                                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐              │
│  │   Auth (Clerk)  │  │   State (Yjs)   │  │   AI Chat       │              │
│  │   Components    │  │   Real-time     │  │   Interface     │              │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘              │
│                                                                              │
├──────────────────────────────────────────────────────────────────────────────┤
│                              API LAYER                                        │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐              │
│  │   Server        │  │   API Routes    │  │   Webhooks      │              │
│  │   Actions       │  │   (REST)        │  │   (Clerk/Stripe)│              │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘              │
│                                                                              │
├──────────────────────────────────────────────────────────────────────────────┤
│                              BACKEND (Convex)                                 │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐              │
│  │   Queries       │  │   Mutations     │  │   Actions       │              │
│  │   (Read)        │  │   (Write)       │  │   (External)    │              │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘              │
│                                                                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐              │
│  │   Scheduled     │  │   HTTP Actions  │  │   Real-time     │              │
│  │   Jobs          │  │   (Webhooks)    │  │   Subscriptions │              │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘              │
│                                                                              │
├──────────────────────────────────────────────────────────────────────────────┤
│                              EXTERNAL SERVICES                                │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐              │
│  │   Clerk         │  │   Stripe        │  │   AI Providers  │              │
│  │   (Auth)        │  │   (Payments)    │  │   (LLM APIs)    │              │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘              │
│                                                                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐              │
│  │   Resend        │  │   Upstash       │  │   Vercel        │              │
│  │   (Email)       │  │   (Rate Limit)  │  │   (Hosting)     │              │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘              │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## Data Flow Architecture

### Workflow Execution Flow

```
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│  User   │───▶│ Editor  │───▶│ Convex  │───▶│ Router  │───▶│   AI    │
│ Action  │    │  (UI)   │    │(Backend)│    │ (Smart) │    │Provider │
└─────────┘    └─────────┘    └─────────┘    └─────────┘    └─────────┘
                                  │              │              │
                                  │              ▼              │
                                  │         ┌─────────┐        │
                                  │         │ Select  │        │
                                  │         │ Best AI │        │
                                  │         └─────────┘        │
                                  │              │              │
                                  ▼              │              ▼
                             ┌─────────┐        │         ┌─────────┐
                             │  Save   │◀───────┴─────────│Response │
                             │ Result  │                  │         │
                             └─────────┘                  └─────────┘
                                  │
                                  ▼
                             ┌─────────┐
                             │Real-time│
                             │  Sync   │
                             └─────────┘
```

### Real-time Collaboration Flow

```
┌──────────┐                              ┌──────────┐
│  User A  │                              │  User B  │
└────┬─────┘                              └────┬─────┘
     │                                         │
     │  Edit Block                             │
     │     │                                   │
     ▼     ▼                                   │
┌──────────────┐                               │
│   Yjs Doc    │◀──────────────────────────────┘
│   (CRDT)     │
└──────┬───────┘
       │
       │  Broadcast Changes
       │
       ▼
┌──────────────┐      ┌──────────────┐
│   Convex     │─────▶│   All        │
│   Real-time  │      │   Clients    │
└──────────────┘      └──────────────┘
```

---

## Database Schema (Convex)

### Core Tables

```typescript
// convex/schema.ts

import { defineSchema, defineTable } from "convex/server";
import { v } from "convex/values";

export default defineSchema({
  // Users (synced from Clerk)
  users: defineTable({
    clerkId: v.string(),
    email: v.string(),
    name: v.optional(v.string()),
    imageUrl: v.optional(v.string()),
    // Subscription
    stripeCustomerId: v.optional(v.string()),
    subscriptionId: v.optional(v.string()),
    subscriptionStatus: v.optional(v.string()),
    subscriptionPlan: v.optional(v.string()),
    // Usage
    tokensUsed: v.number(),
    tokensLimit: v.number(),
    // Timestamps
    createdAt: v.number(),
    updatedAt: v.number(),
  })
    .index("by_clerkId", ["clerkId"])
    .index("by_email", ["email"])
    .index("by_stripeCustomerId", ["stripeCustomerId"]),

  // Workspaces (for teams)
  workspaces: defineTable({
    name: v.string(),
    slug: v.string(),
    ownerId: v.id("users"),
    plan: v.string(), // free, creator, pro, team, enterprise
    settings: v.optional(v.any()),
    createdAt: v.number(),
    updatedAt: v.number(),
  })
    .index("by_ownerId", ["ownerId"])
    .index("by_slug", ["slug"]),

  // Workspace members
  workspaceMembers: defineTable({
    workspaceId: v.id("workspaces"),
    userId: v.id("users"),
    role: v.string(), // owner, admin, member, viewer
    joinedAt: v.number(),
  })
    .index("by_workspaceId", ["workspaceId"])
    .index("by_userId", ["userId"])
    .index("by_workspace_user", ["workspaceId", "userId"]),

  // Workflows
  workflows: defineTable({
    workspaceId: v.id("workspaces"),
    createdBy: v.id("users"),
    name: v.string(),
    description: v.optional(v.string()),
    // React Flow data
    blocks: v.array(v.any()), // Node[]
    connections: v.array(v.any()), // Edge[]
    viewport: v.optional(v.any()), // Viewport
    // Metadata
    isTemplate: v.boolean(),
    isPublic: v.boolean(),
    status: v.string(), // draft, active, archived
    // Versioning
    version: v.number(),
    parentVersionId: v.optional(v.id("workflowVersions")),
    // Stats
    executionCount: v.number(),
    lastExecutedAt: v.optional(v.number()),
    // Timestamps
    createdAt: v.number(),
    updatedAt: v.number(),
  })
    .index("by_workspaceId", ["workspaceId"])
    .index("by_createdBy", ["createdBy"])
    .index("by_workspace_status", ["workspaceId", "status"])
    .index("by_isTemplate", ["isTemplate"])
    .index("by_isPublic", ["isPublic"]),

  // Workflow versions (for git-like history)
  workflowVersions: defineTable({
    workflowId: v.id("workflows"),
    version: v.number(),
    blocks: v.array(v.any()),
    connections: v.array(v.any()),
    message: v.optional(v.string()),
    createdBy: v.id("users"),
    createdAt: v.number(),
  })
    .index("by_workflowId", ["workflowId"])
    .index("by_workflow_version", ["workflowId", "version"]),

  // Workflow executions
  executions: defineTable({
    workflowId: v.id("workflows"),
    workspaceId: v.id("workspaces"),
    userId: v.id("users"),
    // Status
    status: v.string(), // pending, running, completed, failed, cancelled
    // Input/Output
    input: v.any(),
    output: v.optional(v.any()),
    // Execution details
    blockResults: v.array(v.any()),
    currentBlockId: v.optional(v.string()),
    // Cost tracking
    tokensUsed: v.number(),
    costUsd: v.number(),
    // Timing
    startedAt: v.number(),
    completedAt: v.optional(v.number()),
    durationMs: v.optional(v.number()),
    // Error
    error: v.optional(v.string()),
  })
    .index("by_workflowId", ["workflowId"])
    .index("by_workspaceId", ["workspaceId"])
    .index("by_userId", ["userId"])
    .index("by_status", ["status"]),

  // Block templates (marketplace)
  blockTemplates: defineTable({
    createdBy: v.id("users"),
    name: v.string(),
    description: v.string(),
    category: v.string(),
    // Block data
    type: v.string(), // input, ai, output, condition, transform
    config: v.any(),
    // Marketplace
    isPublic: v.boolean(),
    downloads: v.number(),
    rating: v.optional(v.number()),
    // Timestamps
    createdAt: v.number(),
    updatedAt: v.number(),
  })
    .index("by_createdBy", ["createdBy"])
    .index("by_category", ["category"])
    .index("by_isPublic", ["isPublic"])
    .index("by_downloads", ["downloads"]),

  // Comments (collaboration)
  comments: defineTable({
    workflowId: v.id("workflows"),
    userId: v.id("users"),
    blockId: v.optional(v.string()),
    content: v.string(),
    // Thread
    parentId: v.optional(v.id("comments")),
    // Status
    resolved: v.boolean(),
    // Timestamps
    createdAt: v.number(),
    updatedAt: v.number(),
  })
    .index("by_workflowId", ["workflowId"])
    .index("by_blockId", ["blockId"])
    .index("by_parentId", ["parentId"]),

  // AI Provider configs
  aiProviderConfigs: defineTable({
    workspaceId: v.id("workspaces"),
    provider: v.string(), // claude, openai, gemini, llama
    apiKey: v.optional(v.string()), // Encrypted, for BYOK
    isEnabled: v.boolean(),
    priority: v.number(),
    maxTokensPerRequest: v.optional(v.number()),
    createdAt: v.number(),
    updatedAt: v.number(),
  })
    .index("by_workspaceId", ["workspaceId"])
    .index("by_workspace_provider", ["workspaceId", "provider"]),

  // Usage tracking
  usageRecords: defineTable({
    workspaceId: v.id("workspaces"),
    userId: v.id("users"),
    executionId: v.id("executions"),
    provider: v.string(),
    model: v.string(),
    inputTokens: v.number(),
    outputTokens: v.number(),
    totalTokens: v.number(),
    costUsd: v.number(),
    createdAt: v.number(),
  })
    .index("by_workspaceId", ["workspaceId"])
    .index("by_userId", ["userId"])
    .index("by_workspace_date", ["workspaceId", "createdAt"]),
});
```

---

## AI Router Architecture

### Provider Selection Logic

```typescript
// lib/ai/router.ts

interface ProviderConfig {
  id: string;
  name: string;
  costPer1MTokens: number;
  maxContextTokens: number;
  strengths: string[];
  latencyMs: number;
  reliability: number; // 0-1
}

const providers: Record<string, ProviderConfig> = {
  'claude-3-5-sonnet': {
    id: 'claude-3-5-sonnet',
    name: 'Claude 3.5 Sonnet',
    costPer1MTokens: 15,
    maxContextTokens: 200000,
    strengths: ['nuance', 'long-form', 'analysis', 'coding'],
    latencyMs: 800,
    reliability: 0.99,
  },
  'gpt-4-turbo': {
    id: 'gpt-4-turbo',
    name: 'GPT-4 Turbo',
    costPer1MTokens: 30,
    maxContextTokens: 128000,
    strengths: ['creative', 'brainstorm', 'general'],
    latencyMs: 1200,
    reliability: 0.98,
  },
  'gemini-1.5-pro': {
    id: 'gemini-1.5-pro',
    name: 'Gemini 1.5 Pro',
    costPer1MTokens: 7,
    maxContextTokens: 1000000,
    strengths: ['long-context', 'research', 'multimodal'],
    latencyMs: 600,
    reliability: 0.97,
  },
  'llama-3-70b': {
    id: 'llama-3-70b',
    name: 'Llama 3 70B',
    costPer1MTokens: 2,
    maxContextTokens: 8192,
    strengths: ['bulk', 'simple', 'cost-effective'],
    latencyMs: 400,
    reliability: 0.95,
  },
};

interface RoutingContext {
  taskType: string;
  estimatedTokens: number;
  priority: 'speed' | 'quality' | 'cost';
  requiredStrengths?: string[];
  userPreference?: string;
}

export function selectProvider(context: RoutingContext): string {
  const { taskType, estimatedTokens, priority, requiredStrengths } = context;

  // Filter by context length requirement
  const viable = Object.values(providers).filter(
    p => p.maxContextTokens >= estimatedTokens
  );

  // Filter by required strengths
  const matching = requiredStrengths
    ? viable.filter(p =>
        requiredStrengths.some(s => p.strengths.includes(s))
      )
    : viable;

  // Sort by priority
  switch (priority) {
    case 'speed':
      return matching.sort((a, b) => a.latencyMs - b.latencyMs)[0].id;
    case 'cost':
      return matching.sort((a, b) => a.costPer1MTokens - b.costPer1MTokens)[0].id;
    case 'quality':
    default:
      // Quality = reliability * inverse_cost * strength_match
      return matching.sort((a, b) => {
        const scoreA = a.reliability * (1 / a.costPer1MTokens);
        const scoreB = b.reliability * (1 / b.costPer1MTokens);
        return scoreB - scoreA;
      })[0].id;
  }
}
```

### Fallback Chain

```typescript
// lib/ai/fallback.ts

const fallbackChain: Record<string, string[]> = {
  'claude-3-5-sonnet': ['gpt-4-turbo', 'gemini-1.5-pro', 'llama-3-70b'],
  'gpt-4-turbo': ['claude-3-5-sonnet', 'gemini-1.5-pro', 'llama-3-70b'],
  'gemini-1.5-pro': ['claude-3-5-sonnet', 'gpt-4-turbo', 'llama-3-70b'],
  'llama-3-70b': ['gemini-1.5-pro', 'claude-3-5-sonnet', 'gpt-4-turbo'],
};

export async function executeWithFallback(
  prompt: string,
  preferredProvider: string,
  maxRetries = 3
): Promise<AIResponse> {
  const chain = [preferredProvider, ...fallbackChain[preferredProvider]];

  for (let i = 0; i < Math.min(maxRetries, chain.length); i++) {
    try {
      const provider = chain[i];
      const response = await callProvider(provider, prompt);
      return { ...response, actualProvider: provider };
    } catch (error) {
      console.error(`Provider ${chain[i]} failed:`, error);
      continue;
    }
  }

  throw new Error('All providers failed');
}
```

---

## Block System Architecture

### Block Types

```typescript
// types/blocks.ts

type BlockType =
  | 'input'      // User input, file upload, API fetch
  | 'ai'         // AI generation
  | 'output'     // Display, export, publish
  | 'condition'  // If/else, switch
  | 'transform'  // Text manipulation, format
  | 'loop'       // Iterate over data
  | 'merge'      // Combine multiple inputs
  | 'split'      // Split data into branches
  | 'human'      // Human review/approval
  | 'webhook'    // External API call
  | 'delay'      // Wait/schedule

interface Block {
  id: string;
  type: BlockType;
  position: { x: number; y: number };
  data: BlockData;
  config: BlockConfig;
}

interface BlockData {
  label: string;
  description?: string;
  icon?: string;
  color?: string;
}

interface BlockConfig {
  // Type-specific config
  [key: string]: any;
}

// Example: AI Block config
interface AIBlockConfig {
  provider?: string; // Override default routing
  model?: string;
  systemPrompt: string;
  userPromptTemplate: string;
  temperature: number;
  maxTokens: number;
  outputFormat: 'text' | 'json' | 'markdown';
}
```

### Execution Engine

```typescript
// lib/execution/engine.ts

interface ExecutionContext {
  workflowId: string;
  executionId: string;
  userId: string;
  variables: Record<string, any>;
  blockResults: Record<string, any>;
}

export async function executeWorkflow(
  workflow: Workflow,
  input: Record<string, any>,
  ctx: ExecutionContext
): Promise<ExecutionResult> {
  const { blocks, connections } = workflow;

  // Build execution graph
  const graph = buildGraph(blocks, connections);

  // Topological sort for execution order
  const executionOrder = topologicalSort(graph);

  // Execute blocks in order
  for (const blockId of executionOrder) {
    const block = blocks.find(b => b.id === blockId);
    if (!block) continue;

    // Update status
    await updateExecutionStatus(ctx.executionId, {
      currentBlockId: blockId,
      status: 'running',
    });

    // Get inputs from connected blocks
    const inputs = getBlockInputs(blockId, connections, ctx.blockResults);

    // Execute block
    try {
      const result = await executeBlock(block, inputs, ctx);
      ctx.blockResults[blockId] = result;
    } catch (error) {
      // Handle error, potentially continue or abort
      return handleBlockError(block, error, ctx);
    }
  }

  // Get final outputs
  const outputBlocks = blocks.filter(b => b.type === 'output');
  const outputs = outputBlocks.map(b => ctx.blockResults[b.id]);

  return {
    status: 'completed',
    outputs,
    blockResults: ctx.blockResults,
    tokensUsed: calculateTotalTokens(ctx),
  };
}
```

---

## Collaboration Architecture (Yjs)

### Document Structure

```typescript
// lib/collaboration/yjs.ts

import * as Y from 'yjs';
import { WebsocketProvider } from 'y-websocket';

export function initCollaboration(workflowId: string) {
  // Create Yjs document
  const ydoc = new Y.Doc();

  // Define shared types
  const yBlocks = ydoc.getArray<Y.Map<any>>('blocks');
  const yConnections = ydoc.getArray<Y.Map<any>>('connections');
  const yViewport = ydoc.getMap('viewport');
  const yMeta = ydoc.getMap('meta');
  const yPresence = ydoc.getMap('presence');

  // Connect to Convex sync server
  const provider = new WebsocketProvider(
    process.env.NEXT_PUBLIC_CONVEX_WS_URL!,
    `workflow-${workflowId}`,
    ydoc
  );

  // Handle awareness (cursors, selections)
  provider.awareness.setLocalState({
    user: getCurrentUser(),
    cursor: null,
    selection: null,
  });

  return {
    ydoc,
    yBlocks,
    yConnections,
    yViewport,
    yMeta,
    yPresence,
    provider,
    awareness: provider.awareness,
  };
}

// React hook for collaboration
export function useCollaboration(workflowId: string) {
  const [collab, setCollab] = useState<CollaborationState | null>(null);

  useEffect(() => {
    const state = initCollaboration(workflowId);
    setCollab(state);

    return () => {
      state.provider.destroy();
      state.ydoc.destroy();
    };
  }, [workflowId]);

  return collab;
}
```

### Presence & Cursors

```typescript
// components/collaboration/Cursors.tsx

export function CollaborativeCursors() {
  const { awareness } = useCollaboration();
  const [cursors, setCursors] = useState<Map<number, CursorState>>(new Map());

  useEffect(() => {
    const handleChange = () => {
      const states = new Map();
      awareness.getStates().forEach((state, clientId) => {
        if (clientId !== awareness.clientID && state.cursor) {
          states.set(clientId, state);
        }
      });
      setCursors(states);
    };

    awareness.on('change', handleChange);
    return () => awareness.off('change', handleChange);
  }, [awareness]);

  return (
    <>
      {Array.from(cursors.entries()).map(([clientId, state]) => (
        <Cursor
          key={clientId}
          x={state.cursor.x}
          y={state.cursor.y}
          color={state.user.color}
          name={state.user.name}
        />
      ))}
    </>
  );
}
```

---

## Security Architecture

### Authentication Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  Client  │────▶│  Clerk   │────▶│  JWT     │────▶│  Convex  │
│  (UI)    │     │  Auth    │     │  Token   │     │  Backend │
└──────────┘     └──────────┘     └──────────┘     └──────────┘
                      │
                      ▼
                ┌──────────┐
                │  Webhook │───▶ Sync user to Convex
                │  (User)  │
                └──────────┘
```

### Authorization Matrix

| Resource | Owner | Admin | Member | Viewer |
|----------|-------|-------|--------|--------|
| View workflow | ✅ | ✅ | ✅ | ✅ |
| Edit workflow | ✅ | ✅ | ✅ | ❌ |
| Delete workflow | ✅ | ✅ | ❌ | ❌ |
| Execute workflow | ✅ | ✅ | ✅ | ❌ |
| Manage members | ✅ | ✅ | ❌ | ❌ |
| Billing | ✅ | ❌ | ❌ | ❌ |

### Data Encryption

```typescript
// At rest: Convex handles encryption
// In transit: HTTPS/TLS
// AI API keys: AES-256 encrypted in DB

import { encrypt, decrypt } from '@/lib/crypto';

// Store API key
const encryptedKey = await encrypt(apiKey, process.env.ENCRYPTION_KEY);
await ctx.db.patch(configId, { apiKey: encryptedKey });

// Retrieve API key
const config = await ctx.db.get(configId);
const apiKey = await decrypt(config.apiKey, process.env.ENCRYPTION_KEY);
```

---

## Deployment Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              PRODUCTION                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐                     │
│  │   Vercel    │    │   Convex    │    │   Upstash   │                     │
│  │   Edge      │    │   Cloud     │    │   Redis     │                     │
│  │   Network   │    │   (DB)      │    │   (Limits)  │                     │
│  └─────────────┘    └─────────────┘    └─────────────┘                     │
│         │                 │                  │                              │
│         └────────────────┬┴─────────────────┘                              │
│                          │                                                  │
│                    ┌─────────────┐                                          │
│                    │   Vercel    │                                          │
│                    │   KV/Blob   │                                          │
│                    │   (Cache)   │                                          │
│                    └─────────────┘                                          │
│                                                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                          EXTERNAL SERVICES                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐         │
│  │ Clerk │  │Stripe │  │Claude │  │OpenAI │  │Gemini │  │Resend │         │
│  └───────┘  └───────┘  └───────┘  └───────┘  └───────┘  └───────┘         │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Scaling Considerations

### Horizontal Scaling
- Vercel: Auto-scales edge functions
- Convex: Managed scaling
- AI: Rate limiting + queue for burst

### Caching Strategy
```
┌─────────────┐
│   Browser   │──▶ React Query (client cache)
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Vercel    │──▶ Edge cache (static)
│   Edge      │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Upstash   │──▶ Redis (API results, rate limits)
│   Redis     │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Convex    │──▶ Database (source of truth)
└─────────────┘
```

---

*Document Version: 1.0*
*Last Updated: January 2026*
