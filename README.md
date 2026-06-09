# Writings

  Long-form writing on AI agent security, decision-rights, reversibility-graded authority, and the architectural primitives underneath them.

  Short canonical versions land first on third-party publications (newsletters, magazines, preprint servers). This repo is the long-form home: full
  version of each piece with every citation, every figure, every example that the short version had to drop.

## Index

 - **June 9, 2026** — [Action-Class Authority for AI Agents: A Verification-Side Reference](https://github.com/Mayur021/action-class-authority) — Whitepaper v1.0 (~28 pages, 18 chapters). Four-class reversibility taxonomy, manifest-declared classification, worst-case chain rule. Anchored in OWASP AISVS C9.2.6 + C9.2.7 (proposed for 1.01). Standalone repo.
  - **June 3, 2026** — [Per-Step Authorization Is Not Chain Authorization. The Worst-Case Action Class
  Primitive.](2026-06-03-chain-authorization-convergence/) — Five independent standards-track and executive-framework surfaces converging on a
  shared architectural primitive: manifest plus deterministic gate plus chain rule plus four-field audit schema.
  - **June 2, 2026** — [What I Learned Contributing Across Five Standards Surfaces in Five
  Months](2026-06-02-contributing-across-standards-surfaces/) — Maintainer dynamics, vocabulary, issue-before-PR, prior art, reference
  implementations across OWASP AISVS / SPVS / Cornucopia / GenAI ASI and CSA NHI.
  - **May 30, 2026** — [Investigation Is Reversible. Actuation Is Not. The Architectural Floor for Agentic
  AI.](2026-06-02-investigation-vs-actuation/) — The read/write architectural fold as design primitive: where the gate lives, why the asymmetry is
  binary not gradient.
  - **May 22, 2026** — [The Decision-Rights Plane: An Architectural Gap in AI Security](2026-06-02-decision-rights-plane/) — Why the bottom two
  layers of AI security lack the right primitive, and how to contribute to closing the gap across standards surfaces.

  ## License

  All articles in this repo are licensed CC-BY-4.0 unless explicitly stated otherwise. Quote, reuse, and translate with attribution.

  ## Author

  Mayur Agnihotri. LinkedIn: [linkedin.com/in/mayuragnihotri](https://www.linkedin.com/in/mayuragnihotri/). GitHub:
  [@Mayur021](https://github.com/Mayur021).

  Reference implementations of the architectural primitives discussed in these pieces:

  - [aisvs-action-class-reference](https://github.com/Mayur021/aisvs-action-class-reference): manifest-declared action class and worst-case chain
  rule.
  - [nhi-runtime-decision-rights](https://github.com/Mayur021/nhi-runtime-decision-rights): non-human identity runtime decision-rights.
