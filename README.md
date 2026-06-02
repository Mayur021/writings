# Writings

  Long-form writing on AI agent security, decision-rights, reversibility-graded authority, and the architectural primitives underneath them.

  Short canonical versions land first on third-party publications (newsletters, magazines, preprint servers). This repo is the long-form home: full
  version of each piece with every citation, every figure, every example that the short version had to drop.

  ## Index

  | Date | Title | Topic |
  |---|---|---|
  | 2026-06-02 | [The Decision-Rights Plane: An Architectural Gap in AI Security](2026-06-02-decision-rights-plane/) | Why the bottom two layers of
  AI security lack the right primitive, and a working method for contributing across multiple standards surfaces |

  (More entries land here as the work continues.)

  ## Format conventions

  Each piece lives in its own folder under the repo root:

  YYYY-MM-DD-short-slug/
  ├── README.md         # long-form article
  ├── figures/          # any diagrams or images
  └── sources.md        # full reference list (when present)

  Articles are written in markdown. Figures are PNG.

  ## License

  All articles in this repo are licensed CC-BY-4.0 unless explicitly stated otherwise. Quote, reuse, and translate with attribution.

  ## Author

  Mayur Agnihotri. LinkedIn: [linkedin.com/in/mayuragnihotri](https://www.linkedin.com/in/mayuragnihotri/). GitHub:
  [@Mayur021](https://github.com/Mayur021).

  Reference implementations of the architectural primitives discussed in these pieces:

  - [aisvs-action-class-reference](https://github.com/Mayur021/aisvs-action-class-reference): manifest-declared action class and worst-case chain
  rule.
  - [nhi-runtime-decision-rights](https://github.com/Mayur021/nhi-runtime-decision-rights): non-human identity runtime decision-rights.
