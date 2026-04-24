# Búsqueda Semántica: Principios Core

## 1) Validar antes de buscar

- Verificar que `codesearch doctor` pase
- Confirmar que el índice esté listo
- Identificar la base de datos activa

## 2) Descubrir primero, concluir después

- Start broad con búsqueda semántica
- Narrow gradualmente con `filter_path` y mejor wording
- Confirmar comportamiento leyendo archivos concretos

## 3) Path scoping es mandatorio

- Codesearch puede usar un índice padre/global
- Siempre restringir scope para trabajo específico:
  - MCP: `filter_path`
  - CLI: `--filter-path`

## 4) Referencias antes de refactors

- Antes de cambiar un símbolo, ejecutar reference lookup
- Usar resultados para análisis de impacto y planificación de tests

## 5) Estrategia iterativa de queries

- Query 1: nivel intent (feature o workflow)
- Query 2: términos de dominio (vocabulario de negocio)
- Query 3: nivel símbolo (clase/método/variable)

## 6) Validar frescura

- Check index status antes de investigación profunda
- Si resultados se ven stale, reindex o usar sync options

## 7) Español e inglés

- Codesearch soporta ambos idiomas
- Para mejor resultados en español, usar términos de dominio en español
- Considerar modelo `e5-multilingual` para mejor soporte multilingüe
