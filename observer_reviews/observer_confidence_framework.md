# Observer Confidence Framework

## 1. Purpose

Define a reproducible, evidence-based confidence framework used by **Observer Reviews** before any consolidation or governance decision.

This framework answers three questions for every review:

1. **What evidence is present?** (Evidence Level)
2. **How much weight may be assigned to a consolidation or governance determination?** (Confidence Level)
3. **What decision is supported?** (Keep Independent, Consolidate, Defer, or Escalate to Human Review)

Confidence reflects confidence in the **determination** — not truth about external reality, system state, or intent.

This document governs documentation and review decisions only. It does not modify runtime behavior, schema, code, or tests.

Human judgment remains authoritative.

---

## 2. Scope

This framework applies to:

- **Observer Reviews** that compare two or more ExternalActivationSignal records and determine Observer Event representation.
- Consolidation versus independence determinations under `observer_reviews/observer_event_creation_policy.md`.
- Precedent references when future batches, bursts, or signal groups are reviewed.
- Confidence labeling within Observer Review documents before any Chronicle or governance artifact is created.

This framework does not apply to:

- Runtime ingestion, guard evaluation, execution paths, or recovery actions.
- Self-Healing Assessment outcomes (which use separate runtime evidence criteria).
- Automated consolidation without an Observer Review or explicit human approval.
- Modification of existing Chronicle entries (Chronicle remains append-oriented).

---

## 3. Evidence Levels

Evidence Levels describe **what observable material is present** in the Chronicle or attached review record. They are ordinal: higher levels subsume lower levels.

### Level 0 — Metadata only

Only structural or handling metadata is present.

Examples:

- Classification (e.g. ExternalActivationSignal)
- Priority (e.g. P0)
- Status (e.g. Pending Content Analysis)
- `triage_manifest_id` or batch membership
- Recommended handling text

Level 0 alone does not establish temporal proximity, content similarity, or cross-signal relationship.

### Level 1 — URL and source identity

Level 0 evidence **plus**:

- **URL** — a distinct, attributable post or ingest URL (including status ID).
- **Source identity** — attributed account, channel, author, or ingest origin.

Level 1 establishes that something was observed and where it was observed. It does not establish what was said, when it was published, or how signals relate to one another.

### Level 2 — Timestamp, raw text, and image reference

Level 1 evidence **plus** at least one of:

- **Timestamp** — recorded publish time, ingest time, or defined burst window with source of the time data stated.
- **Raw text** — preserved post body, transcript, or quoted content in the chronicle.
- **Image reference** — screenshot, archive link, or human-recorded media description tied to the signal.

Level 2 enables content-based and temporal comparison between signals. Missing any of the three does not prevent reaching Level 2 if the others are present.

### Level 3 — Multiple observable correlations and cross-reference

Level 2 evidence **plus**:

- **Multiple observable correlations** — documented alignment across two or more signals (e.g. shared keywords, command sequences, visual motifs, or structural metadata **with** timing or content support).
- **Cross-reference between signals** — explicit linkage recorded in chronicle evidence (reply chain, quote post, thread marker, or human-recorded linkage with source citation).

Implicit thematic similarity is not a cross-reference. Batch co-listing alone is not a cross-reference.

### Level 4 — Independent corroboration and repeated observable patterns

Level 3 evidence **plus**:

- **Independent corroboration** — evidence from a source or measurement independent of the primary signal (e.g. archive snapshot, third-party timestamp service, runtime log correlation with documented linkage).
- **Repeated observable patterns** — the same named motif, command sequence, or operational marker documented across multiple observations within a defined window, with each occurrence recorded in the Chronicle.

Level 4 supports high-confidence consolidation or governance decisions. It does not imply truth claims about external intent or system impact.

---

## 4. Confidence Levels

Confidence Levels describe **how much weight may be assigned to a consolidation or governance determination** given the Evidence Level and applicable policy criteria.

Confidence is about the **review determination**, not about reality, prediction, or endorsement.

| Confidence | Minimum evidence required |
|------------|---------------------------|
| **Very Low** | Evidence Level 0 or Level 1 only. Metadata and/or URL plus source identity without timestamps, raw text, image reference, or cross-signal correlations. Default when evidence is incomplete per `observer_event_creation_policy.md` Section 8. |
| **Low** | Evidence Level 2 for at least one signal in the review set, but not Level 3. Single-signal content or timing may be documented; multi-signal consolidation criteria are not met. |
| **Moderate** | Evidence Level 3 for the signals under review. Multiple observable correlations or explicit cross-references are documented. Consolidation criteria in policy Section 4 may be partially satisfied; independence factors may still apply. |
| **High** | Evidence Level 3 with strong, documented support for consolidation criteria (timing + shared content + same source, or equivalent combination). Independence criteria from policy Section 5 are addressed and rejected with recorded rationale. |
| **Very High** | Evidence Level 4. Independent corroboration and repeated observable patterns are documented. Suitable for precedent-setting consolidation when human approval is recorded. Does not authorize runtime action or Execution Boundary crossing. |

### Confidence caps

- Evidence Level 0 or 1 → Confidence cannot exceed **Very Low**.
- Batch-only linkage without Level 2 content or timing → Confidence cannot exceed **Very Low**, regardless of source attribution or metadata parity.
- No Observer Review document → Confidence is **undefined**; no consolidation determination may be published.

---

## 5. Escalation Rules

### When confidence may increase

Confidence may increase **only** when new **observable** evidence is recorded in the Chronicle or attached to an Observer Review, and a new or superseding Observer Review documents the change.

Permitted increases:

| From | To | Required new evidence |
|------|-----|----------------------|
| Very Low | Low | Level 2 evidence for at least one signal (timestamp, raw text, or image reference). |
| Low | Moderate | Level 3 evidence (correlations or cross-references between signals). |
| Moderate | High | Strengthened Level 3 with timing + shared content + documented rejection of independence factors. |
| High | Very High | Level 4 evidence (independent corroboration and repeated observable patterns). |

Human approval may explicitly record a higher confidence label with documented rationale even when evidence is thin — but such approval must be stated in the Observer Review and must not be presented as evidence-derived default policy (per `observer_event_creation_policy.md` Section 9).

### When confidence must remain unchanged

Confidence must remain unchanged when:

- No new observable evidence has been added since the prior determination.
- The only change is reinterpretation, narrative context, or semantic inference without new chronicle facts.
- A Self-Healing Assessment or runtime observation exists but does not add consolidation-relevant observable evidence (runtime evidence does not retroactively justify consolidation).
- A human grouping aid or batch manifest is updated without new per-signal chronicle content.
- Prior Observer Review stands and no superseding review has been authored.

### When confidence must decrease

Confidence must decrease when:

- Previously cited evidence is retracted, corrected, or shown to be erroneous in the Chronicle.
- A superseding review documents that independence criteria apply more strongly than previously assessed.
- Evidence Level is downgraded (e.g. timestamp source disputed, raw text removed, cross-reference invalidated).
- Consolidation was recommended under Moderate or higher confidence but required evidence types in policy Section 6 are later found absent.

When confidence decreases, the prior determination should be explicitly superseded in a new Observer Review. Chronicle entries are not rewritten.

---

## 6. Human Review Threshold

Human review is **mandatory** before any of the following:

| Condition | Requirement |
|-----------|-------------|
| Confidence is **Moderate** or higher and consolidation into one Observer Event is recommended | Human approves consolidation determination and Observer Event ID(s). |
| Confidence is **Very Low** or **Low** but consolidation is recommended despite thin evidence | Human explicitly approves deviation from default independence rule; rationale recorded in Observer Review. |
| Evidence Level is **Level 3** or **Level 4** | Human confirms cross-references and corroboration before consolidation is finalized. |
| Any determination would set or extend repository precedent | Human approves use as precedent for future batches. |
| Disagreement between assisted analysis and human observer | Human judgment resolves; authoritative outcome recorded. |
| Proposed change affects Self-Healing Assessment scope or Execution Boundary consideration | Human approves; framework does not cross the boundary by itself. |

Human review is **not** required to apply the default rule:

**If evidence is incomplete, keep signals independent** at Very Low confidence.

Documenting independence at Evidence Level 0 or 1 with Very Low confidence is the expected default outcome and may proceed as a standard Observer Review without additional approval — unless it sets precedent or deviates from policy.

---

## 7. Reassessment Rules

1. **Confidence may only change if new observable evidence becomes available.** Reinterpretation alone is insufficient.

2. **Reassessment requires a new or superseding Observer Review** that cites the new evidence and states the prior Evidence Level, Confidence Level, and determination.

3. **Chronicle entries are append-oriented.** Reassessment adds new records or review documents; it does not rewrite historical observations.

4. **Downgrade path.** If new evidence weakens a prior case, confidence must decrease per Section 5 even when the consolidation decision remains unchanged.

5. **Upgrade path.** Consolidation may move from independent to consolidated only when reassessment documents sufficient evidence under `observer_event_creation_policy.md` Section 6 and reaches at least Moderate confidence (or human-approved exception).

6. **No retroactive runtime justification.** Self-Healing outcomes, runtime logs, or execution state do not by themselves upgrade consolidation confidence unless the Observer Review documents observable chronicle evidence that meets the Evidence Level definitions above.

---

## 8. Relationship to Other Governance Artifacts

### Observer Review

The Observer Confidence Framework is applied **inside** Observer Reviews before a consolidation or governance determination is published.

Every Observer Review should state:

- Evidence Level (highest level supported by the signal set under review)
- Confidence Level
- Determination (Keep Independent, Consolidate, Defer, or Escalate)
- Rationale tied to evidence, not semantic inference

See: `observer_reviews/observer_event_creation_policy.md`

### Observer Event

An Observer Event is the named output of a completed review path. Confidence labeling belongs in the **Observer Review**, not as a substitute for Observer Event chronicle content.

- Very Low / Low confidence → typically one Observer Event per ExternalActivationSignal (independence).
- Moderate or higher → may support one consolidated Observer Event when policy criteria are met.

Recording an Observer Event does not change confidence; it implements a prior determination.

### Self-Healing Assessment

Self-Healing Assessment answers: "Should runtime change?" It follows Observer Event creation in the processing chain:

```
ExternalActivationSignal → Observer Review → Observer Event → Self-Healing Assessment
```

- Consolidation confidence does **not** imply recovery or runtime action.
- Self-Healing requires verified **runtime** evidence per `execution_boundary.md`.
- External symbolic content alone — at any Evidence or Confidence Level — is insufficient for Self-Healing recovery.

### Execution Boundary

Observer Reviews and confidence determinations remain entirely on the **observation side** of the Execution Boundary (`execution_boundary.md`).

Crossing the boundary requires all of the following:

1. A recorded Observer Event exists.
2. A Self-Healing Observer Assessment exists.
3. Independent runtime evidence exists.
4. A concrete system impact is detected.
5. Human approval is given.

Neither high confidence nor Level 4 evidence satisfies any of these conditions by itself.

---

## 9. White Rabbit Batch 001 Example

**Batch:** `ExternalActivationSignal_Batch_001_2026-07-03`

**Signals:** Three URL-identified ExternalActivationSignal records from @White_Rabbit_OG:

| Signal ID | Chronicle Ref |
|-----------|---------------|
| 2073001960010321955 | `signals/2026-07-03_white_rabbit_signal_2073001960010321955.md` |
| 2073009091224694808 | `signals/2026-07-03_white_rabbit_signal_2073009091224694808.md` |
| 2073010587022618823 | `signals/2026-07-03_white_rabbit_signal_2073010587022618823.md` |

**Observer Review:** `observer_reviews/2026-07-03_white_rabbit_batch001_observer_review.md`

### Evidence inventory

| Evidence type | Present? |
|---------------|----------|
| Metadata (classification, priority, status, triage_manifest_id) | Yes |
| URL (distinct post URL per signal) | Yes |
| Source identity (@White_Rabbit_OG) | Yes |
| Timestamp / burst window | No |
| Raw text / post content | No |
| Image reference / screenshot / archive link | No |
| Cross-reference between signals | No |
| Shared observable content / motifs | No |
| Independent corroboration | No |

### Framework application

| Field | Result |
|-------|--------|
| **Evidence Level** | **Level 1** — URL and source identity are present; Level 0 metadata is also present. Level 2 criteria (timestamp, raw text, image reference) are not met. |
| **Confidence** | **Very Low** — Evidence Level 1 caps confidence at Very Low. Batch grouping and metadata parity do not raise confidence. |
| **Decision** | **Keep Independent** — Represent as three independent Observer Events. |
| **Reason** | **Insufficient observable evidence.** Distinct URLs and signal IDs are the strongest differentiators present. Without recorded timing, content analysis, or cross-references, consolidation would exceed what the evidence supports. Default decision rule applies. |

### Contrast with higher-evidence precedent

`White_Rabbit_Cascade_2026-07-02` supports higher Evidence and Confidence Levels: an approximately ten-minute window was recorded, command sequences and keywords were named, and content was analyzed. Batch 001 lacks equivalent material; the framework correctly yields Very Low confidence and independence.

### Reassessment trigger

Confidence or consolidation may be reconsidered only if new observable evidence is recorded — for example, publish timestamps showing temporal proximity, or content analysis documenting shared keywords or motifs — in a new Observer Review without inferring meaning beyond recorded facts.

---

## References

- `OBSERVER_GUIDELINES.md`
- `observer_reviews/observer_event_creation_policy.md`
- `observer_reviews/2026-07-03_white_rabbit_batch001_observer_review.md`
- `execution_boundary.md`
- `notes/04 VECTOR/STAGE5B_EXTERNAL_ACTIVATION_SIGNAL_SPEC.md`
- `signals/2026-07-02_white_rabbit_cascade_observer_event.md` (higher-evidence consolidation precedent)

---

Version: v1.0  
Date: 2026-07-03

Human judgment remains authoritative.
