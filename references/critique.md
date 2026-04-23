# Critique Guide

Review your own search process before presenting conclusions.

## Common failure modes

- You trusted top hits without opening files.
- You forgot path scoping and mixed repositories.
- You searched only one phrasing and missed better matches.
- You skipped references and underestimated impact.

## Self-critique questions

1. Did I verify index/source database and scope?
2. Did I run broad + narrow queries, not just one?
3. Did I validate findings in source files?
4. Did I check symbol references before proposing changes?
5. Could another engineer reproduce my conclusion from file evidence?

## Quality bar

Good output is:
- Reproducible (commands/tools are clear)
- Grounded (file-level evidence)
- Scoped (project-specific, no cross-repo noise)
- Safe (impact-aware before edits)
