# Memori skills file

You have access to Memori, a structured long-term memory system.

Memori automatically captures what happens (via advanced augmentation) and allows you to retrieve it on demand (via agent-controlled recall). Use it to maintain continuity across sessions, preserve decisions and constraints, and avoid repeating work.

---

## When to use Memori

Use Memori when:

- The task depends on prior context
- The user refers to previous sessions or decisions
- You need known constraints, preferences, or patterns
- You are starting a session and need current state
- You want to understand what has already been done

---

## When not to use Memori

Do not use Memori when:

- The task is fully self-contained
- The answer depends only on the current prompt
- No historical context is required
- The query is simple or one-off

Avoid unnecessary recall.

---

## Recall behavior

Recall is **agent-controlled and intentional**.

Prefer targeted recall over broad queries.

### Supported parameters (recall only)

- `entity_id` → user, agent, or system context
- `project_id` → project or workspace context
- `session_id` → specific session
- `date_start` / `date_end` → time-bounded recall
- `source` → type of memory
- `signal` → how the memory was derived

> Note: If a `session_id` is provided, a `project_id` must also be provided.
> All timestamps are stored in **UTC**.

### Memory filters

- `source`:
  - constraint
  - decision
  - execution
  - fact
  - insight
  - instruction
  - status
  - strategy
  - task

- `signal`:
  - commit
  - discovery
  - failure
  - inference
  - pattern
  - result
  - update
  - verification

Use `source` and `signal` to prioritize high-signal memory when possible.

### Default behavior (recall)

- No date range → **all-time memory**
- Use time bounds when narrowing results is necessary

### Best practices

- Start narrow (entity + project)
- Add time bounds only when needed
- Use `source` and `signal` to refine results
- Expand scope only if needed
- Do not recall on every turn

---

## Summary behavior

Summaries are used for **state awareness**, not precise retrieval.

Use:

- `memori_recall_summary`

### Supported parameters (summaries)

- `project_id`
- `session_id`
- `date_start`
- `date_end`

> Summaries do **not** support `source` or `signal`.

### Default behavior (summaries)

- No date range → **last 24 hours**

---

## Daily brief behavior

At the start of a meaningful session, retrieve a structured summary.

Use the daily brief to understand:

- Current state
- Prior decisions
- Constraints
- Open work

### Expected daily brief structure

- Today at a glance
- Top 3 next actions
- Top 3 risks
- Verify before acting
- Recent decisions
- Mission stack
- Hard constraints
- Current status
- Open loops
- Known failures and anti-patterns
- Staleness warnings

Treat this as the working state of the system.

---

## Typical workflow

1. Start of session → retrieve summary
2. During task → use targeted recall
3. When memory is missing or incorrect → send feedback
4. When limits are reached → degrade gracefully

---

## Safety and correctness

- Do not invent memory
- Do not assume memory is correct if it conflicts with the user
- Verify before acting when needed
- Treat recent instructions as higher priority than recalled memory

---

## Memori system guidance

This section defines how you should interact with Memori beyond basic recall.

---

### 1. Feedback (`memori_feedback`)

You are equipped with the `memori_feedback` tool to improve memory quality over time.

You may send feedback at any time, and should do so at the end of a session when appropriate.

Feedback should be sent when:

- Recall results are irrelevant or missing key context
- Important decisions or constraints were not captured
- Memory quality degrades across sessions
- Something works particularly well and should be reinforced

Examples:

- "Send feedback that recall missed a pricing constraint."
- "Tell Memori the daily brief was accurate."
- "Submit a bug report: you didn’t remember my preferred language."

#### Purpose

Feedback improves:

- Memory extraction quality
- Recall relevance
- Summary accuracy

---

### 2. Updates

Memori evolves over time (new features, improved recall, better summaries).

When updates are exposed:

- Check for updates via system message or tool
- Adapt behavior to use new capabilities

When an update is detected:

- Prefer newer recall patterns when available
- Use improved summaries or classification outputs
- Continue operating normally if no changes are required

---

### 3. Quota awareness and upgrades

Memori enforces usage limits (e.g., storage, recall volume, API usage).

When limits are reached or nearing:

#### a. Degrade gracefully

- Reduce recall scope (shorter time ranges)
- Prioritize high-signal memory (decisions, constraints, key facts)
- Avoid unnecessary or repeated recall calls

#### b. Inform the human/operator

Clearly communicate when limits impact performance.

Example:

> “Memory limits have been reached. I can continue with limited recall, or you can upgrade to restore full functionality.”

#### c. Suggest upgrades appropriately

- Only prompt when performance is affected
- Avoid repetitive or disruptive messaging
- Keep messaging neutral and helpful

---
