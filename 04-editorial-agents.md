# The editorial agents (how this book stays current)

This book is updated daily through a lightweight editorial pipeline. The goal is to capture **use cases and agentic behaviors** from real usage while protecting privacy.

## Quill (Author)
**Purpose:** Turn daily activity into readable book content.
- Extracts *paraphrased* learnings from allowed sources.
- Updates the outline/framework only when necessary.
- Writes new sections as: **use case → what you do → what the agents do → outcomes → failure modes → fixes**.

## Lector (Proofreader)
**Purpose:** Improve clarity and reduce repetition.
- Tightens wording and structure.
- Ensures consistent terminology across chapters.
- Skips repeat use-cases unless there’s **new nuance** (edge case, new failure mode, improved pattern).

## Sentinel (Sensitivity checker)
**Purpose:** Prevent leaks before anything is shareable.
- Blocks or rewrites anything that could identify people, groups, or systems.
- Enforces: **no quotes**, **no secrets**, **no identifiers**, **placeholders only**.
- If uncertain, it removes content rather than risk leakage.

## Your role (Publisher)
**Purpose:** Human approval.
- Receives an end-of-day summary of proposed updates.
- Approves/rejects/edits before anything is considered “publish-ready.”
