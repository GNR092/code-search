# Example Session

Practical CLI + MCP flow for a real question.

## Goal

Find where `site_name` is configured and used in an admin settings flow.

## MCP path

```text
codesearch_index_status()
codesearch_find_databases()
codesearch_semantic_search(
  query="site name setting in admin configuration",
  compact=true,
  limit=10,
  filter_path="Documents/Proyectos/mi-proyecto"
)
codesearch_find_references(symbol="Setting", limit=30)
```

Then open returned files and verify lines with file-read tools.

## CLI path

```bash
codesearch doctor
codesearch index --list
codesearch search "site_name setting in admin configuration" \
  --max-results 10 \
  --scores \
  --filter-path "Documents/Proyectos/mi-proyecto/"
```

## Expected output style

- Mention exact files found
- Explain the flow (seed/default -> controller update -> blade usage)
- Note caveats (parent shared index, stale index risk, model mismatch)

## Example: Spanish Codebase

```bash
codesearch search "modelo de factura en base de datos" --scores
```

```text
codesearch_semantic_search(
  query="modelo de factura en base de datos",
  limit=5
)
```

## Example: Model Selection

If searching Spanish code, consider:

```bash
codesearch setup --model e5-multilingual
codesearch index --force
codesearch search "dónde se valida el presupuesto" --filter-path "ERP/"
```

## Example: Using Sync

If codebase was recently modified:

```bash
codesearch search "nueva función de cfdi" --sync --filter-path "ERP/"
```

## Advanced: Reranking

For better accuracy on complex queries:

```bash
codesearch search "entrypoint para procesar facturas CFDI" \
  --rerank \
  --rerank-top 20 \
  --filter-path "ERP/"
```
