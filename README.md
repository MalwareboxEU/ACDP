<div align="center">

# ACDP

### Actor-Centric Defensive Prioritization

*A methodology for aligning defensive investment with adversary intent.*

[**Read the Paper**](https://acdp.malwarebox.eu) · [**Original Blog Post**](https://blog.synapticsystems.de/actor-centric-defensive-prioritization-acdp/) · [**Malwarebox**](https://malwarebox.eu)

---

</div>

## What is ACDP?

ACDP is a prioritization methodology that sits **one level above** existing security frameworks like MITRE ATT&CK, NIST CSF, and FAIR. It uses their outputs as inputs and produces a **reasoned ordering** of defensive actions against a specific adversary - based on how much each action disrupts that adversary's strategy and how much damage it prevents if that disruption fails.

ACDP does not replace existing frameworks. It resolves a gap they leave open: **which of these recommended controls should I implement first, against the adversary I actually face?**

> *If defensive priorities do not change when attacker intent changes, prioritization is no longer strategic.*

## Why It Exists

Threat intelligence has matured rapidly. Defensive planning has not kept pace. Organizations frequently operate with sound technical controls and comprehensive best-practice catalogs, yet still fail to prevent strategic losses - because their defensive investments are not aligned with the specific objectives of the adversaries they face.

Most prioritization today happens implicitly, inherited from compliance frameworks, risk heat maps, or executive preference. When attacker intent shifts, these prioritizations rarely shift with it. ACDP exists to make prioritization **explicit, defensible, and actor-aware**.

## The Four Scoring Axes

Each candidate control is scored on a 1–5 ordinal scale across four axes:

| Axis | Name | Question |
|------|------|----------|
| **ADV** | Actor Disruption Value | How strongly does this control interfere with the actor's strategy? |
| **IRR** | Impact Risk Reduction | How much damage does this prevent if the actor succeeds elsewhere? |
| **CC** | Cost and Complexity | How realistic is implementation under current constraints? |
| **DDT** | Detection-to-Decision Time | Does this provide signal early enough to change outcomes? |

## Actor-Specific Weighting

Axes are weighted according to the adversary being prioritized against. The weighting **encodes strategic assumptions** about the adversary's objectives. For a destructive, state-aligned actor prioritizing strategic impact over access longevity, the recommended weighting is:

| Axis | Weight | Rationale |
|------|:------:|-----------|
| ADV  | 35% | Disrupting strategy is primary goal |
| IRR  | 35% | Catastrophic outcomes must be bounded |
| CC   | 15% | Feasibility constraint, not primary driver |
| DDT  | 15% | Detection valuable but recovery trumps it |

The weighting is **not fixed**. Against a financially motivated crimeware operator prioritizing access persistence, DDT weighting rises significantly. Against an espionage actor prioritizing stealth, IRR weighting drops relative to ADV. The key property of ACDP is not any specific weighting but the requirement that weightings be made **explicit and justified** against an adversary profile.

## Priority Index

```
PI = (ADV × w_adv) + (IRR × w_irr) + (CC × w_cc) + (DDT × w_ddt)
```

Controls are ranked by PI and grouped into tiers:

| Tier | PI Range | Meaning |
|------|:--------:|---------|
| Tier 1 | ≥ 4.0 | Immediate - implement now |
| Tier 2 | 3.0 – 3.99 | Planned - implement next cycle |
| Tier 3 | 2.0 – 2.99 | Deferred - implement if resources permit |
| Tier 4 | < 2.0 | Deprioritized - revisit only if actor profile changes |

## Worked Example · Sandworm

Six common defensive controls scored against a destructive state-aligned adversary profile:

| Control | ADV | IRR | CC | DDT | PI | Tier |
|---------|:---:|:---:|:--:|:---:|:--:|:----:|
| Immutable offline backups      | 5 | 5 | 3 | 5 | **4.70** | Tier 1 |
| Edge inventory & patching      | 4 | 4 | 4 | 4 | **4.00** | Tier 1 |
| Historical DNS analysis        | 4 | 3 | 4 | 4 | **3.75** | Tier 2 |
| PowerShell logging             | 3 | 2 | 4 | 3 | **2.80** | Tier 2 |
| Scheduled task auditing        | 3 | 2 | 3 | 3 | **2.75** | Tier 3 |
| Awareness training             | 1 | 1 | 5 | 1 | **1.90** | Tier 4 |

Several results conflict with conventional prioritization wisdom, and **this conflict is the analytical value of the methodology**:

- Recovery outranks detection against a destructive actor
- Infrastructure hygiene outranks user behavior
- Boring controls dominate; visibility and effectiveness are weakly correlated
- Awareness training ranks last - against *this* adversary, not in general

## Positioning Relative to Existing Frameworks

ACDP is designed to compose with existing frameworks, not compete with them.

| Framework | What It Answers | What ACDP Adds |
|-----------|-----------------|----------------|
| MITRE ATT&CK | What adversaries do | Which counter-techniques to prioritize |
| NIST CSF / CIS | What good practice looks like | Which controls matter most now |
| FAIR / risk quant | What impact costs | Which impacts a specific actor targets |
| IIM | How infrastructure is structured | How to defend against that structure |

ACDP consumes the outputs of these frameworks and produces a prioritization that is **actor-specific, explicit, and adaptable** when the threat landscape moves.

## Limitations

ACDP is deliberately opinionated and carries corresponding limitations:

- **Depends on threat intelligence quality.** Incorrect assumptions about adversary intent produce systematically misaligned priorities.
- **Does not design controls.** It orders candidate controls. Baseline security hygiene is assumed to exist.
- **Resists exhaustive coverage.** It forces decisions about what *not* to prioritize - which may be politically difficult in organizations invested in comprehensive compliance narratives.
- **Weighting is judgment.** The weighting vector encodes strategic assumptions that cannot be derived from data alone. ACDP requires these be explicit; it does not eliminate them.

## Integration with Malwarebox

ACDP is part of the [**Malwarebox**](https://malwarebox.eu) research ecosystem, alongside:

- [**Kraken**](https://kraken.malwarebox.eu) - continuous observation platform for adversary infrastructure
- **IIM** - Infrastructure Intelligence Model for structural adversary analysis
- **ACDP** - this methodology

The three components form an analytical loop: observation produces structure (Kraken → IIM), structure produces priority (IIM → ACDP), and priority drives further observation (ACDP → Kraken). ACDP is usable standalone with threat intelligence from any source. Integration with the broader ecosystem amplifies its precision by making inputs auditable and scoring reproducible across teams and over time.

## Paper

The full technical paper is available at [**acdp.malwarebox.eu**](https://acdp.malwarebox.eu).

It covers the methodology in depth, including:

- Positioning relative to behavioral catalogs, control frameworks, and risk quantification
- The ACDP perspective - three framing shifts from conventional prioritization
- Detailed scoring framework with axis anchor descriptions
- Actor-specific weighting rationale
- Full Sandworm worked example
- Limitations and trade-offs
- Integration with broader intelligence workflows

## Contributing

ACDP is an open methodology. Contributions are welcome in several forms:

- **Actor profile templates** - weighting vectors and rationale for specific threat actor categories
- **Worked examples** - fully scored prioritizations against named adversaries
- **Critique** - challenges to the methodology, its assumptions, or specific scoring decisions
- **Tooling** - scripts, templates, or integrations that automate parts of ACDP workflows

Open an issue or pull request. Substantive critique is explicitly welcomed - the value of the methodology is that its outputs are **contestable**.

## Citation

If you reference ACDP in published work, please cite:

```
Dost, R. (2026). Actor-Centric Defensive Prioritization (ACDP):
A Methodology for Aligning Defensive Investment with Adversary Intent.
Malwarebox Research. https://acdp.malwarebox.eu
```

## License

This methodology and its documentation are released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). You may use, adapt, and redistribute it freely with attribution.

---

<div align="center">

**Malwarebox** · *In varietate concordia* 🇪🇺

[Website](https://malwarebox.eu) · [Blog](https://blog.synapticsystems.de) · [Kraken](https://kraken.malwarebox.eu)

</div>
