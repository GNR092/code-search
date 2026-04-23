# Validation Checklist

Use this checklist before finalizing findings from codesearch.

## Index and scope

- [ ] Index is ready (`codesearch_index_status` or `codesearch doctor`)
- [ ] Active database identified (`codesearch_find_databases` or `codesearch index --list`)
- [ ] Search constrained to target project/path

## Search quality

- [ ] At least one broad semantic query executed
- [ ] At least one narrowed query executed (path/domain/symbol)
- [ ] Results include relevant project files (not unrelated repos)

## Refactor safety

- [ ] `find_references` run for touched symbols
- [ ] Potential call sites reviewed
- [ ] Risky/high-impact usages noted

## Evidence quality

- [ ] Final answer references concrete files
- [ ] Claims validated by reading code, not just vector hits
- [ ] Ambiguities or confidence limits explicitly stated
