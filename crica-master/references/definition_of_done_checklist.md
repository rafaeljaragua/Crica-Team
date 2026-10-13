# Checklist Oficial: Definition of Done (DoD) de Alto Rigor da Squad Crica

Nenhuma tarefa, feature, refatoração ou sprint é considerada concluída (*Done*) sem a validação fática e sem complacência deste checklist pelo `crica-master`.

> ⚠️ **Princípio da Evidência Fática**: É terminantemente proibido marcar um portão como `✅ Atendido` por presunção ou intenção. Cada portão atendido **deve registrar a evidência concreta** (arquivo + linhas inspecionadas, comando executado + saída no terminal, ou cenário de teste validado). Na ausência de evidência verificada, o portão é marcado como `⛔ Rejeitado` e a entrega volta para retrabalho.

**Status possíveis por portão**:
- `✅ Aprovado (com evidência)`: Verificado no código/terminal com prova concreta.
- `⛔ Rejeitado (Retrabalho)`: Não cumpre a régua técnica ou falta acabamento/tratamento de exceção.
- `N/A`: Skill não acionada no playbook — exige justificativa técnica formal de 1 linha.

Um único portão `⛔ Rejeitado` **bloqueia a entrega**. O usuário NÃO deve ser incomodado para apontar erros que a própria squad tinha a obrigação de pegar.

---

## 🛡️ Portão Zero: Smoke Gate de Tolerância Zero (Anti-Amadorismo)

Antes de auditar os 10 portões especialistas, o `crica-master` verifica se a entrega não contém gafes óbvias:
- [ ] **Sem botões órfãos/falsos**: Nenhum botão sem `onClick`, com handler vazio (`() => {}`), `href="#"` ou console.log de teste.
- [ ] **Sem ações cegas (Feedback visual obrigatório)**: Toda chamada assíncrona desabilita o botão para evitar clique duplo, exibe spinner/loading state e toast/alerta de sucesso ou erro amigável.
- [ ] **Sem telas mortas (Os 4 Estados Obrigatórios)**: O componente trata os estados **Ideal**, **Carregando (Skeleton/Spinner)**, **Vazio (Empty State com CTA)** e **Erro (com botão de tentar novamente)**.
- [ ] **Sem erros no console**: Zero advertências de unhandled promise rejections, hydration errors ou React keys duplicadas/faltantes.

---

## 🎯 Os 10 Portões de Aceitação com Evidência Fática

### 1. Produto & Negócio (`crica-po`)
- [ ] **Critérios BDD/Gherkin atendidos**: Cada cenário foi implementado tanto no caminho feliz quanto nos fluxos de exceção.
- [ ] **Product Acceptance Sign-off emitido**: O PO auditou a entrega e confirmou que a experiência não possui becos sem saída (*dead ends*).
- *Evidência obrigatória*: Referência aos cenários Gherkin e lista de fluxos alternativos testados.

### 2. Comunicação & Tom (`crica-copy`)
- [ ] **Microcopy de alta precisão**: Zero termos genéricos de IA (*"desbloqueie seu potencial"*, *"revolucione sua gestão"*).
- [ ] **Mensagens de erro resolutivas**: Mensagens de erro explicam o que deu errado E o que o usuário deve fazer para resolver (nunca apenas *"Ocorreu um erro"*).
- *Evidência obrigatória*: Lista dos textos/mensagens revisados e arquivo alterado.

### 3. Estética & Acessibilidade Visual (`crica-ui`)
- [ ] **Design Tokens & Consistência**: Cores, espaçamentos, tipografia e sombras vindos das variáveis do sistema, sem valores mágicos (`style={{ margin: '13px' }}`).
- [ ] **Responsividade & Toque**: Testado visualmente em mobile (375px), tablet e desktop; alvos de toque >= 44x44px.
- [ ] **Acessibilidade básica**: Contraste WCAG AA (mínimo 4.5:1 para texto normal), foco visível para teclado (`:focus-visible`).
- *Evidência obrigatória*: Classes/tokens utilizados e validação de contraste e breakpoints.

### 4. Arquitetura & Tipagem Estrita (`crica-arch`)
- [ ] **Zero `any` / Zero `as unknown as X`**: Tipagem TypeScript estrita de ponta a ponta.
- [ ] **Desacoplamento de camadas**: Componentes visuais não fazem queries diretas nem manipulam lógica de negócio complexa (isolada em hooks/services/actions).
- [ ] **Validação de entrada na fronteira**: Schemas Zod/validações presentes em todas as bordas de dados (APIs, formulários, server actions).
- *Evidência obrigatória*: Arquivos de schemas e interfaces inspecionados.

### 5. Desempenho & Eficiência (`crica-fast`)
- [ ] **Sem N+1 e sem overfetching**: Projeção de colunas estrita nas consultas (`select id, name`, nunca `select *`).
- [ ] **Paginação & Limites**: Nenhum endpoint ou query retorna listas infinitas sem limite/cursor.
- [ ] **Renderizações controladas**: Sem re-renders desnecessários em cascata em loops ou listas longas.
- *Evidência obrigatória*: Trecho da query com `select` explícito e paginação implementada.

### 6. Segurança, Proteção de Dados & Riscos Jurídicos Globais (`crica-seg`)
- [ ] **RLS e Autorização de Servidor**: Toda mutação valida a sessão do usuário no backend/server action, nunca confiando em IDs vindos do frontend.
- [ ] **Zero segredos expostos**: Nenhuma chave de API privada, service role key ou token em código client-side ou versionado no Git.
- [ ] **Proteção contra injeção e abuso**: Parâmetros sanitizados e rate limiting previsto em rotas públicas.
- [ ] **Os 11 Riscos Jurídicos Globais Auditados**:
  - *1. Idade*: Verificação real de idade se houver menores (ECA/COPPA/GDPR Art. 8).
  - *2. Fontes*: Fontes auto-hospedadas (zero CDN externo como Google Fonts).
  - *3. Gravação*: Hotjar/Clarity sob consentimento e com máscara total de inputs (`mask-all-inputs`).
  - *4. E-mails*: CNPJ/endereço físico e link de descadastro funcional (CAN-SPAM/CDC).
  - *5. Assinaturas*: Preço, periodicidade e renovação ao lado do botão de assinar; cancelamento self-service.
  - *6. Copyright*: Canal de denúncia e fluxo de Notice & Takedown se houver UGC (DMCA/Marco Civil).
  - *7. Acessibilidade*: Conformidade WCAG AA contra litígios (ADA Title III / EAA / LBI).
  - *8. Cookies Simétricos*: Botão de `[Rejeitar]` não essenciais no 1º nível, sem dark patterns (GDPR/CNIL/ANPD).
  - *9. Reembolso Legal*: Prazo de reflexão de 7 dias (CDC 49) / 14 dias (UE) respeitado sem cláusula abusiva.
  - *10. Exclusão de Conta*: Botão self-service nas configurações que efetivamente apaga registros e arquivos (Apple/Google/LGPD Art. 18).
  - *11. Transparência de IA*: Disclosure de conteúdo gerado e garantia de não-treinamento com dados privados (EU AI Act).
- *Evidência obrigatória*: Verificação de políticas RLS, conferência do `.env.example` e status de conformidade dos 11 riscos globais aplicáveis.



### 7. Confiabilidade & Testes (`crica-tester`)
- [ ] **Varredura operacional executada**: Teste real do fluxo de ponta a ponta (envio de formulário, clique de botão, navegação de rotas).
- [ ] **Validação de cenários de borda**: Testado com dados limítrofes (strings longas, caracteres especiais, valores vazios, timeouts simulados).
- [ ] **Navegação por teclado**: Formulários e modais operáveis via `Tab`, `Enter` e `Esc`.
- *Evidência obrigatória*: Script de teste rodado com sucesso ou relatório de execução de passos com status OK.

### 8. Descoberta & Metadados (`crica-seo`)
- [ ] **Metadados únicos por rota pública**: `title`, `meta description`, `openGraph` e canonical configurados sem textos padrão ("Create Next App").
- [ ] **Estrutura semântica**: Um único `h1` por página com hierarquia correta de headings (`h2`, `h3`).
- *Evidência obrigatória*: Trecho do metadata/head da página inspecionado.

### 9. Infraestrutura & Build (`crica-ops`)
- [ ] **Typecheck e Lint limpos**: `tsc --noEmit` (ou equivalente) executado sem erros de tipagem.
- [ ] **Build de produção funcional**: `npm run build` (ou comando de bundle) finalizado com sucesso no ambiente local.
- *Evidência obrigatória*: Saída do comando no terminal com código de saída 0.

### 10. Documentação & Rastreabilidade (`crica-docs`)
- [ ] **Changelog e referências atualizados**: Registro no `CHANGELOG.md` e atualização do README ou schema OpenAPI se novas rotas/variáveis foram criadas.
- *Evidência obrigatória*: Arquivo de documentação alterado com a descrição da mudança.

---

## 👤 Donos e Responsáveis por Temas Compartilhados

| Tema | Dono (assina o portão) | Colaboradores | Regra de Verificação |
| :--- | :--- | :--- | :--- |
| Acessibilidade | `crica-ui` (visual) / `crica-tester` (operacional) | `crica-seo` | ui: contraste, foco visível · tester: teclado, traps de foco · seo: tags alt e semântica |
| Segredos & `.env` | `crica-seg` | `crica-ops` | seg: varredura em código e gitignore · ops: consistência de `.env.example` |
| Overfetching | `crica-fast` (performance) | `crica-seg` (privacidade) | fast: payload mínimo · seg: garantia de que campos confidenciais não vazam |
| Migrations | `crica-arch` | `crica-fast`, `crica-ops` | arch: modelagem · fast: índices para queries frequentes · ops: rollback seguro |

