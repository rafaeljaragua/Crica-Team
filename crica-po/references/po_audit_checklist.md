# 📋 Checklist Exaustivo de Auditoria de Produto & Padrão de Engenharia (CRICA-PO)

Este documento contém o roteiro de verificação 360° para diagnosticar a maturidade de produto, usabilidade de alto padrão, integridade de fluxos e viabilidade econômica de aplicações e produtos digitais.

---

## 1. Proposta de Valor, Job-To-Be-Done (JTBD) & Clareza Imediata
- [ ] **Dor Aguda vs. "Vitamina"**: O produto resolve um problema que custa tempo/dinheiro real ou é apenas uma conveniência secundária dispensável?
- [ ] **Teste dos 5 Segundos**: Ao abrir a tela inicial ou dashboard, um usuário de primeira viagem entende o propósito e a principal ação em menos de 5 segundos?
- [ ] **Desobstrução do Core Loop**: O fluxo principal está livre de etapas burocráticas preliminares?
- [ ] **Eliminação de Feature Bloat**: Não há elementos periféricos poluindo a interface e distraindo do objetivo central da tela.

---

## 2. Onboarding, Ativação & "Aha! Moment"
- [ ] **Time to Value (TTV) < 2 Minutos**: O usuário experimenta o benefício real do produto em menos de 2 minutos após o acesso?
- [ ] **Fricção Zero no Primeiro Uso**: O produto oferece templates pré-carregados, dados de exemplo (*sample data*) ou assistente de primeiro uso para evitar a angústia da tela em branco?
- [ ] **Checklist de Progresso Visual**: Há indicação visual de avanço para orientar os primeiros passos essenciais.

---

## 3. Blindagem de UX, Os 4 Estados & Erradicação de Vibe-Coding
- [ ] **Erradicação de Becos sem Saída (Dead Ends)**: Toda tarefa concluída direciona explicitamente para a **Next Best Action** (Ação Recomendada Seguinte). Proibido deixar a tela estática ou o usuário sem orientação.
- [ ] **A Matriz dos 4 Estados Completa**:
  - [ ] **Ideal**: Layout completo, legível, com dados formatados e hierarquia clara.
  - [ ] **Loading**: Skeletons que espelham o formato final e bloqueio de cliques repetitivos.
  - [ ] **Empty**: Ilustração sutil + texto explicativo + botão de ação primário para criar o primeiro registro.
  - [ ] **Error**: Alerta explicativo em linguagem humana + botão de tentar novamente (sem recarregar a aplicação).
- [ ] **Sem Botões ou Menus "Zumbis"**: Todo botão na interface possui comportamento completo implementado.
- [ ] **Prevenção de Perda de Dados**: Ao falhar uma requisição ou encontrar erro de validação, os dados digitados pelo usuário no formulário são rigorosamente preservados.

---

## 4. Retenção, Hábitos & Efeito de Rede
- [ ] **Gatilhos Externos Acionáveis**: Há notificações por e-mail, push ou mensagens com links diretos para a ação necessária.
- [ ] **Histórico Cumulativo de Valor (Moat de Dados)**: O uso contínuo acumula registros, métricas ou dados que tornam natural e vantajoso continuar na plataforma.
- [ ] **Loops de Colaboração**: Ações geram histórico auditável e facilitam o trabalho entre múltiplos membros de um time.

---

## 5. Monetização, Precificação & Gatilhos de Paywall
- [ ] **Alinhamento da Métrica de Valor**: A cobrança é atrelada ao que gera valor mensurável (volume de processamento, receita gerada, contatos ativos, assentos).
- [ ] **Paywall Contextual no Momento de Desejo**: O paywall surge quando o usuário tenta executar uma ação de alto valor após constatar a utilidade da ferramenta, e não como uma barreira cega no início.
- [ ] **Transparência de Consumo**: O usuário consegue visualizar sua cota atual e quanto ainda possui antes de esgotar o limite.
- [ ] **Upgrade sem Fricção**: Fluxo de contratação direto com 1 clique para checkout.

---

## 6. Automação Inteligente & Confiabilidade Técnica
- [ ] **Automação de Fundo (Background Value)**: Tarefas repetitivas são resolvidas automaticamente nos bastidores sem exigir esforço manual do usuário.
- [ ] **Resiliência a Falhas de Serviços**: Se uma API externa falhar ou demorar, a interface exibe feedback elegante de espera e permite retentativa manual.
- [ ] **Auditoria de Custos por Ação**: O consumo computacional ou de APIs é justificado pelo valor gerado e coberto pelo modelo de precificação.

---

## 7. Critérios de Aceite para Product Sign-off (Auditoria Prévia de PO)
O PO **só emite seu Sign-off** se a implementação passar nestes 5 testes rápidos:
1. *O fluxo pode ser concluído de ponta a ponta sem consultar documentação externa?*
2. *Se a internet for desligada durante a ação, a tela reage com dignidade sem quebrar ou sumir?*
3. *Se a lista estiver com 0 itens, o componente de empty state convida a criar o primeiro?*
4. *O usuário é sempre conduzido à próxima etapa após terminar uma ação?*
5. *O produto parece construído por um time experiente e confiável?*
