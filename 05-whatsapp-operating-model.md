# WhatsApp operating model (how to run OpenClaw in the real world)

This chapter is a practical operating model for using OpenClaw heavily across WhatsApp DMs and multiple groups.

## The problem this solves
When an assistant is used across many groups and DMs, the default failure mode is chaos:
- the wrong agent responds in the wrong place
- the operator’s DM becomes noisy
- groups want autonomy, but you still need safety and control
- multi-step workflows fall apart if context resets too aggressively

---

## Pattern: the front door (routing)
**What it is:** A single triage entry-point that decides “who should handle this.”

**When to use:**
- You have many inbound conversations.
- You want predictable behavior across DMs and groups.

**How it behaves (agentic):**
- Asks clarifying questions.
- Classifies the request (info, task, change request, escalation).
- Routes to the right specialist agent.

**Inputs:**
- The inbound message + basic context (DM vs group).

**Outputs:**
- A clarified request, or a handoff to another agent.

**Guardrails:**
- Prefer limited tool access on the front door.
- If the request could cause side effects, switch to propose → approve → execute.

**Failure modes:**
- Everything gets routed to the operator DM.

**Fix:**
- Keep the operator DM as a control channel; route most traffic elsewhere.

---

## Pattern: operator control channel (keep it clean)
**What it is:** A dedicated DM used as a command center for high-impact decisions.

**When to use:**
- You need a place for approvals, configuration changes, and sensitive work.

**How it behaves (agentic):**
- Treats this channel as the authority for approvals.
- Summarizes proposals and waits for explicit confirmation.

**Inputs:**
- Plans/proposals from other agents.

**Outputs:**
- Approvals, rejections, constraints, and final decisions.

**Guardrails:**
- Do not allow random group chatter to flood this channel.

---

## Pattern: group PM agent (opt-in per group)
**What it is:** A PM-like agent dedicated to a specific group.

**When to use:**
- The group needs task capture, follow-ups, summaries, and coordination.

**How it behaves (agentic):**
- Converts chat into tasks.
- Asks for missing details (owner, deadline, success criteria).
- Follows up and produces status summaries.

**Inputs:**
- Group messages (invoked via mention-gating).

**Outputs:**
- Task lists, next steps, weekly/daily summaries (all paraphrased in the book).

**Guardrails:**
- Enable per group (not global).
- Keep tool access aligned to PM work.

**Failure modes:**
- The agent spams the group.

**Fix:**
- Require mention-gating + reply-context invocation.

---

## Pattern: mention-gating + reply context
**What it is:** The assistant responds only when invoked.

**When to use:**
- Any busy group where constant bot replies would be disruptive.

**How it behaves (agentic):**
- Ignores non-invocations.
- Responds when the call sign is present, or when a user replies to the assistant’s last message.

**Inputs:**
- Group message + invocation signal.

**Outputs:**
- A response that stays within the requested scope.

**Guardrails:**
- Keep invocation rules simple and consistent.

---

## Pattern: propose → approve → execute
**What it is:** A safety gate that separates planning from action.

**When to use:**
- The request changes agent behavior, touches external systems, or has side effects.

**How it behaves (agentic):**
1) Propose: draft the plan and risks.
2) Approve: wait for explicit operator confirmation.
3) Execute: run only what was approved.

**Inputs:**
- A change request.

**Outputs:**
- A plan, an approval decision, and (if approved) executed changes.

**Guardrails:**
- No silent execution.

---

## Pattern: continuity without creepiness
**What it is:** Keep context long enough to finish work, but reset to self-heal.

**When to use:**
- Tasking, follow-ups, debugging, or any multi-step workflow.

**How it behaves (agentic):**
- Maintains short-term context.
- Resets after idle time to prevent stale responders.

**Failure modes:**
- Resets too fast → re-asks questions.
- Never resets → stale session issues.

**Fix:**
- Choose an idle reset that matches how teams actually work.
