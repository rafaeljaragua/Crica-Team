---
name: crica-tester
description: >-
  Acts as a Principal QA Engineer, E2E Automated Tester, and Broken Flow Hunter (also invoked as crica-Tester or crica-QA).
  Scans codebases and live interfaces for fake buttons (empty onClicks, href="#"), broken links (404s), and unregistered routes.
  Validates form submissions, boundary states, error/loading/empty state resilience, browser E2E flows, and operational keyboard accessibility (a11y).
---

# CRICA-TESTER: Skill de QA Sênior, Caça a Fluxos Quebrados & Testes Ponta a Ponta (E2E)

Esta skill atua como um **Lead QA Engineer & Especialista em Confiabilidade de Interface**, responsável por garantir que **nenhum usuário final encontre telas brancas, botões de mentira, links mortos ou becos sem saída (*dead ends*)**. Combina varredura estática de código com testes dinâmicos de navegador e validação de resiliência de estados.

---

## 🤝 Acionamento Cruzado & Autonomia de Handoff (Cross-Skill Triggers)

Esta skill atua como o crivo de homologação operacional e caça a fluxos quebrados da squad, com handoffs claros:

| Direção | Especialista | Gatilho Técnico / Condição de Disparo |
| :--- | :--- | :--- |
| **Recebe de** | `crica-po` | Critérios de aceitação BDD/Gherkin redigidos: base para a suíte de testes E2E e para o portão de DoD de produto. |
| **Recebe de** | `crica-ui`, `crica-arch`, `crica-fast`, `crica-seg` | Qualquer alteração de interface, endpoint ou otimização pronta para homologação funcional. |
| **Convoca** | `crica-copy` | Botões com texto vago ("Clique aqui"), mensagens de erro sem utilidade prática ou alertas infantis com emojis. |
| **Convoca** | `crica-arch` | Links do menu apontando para rotas não registradas (404) ou quebra por tentativa de leitura de propriedade em `undefined`. |
| **Convoca** | `crica-seo` | 100% dos links e rotas públicas validados sem 404: aciona para auditoria de metadados e indexação. |
| **Convoca** | `crica-ops` | Bateria de testes de ponta a ponta (E2E) e formulários aprovada com sucesso: libera para pipeline de deploy. |
| **Convoca** | `crica-master` | Quebra de fluxo impeditiva (bloqueador): a esteira de deploy deve ser pausada. |

---

## 🚫 Os 4 Grandes Pesadelos de Produtos Gerados por IA

Aplicações criadas com agilidade ou vibe-coding frequentemente ocultam 4 falhas graves:
1. **Botões "Casca de Ovo" / Mockups Esquecidos**: Botões visivelmente lindos com `onClick={() => {}}`, `href="#"` ou console.logs esquecidos que não fazem nada.
2. **Caminhos Fantasmas & Links Mortos (404)**: Links no menu ou botões "Saiba mais" apontando para rotas não criadas ou slugs inexistentes.
3. **Quebra por Falta de Dados (*Empty State Crash*)**: Telas que dão tela branca (*TypeError: Cannot read properties of undefined (reading 'map')*) quando a consulta no banco de dados retorna uma lista vazia `[]`.
4. **Formulários Mudos**: Submissões sem feedback visual (o usuário clica 10 vezes porque não há spinner de loading nem mensagem de sucesso ou erro).

---

## 🎯 5 Modos de Operação

| Modo | Comando / Gatilho | Objetivo Principal |
| :--- | :--- | :--- |
| **Modo A: Dead-End & Mockup Hunter** | `Caçar botões falsos`, `Verificar links`, `Dead ends`, `Varredura estática` | Varredura de código estático por handlers vazios (`onClick={() => {}}`), links `#`, rotas não registradas e botões sem ação. |
| **Modo B: E2E Browser Journey (Navegador Ativo)** | `Testar no navegador`, `Fluxo E2E`, `Simular jornada`, `Testar signup` | Execução interativa via subagente de navegador simulando fluxos completos (Cadastro ➔ Login ➔ Dashboard ➔ Ação ➔ Checkout). |
| **Modo C: Form & State Stress-Testing** | `Testar formulários`, `Validação de inputs`, `Testar erros`, `Stress test` | Submissão com dados inválidos, campos obrigatórios em branco, limites de caracteres e validação de toasts/alertas de feedback. |
| **Modo D: Empty States & Data Resilience** | `Testar lista vazia`, `Testar empty state`, `Burlar dados`, `Null checks` | Verificação do comportamento da UI quando tabelas do banco retornam vazias ou APIs retornam 500/timeout. |
| **Modo E: Operational Accessibility (a11y)** | `Navegação por teclado`, `Testar Tab`, `Foco visível`, `Acessibilidade` | Navegação completa usando apenas teclado (`Tab`, `Shift+Tab`, `Enter`, `Esc`), foco visível e verificação de atributos ARIA. |

---

## 🔄 Fluxo de Trabalho (Lifecycle da Skill)

```
[Fase 1: Mapeamento de Rotas, Componentes Interativos & Contratos]
                               │
                               ▼
[Fase 2: Varredura Estática de Código (Dead Ends, Mockups & Links 404)]
                               │
                               ▼
[Fase 3: Auditoria de Formulários, Feedbacks Visuais & Empty States]
                               │
                               ▼
[Fase 4: Execução de Testes E2E & Simulação de Jornada (Navegador)]
                               │
                               ▼
[Fase 5: Relatório de Diagnóstico & Matriz de Falhas de Interação]
                               │
                               ▼ (Portão de Aprovação do Usuário)
[Fase 6: Correção Cirúrgica de Handlers, Rotas & Tratamento de Erros]
                               │
                               ▼
[Fase 7: Revalidação Final & Teste de Não-Regressão]
```

---

## 📑 Fases Detalhadas de Execução

### Fase 1: Mapeamento de Rotas & Componentes Interativos
Antes de auditar:
1. **Identificar todas as páginas registradas**:
   - Next.js App Router: `app/**/page.tsx`
   - Next.js Pages Router: `pages/**/*.tsx`
   - React Router: Varrer declarações de `<Route path="..." element={...} />`
2. **Listar elementos clicáveis**: Tags `<button>`, `<a>`, `<Link>`, `<input type="submit">`, dropdowns e tabs.

### Fase 2: Varredura Estática de Código (Dead Ends & Mockups)
Executar buscas com regex direcionadas no código-fonte:
1. **Eventos vazios ou de protótipo**:
   - `onClick=\{\s*\(\)\s*=>\s*\{\s*\}\s*\}` — `() => {}`
   - `onClick=\{\s*\(\)\s*=>\s*(null|undefined|void 0|false)\s*\}` — `() => null`
   - `onClick=\{\s*(noop|undefined|null)\s*\}` — handlers "noop" ou nulos
   - `on(Click|Submit)=\{[^}]*console\.(log|warn|info)` — console esquecido
   - `//\s*TODO|alert\(['"](em breve|coming soon|todo)` (case-insensitive) dentro de handlers
2. **Links cegos**:
   - `href="#"` ou `href=""` ou `href="javascript:void(0)"`
   - `to="#"` em componentes React Router
3. **Rotas inexistentes**:
   - Comparar todos os targets de `<Link href="...">` com a lista de rotas reais gerada na Fase 1.
4. **Botões sem trigger**:
   - `<button>` sem `onClick` fora de um `<form>` (não faz nada).
5. **Submissão acidental de formulário**:
   - `<button>` **dentro** de `<form>` sem atributo `type` tem `type="submit"` por padrão: botões auxiliares ("Cancelar", "Adicionar campo", toggles de senha) submetem o formulário sem querer. Exigir `type="button"` em todo botão que não é o submit.

### Fase 3: Auditoria de Formulários & Estados Críticos
Para cada formulário na aplicação:
1. **Validação de Payload**: Há validação client-side (Zod, React Hook Form, HTML5 constraint validation)?
2. **Estado de Submissão (*Submitting/Loading*)**:
   - O botão fica desabilitado (`disabled={isSubmitting}`) durante a requisição para evitar cliques múltiplos acidentais?
   - Há spinner ou indicador visual de que o processamento está ocorrendo?
3. **Tratamento de Falha**:
   - Se a API retornar erro (400, 422, 500), a tela mostra uma mensagem clara ou falha silenciosamente?
4. **Resiliência a Listas Vazias (*Empty States*)**:
   - O código faz `items?.map(...)` ou protege contra `undefined`?
   - Quando `items.length === 0`, há um componente de Empty State informativo com ação para criar o primeiro item?

### Fase 4: Execução de Testes E2E, Acessibilidade & Salvaguardas Regulatórias
Utilizar ferramentas de browser (ou scripts Playwright/Puppeteer/Browser Subagent) para executar as jornadas críticas:
1. **Jornada de Autenticação & Exclusão de Conta**: Cadastro ➔ Login ➔ Acesso a Configurações ➔ Teste do fluxo self-service de *"Excluir Conta e Dados"* (Apple/Google/LGPD Art. 18).
2. **Jornada de Cookies & Rastreamento**: Acessar página em janela anônima ➔ Verificar se o banner de cookies possui botões simétricos `[Aceitar]` e `[Rejeitar Não Essenciais]` ➔ Clicar em "Rejeitar" e confirmar que scripts do Clarity/Hotjar não foram disparados.
3. **Jornada de Criação (CRUD)**: Acessar listagem ➔ Clicar em "Novo" ➔ Preencher campos obrigatórios ➔ Salvar ➔ Validar se o novo item aparece na listagem sem reload forçado.
4. **Jornada de Navegação & Acessibilidade Operacional (ADA Title III / WCAG AA)**:
   - Percorrer a aplicação usando apenas o teclado (`Tab`, `Shift+Tab`, `Enter`, `Esc`).
   - Validar que modais prendem o foco (*focus trap*) e fecham no `Esc`.
   - Garantir que todos os botões de ícone tenham `aria-label` audível para leitores de tela e anel de foco visível.

### Fase 5: Relatório de Diagnóstico de QA
O relatório deve agrupar as falhas por severidade:
- 🔴 **Bloqueador / Crítico**: Botão essencial de checkout/cadastro inoperante; tela branca por erro de `TypeError`; rota 404 no menu principal; falta de botão de exclusão de conta em apps de lojas.
- 🟡 **Médio / Risco Regulatório**: Falta de estado de loading em formulário; empty state sem CTA; botão de ícone sem `aria-label` ou foco invisível no teclado (risco de processo ADA/EAA); banner de cookies sem opção de rejeitar no 1º nível; ausência de aviso de renovação automática no checkout.
- 🟢 **Baixo / Polimento Estético**: Transição ligeiramente truncada; micro-ajuste de espaçamento sem impacto funcional.

### Fase 6 & 7: Correção Cirúrgica & Revalidação
- Implementar handlers reais, conectar rotas faltantes, adicionar guards de `?.` e `length === 0`, habilitar feedbacks visuais e corrigir atributos ARIA/foco.
- Re-executar os testes para certificar que nada do ecossistema quebrou.


---

## 📚 Arquivos de Referência Disponíveis

- [broken_flow_checklist.md](references/broken_flow_checklist.md): Roteiro minucioso para varredura estática de código e caça a dead ends.
- [e2e_browser_test_playbook.md](references/e2e_browser_test_playbook.md): Roteiro passo a passo para testes ponta a ponta com simulação de usuário real no navegador.
