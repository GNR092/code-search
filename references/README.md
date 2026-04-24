# Code Search

Búsqueda semántica de código en español e inglés. Busca patrones de código, símbolos, referencias y navega el codebase eficientemente.

**Compatibilidad**: `codesearch v0.1.200+149`  
**Disponible en**: Windows y Linux

---

## Interfaces

| Método | Comando | Ventaja |
|--------|---------|---------|
| **MCP** | `codesearch_semantic_search()`, `codesearch_find_references()`, etc. | Resultados inline, sin cambio de contexto |
| **CLI** | `codesearch search`, `codesearch index`, etc. | Checks manuales, diagnósticos, setup |

---

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
codesearch_index_status()
```

Si MCP no responde → hacer fallback a CLI.

### 3. Índice listo

```bash
codesearch doctor
```

```text
codesearch_index_status()
```

Si no está listo → `codesearch index` o `codesearch setup`.

---

## Comandos CLI Verificados

### Help y diagnóstico
```bash
codesearch --help
codesearch search --help
codesearch index --help
codesearch doctor
codesearch stats
```

### Indexación
```bash
codesearch index [PATH]              # Indexar directorio
codesearch index --list              # Ver estado de índices
codesearch index --force             # Re-indexar todo
codesearch index --dry-run           # Ver qué se indexaría
codesearch serve [PATH] -p 4444      # Servidor con watch en tiempo real
```

### Búsqueda
```bash
codesearch search "query" --max-results 5 --scores
codesearch search "query" --filter-path "mi-proyecto/"
codesearch search "query" --sync        # Re-indexar cambios antes de buscar
codesearch search "query" --json        # Output JSON para agentes
```

### Modelos
```bash
codesearch --model jina-code search "query"  # Requiere re-index si el modelo es diferente
```

### Cache
```bash
codesearch cache stats
codesearch cache clear
```

---

## MCP Tools

| Tool | Función |
|------|---------|
| `codesearch_index_status()` | Estado del índice |
| `codesearch_find_databases()` | Listar bases de datos disponibles |
| `codesearch_semantic_search()` | Búsqueda semántica |
| `codesearch_find_references()` | Encontrar usos de un símbolo |

### Ejemplo MCP

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

---

## Modelos Disponibles

| Modelo | Dimensiones | Uso recomendado |
|--------|-------------|-----------------|
| `minilm-l6-q` (default) | 384 | Balance velocidad/precisión |
| `minilm-l12-q` | 384 | Mejor precisión |
| `bge-small-q` | 384 | Rápido |
| `jina-code` | 768 | Código (requiere re-index) |
| `e5-multilingual` | 384 | Multilingüe (español/inglés) |
| `nomic-v1.5` | 768 | Alta calidad |

**Nota**: Cambiar modelo requiere re-indexar (`codesearch index --force`).

---

## Workflow Recomendado

```
1. VALIDAR
   ├─ codesearch doctor
   └─ codesearch index --list

2. CONSULTA BROAD
   └─ codesearch_semantic_search(query="intent", limit=10)

3. REFINAR
   ├─ filter_path para scoping de proyecto
   └─ Términos de dominio (CFDI, factura, requisición...)

4. REFERENCIAS
   └─ codesearch_find_references(symbol="MiClase")

5. VERIFICAR
   └─ Leer archivos reales con tools de file read
```

---

## Path Scoping (Crítico)

Codesearch puede usar un índice padre/global. **Siempre usar filtro de path**:

```bash
--filter-path "Documents/Proyectos/mi-proyecto/"
```

```text
filter_path="Documents/Proyectos/mi-proyecto/"
```

Sin filtro, los resultados pueden incluir código de otros repositorios.

---

## Tips de Consultas

| Tipo | Ejemplo |
|------|---------|
| Verbo + intención | "where invoice status changes to paid" |
| Términos dominio | "CFDI", "regimen fiscal", "timbrado" |
| Puntos de entrada | "entrypoint for admin settings update" |
| Español | "dónde cambia el status de factura a pagada" |
| Español | "busca la función que genera cfdi" |
| Español | "encuentra donde se valida el presupuesto" |

**Iterar**: query broad → query refinada → referencias → verificación

---

## Opciones Avanzadas

| Opción | Descripción |
|--------|-------------|
| `--sync` | Re-index archivos modificados antes de buscar |
| `--scores` | Mostrar relevancia de resultados |
| `--content` | Mostrar contenido completo del chunk |
| `--rerank` | Neural reranking con Jina Reranker |
| `--vector-only` | Solo búsqueda vectorial (sin FTS híbrido) |
| `--rrf-k 60` | Parámetro de fusión RRF (default 20) |

---

## Troubleshooting

| Problema | Solución |
|----------|----------|
| `IndexWriter lock busy` | Detener `codesearch serve` o jobs paralelos |
| `index not found` | `codesearch setup` && `codesearch index` |
| `Invalid vector dimensions` | Re-index con el modelo correcto |
| Demasiados resultados cruzados | Usar `--filter-path` |
| Baja relevancia | Probar `e5-multilingual` para español |
| Índice obsoleto | `codesearch index --force` |

---

## Referencias Internas

- `principles.md` - Principios de búsqueda
- `validation.md` - Checklist de validación
- `critique.md` - Auto-revisión de calidad
- `example.md` - Ejemplo end-to-end CLI + MCP

---

## Instalación

```bash
# Descargar modelos
codesearch setup

# Indexar proyecto actual
codesearch index

# O indexar proyecto específico
codesearch index /path/a/proyecto

# Servidor con watch en tiempo real
codesearch serve /path/a/proyecto -p 4444
```
