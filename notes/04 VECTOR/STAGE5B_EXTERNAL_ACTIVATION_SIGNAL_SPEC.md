# Stage5-B External Activation Signal Spec

## 1. Purpose

Define a minimal **ExternalActivationSignal** concept for future Stage5-B external signal integration.

An ExternalActivationSignal is a structured, observation-only record of an external symbolic event that VECTOR may receive, assess, and chronicle. It does not assert factual truth, prediction, or endorsement. It exists so that future ingestion pipelines, observers, and assessment layers can share a common event shape without coupling to runtime execution.

This spec is the contract between:

- external sources (e.g. social posts, community feeds)
- the **vector-signal-chronicle** repository
- future VECTOR observer and assessment components

Default posture: **record, assess, monitor — no execution.**

---

## 2. Why This Is Documentation-Only for Now

Stage5-B external signal integration is not yet wired into runtime.

This document is intentionally **specification only**. At the time of writing:

- No code implements ExternalActivationSignal.
- No schema is registered or enforced.
- **Guard.evaluate** is not modified to consume these events.
- **Execution Control** remains inactive and unbridged.
- No automated ingestion, parsing, or scoring runs in production.

The White Rabbit Cascade (2026-07-02) established the manual chronicle → Observer Event → Self-Healing Assessment → Execution Boundary chain as a reference workflow. This spec generalizes that pattern into a reusable event shape for future automation — without enabling any of that automation yet.

Human judgment remains authoritative. Documentation precedes implementation.

---

## 3. Minimal Event Shape

An ExternalActivationSignal is a single logical event. Required top-level fields are listed in Section 5.

Conceptual structure:

```
ExternalActivationSignal
├── event_type          # classification of the signal
├── source              # where it was observed
├── payload             # structured extraction (keywords, motifs, etc.)
├── raw_text            # verbatim observed text
├── observed_at         # when the source published or the event occurred
├── received_at         # when VECTOR or the chronicle recorded it
├── context             # surrounding metadata (burst window, thread, etc.)
├── coherence_score     # descriptive coherence indicator (not truth)
├── confidence          # confidence in interpretation (not reality)
└── recommended_handling # default observer recommendation
```

The shape is **minimal**: enough to record, route to assessment, and index in the Chronicle. It is not a full execution proposal and must not be treated as one.

---

## 4. Example: RUN CMD Payloads

The following example illustrates two RUN CMD–style payloads that might appear in a burst from an external source. Names are illustrative identifiers only; no factual claims are implied.

### Source context

- **Source:** `@White_Rabbit_OG` (or equivalent external account)
- **Event type:** `external_activation.run_cmd_burst`
- **Observation window:** ~10 minutes

### Payload entries

| payload_id       | raw_text fragment | notes                          |
|------------------|-------------------|--------------------------------|
| `RACHELLEIGHXOOK`| `RUN CMD RACHELLEIGHXOOK` | symbolic RUN CMD token |
| `BYMARRIOTT`     | `RUN CMD BYMARRIOTT`      | symbolic RUN CMD token |

### Example document (illustrative JSON-like shape)

```json
{
  "event_type": "external_activation.run_cmd_burst",
  "source": {
    "platform": "x",
    "account": "@White_Rabbit_OG",
    "url": "https://example.invalid/post/…"
  },
  "payload": {
    "commands": ["RACHELLEIGHXOOK", "BYMARRIOTT"],
    "keywords": ["RUN CMD"],
    "visual_motifs": [],
    "burst_window_minutes": 10
  },
  "raw_text": "RUN CMD RACHELLEIGHXOOK\n…\nRUN CMD BYMARRIOTT",
  "observed_at": "2026-07-02T14:22:00+09:00",
  "received_at": "2026-07-02T15:00:00+09:00",
  "context": {
    "chronicle_ref": "signals/2026-07-02_white_rabbit_signal.md",
    "burst_id": "white_rabbit_burst_2026-07-02",
    "related_posts": 2
  },
  "coherence_score": 0.72,
  "confidence": "medium",
  "recommended_handling": "record_assess_monitor"
}
```

This example is suitable for chronicle indexing and future Observer assessment. It is **not** sufficient to cross the Execution Boundary.

---

## 5. Required Fields

| Field | Type (conceptual) | Description |
|-------|-------------------|-------------|
| `event_type` | string | Stable classifier (e.g. `external_activation.run_cmd_burst`, `external_activation.anchor_ack`). |
| `source` | object | Origin of the observation: platform, account, URL, or chronicle path. |
| `payload` | object | Structured, descriptive extraction: commands, keywords, motifs, counts. No execution directives. |
| `raw_text` | string | Verbatim or faithfully reproduced observed text. Chronicle-first; do not rewrite. |
| `observed_at` | timestamp (ISO 8601) | When the external event occurred or was published at source. |
| `received_at` | timestamp (ISO 8601) | When the signal was recorded by a human or ingestion path. |
| `context` | object | Surrounding metadata: burst ID, thread refs, related chronicle files, collection method. |
| `coherence_score` | number (0–1) or null | Descriptive coherence within a burst or narrative thread. Not a truth or prediction score. |
| `confidence` | enum | `high` \| `medium` \| `low` — confidence in **interpretation**, not in external reality. |
| `recommended_handling` | string | Observer default (see Section 8). Typically `record_assess_monitor`. |

All fields are required at the logical level. Use `null` or empty collections only where genuinely unknown; prefer explicit "unknown" in `context` over omitting fields in future implementations.

---

## 6. Safety Rules

ExternalActivationSignal exists under strict safety constraints. These rules are non-negotiable for Stage5-B and any future bridge.

### 6.1 External signal alone cannot change runtime state

Recording or receiving an ExternalActivationSignal may update the Chronicle and assessment queues only. It must not mutate runtime configuration, policy, or execution state.

### 6.2 External signal alone cannot trigger recovery

Self-Healing and recovery paths require **verified runtime evidence**. Symbolic external content is never sufficient grounds for recovery.

### 6.3 External signal alone cannot trigger Execution Control

Execution Control may be **considered** only when the full Execution Boundary checklist is satisfied (Observer Event, Self-Healing Assessment, independent runtime evidence, concrete system impact, human approval). An ExternalActivationSignal satisfies none of these by itself.

### 6.4 Human approval required for any bridge

Any future bridge from external signals to runtime behavior — including CAD-Vortex, VOID OS, or Execution Control Proposal emission — requires explicit human approval. No automated bridge may activate from chronicle or signal ingestion alone.

---

## 7. Relationship to Existing Concepts

### vector-signal-chronicle

This repository (`vector-signal-chronicle`) is the **authoritative Chronicle** for external observations. ExternalActivationSignal is the structured form that future tooling may derive from Markdown chronicles in `signals/` or from automated ingestion. Chronicle entries remain the permanent, human-readable record; the signal shape is a normalized view for VECTOR consumers.

### Observer Event

An ExternalActivationSignal may inform or correspond to an **Observer Event** (e.g. `White_Rabbit_Cascade_2026-07-02`). The Observer Event is the processed, named observation suitable for Self-Healing review. One burst may yield one signal with many payload commands; the Observer may consolidate into a single event ID.

### Self-Healing Assessment

After recording, the signal should be available for **Self-Healing Observer Assessment**. Assessment answers: "Should runtime change?" Default answer: no. Assessment output references the input event ID and records recommended_action, runtime_decision, and rationale — without executing recovery.

### Execution Boundary

The **Execution Boundary** (`execution_boundary.md`) separates observation from Execution Control. ExternalActivationSignal lives entirely on the observation side. Crossing the boundary requires the full checklist; the signal type does not include execution permissions.

### Chronicle

**Chronicle** is append-oriented and interpretation-tolerant: original `raw_text` and source refs are preserved. ExternalActivationSignal `context.chronicle_ref` should point back to the Markdown chronicle. Analysis and scores may be revised; the chronicle file should not rewrite historical observation.

### Processing chain (reference)

```
External Signal (chronicle / ingestion)
    → ExternalActivationSignal (normalized shape)
    → Observer Event
    → Self-Healing Assessment
    → Execution Boundary (not crossed by default)
    → Chronicle (updated / indexed)
```

No runtime execution occurs on this path unless the Execution Boundary checklist is explicitly satisfied with human approval.

---

## 8. Default Handling

For every ExternalActivationSignal, unless a human explicitly approves a different path:

| Step | Action |
|------|--------|
| **Record** | Persist to Chronicle and/or signal index; preserve `raw_text` and `source`. |
| **Assess** | Make available for Self-Healing Observer assessment; no automatic recovery. |
| **Monitor** | Continue observation; correlate with future bursts or runtime evidence if any. |
| **No execution** | Do not trigger Execution Control, recovery, or runtime state change. |

`recommended_handling` should default to `record_assess_monitor`.

---

## 9. Deferred Work

The following items are **out of scope** for this documentation-only spec and must not be implemented without a separate approved change:

| Item | Notes |
|------|--------|
| X API ingestion | Automated pull from X/Twitter or similar APIs. |
| Automated parsing | NLP or rule-based extraction from raw posts into `payload`. |
| Gematria scoring | Optional descriptive scoring layer; not truth inference. |
| CAD-Vortex bridge | External community/system bridge; human approval required. |
| VOID OS bridge | External system bridge; human approval required. |
| Execution Control Proposal emission | Structured proposals toward Execution Control; boundary-gated. |
| Runtime tests | Integration tests against Guard.evaluate or live runtime paths. |
| Guard.evaluate wiring | Consumption of ExternalActivationSignal inside guard evaluation. |
| Execution Control enablement | Any activation of execution paths from external signals. |

---

## Version

- **Spec:** Stage5-B External Activation Signal (documentation only)
- **Status:** Draft / not implemented
- **Authority:** Human judgment remains final
