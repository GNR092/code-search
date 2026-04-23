---
name: code-search
description: Semantic code search and navigation using codesearch CLI. Find code patterns, symbols, references, and understand codebases efficiently.
---

# Code Search

Semantic code search for CLI and MCP workflows.

Tested with `codesearch v0.1.200+149`.

## Scope

Use this skill when you need:
- Fast orientation in unfamiliar repositories
- Semantic discovery across many files
- Symbol usage impact analysis before refactors

Do not use this for trivial single-file lookups; direct file reads are faster.

## Interfaces

- **CLI**: `codesearch ...`
- **MCP tools**: `codesearch_index_status`, `codesearch_semantic_search`, `codesearch_find_references`, `codesearch_find_databases`

Prefer MCP in-agent. Use CLI for manual checks, setup, and diagnostics.

## Preflight Validation (Required)

Before using this skill, validate tool availability.

### 1) Validate CLI exists

```bash
codesearch --version
```

If command is not found:
- Do not attempt codesearch commands.
- Report that codesearch CLI is missing.
- Suggest installing codesearch first, then rerun.

### 2) Validate MCP tools exist (in-agent)

Try a lightweight MCP call first:

```text
codesearch_find_databases()
```

If MCP tools are unavailable:
- Do not attempt MCP calls.
- Fall back to CLI workflow (if CLI exists).

### 3) Validate index readiness

- MCP: `codesearch_index_status()`
- CLI: `codesearch doctor`

If index is not ready, run setup/index steps before searching.

## Verified CLI Commands

### Main and subcommand help
```bash
codesearch --help
codesearch search --help
codesearch index --help
codesearch mcp --help
```

### Health and index state
```bash
codesearch doctor
codesearch stats
codesearch index --list
```

### Search examples
```bash
codesearch search "site_name setting in admin configuration" --max-results 5 --scores
codesearch search "site_name setting" --filter-path "Documents/Residentes/Proyectos/facturacion_alejandro/"
```

## Verified MCP Calls

```text
codesearch_index_status()
codesearch_find_databases()
codesearch_semantic_search(query="site name setting in admin configuration", compact=true, limit=5, filter_path="Documents/Residentes/Proyectos/facturacion_alejandro")
codesearch_find_references(symbol="Setting", limit=10)
```

## Recommended Workflow

1. Check index readiness:
   - MCP: `codesearch_index_status()`
   - CLI: `codesearch doctor`
2. Check which DB is active:
   - MCP: `codesearch_find_databases()`
   - CLI: `codesearch index --list`
3. Run broad semantic search.
4. Narrow with path filters.
5. Run reference lookup for target symbols.
6. Open exact files/lines with file read tools to verify behavior.

## Important Behavior (Observed)

- Codesearch may use the nearest parent index (not always project-local).
- If a parent index exists, `codesearch index --add .` can fail with:
  - "You cannot create a separate index for a subdirectory"
- `--path` is not always a strict result filter in shared indexes.
- `--filter-path` is the reliable way to constrain results.

## Troubleshooting

- `IndexWriter lock busy` / `Failed to acquire Lockfile`:
  - Stop concurrent index writers (`codesearch serve`, parallel index jobs), then retry.
- `index not found`:
  - Run `codesearch setup` and then `codesearch index`.
- Too many cross-project results:
  - Use `--filter-path` (CLI) or `filter_path` (MCP).
- Weak relevance:
  - Reindex and test another model (`bge-small`, `jina-code`).

## Query Writing Tips

- Use verbs + intent: "where invoice status changes to paid"
- Use domain terms: "CFDI", "regimen fiscal", "timbrado"
- Ask for flow points: "entrypoint for admin settings update"

Iterate as: broad query -> filtered query -> references -> file verification.

## Deep Dives

- `references/principles.md` - Search principles and strategy
- `references/validation.md` - Pre-delivery validation checklist
- `references/critique.md` - Self-critique for search quality
- `references/example.md` - End-to-end CLI + MCP example
