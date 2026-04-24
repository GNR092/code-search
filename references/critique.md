# Critique Guide

Review your own search process before presenting conclusions.

## Common failure modes

- You trusted top hits without opening files
- You forgot path scoping and mixed repositories
- You searched only one phrasing and missed better matches
- You skipped references and underestimated impact
- You used wrong model dimensions (e.g., jina-code 768 on minilm index 384)
- You didn't check if index was stale or outdated

## Self-critique questions

1. Did I verify index/source database and scope?
2. Did I run `codesearch doctor` first?
3. Did I run broad + narrow queries, not just one?
4. Did I validate findings in source files?
5. Did I check symbol references before proposing changes?
6. Did I use appropriate model for the codebase language?
7. Could another engineer reproduce my conclusion from file evidence?

## Quality bar

Good output is:
- **Reproducible** - commands/tools are clear
- **Grounded** - file-level evidence
- **Scoped** - project-specific, no cross-repo noise
- **Safe** - impact-aware before edits
- **Fresh** - index was up-to-date or `--sync` used
