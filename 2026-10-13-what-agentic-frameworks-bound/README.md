# What the Agentic Governance Frameworks Actually Bound

*Seven principles, five dispositions, one receipt format and one identity standard. Measured against a single question: does it bound a step, or a sequence?*

**Date**: October 13, 2026
**Author**: Mayur Agnihotri
**Reading time**: ~11 minutes

## TL;DR

Four weeks of publishing has given the agentic security field its clearest framework yet, and a measurable gap. Satya Nadella's seven principles of observability, the OWASP Agent Control Standard's disposition vocabulary, the OWASP Agentic Skills Top 10 execution receipts, and the identity layer's recommended delegation mechanism all bound one actor or one step. None of them bounds a sequence. The sharpest case is RFC 8693, which records the entire delegation chain in nested `act` claims and then states that prior actors in those claims "are informational only and are not to be considered in access control decisions". The chain is written down and excluded from the decision by the specification itself. One published requirement does bound the sequence: OWASP AISVS 1.01 requirement 9.2.10, released on October 5, 2026. This piece measures each framework against the same question and shows where the single exception sits.

---

## The question

An agent reads a flagged dataset and exports it in chunks. Each chunk export is individually reversible through a documented delete-from-destination capability, so every per-step class is honestly `reversible`. The agent stays inside its mandate at every step. No policy is violated. Somewhere past the middle of the sequence, enough of the dataset is out that reconstruction is possible, and the effect stops being reversible.

Nobody exceeded their authority. There is no step to point at.

This is not a hypothetical about misaligned models. It is a property of composition: a chain of individually permitted, individually reversible actions can reach an outcome that no single action declared and no single approval covered. The question any governance framework has to answer is whether its controls can see that, and the honest way to measure a framework is to ask what unit it binds.

---

## Nadella's seven principles bound a model

"Models as Insider Risks in the Super Intelligence Era", published October 10, 2026, is the most senior statement the field has. Its central move is right and worth quoting: the controls that govern what a model can access and what actions it can take "must sit outside the model", building on the information security principle "that a program must not be able to bypass or tamper with the mechanisms that enforce its permissions".

It then lists seven principles of observability: model diversity, observe everything, verifiability, independent controls, independent auditability, containment, and incident disclosure.

Read them for the unit each one binds. Model diversity: "No one model should become the sole dependency for an important outcome or be responsible for verifying its own work." Independent controls: "Orgs should be able to independently determine what a model can access and what actions it can take." Independent auditability: "No single model should control both a system's behavior and the evidence required to determine whether that behavior is aligned with the original intent." Containment: "An authorized person should always be able to pause or shut down a model mid-task."

Every one of those is a statement about one model. Verifiability and incident disclosure widen to "the entire system" and "these systems", but widening to a system is not the same as bounding a sequence. Nothing in the seven asks what a chain of individually permitted steps adds up to.

That is not a criticism of the essay, which is a containment argument and a good one. It is a measurement. The unit is the model.

---

## The Agent Control Standard bounds a call

The OWASP Agent Control Standard defines the disposition vocabulary a Guardian returns for a step: ALLOW, DENY, MODIFY, ASK, DEFER. Its section 6 is explicit that "A hook is a control point only if the Observed Agent waits for the verdict and applies it", and that a framework emitting hooks without applying the verdict is not conformant. That is a strong rule, and it is a rule about one call.

The standard is also unusually honest about its own failure posture, which is worth reading for anyone building on it. `on_decision_failure` defaults to `proceed`, so that "a slow, erroring, or unreachable Guardian does not halt production", and the startup posture defaults to `proceed`, which starts a session unguarded when the handshake never completes. The specification states the consequence itself: "Fail-open trades enforcement for availability under disruption: an adversary who can disrupt the channel converts control into audit."

There is one deliberate asymmetry worth borrowing regardless of what you are building. `timeout_decision` on a DEFER defaults to `deny`, which the spec notes is "deliberately the opposite of `on_decision_failure` defaulting to `proceed`", because "a degraded Guardian that can still emit DEFER fails closed where a silent one fails open". Silence and an unresolved concern are different events. Few systems distinguish them.

But the disposition answers a call, and the next call starts clean.

---

## The execution receipts bound one execution

OWASP Agentic Skills Top 10, AST09, carries an Execution Receipts subsection defining a bilateral receipt schema: an admission record signed before a skill runs, capturing the manifest hash, requested scope, declared identity and policy decision, and an outcome record signed after it completes, capturing actual side effects. Both share a content-derived reference so a third party can confirm that a specific execution was admitted and produced a specific outcome without trusting the runtime that emitted the records.

That is a genuinely good design, and it makes the declared-versus-actual gap machine-checkable for one execution. Declared reversibility goes in the admission record; the class actually reached goes in the outcome record; a verifier compares them.

The limit is in the identifier. `attempt_id` pairs a single admission with its own outcome. There is no field linking one skill's outcome to the next skill's admission. A verifier can confirm each hop in isolation and cannot walk the chain, because the receipts carry no causal link to walk. The proposed fix, a `predecessor_ref` carrying the hash of the prior outcome record, changes the admission record shape and has been deferred to the schema side.

So two skills can each produce clean, independently verifiable receipts while the sequence reaches somewhere neither declared. Every receipt verifies. The chain is unevidenced.

---

## The identity layer writes the chain down and then ignores it

This is the sharpest case, because here the chain is not merely unaddressed. It is recorded and then normatively excluded.

The current practical framework for agent IAM recommends OAuth 2.0 Token Exchange, RFC 8693, for delegation, because it "provides delegation and impersonation semantics that preserve the distinction between the agent's own identity and the authority it has been lent". That is the right mechanism and the right reason. The same framework also sees the problem squarely: "When one agent delegates a subtask to another, authority propagates through a chain no human approved step by step." Its answer is per link: "Every link in the chain needs an identity, a scope, an expiration, and a human ultimately accountable for it."

Now read what RFC 8693 does with the chain. Section 4.1 defines the structure: "A chain of delegation can be expressed by nesting one 'act' claim within another. The outermost 'act' claim represents the current actor while nested 'act' claims represent prior actors." The whole delegation history is in the token.

Then it says what to do with it:

> "For the purpose of applying access control policy, the consumer of a token MUST only consider the token's top-level claims and the party identified as the current actor by the 'act' claim. Prior actors identified by any nested 'act' claims are informational only and are not to be considered in access control decisions."

The chain is carried in the token, in order, cryptographically bound. And the specification says in normative language that a consumer applying access control policy must not use it.

That is a deliberate and defensible design decision for the problem RFC 8693 was written to solve in 2020, which was impersonation and delegation between services with human-configured trust relationships. It is not a defect in the RFC. It is a statement about what the token is for. But any agent architecture that adopts RFC 8693 as its delegation mechanism inherits it: the delegation history becomes an audit artifact rather than an input to authorization.

Identity per link is close to solved. What the sequence adds up to is not, and the standard everyone points at says so out loud.

---

## The one requirement that bounds the sequence

OWASP AISVS 1.01 was released on October 5, 2026. Its C9 chapter carries two requirements that do bound a chain.

9.2.10: "Verify that approval gates for multi-step or multi-agent action chains enforce the highest-impact reversibility classification present anywhere in the chain, where the chain is established by a component outside the control of the agent being gated."

9.2.11: "Verify that agent execution which would exceed the approved action chain is blocked until a new approval is obtained, and that the new approval enforces the highest-impact reversibility classification present across the cumulative chain."

Three things in there are load-bearing and easy to skim past.

**The unit is the chain, not the step.** The gate evaluates the worst class present anywhere in the sequence, not the class of the action in front of it.

**The chain's extent is established outside the agent being gated.** This is the half that makes the first half enforceable. A rule that folds authority across a chain needs the chain's boundary fixed by something other than the party being constrained by it. Without that clause, an agent is the authority on the extent of its own delegation, and the rule can be satisfied by declaring a shorter chain. In AISVS 1.0 the sentence simply ended at "present anywhere in the chain"; the clause was added for 1.01.

**Exceeding the approved chain is itself a gated event.** 9.2.11 closes the case where the agent gets a valid approval and then keeps going.

Whether this is adopted is a separate question from whether it is specified. It is specified.

---

## Magnitude is not reversibility

A recurring substitution is worth naming, because it looks like rigour and is not.

Risk matrices commonly score confidence against consequence. That pairing is sound as far as it goes, and the better treatments carve out a grey zone where escalation is mandatory. But consequence is magnitude, and magnitude is a different property from whether an effect can be walked back. A small irreversible action and a large recoverable one sit far apart on consequence and identically on undoability.

AISVS separated the two across its own releases. In 1.0, requirement 9.2.1 read: "Verify that the agent runtime blocks execution of privileged, high-impact, or irreversible actions until explicit human approval is received and verified." In 1.01 it reads: "Verify that the agent runtime blocks execution of privileged or irreversible actions until explicit human approval is received and verified."

Magnitude came out. Undoability stayed.

Any escalation model with a confidence axis and a consequence axis still has a cell it does not see: high confidence, modest consequence, no undo. That combination clears a confidence gate because the checkers agree, clears a consequence gate because the number is small, and leaves a state nobody can restore.

---

## What would close it

Nothing in this piece requires a new framework. The pieces exist and are specified.

**A chain identifier that a verifier can walk.** The receipt work needs the causal link it currently lacks: a reference on the admission record to the prior outcome record, so adjacency is established rather than inferred from timestamps.

**The chain's extent fixed outside the agent.** Already specified in AISVS 9.2.10. The hard part is not the rule but the component that establishes the boundary, which has to be something the gated agent does not control.

**Reversibility as a declared class, separate from impact.** Read-only, reversible, externally reversible, irreversible. Declared per action at deployment rather than asserted per request at runtime, and recorded both as declared and as actually reached.

**An escalation trigger on undoability, not only on uncertainty.** A system that is confident and in agreement can still be about to do something permanent.

The honest state of the field is that the vocabulary converged fast, in roughly eighteen months, and three things stayed open. Maintainers defer the hard sub-problems as aspirational, which is a reasonable scoping call and not a failure. Defaults ship permissive because availability wins in production. And almost nothing yet bounds a sequence rather than a step.

An organisation adopting the current best practice today can classify actions, gate them individually, record what each one did, and still be unable to say what a sequence of permitted steps added up to.

---

## Sources

Every claim above is checkable. In order of appearance:

- Satya Nadella, "Models as Insider Risks in the Super Intelligence Era", October 10, 2026. https://snscratchpad.com/posts/models-as-insider-risks/
- OWASP Agent Control Standard, Instrument Specification v0.1.0, sections 4.1, 6, 6.4. https://genai.owasp.org/resource/agent-control-standard-acs/ and https://github.com/GenAI-Security-Project/agent-control-standard
- OWASP Agentic Skills Top 10, AST09 Execution Receipts.
- "IAM for AI agents: A Practical Enterprise Framework", The Hacker News, September 2026. https://thehackernews.com/2026/09/iam-for-ai-agent.html
- RFC 8693, OAuth 2.0 Token Exchange, section 4.1. https://www.rfc-editor.org/rfc/rfc8693.txt
- OWASP AISVS 1.01, chapter C9, requirements 9.2.1, 9.2.10, 9.2.11. https://github.com/OWASP/AISVS/blob/main/1.01/en/0x10-C09-Orchestration-and-Agentic-Action.md
- OWASP AISVS 1.0, chapter C9, requirement 9.2.1, for the version comparison. https://github.com/OWASP/AISVS/blob/main/1.0/en/0x10-C09-Orchestration-and-Agentic-Action.md
- NIST NCCoE, "Accelerating the Adoption of Software and AI Agent Identity and Authorization", draft. https://www.nccoe.nist.gov/projects/software-and-si-agent-identity-and-authorization

## Related work

- [Per-Step Authorization Is Not Chain Authorization. The Worst-Case Action Class Primitive.](../2026-06-03-chain-authorization-convergence/) June 3, 2026. The argument from first principles, before any of these frameworks published.
- Reference implementations: [aisvs-action-class-reference](https://github.com/Mayur021/aisvs-action-class-reference) and [nhi-runtime-decision-rights](https://github.com/Mayur021/nhi-runtime-decision-rights).

## License

CC BY 4.0. Cite as: Agnihotri, Mayur. "What the Agentic Governance Frameworks Actually Bound." October 13, 2026. https://github.com/Mayur021/writings/blob/main/2026-10-13-what-agentic-frameworks-bound/README.md
