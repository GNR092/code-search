---
name: code-search
description: Búsqueda semántica de código en español e inglés. Busca patrones de código, símbolos, referencias y navega el codebase eficientemente. Usa esta skill cuando digas "busca", "busca código", "búsqueda", "encontrar código", "encuentra", "explora", "search" o cualquier consulta de búsqueda en cualquier idioma.
---

# Code Search

Búsqueda semántica de código en español e inglés. Compatible con `codesearch v0.1.200+149` en Windows y Linux.

## Scope

Usa esta skill cuando necesites:
- Orientación rápida en repositorios desconocidos
- Descubrimiento semántico a través de muchos archivos
- Análisis de impacto de símbolos antes de refactors

No usar para lookups triviales en archivos individuales; reads directos son más rápidos.

## Interfaces

| Método | Comando | Ventaja |
|--------|---------|---------|
| **MCP** | `codesearch_semantic_search()`, `codesearch_find_references()`, etc. | Resultados inline, sin cambio de contexto |
| **CLI** | `codesearch search`, `codesearch index`, etc. | Checks manuales, diagnósticos, setup |

## Validación Obligatoria (Preflight)

Siempre validar antes de usar en una sesión.

### 1. CLI disponible

```bash
codesearch --version
```

Si no existe → reportar que falta y sugerir instalación.

### 2. MCP tools disponibles

```text
codesearch_find_databases()
```

Si MCP no responde → fallback a CLI.

### 3. Índice listo

```bash
codesearch doctor
```

```text
codesearch_index_status()
```

## Workflow Recomendado

```
1. VALIDAR
   ├─ codesearch doctor
   └─ codesearch index --list

2. CONSULTA BROAD
   └─ codesearch_semantic_search(query="intent", limit=10)

3. REFINAR
   ├─ filter_path para scoping
   └─ Términos de dominio (CFDI, factura, requisición...)

4. REFERENCIAS
   └─ codesearch_find_references(symbol="MiClase")

5. VERIFICAR
   └─ Leer archivos reales
```

## Politica de bajo consumo

Usa `code-search` como primer paso para ubicar archivos, símbolos y referencias. No abras archivos completos ni recorras carpetas grandes antes de tener resultados compactos.

Presupuesto recomendado:

- Exploracion inicial: 5 a 8 resultados.
- Refinamiento: 3 a 5 resultados.
- Lectura directa: maximo 1 a 3 archivos, preferentemente fragmentos.
- Documentacion: generar desde resumen compacto, no desde codigo completo.

## Exclusiones obligatorias

Ignora por completo estas carpetas durante busquedas, indexacion, lectura de resultados y resumenes:

- `.git/`
- `__pycache__/`
- `.pytest_cache/`
- `.mypy_cache/`
- `.ruff_cache/`
- `node_modules/`
- `vendor/`
- `dist/`
- `build/`
- `coverage/`
- `.codesearch.db/`

Si algun resultado aparece dentro de una ruta excluida, descartalo y repite la busqueda con `filter_path` mas especifico.

Fallback textual con exclusiones, solo cuando la busqueda semantica no pueda resolverlo:

```bash
rg "texto_a_buscar" \
  --glob '!**/.git/**' \
  --glob '!**/__pycache__/**' \
  --glob '!**/node_modules/**' \
  --glob '!**/vendor/**' \
  --glob '!**/dist/**' \
  --glob '!**/build/**' \
  --glob '!**/coverage/**' \
  --glob '!**/.codesearch.db/**'
```

## Modelos Disponibles

| Modelo | Dimensiones | Uso |
|--------|-------------|-----|
| `minilm-l6-q` | 384 | Default, balance velocidad/precisión |
| `bge-small-q` | 384 | Rápido |
| `minilm-l12-q` | 384 | Mejor precisión |
| `jina-code` | 768 | Código (requiere re-index) |
| `e5-multilingual` | 384 | Multilingüe (español/inglés) |
| `nomic-v1.5` | 768 | Alta calidad |

**Nota**: Cambiar modelo requiere re-indexar (`codesearch index --force`).

## Comandos CLI Verificados

### Diagnóstico
```bash
codesearch doctor
codesearch stats
codesearch index --list
```

### Indexación
```bash
codesearch index [PATH]              # Indexar
codesearch index --force            # Re-indexar
codesearch serve [PATH] -p 4444     # Servidor con watch
```

### Búsqueda
```bash
codesearch search "query" --max-results 5 --scores
codesearch search "query" --filter-path "proyecto/"
codesearch search "query" --sync          # Re-index cambios antes de buscar
```

## MCP Tools

- `codesearch_index_status()` - Estado del índice
- `codesearch_find_databases()` - Listar DBs
- `codesearch_semantic_search()` - Búsqueda semántica
- `codesearch_find_references()` - Usos de un símbolo

## Path Scoping (Crítico)

Codesearch puede usar un índice padre/global. **Siempre usar filtro**:

```bash
--filter-path "Documents/Proyectos/mi-proyecto/"
```

```text
filter_path="Documents/Proyectos/mi-proyecto/"
```

## Tips de Consultas

| Tipo | Ejemplo |
|------|---------|
| Verbo + intención | "where invoice status changes to paid" |
| Términos dominio | "CFDI", "regimen fiscal", "timbrado" |
| Español | "dónde cambia el status de factura a pagada" |
| Español | "busca la función que genera cfdi" |

## Opciones Avanzadas

| Opción | Descripción |
|--------|-------------|
| `--sync` | Re-index archivos modificados antes de buscar |
| `--scores` | Mostrar relevancia |
| `--rerank` | Neural reranking |
| `--vector-only` | Solo búsqueda vectorial |

## Troubleshooting

| Problema | Solución |
|----------|----------|
| `IndexWriter lock busy` | Detener `codesearch serve` |
| `index not found` | `codesearch setup` && `codesearch index` |
| `Invalid vector dimensions` | Re-index con modelo correcto |
| Baja relevancia | Probar `e5-multilingual` para español |

## Referencias

- `references/README.md` - Guía completa
- `references/principles.md` - Principios de búsqueda
- `references/validation.md` - Checklist de validación
- `references/critique.md` - Auto-revisión de calidad
- `references/example.md` - Ejemplos end-to-end
