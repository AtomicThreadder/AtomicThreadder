# Atomic Threadder: Stateless AI Turns with Sovereign Context

**Sidecar Summarization + Human Governance + ASHA Code Precision**

*The Definitive Architecture for Long-Running, High-Precision AI Coding Sessions*

---

## Core Principle

**The model remains stateless. Context and code state remain sovereign to the engineer.**

Gemini 3.1 PRO executes every turn as a pure stateless function. It receives only the current prompt. No history, no thread state, and no prior artifacts are ever sent to it.

Continuity is achieved through a clean three-layer system controlled entirely by the local application:

1. **Initial Prompt / Response** — Handled by Gemini 3.1 PRO
2. **Sidecar Summary** — Produced by Gemini 3.5 and reviewed by the engineer
3. **ASHA Integration** — Cryptographic, AST-bound code state management

This separation delivers coherent, multi-turn coding work without context bloat, attention degradation, or fragile state inside the model.

---

## The Atomic Threadder Turn Cycle

**Every turn follows this disciplined flow:**

1. **Engineer submits prompt** in a clean input field.

2. **Sent to Gemini 3.1 PRO**  
   The primary model receives only the current prompt and executes the task. It has no knowledge of previous turns.

3. **Response rendered** in the main Thread View.

4. **Sidecar Summary generated**  
   The `[Prompt + Response]` pair is sent to Gemini 3.5.  
   It produces a concise natural-language summary capturing objective, progress, decisions, and next intent.

5. **Human Governance**  
   The summary appears in a dedicated **Continuation Context** panel — clearly separated from the input box.  
   The engineer reviews, edits, strengthens, or prunes it before the next turn. This step is mandatory and non-negotiable.

6. **Next Turn Assembly**  
   When submitted, the refined summary travels with the new prompt. Gemini 3.1 PRO receives high-signal, human-approved context while remaining stateless.

7. **Code Precision via ASHA**  
   When the task involves modifying code, the system does not rely on the summary. Instead, **ASHA** retrieves the exact current code block using its cryptographic `content_hash` and `module_instance_id`.  
   The precise block + the System Constraint Directive are injected cleanly. The model returns only the modified block. No whole files. No drift.

8. **Thread Control**  
   Atomic Threadder manages thread begin/end entirely locally. Engineers can start, end, or branch threads at will. All history remains fully searchable in PostgreSQL 18.

---

## Why This Architecture Is Superior

- **True Statelessness** — Gemini 3.1 PRO never accumulates state. Every call is fresh.
- **Human-Governed Continuity** — The engineer, not the model, controls what context carries forward. Natural language summaries are reviewed every turn.
- **Cryptographic Code Precision** — ASHA delivers exact, versioned, AST-bound code blocks. No reliance on model memory or fragile JSON artifacts.
- **Bounded Token Cost** — Context remains relevant and compact regardless of session length.
- **Clean User Experience** — The input box stays clean. Continuation context lives in its own dedicated, editable panel.
- **Powerful Branching & Auditability** — Every turn, summary, and code change is recorded locally with full lineage. Branching is trivial and safe.
- **Correct Foundation** — All persistence uses **PostgreSQL 18**. Any semantic capabilities would leverage `pgvector`.

---

## The Three Layers — Clearly Separated

| Layer                    | Purpose                              | Model / System     | Human Role          | Strength |
|--------------------------|--------------------------------------|--------------------|---------------------|----------|
| **Prompt / Response**    | Task execution                       | Gemini 3.1 PRO     | Submits & reviews   | High reasoning quality |
| **Sidecar Summary**      | High-level intent & continuity       | Gemini 3.5         | Mandatory review & edit | Low friction, high control |
| **ASHA Integration**     | Exact code state & surgical edits    | ASHA + Orchestrator| Triggers when needed| Cryptographic precision, zero drift |

This separation is the decisive advantage. Intent lives in reviewed natural language. Code lives in cryptographic blocks. The model stays stateless.

---

## Benefits

- Long-running coding sessions that feel persistent to the engineer but remain strictly stateless to the model.
- Dramatically lower risk of context degradation or hallucinated history.
- Precise, auditable code modifications with no file-level drift.
- Excellent token economics and predictable performance.
- Full local control over threads, branching, and history.
- Clean, professional user experience with no polluted input fields.
- Production-grade foundation on PostgreSQL 18 with ASHA as the code integrity layer.

---

## Judgement

This is the architecture that wins.

- It does not ask the heavy model to carry state it was never designed to manage reliably.
- It does not hide context inside opaque model memory.
- It keeps the engineer in the loop at the exact point where human judgment adds the most value.
- It uses the right tool for each job: Gemini 3.1 PRO for reasoning, Gemini 3.5 for summarization, and ASHA for code truth.

**Atomic Threadder delivers stateless inference with sovereign, human-governed context and cryptographic code precision.**

---

*Definitive reference • Atomic Threadder Protocol • June 2026*