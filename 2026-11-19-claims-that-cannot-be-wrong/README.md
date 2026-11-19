# Two Claims That Cannot Be Wrong

*A detection count and an autonomy label are the two numbers the security operations market runs on. Neither can be contradicted by evidence. The falsifiable versions exist, and almost nobody publishes them.*

**Date**: November 19, 2026
**Author**: Mayur Agnihotri
**Reading time**: ~10 minutes

## TL;DR

Two sentences appear in nearly every security operations product claim. "Three thousand detections out of the box." "Autonomous triage." Neither can be shown to be false, because neither says anything a measurement could contradict. A detection count says nothing about which detections can fire against the data a particular pipeline actually populates. An autonomy label says nothing about what runs unattended, what waits for approval, and who verified either. The falsifiable versions of both claims exist. For coverage, it is the count of enabled rules whose required fields are populated in this environment, and across published vendor documentation only one product ships that number, with the practice itself written up as guidance by another. For autonomy, it is a position on a ladder from manual to end to end, declared separately from whether the capability is generally available and separately again from who observed it working. The standards cannot yet express the second one: a 291 control pipeline verification standard published this month requires human approval for irreversible operations and says nothing about how a system decides which operations are irreversible.

---

## The question

A buyer asks two things of every security operations product. What will it catch, and what will it do on its own.

The answers arrive as a number and an adjective. Three thousand detections. Autonomous investigation. Both are true in the only sense that matters to the person who wrote them, and neither is a claim in the scientific sense, because no observation could contradict either one. Count the detections and you will find three thousand. Watch the product work and you will see it investigate. The claims survive contact with the product because they were never exposed to it.

This is not an accusation of dishonesty. It is an observation about vocabulary. A market that has not agreed on a falsifiable way to state a capability will state it in whatever way is available, and what is available is a count and an adjective.

---

## The count is not the coverage

A detection rule fires when a condition is met in a field. If the field is never populated, the rule cannot fire, whatever its source is doing and whatever the catalogue says.

That is not an edge case. A rule written for one vendor's Windows event schema will reference fields that a different collection path never produces. A parser that extracts eighteen of twenty five fields leaves every rule depending on the other seven permanently inert. A rule can be enabled, correctly written, pointed at a live source, and structurally incapable of matching anything.

So the number a buyer is given, the catalogue size, is the one number guaranteed not to move when the product is deployed badly. It is identical on the best and worst day of an implementation. The number that moves is how many of those rules can fire here, against these fields, in this environment, and that number is not in the brochure.

The same applies one level up. MITRE ATT&CK coverage is routinely published as a percentage of techniques with at least one mapped rule. A guide on technique mapping scores each technique zero for none, one for partial, two for strong. That tells a reader where rules exist. It does not tell them whether those rules are being fed. A product can show eighty percent technique coverage with half its rules starving, and both numbers are correct.

---

## The industry has already written the problem down

This is not an unknown gap waiting to be discovered. It has been described precisely, in public, by people selling into the same market.

SOC Prime's guidance on detection validation and decay states the mechanism directly: a rule can parse cleanly and reference valid fields and still return zero matches, because the field it keys on is empty, or renamed, or has changed meaning. Their recommended checklist includes a fire rate baseline per rule and a required field null rate monitored continuously. That is the right measurement, described by a vendor, as advice to practitioners.

Exabeam ships the closest thing to it that appears in published product documentation. Outcomes Navigator counts a rule as satisfied when every field it requires has been actively parsed in the past thirty days, where actively parsed means a parser used in that window. Rules declare their required fields as attributes. Coverage credit goes only to satisfied rules, and scores recalculate at least daily.

Two details in that documentation are worth more than the feature. The first is that Exabeam distinguishes satisfied from firing, and says so: a rule with every required field present may still not trigger, because event builder conditions, model convergence or rule dependencies are unmet. The second is that roughly ten percent of Advanced Analytics rules carry session end or sequence end expressions and are excluded from the calculation as unsupported, with correlation rule coverage described as approximate. A vendor publishing the limits of its own metric is doing something rarer than publishing the metric.

Set those two together and the state of the field is clear. The measurement has been articulated as guidance. One product documents a version of it. Across published vendor documentation, no other source presents per rule field population, the share of events in which a rule's required fields are non null, as a formal coverage measure. Products may compute it without publishing it; what is missing is the number appearing where a buyer can hold a vendor to it.

---

## The adjective is not the autonomy

The second claim fails the same way, and it matters more, because the thing being claimed can take irreversible action.

Autonomous triage can mean the product suggests a verdict and an analyst accepts it. It can mean the product closes the alert and nobody sees it. Those are different products with the same adjective on the box, and the difference is the entire risk posture of the deployment.

The AI SecOps Evaluation Framework published by SecOps Unpacked, an independent research directory, takes the useful step of refusing to accept the adjective. It records three separate things per capability and keeps them apart on purpose. Delivery, meaning whether the capability exists and what is needed to use it, with roadmap and preview items carrying no further score at all. Autonomy, on a five point ladder: zero manual, assistance, requires human approval, allows a human veto, runs end to end. And evidence, on its own ladder from vendor claim through product documentation, recorded demonstration, observed demonstration and finally validation in the evaluator's own environment.

The framework's own wording is explicit that autonomy measures human involvement rather than model involvement, and that it can be delivered through automation, through reasoning, or through both. It is not an artificial intelligence score. That distinction alone disposes of a large amount of current marketing, in which the presence of a model is offered as evidence of autonomy and the absence of a human is left unstated.

Three declarations instead of one adjective is a small change that makes the claim falsifiable. A product that declares generally available, runs end to end, validated in our environment has said something that an evaluation could contradict. A product that says autonomous has not.

---

## Why the standards cannot close it yet

A buyer who wanted to write this into a requirement would reach for a standard, and would not currently find the control.

The OWASP Secure Pipeline Verification Standard released version 2.0 this month, consolidating three earlier versions into a single set of 291 controls across five lifecycle phases from plan to operate. It is a serious piece of work and it carries the concept the argument needs. Requirement 5.6.1 verifies that agents are prevented from performing irreversible high impact operations without explicit out of band human approval. Ten controls reference blast radius.

The gap sits one layer beneath that sentence. Nothing in the standard says how a system decides which operations are irreversible. Searching all 291 controls for decision rights returns nothing. Confidence threshold returns nothing. Escalation path returns nothing. The requirement to gate on irreversibility exists; the mechanism that determines what counts as irreversible, who holds the authority to approve, and what happens when the approval does not arrive, does not.

That is the same shape as the coverage problem. The right thing is stated at the level of outcome and left unspecified at the level that would make it verifiable. A requirement to gate irreversible operations, in a standard that does not define how irreversibility is established, can be satisfied by a product that asserts a label on each action and gates on the label.

---

## What the falsifiable versions look like

For coverage, three numbers instead of one, and none of them is the catalogue size.

How many enabled rules can fire in this environment, against the fields this pipeline currently populates. Which fields are missing, named, with the count of rules each one blocks. And which sources are present in telemetry but not configured, so the gap between what is arriving and what is being used is visible rather than inferred.

The first number is the one that moves. It is lower than the catalogue count, always, and the distance between them is the honest description of an implementation. A product reporting it is making a claim that can be wrong, which is the only kind worth anything.

For autonomy, the three declarations, kept apart.

What is available today and who builds it, before anything else is scored. What human involvement remains, as a position on a ladder rather than an adjective. And what evidence supports the assessment, with a vendor's own demonstration ranked below a practitioner's validation in their own estate, because those are not the same thing and collapsing them is how a demo becomes a procurement decision.

---

## What to ask

Three questions, none of which requires technical access to answer, and all of which a product either answers or does not.

**How many of your enabled detections can fire against my data today, and which fields are blocking the rest.** A product that cannot answer this does not know. A product that answers with the catalogue count has answered a different question.

**For each thing you call autonomous, where does it sit between fully manual and fully unattended, and what happens when the approval times out.** The timeout is the revealing half. A gate that expires to allow is not a gate.

**What is the highest evidence level behind each capability claim, and which of them is only a demonstration.** Not to catch anyone out. To know which parts of the deployment are a forecast.

A vendor whose claims cannot be contradicted by ground truth is not necessarily exaggerating. They may be using the only vocabulary the market has offered them. The vocabulary exists now, in an evaluation framework that separates delivery from autonomy from evidence, and in one product's documentation that counts rules by whether their fields are parsed. What is missing is the expectation that a claim should be the kind of thing that could turn out to be false.

---

## Sources

- SecOps Unpacked, AI SecOps Evaluation Framework v3.2.0, framework guide. Delivery, autonomy and evidence vocabulary; the five point autonomy ladder; the statement that autonomy is not an LLM only score. https://secops-unpacked.ai/asef/guide
- Exabeam, Outcomes Navigator documentation, coverage calculation. Satisfied rules, required fields actively parsed in the past thirty days, the satisfied versus firing distinction, and the unsupported rule exclusion. https://docs.exabeam.com/en/outcomes-navigator/all/outcomes-navigator-guide/outcomes-navigator-coverage-calculation.html
- Exabeam, Outcomes Navigator, Advanced Analytics rules coverage calculation. https://docs.exabeam.com/en/outcomes-navigator/all/outcomes-navigator-guide/outcomes-navigator-coverage-calculation/advanced-analytics-rules-coverage-calculation.html
- Exabeam, Outcomes Navigator, correlation rules coverage calculation. https://docs.exabeam.com/en/outcomes-navigator/all/outcomes-navigator-guide/outcomes-navigator-coverage-calculation/correlation-rules-coverage-calculation.html
- SOC Prime, "Detection Validation and Decay: Stop Silent Failures". Fire rate baseline and required field null rate as a continuous check. https://socprime.com/blog/detection-validation-and-decay/
- OWASP Secure Pipeline Verification Standard (SPVS) 2.0, consolidated requirements, requirement 5.6.1 and the blast radius controls. https://github.com/OWASP/www-project-spvs
- OWASP AISVS, chapter C9, requirements 9.2.1, 9.2.3, 9.2.4 and 9.2.10, for the reversibility classification argument. https://github.com/OWASP/AISVS

## Related work

- [What the Agentic Governance Frameworks Actually Bound](../2026-10-13-what-agentic-frameworks-bound/) October 13, 2026. The same measurement applied to governance frameworks: does the control bound a step or a sequence.
- [Per-Step Authorization Is Not Chain Authorization. The Worst-Case Action Class Primitive.](../2026-06-03-chain-authorization-convergence/) June 3, 2026. Why a label asserted per request is weaker than a class declared per action.
- Reference implementations: [aisvs-action-class-reference](https://github.com/Mayur021/aisvs-action-class-reference) and [nhi-runtime-decision-rights](https://github.com/Mayur021/nhi-runtime-decision-rights).

## License

CC BY 4.0. Cite as: Agnihotri, Mayur. "Two Claims That Cannot Be Wrong." November 19, 2026. https://github.com/Mayur021/writings/blob/main/2026-11-19-claims-that-cannot-be-wrong/README.md
