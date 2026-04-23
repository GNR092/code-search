# Code Search Principles

Core principles for reliable semantic code exploration.

## 1) Discover first, conclude later

- Start broad with semantic search.
- Narrow gradually with `filter_path` and better query wording.
- Confirm behavior by reading concrete files and lines before answering.

## 2) Path scoping is mandatory in shared indexes

- Codesearch can use a parent/global index.
- Always constrain scope for project-specific work:
  - MCP: `filter_path`
  - CLI: `--filter-path`

## 3) References before refactors

- Before changing a symbol, run a references lookup.
- Use results for impact analysis and test planning.

## 4) Iterative query strategy

- Query 1: intent-level (feature or workflow)
- Query 2: domain terms (business vocabulary)
- Query 3: symbol-level (class/method/variable)

## 5) Validate freshness

- Check index status before deep investigation.
- If results look stale, reindex or use sync options.
