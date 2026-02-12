# Workflows & recipes (practical patterns)

These are end-to-end workflows described in a way that works for both humans and AI agents.

---

## Workflow: turn group chat into a PM system
**What it is:** Convert discussions into tasks, owners, and follow-ups.

**When to use:**
- Action items are getting lost in chat.
- A group needs coordination but not heavy process.

**How it behaves (agentic):**
- Captures tasks (paraphrased).
- Asks for missing details.
- Produces a status summary on request.

**Inputs:**
- Group messages (invoked via mention-gating).

**Outputs:**
- A short task list and next steps (placeholders only in the book).

**Guardrails:**
- Opt-in per group.
- Mention-gating to prevent noise.
- Operator approval for actions with side effects.

**Failure modes:**
- Too many tasks with no owners.

**Fix:**
- Require “owner + next step” for every task.

---

## Workflow: propose → approve → execute (safe automation)
**What it is:** A controlled way to change behavior or run impactful actions.

**When to use:**
- Any change request.
- Any action that touches external systems.

**How it behaves (agentic):**
- Proposes a plan in plain language.
- Waits for explicit approval.
- Executes only what was approved.

**Inputs:**
- A requested change or operation.

**Outputs:**
- Plan, decision, and (if approved) execution summary.

**Guardrails:**
- No silent execution.
- If ambiguous, ask a question.

---

## Workflow: low-friction group interaction
**What it is:** Make group usage feel conversational.

**When to use:**
- Teams don’t want to remember commands.

**How it behaves (agentic):**
- Uses reply context to treat replies as invocations.
- Keeps responses scoped to the thread.

**Inputs:**
- A reply to the assistant’s last message.

**Outputs:**
- A response that continues the workflow.

**Guardrails:**
- Keep invocation rules consistent.

---

## Workflow: privacy-safe recap mode
**What it is:** A way to produce summaries that are safe to share.

**When to use:**
- Any time content might leave private chats.

**How it behaves (agentic):**
- Paraphrases only (no quotes).
- Uses placeholders only.
- States missing inputs instead of guessing.

**Outputs:**
- A minimal but safe summary.
