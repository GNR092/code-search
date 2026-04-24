# Validation Checklist

Use this checklist before finalizing findings from codesearch.

## Tool availability

- [ ] CLI available (`codesearch --version` works)
- [ ] MCP tools available (`codesearch_find_databases` responds)
- [ ] Fallback to CLI documented if MCP unavailable

## Index and scope

- [ ] `codesearch doctor` passes (no errors)
- [ ] Index is ready (`codesearch_index_status` shows "Indexed")
- [ ] Active database identified
- [ ] Search constrained to target project/path with `filter_path`

## Search quality

- [ ] At least one broad semantic query executed
- [ ] At least one narrowed query (path/domain/symbol)
- [ ] Results include relevant project files (not unrelated repos)
- [ ] At least one query in Spanish (if target is Spanish codebase)

## Model selection

- [ ] Correct model for use case (default minilm-l6-q for speed, e5-multilingual for Spanish)
- [ ] Model dimensions match index (jina-code is 768, minilm-l6-q is 384)

## Refactor safety

- [ ] `find_references` run for touched symbols
- [ ] Potential call sites reviewed
- [ ] Risky/high-impact usages noted

## Evidence quality

- [ ] Final answer references concrete files
- [ ] Claims validated by reading code, not just vector hits
- [ ] Ambiguities or confidence limits explicitly stated

## Advanced features (if used)

- [ ] `--sync` used if codebase recently changed
- [ ] `--rerank` considered for better accuracy
- [ ] `--vector-only` used if FTS causing noise
