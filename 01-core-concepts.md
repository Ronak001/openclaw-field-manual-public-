# Core concepts (plain-English)

This chapter is the mental model. If you get these concepts, everything else becomes easier.

## Agents
**What an agent is:** A role with a job.

In OpenClaw, you don’t have one monolithic bot. You have *agents* that behave differently depending on where they’re deployed and what they’re allowed to do.

**Think of agents like specialists:**
- A *front-door* agent that triages incoming messages.
- A *control-plane* agent that you keep clean for decisions and high-impact actions.
- A *group PM* agent that runs day-to-day workflows inside specific groups.

**Why this matters:**
- It prevents “one bot doing everything everywhere,” which is how you get noise, mistakes, and confusion.
- It lets you scale to many groups without losing control.

**Common agent roles (examples):**
- **Gatekeeper (front door):**
  - Handles most inbound traffic first.
  - Asks clarifying questions.
  - Routes work to the right place.
  - Usually *restricted* from powerful tools.

- **Worker (control channel):**
  - Your private command center.
  - Used for configuration, sensitive decisions, and high-impact operations.
  - Should not be polluted by random group chatter.

- **Program Manager (group PM):**
  - Opt-in per group.
  - Turns conversations into tasks, follow-ups, summaries, and coordination.
  - Operates under mention-gating so it doesn’t spam.

> Book rule: we describe behaviors and patterns, not private identifiers. Any examples use placeholders like `<GROUP_NAME>` and `<USER>`.

---

## Sessions
**What a session is:** The agent’s *working context* for a particular conversation.

A session is what allows the agent to:
- remember the last few turns,
- keep track of a multi-step workflow,
- and avoid re-asking the same questions repeatedly.

**Why sessions reset:** Sessions need to self-heal.
- Group conversations are noisy.
- Context can go stale.
- Long-running sessions can cause the wrong agent to keep responding after routing changes.

So OpenClaw uses reset rules (often time-based) to keep things fresh.

### Stale sessions (the “haunted bot” problem)
**Symptom:** You change routing, but an older responder continues replying in a group.

**What’s really happening:**
- A previous session still exists and is still eligible to respond.

**Fix pattern (operational):**
- Identify the responding agent/session.
- Reset/purge the stale session so only the intended agent responds.

### Continuity vs. stability tradeoff
If sessions reset too quickly, the agent can’t complete workflows.
If sessions never reset, you risk stale behavior.

A good default is:
- long enough to finish a workflow,
- short enough to self-heal automatically.

---

## Tools
**What it means when an agent “uses a tool”:** It takes an action outside the chat.

Examples of tool-like actions:
- checking a calendar
- browsing a website
- reading/writing files
- scheduling reminders
- sending messages

**Why tools are gated:**
Tool access turns an assistant from “talking” into “doing.”

So a safe setup usually follows:
- front-door agents: limited or no tool access
- control-plane agent: tool access, but requires explicit approval for risky actions
- group PM agents: narrow, safe tool access aligned to their job

**Reliability reality:**
- External sites can block automated browsing.
- Some actions require human consent (e.g., first-time permissions).

A good agent should:
- detect the blockage,
- switch to a fallback,
- and ask for approval when needed.

---

## Control + safety (propose → approve → execute)
**The core idea:** separate *suggesting* from *doing*.

**Propose:**
- The agent drafts a plan in plain language.
- It lists what will change and any risks.

**Approve:**
- A human (the operator) explicitly confirms.

**Execute:**
- Only then does the agent take actions (especially anything persistent or high-impact).

**Why this works:**
- Teams can collaborate (“here’s what we should do”) without accidentally running changes.
- You preserve operator control for anything that affects safety, privacy, or system behavior.

**Rule of thumb:**
If it changes behavior, touches external systems, or could cause side effects → require approval.
