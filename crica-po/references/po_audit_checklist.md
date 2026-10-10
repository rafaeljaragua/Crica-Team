# 📋 Checklist Exaustivo de Auditoria de Produto (CRICA-PO)

Este documento contém o roteiro de verificação 360° para diagnosticar a maturidade, usabilidade, estratégia de monetização e potencial de escala de produtos digitais.

---

## 1. Proposta de Valor & Job-To-Be-Done (JTBD)
- [ ] **Dor Aguda vs. "Vitamina"**: O produto resolve um problema que custa tempo/dinheiro (dor real) ou é apenas uma conveniência secundária?
- [ ] **Clareza de Proposta (5-Second Test)**: Ao abrir a página inicial / dashboard, um novo visitante entende exatamente o que o produto faz em menos de 5 segundos?
- [ ] **Foco no Core Loop**: O fluxo principal do produto está desobstruído ou exige passos burocráticos desnecessários antes de entregar o primeiro resultado?
- [ ] **Feature Bloat (Excesso de Funcionalidades)**: Existem módulos ou botões secundários competindo pela atenção do usuário e diluindo o valor principal?

---

## 2. Onboarding, Ativação & "Aha! Moment"
- [ ] **Tempo até o Valor (Time to Value - TTV)**: O usuário consegue experimentar o primeiro benefício real (*Aha! Moment*) em menos de 2 minutos após o cadastro?
- [ ] **Fricção de Cadastro**: O formulário pede apenas as informações estritamente necessárias ou exige dados antecipados que poderiam ser coletados depois?
- [ ] **Experiência de Primeiro Uso (FTUX)**: A aplicação oferece dados de exemplo (*sample data*), templates prontos ou um tour contextual para evitar a tela em branco?
- [ ] **Checklist de Ativação**: Existe um guia visual de progresso (ex: "Complete 3 passos para ativar seu assistente") que orienta o usuário no primeiro acesso?

---

## 3. Detecção de Armadilhas de Vibe-Coding & UX Flaws
- [ ] **Becos sem Saída (Dead Ends)**: Após concluir uma tarefa (ex: criar um item, salvar um registro), o sistema apresenta claramente o próximo passo (*Next Best Action*) ou a tela fica estática?
- [ ] **Zombie Features**: Há elementos de interface, botões ou abas que prometem ações mas não têm lógica completa, gerando frustração?
- [ ] **Empty States Educativos**: Quando uma lista ou tabela está vazia, o sistema explica o que deveria estar ali e oferece um botão direto para criar/importar?
- [ ] **Loading & Feedback de Ação**: Operações assíncronas mostram feedback visual imediato (spinners, skeletons, barras de progresso ou toasts de confirmação)?
- [ ] **Tratamento Humanizado de Erros**: As mensagens de erro explicam *o que aconteceu* e *o que o usuário deve fazer para corrigir*, evitando códigos técnicos crus.

---

## 4. Mecanismos de Retenção & Engajamento
- [ ] **Gatilhos Externos (External Triggers)**: O produto envia notificações por e-mail/WhatsApp inteligentes, resumos semanais ou alertas de eventos importantes para trazer o usuário de volta?
- [ ] **Gatilhos Internos (Habit Loops)**: O uso do produto gera um histórico acumulado de valor (dados, relatórios, configurações) que torna custoso para o usuário abandonar a ferramenta (*lock-in positivo*)?
- [ ] **Loops de Notificação de Equipe**: Ações de um membro da equipe geram notificações relevantes para outros membros, estimulando a colaboração?

---

## 5. Estratégia de Monetização, Pricing & Paywall
- [ ] **Alinhamento da Métrica de Valor**: A cobrança é baseada naquilo que o usuário realmente valoriza (ex: créditos processados, faturamento gerado, contatos alcançados, assentos ativos)?
- [ ] **Posicionamento do Paywall**: O pedido de upgrade ocorre no momento de maior desejo/necessidade do usuário (ex: ao atingir o limite de um recurso de alto valor) ou bloqueia prematuramente a descoberta?
- [ ] **Transparência de Limites**: O usuário consegue visualizar facilmente quanto de sua cota gratuita/paga já foi consumida antes de ser bloqueado?
- [ ] **Caminho de Upgrade em 1 Clique**: O processo de upgrade para um plano superior é fluido, com checkout direto e sem atritos desnecessários?
- [ ] **Oportunidades de Add-ons / Microtransações**: Existem recursos opcionais de alto valor que podem ser vendidos à la carte (ex: pacotes de créditos extras, relatórios avançados, suporte prioritário)?

---

## 6. Produto com IA Nativa & FinOps
- [ ] **Automação Invisível vs. Chatbot**: A IA atua de forma proativa preenchendo dados, gerando insights ou categorizando informações nos bastidores, em vez de exigir prompts manuais exaustivos?
- [ ] **Relação Custo de Token vs. Valor Percebido**: O recurso de IA gera um ganho palpável que justifica o custo de inferência na margem de lucro do plano?
- [ ] **Fallback e Confiabilidade**: O que acontece quando a IA falha ou demora para responder? Há mecanismo de retry, timeout elegante e possibilidade de edição manual pelo usuário?

---

## 7. Product-Led Growth (PLG) & Viralidade
- [ ] **Loops de Compartilhamento Nativo**: Os artefatos criados pelo usuário (relatórios, links, dashboards, gráficos) podem ser compartilhados publicamente com marca d'água ou convite para teste?
- [ ] **Convites de Equipe (Multi-tenancy)**: É fácil convidar colegas de trabalho com 1 clique (e-mail ou link de convite rápido)?
- [ ] **Self-Service Onboarding**: O cliente consegue testar, comprar e começar a usar sem depender de contato manual com vendas (venda 100% autônoma)?
