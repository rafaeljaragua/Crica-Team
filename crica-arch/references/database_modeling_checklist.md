# Checklist de Modelagem Relacional & Integridade de Dados

Use este checklist para revisar schemas SQL, migrations e modelagens de banco de dados relacionais (PostgreSQL / Supabase / MySQL).

---

## 🏛️ 1. Padrões de Chaves e Integridade

- [ ] **Chaves Primárias Únicas**: Toda tabela possui uma chave primária explícita (`id UUID DEFAULT gen_random_uuid() PRIMARY KEY` ou `BIGSERIAL PRIMARY KEY`).
- [ ] **Chaves Estrangeiras Declaradas**: Todos os relacionamentos possuem `REFERENCES tabela(id)` explícitos (evite apenas colunas soltas como `user_id text` sem chave estrangeira).
- [ ] **Regras de Cascata Definidas**:
  - `ON DELETE CASCADE` configurado para entidades dependentes diretas (ex: deletar fatura deleta os itens da fatura).
  - `ON DELETE RESTRICT` ou `ON DELETE SET NULL` para entidades com histórico contábil ou auditoria.
- [ ] **Restrições de Unicidade (*Unique Constraints*)**: Campos que não toleram duplicidade (ex: e-mail, slug, código de produto) possuem `UNIQUE` ou índices parciais de unicidade.

---

## ⚡ 2. Estratégia de Índices

- [ ] Toda coluna utilizada frequentemente em cláusulas `WHERE`, `JOIN` ou `ORDER BY` possui um índice:
  - Chaves estrangeiras sempre indexadas: `CREATE INDEX idx_invoices_workspace_id ON invoices(workspace_id);`
  - Filtros frequentes com ordenação de data: `CREATE INDEX idx_events_created_at ON events(created_at DESC);`
- [ ] Campos de texto pesquisados via autocomplete possuem índice trigram (`pg_trgm`) ou índices GIN específicos.

---

## 🕒 3. Auditoria e Rastreabilidade

- [ ] Todas as tabelas de negócio possuem timestamps:
  ```sql
  created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
  ```
- [ ] `updated_at` é atualizado automaticamente — o `DEFAULT now()` só vale no `INSERT`. Use trigger `BEFORE UPDATE` ou o ORM:
  ```sql
  -- Supabase / Postgres com a extensão moddatetime
  CREATE EXTENSION IF NOT EXISTS moddatetime SCHEMA extensions;
  CREATE TRIGGER handle_updated_at
    BEFORE UPDATE ON invoices
    FOR EACH ROW EXECUTE PROCEDURE extensions.moddatetime(updated_at);
  ```
  - Prisma: `updatedAt DateTime @updatedAt` (atualizado pelo client, não pelo banco).
- [ ] Tabelas críticas de transação financeira ou auditoria possuem modelo append-only (sem updates destrutivos, apenas novos eventos).
