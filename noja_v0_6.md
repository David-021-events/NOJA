# Nodes of Judgement Architecture

## **Specification v0.6**

---

### **Status**

Draft. v0.6 supersedes v0.5. This document specifies an architecture for systems that combine machine prediction with accountable human judgment. It is intended for technical and governance practitioners building or auditing such systems.

### **Conventions**

This specification distinguishes **normative** from **non-normative** material. Normative sections define requirements that a conformant system must satisfy. Non-normative sections (marked as such, or appearing as commentary, examples, or rationale) explain and motivate but do not impose requirements.

The keywords MUST, MUST NOT, SHOULD, SHOULD NOT, and MAY in normative sections are to be interpreted as in RFC 2119: MUST denotes an absolute requirement; SHOULD denotes a strong recommendation that may be ignored only with documented justification; MAY denotes a permitted but optional behavior.

---

## **1\. Thesis**

*(Non-normative.)*

Agentic AI is currently designed by collapsing three distinct functions — prediction, judgment, and execution — into a single end-to-end process. Many legacy systems collapse these functions too, by accretion rather than design; §7's audit commentary returns to that condition. What is unique to current AI discourse is the treatment of collapse as a design goal rather than a defect. Prior high-reliability decision systems, from nuclear command to aviation autopilots to financial risk infrastructure, separated these functions explicitly and anchored accountability in identified humans.

Nodes of Judgement Architecture (NOJA) specifies the separation. Within a unit called a *judgment node*, a system predicts, judges, executes, and assigns accountability as four distinguishable functions. Nodes compose into directed networks in which one node's execution becomes another node's prediction. Accountability terminates in a signed artifact that traces back to a named human or institutional role.

NOJA's contribution is structural, not philosophical. It draws on Simon's decision theory, the prediction-economics of Agrawal, Gans and Goldfarb, AI ethics work by Floridi and Dignum, and the High Reliability Organization literature of Weick and Sutcliffe. What it adds is an operational vocabulary and a conformance posture that allow systems to be designed, audited, and revised while remaining accountable at machine speed.

---

## **2\. The judgment node**

*(Normative.)*

A **judgment node** is the unit of structure in NOJA. Every judgment node MUST contain four distinguishable functions, logically ordered for accountability though implementation MAY interleave them, and bounded by a single accountable role.

### **2.1 Prediction**
→ [Diagram: The Judgment Node](https://David-021-events.github.io/YOUR-REPO/diagrams/judgment_node_diagram.html)

The **prediction** function produces probabilities, classifications, forecasts, completions, or candidates describing what is or might be the case. Prediction MUST NOT prescribe action.

A prediction layer MAY carry latent policy commitments — biases, refusal behaviors, value alignments laminated into model weights through training. A conformant system MUST disclose these latent commitments to the extent they are knowable, and MUST treat them as part of the governance trail subject to the same audit requirements as explicit policy. A prediction layer's latent policy commitments do not satisfy the requirements of the judgment function in §2.2.

*Commentary (non-normative).* The strict version of this requirement — that prediction layers contain no implicit policy — is unattainable for systems built on modern language models. The honest version, adopted here, is that latent policy must not be invisible. A system that documents its model's value commitments and audits behavior against them is conformant on this point; a system that ignores them is not.

### **2.2 Judgment**

The **judgment** function applies policy, values, risk tolerance, and context to predictions to produce decisions. Judgment MUST occur in one of two modes:

- **Design-time judgment.** Policies, thresholds, and escalation rules are encoded in advance into a signed artifact, and executed by automation across many instances without further human intervention until the signature is withdrawn or superseded.  
    
- **Human-in-the-loop judgment (HITL).** A live instance routed by the system to a named human is decided by that human, whose signature attaches to the instance.

Both modes are of equal legitimacy when both carry explicit signatures conformant to §4. Judgment includes the authority to deviate from prediction; the prediction is input, not constraint.

*Commentary (non-normative).* HITL is sometimes treated as the higher-trust mode merely because a human is present at the moment of decision. It is not. A system with thin design-time policy and a HITL backstop is not safer than a system with clean design-time policy and a documented escalation path; it is just slower at being unsafe. The more reliable safety mechanism is the human in a quiet room signing logic before it touches live data — not the human catching mistakes in a real-time stream.

### **2.3 Execution**

The **execution** function carries out the decision via one of four mechanisms: deterministic rules, structured algorithms, adaptive agents, or direct human action. The first three are automation; the fourth is manual work.

Execution mechanism selection MUST match the determinism requirements of the decision. Where deterministic behavior is required, execution SHOULD be implemented as rules or algorithms rather than agents. Agents introduce stochastic non-determinism and are appropriate only when the decision space is genuinely open-ended and benefits from adaptive multi-tool coordination.

Agentic execution may internally recurse, plan, self-critique, re-plan, and dynamically spawn subagents or tools. These internal behaviors do not by themselves constitute additional judgment nodes; they are execution machinery within the parent node. The granularity rule (§5) determines when distinct accountable judgment within an agentic process forces decomposition into separate nodes.

### **2.4 Accountability**

Each judgment node MUST have exactly one **accountable role** — a named human or institutional position whose signature terminates the authority chain for the node's outcomes. Accountability MUST NOT be shared across multiple roles within a node. Compound and delegated signature patterns are admitted under §4.5; these decompose to a single accountable role.

Machines MAY simulate agency but MUST NOT bear accountability. Accountability cannot be automated even when judgment is partially encoded.

---

## **3\. The judgment network**

*(Normative.)*

A real system is a **judgment network**: a directed graph of judgment nodes in which one node's execution produces an input that becomes another node's prediction.

A conformant judgment network MUST satisfy three structural properties.

### **3.1 Single ownership per node**

Every node MUST have exactly one accountable role. Where governance committees or boards are involved, they MUST appear as upstream judgment nodes producing signed artifacts that downstream nodes consume — not as shared owners of downstream nodes.

*Commentary (non-normative).* Cosigning patterns common in regulated domains — Compliance and Independent Risk cosigning a credit policy, Legal and Product cosigning a terms-of-service update — decompose under this requirement when the cosigners apply distinct judgment types. A Compliance review is its own upstream judgment node producing a signed compliance opinion that the downstream node consumes; the downstream node remains owned by a single role. Where cosigners apply the *same* judgment type and the cosigning is purely procedural — a four-eyes rule on the same policy text — §4.6's compound signature pattern applies and a single node suffices. The distinction is judgment-type, not headcount.

### **3.2 Edge traceability**

Every edge in the network MUST be traceable. From any execution event, the path back through prediction, judgment artifact, signature, and accountable role MUST be reconstructible without gaps. Edges that cannot be traced render the network non-conformant regardless of the conformance status of its individual nodes.

### **3.3 Recursion**

The network MAY recurse to arbitrary depth. A node's encoded judgments MAY themselves be the execution outputs of upstream judgment nodes — for example, a policy committee whose signed decisions become the rules a downstream automated system enforces. Recursion does not relax any other requirement; each node in a recursive chain MUST independently satisfy all requirements of §2.

### **3.4 The escalation handoff**

*(Non-normative pattern.)* The most common composition pattern in high-stakes domains pairs a design-time signer who establishes encoded policy for routine cases with a named HITL role for cases that breach defined thresholds. NOJA names this the *escalation handoff*. Nuclear command, aviation autopilot, and credit underwriting implement variants. The escalation thresholds are themselves part of the design-time signed artifact and are an act of encoded judgment in their own right.

→ [Diagram: The Escalation Handoff](https://David-021-events.github.io/YOUR-REPO/diagrams/escalation_handoff_diagram.html)

---

## **4\. The signature lifecycle**

*(Normative.)*

A **signature** is the mechanism by which accountability is anchored in a named role. The signature is the load-bearing concept in NOJA: it is what allows a system to operate at machine speed while remaining accountable at human speed.

### **4.1 Signature states**

Every signature MUST exist in exactly one of the following states. State transitions MUST themselves be signed events, recorded with signer, trigger, timestamp, and resulting fallback (where applicable).

| State | Meaning | Execution permitted |
| :---- | :---- | :---- |
| Drafted | Artifact exists; no signature | No |
| Provisional | Signed for shadow / observation only | Logged only, no live action |
| Active | Signed for live execution | Yes |
| Suspended | Execution paused with intent to resume | No; fallback applies |
| Lapsed | Scope conditions (§4.3) no longer obtain | No; fallback applies until re-signing |
| Revoked | Signature withdrawn | No; fallback applies |
| Superseded | Replaced by signed successor | Successor's terms apply |
| Archived | Retained for audit; no live role | No |

Superseded is distinguished from Revoked-with-cut-over by intent: Superseded marks an orderly policy update where the successor was ready at the moment of transition and no fallback was invoked, while Revoked marks a withdrawal for cause where the fallback set is engaged. Both may hand execution to a named successor; preserving the distinction in the state itself keeps orderly succession — the modal case — from being recorded as an exceptional event.

Required transitions: Drafted → Provisional → Active is the standard onboarding path; skipping Provisional MUST be explicitly justified in the signature record. Active → any other state is permitted. Archived is reachable only after live execution governed by the signature has fully drained.

### **4.2 Fallback specification**

Every signature in the Active state MUST name a set of acceptable **fallbacks** — pre-signed policies that govern execution in the interval between the active signature ceasing (through suspension, lapse, or revocation) and a successor being signed.

The signed fallback set MUST include at least one of:

- **Halt.** Execution stops; in-flight items queue for human review.  
- **Safe-mode.** A pre-signed minimal-action policy takes over (e.g., decline all, log only).  
- **Drain-then-halt.** In-flight executions complete under the prior policy; no new executions enter. A maximum drain time MUST be specified.  
- **Cut-over.** Immediate handoff to a named, already-signed successor.

When a transition out of Active occurs, the transition event MUST select from the named fallback set, and the selection MUST itself be signed. A signature in Active state without a named fallback set is non-conformant.

*Commentary (non-normative).* The reason fallback selection is a menu rather than a single pre-commitment is that revocations have heterogeneous causes. A revocation triggered by a discovered fairness defect calls for an immediate safe-mode response; a revocation triggered by a disclosure language update can safely drain. Forcing a single pre-committed fallback would make the framework brittle. Forcing the fallback to come from a pre-signed menu preserves rigor without forcing premature commitment. The fallback set is to revocation what scope is to validity: both make the conditions of handoff explicit at signing time, so the system is never drafting policy under time pressure. A signature without a named fallback set has the same defect as a signature without a scope declaration — it pushes a decision that belongs in the signing artifact into the operational moment when it cannot be made well.

### **4.3 Scope**

Every signature MUST declare its **scope** along four axes:

- **Domain scope.** The decision class the signature governs.  
- **Temporal scope.** A duration or trigger after which the signature requires re-affirmation. A default re-affirmation interval (e.g., quarterly) SHOULD be specified for any signature without an explicit expiry.  
- **Condition scope.** The conditions under which the signature's authority obtains. Condition scope MUST be specified along two sub-axes:  
  - *Environmental conditions* — observable facts about the world (e.g., default rates within a stated band, regulation unchanged, third-party-provider attestation policies unchanged).  
  - *Behavior-envelope conditions* — observable facts about the system's own behavior under sampled inputs (e.g., model version pinned, prompt-injection attack rate below threshold).  
- **Volume scope.** Cumulative volume beyond which review is required.

When any scope axis ceases to obtain, the signature MUST transition to Lapsed. Lapse is detected through monitoring against the scope declaration; the monitoring discipline is part of the audit (§7).

*Commentary (non-normative).* Scope addresses the failure mode where a policy signed under one set of conditions continues to govern execution after those conditions have drifted. A policy validated under one default-rate environment continues to govern when defaults move outside that band; a model authorized for one input distribution continues to run when the distribution shifts. A policy doesn't decay because time passes — it decays because the conditions it was signed under no longer hold.

### **4.4 Mechanics**

A signature, by whatever mechanism, MUST satisfy five properties:

- **Artifact-bound.** The signature attaches to specific bytes (or content hash) of the policy artifact, not to "the policy" abstractly. Modifications invalidate the signature.  
- **Identity-verifiable.** The signer's identity is checkable independently of the system that records the signature.  
- **Timestamped.** By an authority outside the artifact's own control.  
- **Tamper-evident.** The audit log can detect unauthorized modification.  
- **Reconstruction-complete.** From any executed instance, the chain back through fallback states, transitions, and signing events can be replayed.

These properties admit multiple implementations: organizational sign-off in a workflow tool with strong access control (acceptable for low-stakes, high-trust contexts); cryptographic signature on an artifact hash with PKI-backed identity (the high-stakes default); hybrid arrangements. NOJA is mechanism-neutral; conformance is property-bound.

### **4.5 Composite artifacts**

Where the executor is itself a complex object — an LLM, a multi-tool agent, a model ensemble — the signed artifact MUST be a **composite artifact** comprising at minimum:

- The policy text or specification.  
    
- Any system prompts, tool definitions, subagent-spawning policies, and configuration the executor consumes. Where the executor may dynamically spawn subagents or invoke tools at runtime, the *policy* governing what may be spawned and under what conditions is part of the signed composite, even though the spawned instances themselves are runtime state.  
    
- The model identity, version-pinned. For self-hosted models, this is the specific weights. For models accessed through a third-party provider, this is the version identifier as attested by the provider; the provider's version-pin attestation policy MUST itself be declared as an environmental scope condition under §4.3, such that a change in that policy lapses the signature. Continuously-updating models — those whose weights change through online learning between signing events — cannot satisfy version-pinning under the present specification; see §8 for the open question on adaptive systems.  
    
- A behavior envelope: a documented set of tests demonstrating policy adherence under representative inputs. The envelope MUST specify its coverage along three axes:  
    
  - *Input-distribution coverage.* Sampling drawn from the production input distribution.  
  - *Boundary coverage.* Cases at the thresholds and edges defined by the policy.  
  - *Adversarial coverage.* Patterns relevant to the deployment context (prompt injection, jailbreak families, distributional shift cases).


  The envelope's coverage specification is itself part of the signed composite; modifying coverage requires re-signing.

The signature binds to the composite as a whole. Modification of any component invalidates the signature.

*Commentary (non-normative).* This is the requirement that allows NOJA to govern LLM-based systems honestly. A signature on system prompt alone authorizes something that doesn't fully determine behavior, since the model interprets the prompt non-deterministically. Bundling the model identity and behavior envelope into the signed artifact closes the gap. Re-pinning to a new model version is a re-signing event.

The version-pin requirement is operationally fictional for hosted models in its strict form: a provider's version identifier is not a guarantee of byte-identical weights over the identifier's lifetime. The honest version, adopted here, treats the version pin as a contract with the provider whose terms are themselves subject to scope monitoring. A provider that silently updates weights under a stable identifier breaches an environmental scope condition; the signature lapses. This puts the provider's attestation policy where it belongs — in the audit trail, not in a footnote.

The behavior envelope's coverage specification carries load that the deterministic execution case carries implicitly. §5's granularity rule depends on it: for stochastic executors, signature revision is probabilistic rather than deterministic, and what makes a revision "preventing the same error from recurring" is its measurable effect on envelope performance under the same coverage axes. Without an explicit coverage specification, "the envelope passed" is unfalsifiable.

### **4.6 Compound and delegated signatures**

Signatures MAY be **compound** (requiring two or more signers) or **delegated** (a signer authorizes a deputy to sign within bounded scope). Both decompose to single accountable roles:

- A compound signature requirement is itself a signed artifact owned by the role that defined the requirement; cosigners are accountable for their own signatures; the requirement-defining role remains accountable for the requirement.  
- A delegation is itself a signed artifact with its own scope; the delegator remains accountable for actions taken under the delegation.

Signatures are **role-bound rather than person-bound**. When the human in a role changes, the role survives; the successor inherits responsibilities and re-signs active policies as part of role transition. Re-signing on role change is itself a transition event under §4.1.

---

## **5\. The granularity rule**

*(Normative.)*

NOJA does not prescribe a fixed depth of node decomposition. It does, however, impose a constraint on granularity:

**A judgment node is at correct granularity when a rejection of its output can be traced to a single signature that can be revised to prevent the same error from recurring.**

A node MUST be decomposed if either of the following holds:

- The accountable role cannot, in practice, audit the logic that produced a rejected output.  
- The judgment type changes within the node — i.e., one part of the process applies one kind of policy (e.g., quantitative risk thresholds) and another part applies a different kind (e.g., qualitative brand judgment).

A node SHOULD NOT be decomposed when doing so would merely add signature overhead without changing accountability, judgment type, or audit traceability.

For nodes whose execution function involves stochastic executors (e.g., LLM-based agents, ensembles of probabilistic models), the rule is read against the composite artifact's behavior envelope (§4.5). Revision satisfies the rule when the revised signature, evaluated against its envelope coverage axes, demonstrably reduces the error class. The behavior envelope thus carries load that deterministic execution carries implicitly: in the deterministic case, "preventing recurrence" is logically demonstrable from the revised rule; in the stochastic case, it is empirically demonstrable from the revised envelope. Either is conformant; the envelope is what makes the latter possible.

*Commentary (non-normative).* The granularity rule is what distinguishes NOJA as an architecture rather than a vocabulary. Without it, "single accountable owner" can be drawn at any level convenient to the politics of the organization, and a single massive node can absorb an entire end-to-end system with one rubber-stamp signature at the end. The rule forces granularity to follow auditability rather than org-chart convenience.

The rule also resolves the "End-to-End Agency" pattern. A monolithic agent system with one human approver at the output is technically a single judgment node, but rejections by the approver cannot be traced to any specific signature that can be revised. The node is therefore at incorrect granularity and MUST be decomposed. The presence of a HITL approver at the boundary of a monolithic agentic system does not make the internal system accountable; it only makes the approver accountable for the boundary decision. If the internal agentic process contains unrevisable or unsigned judgment-like logic, the system fails the granularity rule regardless of how the boundary is staffed.

A second consequence: worked examples presented as single nodes typically decompose under review. A loan underwriting system, naively described as "PD model plus risk policy plus rule engine plus underwriter," decomposes into at least three nodes once §2.1's latent-policy disclosure and §5's judgment-type rule are applied: a Model Risk node owning the model deployment artifact, a Credit Risk node owning the risk policy, and per-instance underwriter nodes for escalations. This is the spec working as intended; it is also a reason readers should not treat the apparent simplicity of a single-node presentation as evidence of conformance. A clean diagram is a poor proxy for a clean architecture.

---

## **6\. Conformance**

*(Normative.)*

NOJA defines three **conformance levels**. Each level is achievable independently of full system rebuild and represents a meaningful step in audit posture. Within each level, conformance is **property-based**: a system claims conformance against a specific level by satisfying the properties enumerated below. Partial level claims (e.g., "Level 2 except §4.5") MUST disclose the exempted properties.

### **6.1 Level 1: Boundary conformance**

A system is Level-1 conformant when:

- Every decision the system makes is assigned to a named judgment node.  
- Each node has exactly one named accountable role (§2.4).  
- Each node's prediction, judgment, execution, and accountability functions are distinguishable in implementation (§2.1–2.4).  
- The granularity rule (§5) is satisfied for every node.

Level 1 is achievable largely through inventory and ownership assignment. It is the prerequisite for any further conformance.

### **6.2 Level 2: Signature conformance**

A system is Level-2 conformant when, in addition to Level 1:

- Every encoded judgment artifact carries a signature satisfying the mechanics requirements of §4.4.  
- Every HITL decision is recorded against the instance it authorizes, conformant to §4.4.  
- Composite artifacts are bundled and signed as required by §4.5.  
- Every active signature names its fallback set per §4.2.

Level 2 makes accountability operational. It is the level at which a system can answer "who authorized this" for any outcome.

### **6.3 Level 3: Lifecycle conformance**

A system is Level-3 conformant when, in addition to Level 2:

- The full signature state machine of §4.1 is implemented, with transitions themselves signed.  
- Every signature declares its scope along all four axes of §4.3.  
- Lapse detection is operational; signatures whose scope conditions cease to obtain transition automatically to Lapsed.  
- Audit cadence (§7) is established and operating.

Level 3 makes the system robust against policy drift. It is the level at which a system can answer not only "who authorized this" but also "is that authorization still valid under current conditions."

---

## **7\. Audit**

*(Normative.)*

A conformant system MUST sustain an ongoing audit discipline. The audit is not a one-time activity. It is the maintenance discipline that keeps the architecture honest as the system evolves, models change, and policy intent drifts.

The audit MUST execute the following routinely:

### **7.1 Domain mapping**

Identify the operational decision types the system makes, assign each to a single accountable role, and surface domains lacking single ownership for resolution.

### **7.2 Signature inventory**

Identify every encoded judgment driving automated decision-making, including policies, thresholds, prompts, system configurations, and rules embedded in code. Each MUST have a named owner, a recorded signature, and a revision history. Unsigned encoded judgments MUST be backfilled with signatures, replaced, or removed.

### **7.3 Lapse detection**

Monitor scope conditions for every active signature. Signatures whose conditions cease to obtain MUST transition to Lapsed and trigger re-affirmation or revocation.

### **7.4 Granularity review**

Review nodes against the granularity rule of §5. Nodes whose rejections cannot be traced to revisable signatures MUST be decomposed. Nodes whose decomposition produces no audit benefit MAY be merged.

*Commentary (non-normative).* The audit reveals a common condition: most organizations have been operating in a state of pre-agentic collapse for years. They have collapsed prediction and judgment using humans and legacy code rather than AI. NOJA's first audit is rarely a checkbox exercise; it is typically a forced reckoning with which authority chains in the organization actually exist and which have been quietly missing.

---

## **8\. Open questions**

*(Non-normative.)*

This specification leaves several questions intentionally open.

**Latency-bound HITL.** Some decisions cannot wait for human review at any threshold (e.g., autonomous vehicle braking). Such cases MUST be handled by design-time signature; HITL is unavailable. NOJA does not yet specify additional safeguards for this case; the High Reliability Organization literature is the closest existing guidance.

**Multi-stakeholder accountability.** A single decision may owe different answers to different parties (customer, regulator, shareholder). NOJA prescribes a single accountable role per node; how that role coordinates with multiple stakeholders is left to organizational design.

**Conformance certification.** This specification defines conformance levels but does not define a certification body, audit procedure, or compliance regime. Whether NOJA conformance becomes a certified property or remains a self-declared one is a matter for adoption rather than specification.

**Failure-mode taxonomy.** A companion document is intended to specify the diagnostic taxonomy of NOJA non-conformance failure modes — ghost ownership, policy decay, signature laundering, shadow judgment, domain gerrymandering, HITL rubber-stamping. This is deferred to keep the specification focused on requirements rather than diagnostics.

**Latent-policy disclosure under opaque upstream providers.** §2.1 requires disclosure of latent policy commitments "to the extent they are knowable." For systems built on third-party foundation models, what is knowable is partial: model cards, RLHF objectives, and refusal taxonomies are typically published, but training-data composition, reward-model details, and post-training safety adjustments often are not. NOJA does not currently specify a minimum disclosure standard for this case. Whether the threshold is "all that the provider publishes," "all that downstream parties can characterize through behavioral testing," or some combination is left for adoption practice.

**In-flight semantics for streaming and conversational executors.** §4.2's drain-then-halt fallback assumes a discrete unit of in-flight work with a clear lifespan (e.g., a loan application). For conversational agents, streaming inference, or persistent multi-turn sessions, "in-flight" is ill-defined: it might denote a single message, an open conversation, or an unresolved ticket. NOJA does not specify how to draw this boundary. The maximum drain time required by §4.2 partially mitigates the gap but does not close it.

**Adaptive systems and learning loops.** NOJA's signature mechanism (§4.4) is artifact-bound: the signature attaches to specific bytes of the policy artifact, and modifications invalidate the signature. This sits uneasily with systems whose policy artifact changes through use — online-learning models that update weights, retrieval corpora that grow with user interaction, memory layers that accumulate per-session state, agents that update their own prompts. Three sub-cases need separating. *Weight updates* invalidate the signed model identity under §4.5 and currently require re-signing per update at Level 2+, which is operationally incompatible with continuous online learning. *Corpus and memory growth* is data accumulation under a signed policy, and does not invalidate the policy signature unless behavior-envelope conditions are breached; the corpus update policy itself is what gets signed. *Self-modifying agent prompts* invalidate the signature where the prompt is part of the signed composite. NOJA does not yet specify a *signed adaptation envelope* — a pre-signed boundary describing how much endogenous change a system is permitted before re-signing is required, the rate at which weights/corpus/memory may change, the monitoring that detects when adaptation has exceeded the envelope, and the fallback when it does. This is the most consequential gap in the current specification for production AI systems and is the natural extension of the behavior envelope work in §4.5: where the behavior envelope establishes what counts as compliant behavior, the adaptation envelope would establish how much the system is permitted to evolve while still being the thing that was signed.

---

## **Appendix A: Relationship to existing frameworks**

*(Non-normative.)*

NOJA operates at a layer below regulatory frameworks (NIST AI Risk Management Framework, ISO 42001, EU AI Act risk tiers) and above implementation frameworks (model evaluation harnesses, prompt-testing infrastructure, agent orchestration libraries). It is intended to be complementary rather than competitive.

A regulator-facing question like "is this AI system high-risk under the EU AI Act?" is answered by the regulatory framework. The architectural question NOJA addresses is upstream: "regardless of regulatory classification, is this system structured such that someone is accountable for each decision it makes, and can that accountability be traced and revised?"

---

## **Appendix B: Revision history**

*(Non-normative.)*

- v0.3 — Initial draft. Established the four-stage node, the two signature modes, and the audit framing.  
- v0.4 — Adds: signature state machine and fallback specification (§4.1–4.2); scope and lapse (§4.3); composite artifacts (§4.5); compound/delegated signature decomposition (§4.6); granularity rule (§5); three conformance levels (§6). Refines: prediction layer treatment of latent policy commitments (§2.1); audit as ongoing discipline (§7).  
- v0.5 — Adds: behavior envelope coverage specification along three axes (§4.5); granularity rule treatment for stochastic executors (§5); commentary on cosigner decomposition (§3.1); commentary on single-node-to-network expansion (§5). Refines: composite artifact model-identity requirement to admit hosted-model version-pin attestation, with the provider's attestation policy as an environmental scope condition (§4.5 \+ §4.3). Adds open questions on opaque-provider disclosure standards and in-flight semantics for streaming executors (§8).  
- v0.6 — Current. Refines: §1 thesis distinguishes legacy collapse-by-accretion from agentic collapse-by-design and threads to §7's audit commentary; §2 lead replaces "performed in sequence" with "logically ordered for accountability though implementation MAY interleave them"; §4.3 commentary replaces the named-disaster reference with a domain-neutral pattern. Adds: §2.3 paragraph on agentic execution loops and dynamic subagent spawning, cross-referencing §5; §4.1 clarification distinguishing Superseded from Revoked-with-cut-over; §4.2 commentary on the fallback set as parallel to scope; §4.5 enumerates subagent-spawning policies as a composite artifact component and notes that continuously-updating models cannot satisfy version-pinning under the present spec; §5 commentary sharpening the End-to-End Agency treatment. Adds open question on adaptive systems and learning loops, naming the signed adaptation envelope as the concept future work would develop (§8).

