# Checklist Oficial: Definition of Done (DoD) da Squad Crica

Nenhuma tarefa, branch ou entrega é considerada concluída (*Done*) sem o cumprimento rigoroso deste checklist pelo `crica-master`.

**Status possíveis por portão**: ✅ Atendido · ⛔ Bloqueado · N/A (skill não acionada — exige justificativa de 1 linha).
Um único portão ⛔ impede a declaração de *Done*.

---

## 🎯 Os 10 Portões de Aceitação da Squad

- [ ] **1. Produto & Negócio (`crica-po`)**: Os critérios de aceitação BDD/Gherkin definidos na história foram atendidos na totalidade.
- [ ] **2. Comunicação & Tom (`crica-copy`)**: Textos revisados, sem clichês de IA (*AI slop*), sem emojis espalhados e com mensagens de erro claras e orientadas à ação.
- [ ] **3. Estética & Acessibilidade Visual (`crica-ui`)**: Tokens do design system respeitados, hierarquia tipográfica, micro-interações fluidas, contraste WCAG AA e responsividade validada.
- [ ] **4. Arquitetura & Tipagem (`crica-arch`)**: Zero tipos `any` adicionados, separação estrita de camadas (UI desacoplada de dados) e integridade relacional com chaves estrangeiras.
- [ ] **5. Desempenho (`crica-fast`)**: Sem consultas N+1 introduzidas, listagens paginadas, componentes pesados virtualizados ou memoizados, e bundle de frontend sob controle.
- [ ] **6. Segurança (`crica-seg`)**: RLS ativo nas novas tabelas, autenticação validada em Server Actions/rotas de API, e nenhum segredo exposto em código client-side.
- [ ] **7. Confiabilidade (`crica-tester`)**: 100% dos novos botões clicados e validados, formulários com feedback de loading/erro, navegação por teclado e jornada E2E executada com sucesso.
- [ ] **8. Descoberta & Metadados (`crica-seo`)**: Novas páginas públicas com título e descrição únicos, canonical explícito e dados estruturados JSON-LD quando aplicável.
- [ ] **9. Infraestrutura & Build (`crica-ops`)**: Typecheck (`tsc --noEmit`), linter e build de produção executados com sucesso no ambiente local/CI; `.env.example` sincronizado.
- [ ] **10. Documentação (`crica-docs`)**: Endpoints novos documentados em OpenAPI/Swagger, README coerente com o setup atual e `CHANGELOG.md` atualizado com a versão.

---

## 👤 Responsáveis por Temas Compartilhados

Alguns temas são verificados por mais de uma skill. Para evitar trabalho duplicado (ou lacunas), cada tema tem **um dono** que assina o portão e **colaboradores** que apenas reportam achados ao dono.

| Tema | Dono (assina o portão) | Colaboradores | Divisão |
| :--- | :--- | :--- | :--- |
| Acessibilidade | `crica-ui` (visual) / `crica-tester` (operacional) | `crica-seo` | ui: contraste, foco visível, alvos de toque · tester: teclado, ARIA, leitores · seo: `alt` e semântica HTML |
| Segredos & `.env` | `crica-seg` | `crica-ops` | seg: segredos no código/Git/client · ops: `.env.example` sincronizado e validação no startup |
| Overfetching de dados | `crica-fast` (peso do payload) | `crica-seg` (vazamento de PII) | Mesmo achado: fast corrige a projeção; seg confirma que nenhum campo sensível sai |
| Core Web Vitals | `crica-fast` | `crica-seo`, `crica-ui` | seo detecta em rotas públicas; ui evita animações que causem CLS/INP |
| LGPD / Consentimento | `crica-seg` | `crica-seo` | seg define a política e a base legal; seo implementa Consent Mode nos scripts de analytics |
| Migrations de banco | `crica-arch` | `crica-ops`, `crica-fast` | arch modela; fast define índices; ops garante *expand and contract* e rollback |
