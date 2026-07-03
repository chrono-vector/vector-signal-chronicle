# Observer Decision Matrix

## 1. Purpose

Define a standard decision matrix for **Observer Reviews** that maps **Evidence Level**, **Confidence Level**, and **Risk Posture** to supported governance outcomes.

This matrix answers: **Given what is observable, how much risk is present, and how confident is the review determination — what decision may be published?**

The matrix governs documentation and review decisions only. It does not modify runtime behavior, schema, code, or tests.

Human judgment remains authoritative.

---

## 2. Scope

This matrix applies to:

- **Observer Reviews** that compare two or more ExternalActivationSignal records and determine Observer Event representation.
- Consolidation versus independence determinations under `observer_reviews/observer_event_creation_policy.md`.
- Confidence labeling under `observer_reviews/observer_confidence_framework.md`.
- Precedent references when future batches, bursts, or signal groups are reviewed.

This matrix does not apply to:

- Runtime ingestion, guard evaluation, execution paths, or recovery actions.
- Self-Healing Assessment outcomes (which use separate runtime evidence criteria).
- Automated consolidation without an Observer Review or explicit human approval.
- Modification of existing Chronicle entries (Chronicle remains append-oriented).

---

## 3. Inputs

Every Observer Review should evaluate the following inputs before publishing a determination.

### Evidence Level

Ordinal measure of **what observable material is present** in the Chronicle or attached review record. Defined in `observer_confidence_framework.md` Section 3.

| Level | Summary |
|-------|---------|
| **0** | Metadata only (classification, priority, status, batch membership). |
| **1** | Level 0 plus URL and source identity. |
| **2** | Level 1 plus timestamp, raw text, or image reference (at least one). |
| **3** | Level 2 plus multiple observable correlations or explicit cross-reference between signals. |
| **4** | Level 3 plus independent corroboration and repeated observable patterns. |

Evidence Level describes material presence — not truth, intent, or system impact.

### Confidence Level

Measure of **how much weight may be assigned to a consolidation or governance determination** given Evidence Level and applicable policy criteria. Defined in `observer_confidence_framework.md` Section 4.

| Confidence | Typical Evidence Level |
|------------|------------------------|
| **Very Low** | Level 0 or Level 1 |
| **Low** | Level 2 (single-signal content or timing; consolidation criteria not met) |
| **Moderate** | Level 3 |
| **High** | Level 3 with strong consolidation support |
| **Very High** | Level 4 |

Confidence reflects confidence in the **determination**, not external reality.

### Risk Posture

Measure of **what harm could result if the determination is wrong or acted upon prematurely**. Defined in Section 6 of this document.

Risk Posture is evaluated independently of Evidence Level and Confidence Level. A high-confidence consolidation recommendation may still require escalation when Risk Posture is High or Prohibited.

### Human Review Requirement

Boolean gate indicating whether a human must approve the determination before it is finalized or used as precedent.

Human review is **mandatory** when this matrix or `observer_confidence_framework.md` Section 6 requires it. Human review is **not** required to apply the default independence rule at Very Low confidence unless the determination sets precedent or deviates from policy.

---

## 4. Standard Decisions

Observer Reviews publish one of the following standard decisions.

### Keep Independent

Represent each ExternalActivationSignal as its own Observer Event. One signal → one event ID.

**When used:** Evidence is incomplete, independence criteria apply, or consolidation is not supported at current Evidence and Confidence Levels.

This is the **default outcome** when evidence is ambiguous or thin.

### Consolidate

Represent multiple ExternalActivationSignal records as one Observer Event when observable evidence supports a single named observation.

**When used:** Evidence Level 3–4 with Moderate or higher confidence, consolidation criteria met per `observer_event_creation_policy.md` Section 4, required evidence documented per Section 6, and Human Review Requirement satisfied.

Consolidation is a **proposed organizational choice** — not automatic at any Evidence or Confidence Level.

### Defer

Postpone a consolidation or independence determination until additional observable evidence is recorded.

**When used:** Review is in progress, evidence is partially present but insufficient for a final determination, or timing/content analysis is pending without contradicting prior independence factors.

Defer does not imply future consolidation. It records that the review set is not yet decision-ready.

### Escalate to Human Review

Route the determination to human approval before publication or precedent use.

**When used:** Moderate or higher confidence consolidation is recommended, thin-evidence consolidation is proposed, Risk Posture is High or Prohibited, precedent would be set or extended, or assisted analysis and human observer disagree.

Escalation is not failure — it is the required path when stakes exceed automated or default policy thresholds.

### Reject Consolidation

Explicitly decline a consolidation proposal after evaluation, even when some consolidation factors are present.

**When used:** Independence criteria from `observer_event_creation_policy.md` Section 5 outweigh consolidation factors; batch-only linkage was proposed; or prior consolidation recommendation is superseded by new evidence showing signals must remain independent.

Reject Consolidation is distinct from Keep Independent: it records that consolidation was **considered and rejected** with documented rationale.

---

## 5. Decision Matrix

The following rules map inputs to supported decisions. When multiple rules apply, the **most conservative** outcome prevails unless human approval explicitly records a deviation.

### Evidence and confidence rules

| Evidence Level | Confidence | Supported decision | Human review |
|----------------|------------|-------------------|--------------|
| **0–1** | **Very Low** | **Keep Independent** | Not required for default independence |
| **1–2** | **Low** | **Defer** or **Keep Independent** | Not required for independence; required if consolidation is proposed |
| **2–3** | **Moderate** | **Human Review Required** before consolidation | Mandatory before consolidation |
| **3–4** | **High** or **Very High** | **Consolidation may be proposed**, but not automatic | Mandatory before consolidation is finalized |
| **Any** | **Undefined** (no Observer Review) | No determination may be published | N/A |

### Risk posture overrides

| Risk Posture | Override |
|--------------|----------|
| **High** | **Escalate to Human Review** — regardless of Evidence Level or Confidence Level |
| **Prohibited** | **Escalate to Human Review**; consolidation and runtime-adjacent outcomes are blocked pending human disposition |

### Missing evidence rules

| Condition | Default decision |
|-----------|----------------|
| **Missing timestamp** and **no raw content** (no raw text, image reference, or analyzed content) | **Keep Independent** |
| **Distinct URLs** without compensating cross-reference or shared recorded content | **Keep Independent** or **Reject Consolidation** |
| **Batch-only relationship** (sole linkage is co-listing in a Human Intake Manifest) | **Keep Independent** |
| **Evidence incomplete or ambiguous** | **Keep Independent** (see Section 8) |

### Decision selection guide

```
Inputs: Evidence Level + Confidence + Risk Posture
                │
                ▼
        Risk High or Prohibited?
           │ yes          │ no
           ▼              ▼
    Escalate to      Evidence 0–1 +
    Human Review     Very Low confidence?
                          │ yes → Keep Independent
                          │ no
                          ▼
                    Evidence 1–2 + Low?
                          │ yes → Defer or Keep Independent
                          │ no
                          ▼
                    Evidence 2–3 + Moderate?
                          │ yes → Human Review before consolidation
                          │ no
                          ▼
                    Evidence 3–4 + High/Very High?
                          │ yes → Consolidation may be proposed
                          │     (Human Review required; not automatic)
                          │ no
                          ▼
                    Default: Keep Independent
```

---

## 6. Risk Posture

Risk Posture evaluates **downstream harm** if a determination is wrong, premature, or misapplied — not the symbolic content of external signals.

### Low Risk

- Determination affects documentation organization only (independence at default evidence levels).
- No precedent extension beyond established policy defaults.
- No Self-Healing or Execution Boundary scope change proposed.
- Distinct URLs, missing timing, and batch-only linkage are present — standard independence case.

**Typical matrix outcome:** Keep Independent without escalation.

### Moderate Risk

- Consolidation is under consideration but evidence is partial (Level 2 timing or content for some signals only).
- Determination would name Observer Event IDs for the first time.
- Prior precedent exists but current evidence profile differs materially.
- Self-Healing Assessment scope may change (number of events assessed) but no recovery is proposed.

**Typical matrix outcome:** Defer, Keep Independent, or Human Review before consolidation.

### High Risk

- Consolidation across distinct URLs without Level 3 cross-reference or shared recorded content.
- Moderate or higher confidence consolidation recommended.
- Determination would set or extend repository precedent for future batches.
- Proposed change affects Self-Healing Assessment scope or Execution Boundary consideration.
- Disagreement between assisted analysis and human observer.

**Typical matrix outcome:** Escalate to Human Review.

### Prohibited

- Any outcome that would trigger execution, modify runtime behavior, or initiate recovery from the Observer Review path alone.
- Any claim of semantic truth, external intent, or factual certainty about narratives.
- Override of human authority without recorded human approval.
- Consolidation or governance action when required evidence types in `observer_event_creation_policy.md` Section 6 are absent and human exception is not recorded.

**Typical matrix outcome:** Escalate to Human Review; prohibited outcomes must not be published.

---

## 7. Prohibited Outcomes

Observer Reviews must **not**:

| Prohibited action | Rationale |
|-------------------|-----------|
| **Trigger execution** | Observer Reviews are observation-side documentation. Execution Control requires the full Execution Boundary checklist. |
| **Modify runtime behavior** | Reviews do not mutate configuration, guards, policy, or execution state. |
| **Initiate recovery** | Recovery requires verified runtime evidence per `execution_boundary.md`. External symbolic content alone is insufficient. |
| **Claim semantic truth** | Reviews record observable facts and organizational determinations — not correctness, prediction, or endorsement of external narratives. |
| **Override Human Authority** | Human judgment is always authoritative. Assisted tooling may draft reviews; humans approve deviations, consolidation, and precedent. |

These prohibitions apply at every Evidence Level, Confidence Level, and Risk Posture.

---

## 8. Default Rule

**If evidence is incomplete or ambiguous: Keep Independent.**

Each ExternalActivationSignal maps to its own Observer Event until an Observer Review documents sufficient evidence under `observer_event_creation_policy.md` Section 6 and reaches a supported matrix outcome for consolidation.

Incomplete or ambiguous evidence includes:

- Missing timestamps without compensating content analysis.
- Missing raw text, image reference, or analyzed content.
- Batch-only linkage between signals.
- Distinct URLs without cross-reference or shared recorded content.
- Confidence undefined (no Observer Review document).

When in doubt, prefer independence. Consolidation may be reconsidered only when new observable evidence is recorded in the Chronicle without rewriting historical entries.

---

## 9. Relationship to Other Governance Artifacts

### Observer Event Creation Policy

`observer_reviews/observer_event_creation_policy.md` defines **when** consolidation may be considered (consolidation and independence criteria, required evidence, default rule).

This decision matrix defines **what decision** is supported given Evidence Level, Confidence Level, and Risk Posture. The matrix implements policy defaults; it does not replace policy criteria.

### Observer Confidence Framework

`observer_reviews/observer_confidence_framework.md` defines Evidence Levels, Confidence Levels, escalation rules, and human review thresholds.

This decision matrix consumes those inputs and maps them to standard decisions. Every Observer Review should state Evidence Level and Confidence Level per the framework, then apply this matrix for the determination.

### Self-Healing Assessment

Self-Healing Assessment follows Observer Event creation in the processing chain:

```
ExternalActivationSignal → Observer Review → Observer Event → Self-Healing Assessment
```

- Matrix decisions (Keep Independent, Consolidate, Defer, etc.) belong in **Observer Review**.
- Self-Healing Assessment answers: "Should runtime change?" Default: no.
- No matrix outcome implies recovery or runtime action. High confidence consolidation does not lower Self-Healing evidence requirements.

### Execution Boundary

Observer Reviews and matrix determinations remain entirely on the **observation side** of the Execution Boundary (`execution_boundary.md`).

Crossing the boundary requires all of the following:

1. A recorded Observer Event exists.
2. A Self-Healing Observer Assessment exists.
3. Independent runtime evidence exists.
4. A concrete system impact is detected.
5. Human approval is given.

Neither consolidation nor independence satisfies any of these conditions by itself.

---

## 10. White Rabbit Batch 001 Application

**Batch:** `ExternalActivationSignal_Batch_001_2026-07-03`

**Signals:** Three URL-identified ExternalActivationSignal records from @White_Rabbit_OG (signal IDs 2073001960010321955, 2073009091224694808, 2073010587022618823).

**Observer Review:** `observer_reviews/2026-07-03_white_rabbit_batch001_observer_review.md`

### Matrix inputs

| Input | Value |
|-------|-------|
| **Evidence Level** | **Level 1** — URL and source identity present; Level 2 criteria (timestamp, raw text, image reference) not met. |
| **Confidence** | **Very Low** — Evidence Level 1 caps confidence at Very Low. |
| **Risk Posture** | **Low-to-Moderate** — Low for default independence; Moderate if consolidation were proposed (distinct URLs, missing timing, batch-only linkage). |

### Matrix outcome

| Field | Result |
|-------|--------|
| **Decision** | **Keep Independent** — Represent as three independent Observer Events. |
| **Human Review** | **Required for any future reconsideration** — not required to publish the default independence determination at Very Low confidence. |
| **Applicable rules** | Evidence 0–1 + Very Low → Keep Independent; missing timestamp + no raw content → Keep Independent; evidence incomplete → default rule (Section 8). |

### Rationale

Distinct URLs and signal IDs are the strongest observable differentiators present. Without recorded timing or analyzed content, consolidation would exceed what the evidence supports and would elevate Risk Posture to High. The batch manifest is triage convenience only, not governance proof.

### Reconsideration

Consolidation may be reconsidered only if new observable evidence is recorded — for example, publish timestamps showing temporal proximity, or content analysis documenting shared keywords or motifs — in a new or superseding Observer Review. Any reconsideration requires Human Review before consolidation is finalized.

---

## References

- `OBSERVER_GUIDELINES.md`
- `observer_reviews/observer_event_creation_policy.md`
- `observer_reviews/observer_confidence_framework.md`
- `observer_reviews/2026-07-03_white_rabbit_batch001_observer_review.md`
- `execution_boundary.md`
- `notes/04 VECTOR/STAGE5B_EXTERNAL_ACTIVATION_SIGNAL_SPEC.md`
- `signals/2026-07-02_white_rabbit_cascade_observer_event.md` (higher-evidence consolidation precedent)

---

Version: v1.0  
Date: 2026-07-03

Human judgment remains authoritative.
