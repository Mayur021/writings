# Writings

Long-form writing on AI agent security, decision-rights, reversibility-graded authority, and the standards-track work behind them.

Short canonical versions land first on third-party publications (newsletters, magazines, preprint servers). This repo is the long-form home: full version of each piece with every citation, every footnote, every example that the short version had to drop.

## Index

| Date | Title | Topic | Canonical short version |
|---|---|---|---|
| _coming_ | What OWASP AISVS Just Merged: Action-Class Authority Lands | AISVS C9.2.6 + C9.2.7 ratification, four-paper convergence on agent action authority | The Weather Report (theweatherreport.ai) |

(More entries land here as canonical versions publish.)

## Format conventions

Each piece lives in its own folder under the repo root:

```
YYYY-MM-DD-short-slug/
├── README.md         # long-form article
├── figures/          # any diagrams or images
└── sources.md        # full reference list
```

Articles are written in markdown. Figures are PNG or SVG.

## License

All articles in this repo are licensed CC-BY-4.0 unless explicitly stated otherwise. You can quote, reuse, and translate the work with attribution.

## Author

Mayur Agnihotri. LinkedIn: [linkedin.com/in/mayuragnihotri](https://www.linkedin.com/in/mayuragnihotri/). GitHub: [@Mayur021](https://github.com/Mayur021).

Standards-track work: [OWASP AISVS](https://github.com/OWASP/AISVS) (Contributor), [OWASP SPVS](https://github.com/OWASP/www-project-spvs) (Active work), [OWASP Cornucopia](https://github.com/OWASP/www-project-cornucopia) (Active work), [OWASP GenAI Security Project](https://github.com/GenAI-Security-Project) (Active work), CSA NHI v1.0 (Reviewer).

Reference implementations:
- [aisvs-action-class-reference](https://github.com/Mayur021/aisvs-action-class-reference) for AISVS C9.2.6 + C9.2.7.
- [nhi-runtime-decision-rights](https://github.com/Mayur021/nhi-runtime-decision-rights) for NHI runtime decision-rights companion to SPVS V1.3.7.
