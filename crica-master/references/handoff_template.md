# Template de Handoff & Quadro da Squad

Toda skill, ao concluir sua etapa dentro de uma missão orquestrada pelo `crica-master`, emite um bloco de handoff neste formato. Ele é o **único insumo** que a próxima skill pode assumir como verdade.

---

## 📦 Bloco de Handoff

```markdown
### 🔁 Handoff: crica-<origem> → crica-<destino>

**Status da etapa**: ✅ Concluída / ⚠️ Concluída com ressalvas / ⛔ Bloqueada

**Entradas consumidas**
- (Handoff anterior, arquivos lidos, decisões do usuário)

**Artefatos produzidos ou alterados**
- `caminho/do/arquivo.ts` — o que mudou (1 linha)
- `implementation_plan.md` / relatório — se aplicável

**Decisões tomadas**
- (Escolhas técnicas ou de produto que a próxima skill deve respeitar)

**Contratos expostos à próxima skill**
- (Schemas, tipos, endpoints, tokens, textos aprovados, rotas)

**Pendências & riscos**
- (O que ficou fora do escopo, dúvidas abertas, débitos introduzidos)

**Portão de DoD desta skill**: ✅ Atendido / ⛔ Bloqueado / N/A — motivo
```

### Regras
1. **Sem premissas implícitas**: se algo não está no handoff, a próxima skill pergunta ao master em vez de supor.
2. **Concisão**: no máximo ~20 linhas. Detalhes ficam nos artefatos linkados.
3. **Bloqueio propaga**: um handoff `⛔ Bloqueada` faz o master acionar a Fase 4 (Remoção de Impedimentos) antes de seguir.

---

## 🗂️ Modelo de `squad_board.md`

```markdown
# Squad Board — <nome da missão>

**Playbook**: Feature / Auditoria 360° / Crise / Growth
**Objetivo**: <1 frase>

| # | Skill | Etapa | Status | Portão DoD |
| :- | :--- | :--- | :--- | :--- |
| 1 | crica-po | User stories + Gherkin | ✅ Concluída | ✅ |
| 2 | crica-copy | Microcopy da feature | 🔄 Em andamento | — |
| 3 | crica-arch | Schema + services | ⏳ Pendente | — |
| 4 | crica-seo | — | N/A (sem páginas públicas) | N/A |

## Impedimentos ativos
- (nenhum)

## Decisões do usuário
- (registro cronológico)
```
