# Checklist de Auditoria de Performance (CRICA-FAST)

Use este documento durante a Fase 2 da skill. Cada item traz **como localizar** e **como corrigir sem quebrar contratos**.

---

## 1. Banco de Dados & Anti-patterns de Query

| Padrão | Como localizar | Correção |
| :--- | :--- | :--- |
| **N+1 Queries** | Chamadas `findUnique`, `findOne`, `findFirst`, `.from(...).select`, `query(` dentro de `.map(`, `forEach`, `for...of` ou `Promise.all(items.map(...))` | `include` / `with` / `JOIN`, `WHERE id IN (...)` em lote ou `DataLoader` |
| **FK sem índice** | Colunas `*_id` nas migrations sem `CREATE INDEX` correspondente (Postgres **não** cria índice automático em FK) | `CREATE INDEX idx_<tabela>_<coluna> ON <tabela>(<coluna>);` |
| **Filtro/ordenação sem índice** | Colunas em `WHERE`, `ORDER BY`, `GROUP BY` das queries críticas | Índice simples ou composto na ordem do filtro (`(workspace_id, created_at DESC)`) |
| **Overfetching de colunas** | `SELECT *`, `.select('*')`, `findMany()` sem `select` em tabelas com `text` longo, `jsonb` ou blobs | Projeção explícita dos campos consumidos pela UI |
| **RLS custosa (Supabase)** | Policies com `auth.uid()` direto na condição | Envolver em subselect: `(select auth.uid()) = user_id` para avaliar uma vez por query |

**Validação**: `EXPLAIN (ANALYZE, BUFFERS)` antes/depois; procurar `Seq Scan` em tabelas grandes.

---

## 2. Paginação & Transporte de Dados

- [ ] Toda listagem possui `LIMIT`/`take` padrão (ex: 20-50).
- [ ] Teto máximo aplicado no servidor: `Math.min(Number(limit) || 20, 100)`.
- [ ] Bases grandes ou feeds infinitos usam paginação por cursor (`WHERE created_at < $cursor ORDER BY created_at DESC LIMIT n`) em vez de `OFFSET` alto.
- [ ] Respostas JSON não incluem relações ou campos que a tela não usa.
- [ ] Contagens totais (`count(*)`) caras são evitadas ou estimadas em tabelas muito grandes.

---

## 3. Renderização & Ciclo de Vida (Frontend)

- [ ] **Re-render cascades**: estados de alta frequência (input, scroll, timers) não ficam no componente raiz nem em Context global consumido por toda a árvore.
- [ ] **Virtualização**: listas/tabelas com mais de ~50 itens usam `@tanstack/react-virtual` ou `react-window`.
- [ ] **Memoização**: filtros/ordenações de arrays grandes com `useMemo`; callbacks passados a filhos memoizados com `useCallback`. Se o projeto usa o **React Compiler**, não adicionar memoização manual redundante.
- [ ] **Debounce** em buscas que disparam requisições ou filtros pesados a cada tecla.
- [ ] **Server Components** (Next.js App Router): dados buscados no servidor em vez de `useEffect` + fetch no cliente quando possível.

---

## 4. Cache & Gestão de Estado no Cliente

- [ ] Camada de cache de requisições (TanStack Query, SWR, `fetch` com `next.revalidate`/tags).
- [ ] Endpoints públicos/estáticos com `Cache-Control` adequado (`public, s-maxage=..., stale-while-revalidate=...`) — configuração de CDN delegada ao `crica-ops`.
- [ ] Mutações invalidam apenas as chaves de cache afetadas.

---

## 5. Assets, Bundle & Carregamento

- [ ] Imagens com `width`/`height`, `loading="lazy"` abaixo da dobra, formatos WebP/AVIF ou `next/image`.
- [ ] Imagem LCP com `priority`/`fetchpriority="high"` e sem lazy loading.
- [ ] Bibliotecas pesadas (editores ricos, charts, PDF, mapas) via `React.lazy` / `next/dynamic`.
- [ ] Imports de bibliotecas utilitárias sem puxar o pacote inteiro (`lodash-es` por função, ícones individuais).
- [ ] Fontes com `font-display: swap` e subset; `next/font` quando disponível.

---

## 6. Core Web Vitals (limiares "bom" do Google)

| Métrica | Bom | Causas comuns em vibe-coding |
| :--- | :--- | :--- |
| **LCP** | ≤ 2,5 s | Hero com imagem sem prioridade, fontes bloqueantes, SSR lento |
| **INP** | ≤ 200 ms | Handlers síncronos pesados, re-render de árvores grandes no clique |
| **CLS** | ≤ 0,1 | Imagens/iframes sem dimensões, banners injetados após o carregamento, fontes sem fallback métrico |
