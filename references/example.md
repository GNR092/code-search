# Example Session

Practical CLI + MCP flow for a real question.

## Goal

Find where `site_name` is configured and used in an admin settings flow.

## MCP path

```text
codesearch_index_status()
codesearch_find_databases()
codesearch_semantic_search(query="site name setting in admin configuration", compact=true, limit=10, filter_path="Documents/Residentes/Proyectos/facturacion_alejandro")
codesearch_find_references(symbol="Setting", limit=30)
```

Then open returned files and verify lines with file-read tools.

## CLI path

```bash
codesearch doctor
codesearch index --list
codesearch search "site_name setting in admin configuration" --max-results 10 --scores --filter-path "Documents/Residentes/Proyectos/facturacion_alejandro/"
```

## Expected output style

- Mention exact files found.
- Explain the flow (seed/default -> controller update -> blade usage).
- Note caveats (e.g., parent shared index, stale index risk).
