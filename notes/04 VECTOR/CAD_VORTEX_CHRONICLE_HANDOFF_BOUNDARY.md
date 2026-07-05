# CAD-Vortex → Chronicle Handoff Boundary

## 1. Purpose

Define the boundary between **CAD-Vortex** (future intake system) and **vector-signal-chronicle** (observation layer).

This document specifies what CAD-Vortex may hand off, what provenance must accompany a handoff, and what the chronicle must not do with received packages.

**Documentation only.** No CAD-Vortex repository exists yet. No handoff implementation is created by this document.

---

## 2. What CAD-Vortex May Hand Off

CAD-Vortex may prepare and emit **observation-candidate handoff packages** derived from preserved source material indexed in the External Research Archive.

A handoff package may include:

- structured metadata describing the source material
- provenance references back to ERA inventory and source dumps
- descriptive labels, timestamps, and intake classification
- pointers to chronicle-eligible content (not raw archive imports)

A handoff package may **not** include:

- raw archive artifact imports into the chronicle repository
- pre-approved Observer decisions
- validation-admissible evidence claims
- execution proposals or runtime directives
- rewritten or substituted provenance

---

## 3. Required Provenance Fields

Every CAD-Vortex handoff package must carry the following provenance fields. These fields anchor the observation candidate to its preservation source without importing raw archive content.

| Field | Meaning |
|-------|---------|
| `era_inventory_ref` | Reference to the ERA inventory record that catalogs the source material. |
| `era_source_dump_id` | Identifier of the specific ERA source dump containing the material. |
| `era_relative_path` | Path of the item within the source dump, relative to dump root. |
| `era_sha256` | SHA-256 digest of the source item at preservation time. |
| `cad_vortex_inventory_id` | CAD-Vortex inventory identifier for the intake record. |
| `cad_vortex_source_dump_id` | CAD-Vortex source dump identifier linking intake to ERA preservation. |

Missing provenance fields invalidate the handoff package for chronicle registration. The chronicle does not infer or reconstruct missing provenance.

---

## 4. Handoff Package Status

### 4.1 Observation-candidate only

A handoff package is an **observation-candidate only**. It is not a chronicle entry, not an Observer Event, and not governance evidence.

CAD-Vortex handoff does **not** create a `ChronicleRecordV0` automatically. Chronicle record creation requires separate Observer review and human judgment.

### 4.2 REGISTERED ≠ Observer-approved

A handoff package may be marked **REGISTERED** when it has been received and indexed as an observation candidate. **REGISTERED** means intake acknowledgment only — not Observer approval.

### 4.3 Observer-approved ≠ Validation-admissible

An Observer-approved observation candidate is suitable for chronicle recording and assessment. It is **not** automatically validation-admissible in AI_Lab. Validation admission requires explicit AI_Lab boundary review.

---

## 5. Boundary Rules

### 5.1 Handoff must not bypass AI_Lab boundary review

No handoff path may skip AI_Lab validation review when material is proposed for operational adoption. Observation → Validation crossing requires explicit review.

### 5.2 Chronicle does not rewrite provenance

The chronicle preserves provenance fields as received. It does not modify, substitute, or reconstruct ERA or CAD-Vortex provenance anchors. If provenance is incorrect at handoff time, the package must be rejected or re-handled at intake — not corrected silently in the chronicle.

### 5.3 Chronicle does not rewrite evidence

The chronicle records what was observed and assessed. It does not alter source material, re-encode archive content, or present intake metadata as independent runtime evidence.

### 5.4 No raw archive artifacts are imported

Handoff packages carry references and metadata only. Raw files from External Research Archive are not copied into vector-signal-chronicle. ERA remains the preservation locus.

---

## 6. Processing Flow (Reference)

```
External Research Archive (Preservation)
    ↓
CAD-Vortex (Intake) — future; no repository yet
    ↓
Handoff package (observation-candidate + provenance)
    ↓
vector-signal-chronicle (Observation)
    ↓
Observer Review
    ↓
Chronicle entry (if approved)
    ↓
AI_Lab (Validation) — explicit boundary review required
    ↓
vector-runtime-governance-public (Publication)
```

No stage on this path automatically crosses into validation or publication. Each boundary requires explicit human review.

---

## 7. Deferred Work

The following items are **out of scope** for this documentation-only boundary spec:

| Item | Notes |
|------|--------|
| CAD-Vortex repository | No repository exists yet. |
| Handoff implementation | No code, API, or schema enforcement. |
| ChronicleRecordV0 schema | Referenced conceptually; not implemented. |
| ERA integration | External Research Archive is not modified. |
| Raw file import | Explicitly prohibited. |
| Automated Observer approval | Human judgment remains required. |
| AI_Lab wiring | AI_Lab is not modified. |

---

## Version

- **Spec:** CAD-Vortex → Chronicle Handoff Boundary (documentation only)
- **Status:** Draft / not implemented
- **Authority:** Human judgment remains final
