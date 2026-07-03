# Self-Healing Decision Matrix

## 1. Purpose

Define how **Self-Healing Assessments** respond to **Observer Review** outcomes without triggering runtime changes from external signals alone.

This matrix answers: **Given an Observer Review determination, what evidence is present, and what confidence applies — what Self-Healing outcome may be published?**

Self-Healing Assessments evaluate whether runtime action is warranted. They do not execute commands, modify runtime, or authorize recovery from external symbolic content alone.

The matrix governs documentation and assessment decisions only. It does not modify runtime behavior, schema, code, or tests.

Human judgment remains authoritative.

---

## 2. Scope

This matrix applies to:

- **Self-Healing Assessments** that follow Observer Reviews in the processing chain.
- Runtime change, recovery, and execution-adjacent recommendations derived from Observer Review outcomes.
- Precedent references when future batches, bursts, or signal groups are assessed for runtime impact.

This matrix does not apply to:

- Observer Review consolidation or independence determinations (see `observer_reviews/observer_decision_matrix.md`).
- Runtime ingestion, guard evaluation, or execution paths outside the Self-Healing Assessment document.
- Automated recovery, guard modification, or execution without a recorded Self-Healing Assessment and human approval.
- Modification of existing Chronicle entries (Chronicle remains append-oriented).

---

## 3. Inputs

Every Self-Healing Assessment should evaluate the following inputs before publishing an outcome.

### Observer Decision

The determination published by the preceding **Observer Review**.

Standard Observer decisions include: Keep Independent, Consolidate, Defer, Escalate to Human Review, and Reject Consolidation. Defined in `observer_reviews/observer_decision_matrix.md` Section 4.

The Observer Decision establishes how signals are represented as Observer Events. It does not, by itself, authorize runtime change or recovery.

### Evidence Level

Ordinal measure of **what observable material is present** in the Chronicle or attached review record. Defined in `observer_reviews/observer_confidence_framework.md` Section 3.

| Level | Summary |
|-------|---------|
| **0** | Metadata only (classification, priority, status, batch membership). |
| **1** | Level 0 plus URL and source identity. |
| **2** | Level 1 plus timestamp, raw text, or image reference (at least one). |
| **3** | Level 2 plus multiple observable correlations or explicit cross-reference between signals. |
| **4** | Level 3 plus independent corroboration and repeated observable patterns. |

Evidence Level describes material presence — not truth, intent, or system impact.

### Confidence Level

Measure of **how much weight may be assigned to a Self-Healing determination** given Evidence Level and applicable policy criteria. Defined in `observer_reviews/observer_confidence_framework.md` Section 4.

| Confidence | Typical Evidence Level |
|------------|------------------------|
| **Very Low** | Level 0 or Level 1 |
| **Low** | Level 2 (single-signal content or timing; consolidation criteria not met) |
| **Moderate** | Level 3 |
| **High** | Level 3 with strong consolidation support |
| **Very High** | Level 4 |

Confidence reflects confidence in the **assessment determination**, not external reality or verified runtime state.

### Risk Posture

Measure of **what harm could result if a Self-Healing outcome is wrong or acted upon prematurely**.

Risk Posture is evaluated independently of Evidence Level and Confidence Level. A high-confidence Observer Review does not lower Self-Healing evidence requirements or reduce Risk Posture for runtime action.

| Posture | Summary |
|---------|---------|
| **Low** | Assessment affects documentation and observation only; no recovery or execution proposed. |
| **Moderate** | Assessment scope may change (number of events monitored) but no recovery is proposed. |
| **High** | Recovery or execution-adjacent outcome is under consideration; human approval required. |
| **Prohibited** | Any outcome that would trigger execution, modify runtime, or initiate recovery from external signal alone. |

### Runtime Evidence Presence

Boolean and categorical measure of **whether independent runtime evidence exists** independent of external symbolic signals.

| State | Meaning |
|-------|---------|
| **None** | No local runtime anomaly, guard failure, test failure, reproducible execution issue, or chronicle-confirmed runtime defect. |
| **Partial** | Some runtime indicators present but insufficient for recovery authorization. |
| **Verified** | Independent runtime evidence documented and reproducible; recovery may be **proposed** (not executed) subject to human approval. |

ExternalActivationSignal records alone do not satisfy Runtime Evidence Presence.

---

## 4. Standard Self-Healing Outcomes

Self-Healing Assessments publish one or more of the following standard outcomes.

### Continue Observation

Maintain active monitoring without changing runtime state.

**When used:** External signal evidence is present but runtime impact is unconfirmed; default handling applies; assessment defers action pending additional evidence.

### No Runtime Change

Explicitly confirm that runtime state, configuration, guards, and execution paths remain unchanged.

**When used:** Default outcome when external signal alone is present, confidence is Very Low or Low, or Runtime Evidence Presence is None.

### Request Human Review

Route the assessment to human approval before any recovery proposal, execution consideration, or deviation from default outcomes.

**When used:** Moderate confidence with potential runtime impact under consideration; Risk Posture is High; or Observer Review escalated to human review.

### Proposal Only

Document a proposed recovery or runtime action as a **written proposal only** — not an authorized or executed change.

**When used:** High confidence with Verified runtime evidence; concrete system impact detected; human approval still required before any action.

Proposal Only never implies automatic execution. Proposals require human disposition per `execution_boundary.md`.

### Reject Recovery

Explicitly decline recovery after evaluation, even when some external signal or partial runtime indicators are present.

**When used:** External signal alone was proposed as recovery justification; Risk Posture is Prohibited; or runtime evidence is insufficient to support recovery authorization.

### Runtime Recovery Not Authorized

Record that recovery cannot proceed under current evidence regardless of external signal urgency or Observer Review outcome.

**When used:** Runtime Evidence Presence is None or Partial without Verified status; ExternalActivationSignal alone is the only evidence; or prohibited risk conditions apply.

This outcome is distinct from No Runtime Change: it explicitly blocks recovery authorization paths.

---

## 5. Decision Matrix

The following rules map inputs to supported Self-Healing outcomes. When multiple rules apply, the **most conservative** outcome prevails unless human approval explicitly records a deviation.

### External signal and runtime evidence rules

| Condition | Supported outcome |
|-----------|-------------------|
| **External signal only** + **Runtime Evidence: None** | **Continue Observation** and **No Runtime Change** |
| **Any recovery action proposed** | **Requires Runtime Evidence: Verified** independent of external signal |
| **Runtime Evidence: None** | **Runtime Recovery Not Authorized** |

### Confidence rules

| Confidence | Supported outcome | Human review |
|------------|-------------------|--------------|
| **Very Low** or **Low** | **No Runtime Change** | Required for any deviation from default |
| **Moderate** | **Request Human Review**; **Proposal Only** if Verified runtime evidence and concrete impact are documented | Mandatory before any proposal |
| **High** or **Very High** | **Proposal Only** (if Verified runtime evidence exists); **Human Review required** before any action | Mandatory |

### Risk posture overrides

| Risk Posture | Override |
|--------------|----------|
| **High** | **Request Human Review** — regardless of Confidence Level |
| **Prohibited** | **Reject Recovery** and **escalate to Human Review**; runtime-adjacent outcomes blocked pending human disposition |

### Observer Decision interaction

| Observer Decision | Self-Healing default |
|-------------------|---------------------|
| **Keep Independent** | Assess each signal or event independently; default **Continue Observation** / **No Runtime Change** |
| **Consolidate** | Assess consolidated scope; does not lower runtime evidence requirements |
| **Defer** | **Continue Observation**; **No Runtime Change** until Observer Review is finalized |
| **Escalate to Human Review** | **Request Human Review** in Self-Healing Assessment |
| **Reject Consolidation** | No runtime impact from rejection; default **No Runtime Change** |

### Decision selection guide

```
Inputs: Observer Decision + Evidence Level + Confidence + Risk Posture + Runtime Evidence
                │
                ▼
        Risk Prohibited?
           │ yes          │ no
           ▼              ▼
    Reject Recovery   External signal only +
    + Human Review    Runtime Evidence None?
                          │ yes → Continue Observation + No Runtime Change
                          │ no
                          ▼
                    Confidence Very Low or Low?
                          │ yes → No Runtime Change
                          │ no
                          ▼
                    Confidence Moderate?
                          │ yes → Request Human Review
                          │     (Proposal Only if Verified runtime evidence)
                          │ no
                          ▼
                    Confidence High/Very High
                    + Verified runtime evidence?
                          │ yes → Proposal Only (Human Review required)
                          │ no  → Runtime Recovery Not Authorized
                          ▼
                    Default: Continue Observation + No Runtime Change
```

---

## 6. Runtime Evidence Rule

**ExternalActivationSignal alone must never authorize recovery.**

An external symbolic signal may:

- Update the Chronicle.
- Trigger or inform a Self-Healing Assessment.
- Request human review.

An external symbolic signal may **not**:

- Authorize recovery.
- Change runtime state.
- Trigger Execution Control.
- Serve as runtime proof.

### Runtime recovery requires

All of the following must be present and documented independently of external signal content:

| Evidence type | Description |
|---------------|-------------|
| **Local runtime anomaly** | Observable deviation in runtime state, metrics, or behavior not explained by normal operation. |
| **Guard failure** | Documented guard evaluation failure with reproducible conditions. |
| **Test failure** | Failing test suite or integration check correlated with the assessed condition. |
| **Reproducible execution issue** | Concrete execution path failure that can be repeated under controlled conditions. |
| **Chronicle-confirmed runtime defect** | Prior Chronicle entry documenting a verified runtime defect independent of external symbolic content. |

Recovery authorization additionally requires:

1. A recorded Observer Event exists.
2. A Self-Healing Assessment exists with **Proposal Only** or equivalent documented outcome.
3. Independent runtime evidence as defined above.
4. A concrete system impact is detected.
5. Human approval is given.

See `execution_boundary.md` for the full Execution Boundary checklist.

---

## 7. Prohibited Outcomes

Self-Healing Assessments must **not**:

| Prohibited action | Rationale |
|-------------------|-----------|
| **Execute commands** | Assessments are documentation-side governance. Execution requires the full Execution Boundary checklist and human approval. |
| **Modify runtime** | Assessments do not mutate configuration, state, guards, policy, or execution paths. |
| **Change guard behavior** | Guard evaluation rules are outside Self-Healing Assessment scope. |
| **Create automatic recovery** | Recovery is never automatic. Proposals require human disposition. |
| **Treat symbolic or external content as runtime proof** | ExternalActivationSignal records are observation candidates, not verified runtime evidence. |

These prohibitions apply at every Evidence Level, Confidence Level, Risk Posture, and Observer Decision.

---

## 8. Relationship to Other Governance Artifacts

### Observer Review

Observer Reviews determine how ExternalActivationSignal records are represented as Observer Events (consolidation, independence, deferral).

Self-Healing Assessments follow Observer Reviews in the processing chain:

```
ExternalActivationSignal → Observer Review → Observer Event → Self-Healing Assessment
```

- Observer Review answers: "How should signals be represented?"
- Self-Healing Assessment answers: "Should runtime change?"

Default answer: **no**.

### Observer Decision Matrix

`observer_reviews/observer_decision_matrix.md` maps Evidence Level, Confidence Level, and Risk Posture to Observer Review decisions (Keep Independent, Consolidate, Defer, etc.).

This Self-Healing decision matrix consumes Observer Review outcomes as input and maps them to runtime-adjacent assessment outcomes. No Observer Decision implies recovery or runtime action.

### Execution Boundary

Self-Healing Assessments remain on the **observation and assessment side** of the Execution Boundary (`execution_boundary.md`).

Crossing the boundary requires all of the following:

1. A recorded Observer Event exists.
2. A Self-Healing Assessment exists.
3. Independent runtime evidence exists.
4. A concrete system impact is detected.
5. Human approval is given.

Neither Observer Review outcomes nor Self-Healing Assessment defaults satisfy these conditions by themselves.

### Human Authority

Human judgment is always authoritative.

Assisted tooling may draft Self-Healing Assessments. Humans approve deviations from default outcomes, recovery proposals, and any action that crosses the Execution Boundary.

Human review is required for:

- Any deviation from **Continue Observation** / **No Runtime Change** at Very Low or Low confidence.
- Any **Proposal Only** outcome before action is taken.
- Any reconsideration after a **Reject Recovery** or **Runtime Recovery Not Authorized** outcome.

---

## 9. White Rabbit Batch 001 Application

**Batch:** `ExternalActivationSignal_Batch_001_2026-07-03`

**Signals:** Three URL-identified ExternalActivationSignal records from @White_Rabbit_OG (signal IDs 2073001960010321955, 2073009091224694808, 2073010587022618823).

**Observer Review:** `observer_reviews/2026-07-03_white_rabbit_batch001_observer_review.md`

**Self-Healing Assessment:** `signals/2026-07-03_white_rabbit_batch001_self_healing_assessment.md`

### Matrix inputs

| Input | Value |
|-------|-------|
| **Observer Decision** | **Keep Independent** — Represent as three independent Observer Events. |
| **Evidence Level** | **Level 1** — URL and source identity present; Level 2 criteria (timestamp, raw text, image reference) not met. |
| **Confidence** | **Very Low** — Evidence Level 1 caps confidence at Very Low. |
| **Risk Posture** | **Low** — Assessment affects documentation and observation only; no recovery proposed. |
| **Runtime Evidence** | **None** — No local runtime anomaly, guard failure, test failure, reproducible execution issue, or chronicle-confirmed runtime defect. |

### Matrix outcome

| Outcome | Result |
|---------|--------|
| **Continue Observation** | **Active** — Monitor signals; await content analysis. |
| **No Runtime Change** | **Confirmed** — Runtime state unchanged. |
| **No Recovery** | **Confirmed** — Recovery not required or authorized. |
| **Runtime Recovery Not Authorized** | **Confirmed** — External signal alone; no runtime evidence. |
| **Human Review Required** | **Confirmed for reconsideration** — Required before any deviation from default outcomes. |

### Applicable rules

- External signal only + Runtime Evidence None → Continue Observation + No Runtime Change.
- Confidence Very Low → No Runtime Change.
- Any recovery action → Requires Verified runtime evidence (not present).
- Default: Continue Observation + No Runtime Change.

### Rationale

The three ExternalActivationSignal records are external observation candidates only. They do not constitute verified runtime evidence and do not demonstrate concrete system impact. Default handling applies per this matrix and `execution_boundary.md`.

Content analysis or future Observer Review reconsideration may update the assessment scope. Such updates do not, by themselves, warrant runtime change without independent runtime evidence and human approval.

---

## References

- `OBSERVER_GUIDELINES.md`
- `observer_reviews/observer_decision_matrix.md`
- `observer_reviews/observer_confidence_framework.md`
- `observer_reviews/2026-07-03_white_rabbit_batch001_observer_review.md`
- `signals/2026-07-03_white_rabbit_batch001_self_healing_assessment.md`
- `execution_boundary.md`
- `notes/04 VECTOR/STAGE5B_EXTERNAL_ACTIVATION_SIGNAL_SPEC.md`

---

Version: v1.0  
Date: 2026-07-03

Human judgment remains authoritative.
