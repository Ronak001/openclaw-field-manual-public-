# Reliability & debugging (what breaks in practice and how to fix it)

This chapter is written as a playbook. Each section is a failure mode with a fix.

---

## Failure mode: the wrong agent keeps replying
**What it is:** You changed routing, but an older responder still answers.

**When it happens:**
- After changing who should handle a group.
- After reconfiguring bindings/routing.

**How it behaves (agentic):**
- The system appears to “ignore” your new routing because old state is still eligible.

**Inputs:**
- A group conversation with multiple historical responders.

**Outputs:**
- Confusing, inconsistent replies.

**Guardrails:**
- Treat routing changes as incomplete until old responders are retired.

**Fix:**
- Identify which agent/session is responding.
- Reset/purge the stale session.
- Ensure only the intended agent is eligible to reply.

---

## Failure mode: mention-gating doesn’t trigger
**What it is:** You invoke the assistant, but it doesn’t respond.

**Likely causes:**
- The invocation format is different than you assumed.
- The group requires a specific mention pattern.

**Fix:**
- Reply to a recent assistant message (reply context is often more reliable).
- Use the exact trigger pattern your setup expects.

---

## Failure mode: outbound “mentions” don’t notify owners
**What it is:** The assistant writes something that looks like a mention, but no one is notified.

**Why it happens:** Platforms often require special metadata for a true notification.

**Fix:**
- Treat mentions as best-effort.
- Always include a textual fallback (e.g., “Owner: `<USER>`”).

---

## Failure mode: task list appears empty
**What it is:** The assistant claims there are no tasks, but the group believes there are.

**Likely causes:**
- Inconsistent naming conventions.
- A mismatch in how tasks are stored vs read.

**Fix:**
- Standardize conventions.
- Add lightweight validation checks.

---

## Failure mode: external lookup is blocked
**What it is:** The agent can’t access a public site or search results reliably.

**Why it happens:**
- Some sites block automated traffic.
- Some pages require interactive verification.

**Fix:**
- Switch to alternate sources.
- Ask the operator for a screenshot/paste if needed.

---

## Debugging principle: assume state mismatch
When something looks “haunted,” assume one of:
- stale session
- stale routing
- inconsistent conventions
- missing permission

Then force convergence:
- one clear route
- one responding agent
- one canonical convention
