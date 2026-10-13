# Template de Handoff & Quadro da Squad com Governança de Qualidade

Toda skill, ao concluir sua etapa dentro de uma missão orquestrada pelo `crica-master`, emite um bloco de handoff neste formato.
O `crica-master` é obrigado a auditar este bloco: se faltarem evidências fáticas ou se houver pontas soltas (erros óbvios, botões sem handler, falta de estados de erro), **o Master emite um parecer de VETO e devolve a etapa para retrabalho**.

---

## 📦 Bloco de Handoff com Evidências

```markdown
### 🔁 Handoff: crica-<origem> → crica-<destino>

**Status da etapa**: 
- [ ] ✅ Concluída com Evidências Fáticas
- [ ] ⚠️ Concluída com Ressalvas Justificadas
- [ ] ⛔ Bloqueada / Necessita Escalada

**Entradas consumidas**
- (Handoff anterior, arquivos analisados, decisões do usuário)

**Artefatos produzidos ou alterados**
- `caminho/do/arquivo.ts` — descrição exata da modificação
- `implementation_plan.md` / relatório — se aplicável

**Evidências fáticas da entrega (Obrigatório)**
- **Linhas/Trechos auditados**: (Ex: `UserButton.tsx:24-48` implementou estado de loading e desabilitação)
- **Comandos/Testes executados**: (Ex: `npm run lint` ou `npx vitest run` com exit code 0)
- **Estados cobertos**: (Ideal ✅ | Loading ✅ | Empty ✅ | Error ✅)

**Casos de borda e tratamento de erros verificados**
- (Como a entrega se comporta se a rede falhar, se o dado vier nulo ou se o input for inválido)

**Decisões tomadas & Contratos expostos**
- (Schemas Zod, interfaces TypeScript, tokens CSS, rotas criadas)

**Pendências, riscos & débitos identificados**
- (Nenhum / Listar estritamente o que ficou pendente e por que)

---

### 🧐 Parecer de Auditoria do crica-master:
- **Status do Gate**: 🟢 Aprovado para avanço | 🔴 VETADO - Devolvido para Retrabalho
- **Justificativa / Apontamento de Veto**: (Se vetado, listar exatamente o arquivo e a correção cirúrgica necessária)
```

### Regras de Ouro
1. **Sem premissas implícitas**: se algo não está no handoff com evidência, a próxima skill não assume que existe.
2. **Proibido "auto-aprovação cega"**: o Master nunca carimba avanço sem inspecionar as evidências fáticas.
3. **Loop de Veto**: em caso de 🔴 VETADO, o Master reabre a fase da skill de origem com o apontamento antes de tocar a esteira adiante.

---

## 🗂️ Modelo de `squad_board.md`

```markdown
# Squad Board — <nome da missão>

**Playbook**: Feature / Auditoria 360° / Crise / Growth
**Objetivo**: <1 frase executiva>
**Auditor Geral**: crica-master (Padrão Sênior / Tolerância Zero a Amadorismo)

| # | Skill | Etapa | Status | Evidência Principal | Portão DoD |
| :- | :--- | :--- | :--- | :--- | :--- |
| 1 | crica-po | User stories + Gherkin + 4 Estados | ✅ Aprovado | spec_v1.md com 8 cenários | ✅ Aprovado |
| 2 | crica-arch | Schemas Zod + Services | ✅ Aprovado | auth.schema.ts:1-42 | ✅ Aprovado |
| 3 | crica-ui | Componentes + Microcopy | 🔄 Em andamento | — | — |
| 4 | crica-tester | E2E + Smoke Gate | ⏳ Pendente | — | — |
| 5 | crica-ops | Build + Typecheck | ⏳ Pendente | — | — |

## Ciclos de Retrabalho (Rework Log)
- (Ex: *crica-ui vetado na rodada 1 por botão sem disabled durante mutation. Corrigido na rodada 2.*)

## Impedimentos ativos
- (nenhum)

## Decisões do usuário
- (registro cronológico)
```
