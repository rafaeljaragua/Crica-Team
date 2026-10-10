# Checklist de Prontidão para Deploy & Governança de Ambiente

Use este checklist antes de promover qualquer aplicação do ambiente local para Staging ou Produção.

---

## 🔐 1. Governança de Variáveis de Ambiente (.env)

- [ ] **Arquivo `.env.example` Completo**: Todas as variáveis necessárias para subir o projeto localmente estão documentadas sem valores reais.
- [ ] **Separação de Chaves Públicas e Privadas**:
  - Chaves expostas no frontend usam prefixo explícito (`NEXT_PUBLIC_` ou `VITE_`).
  - Secrets do backend (chaves de serviço Supabase, tokens Stripe, senhas de banco) nunca têm prefixo público e nunca são importadas em componentes do cliente.
- [ ] **Fail-Fast no Startup**: A aplicação valida as variáveis essenciais na inicialização e encerra imediatamente com erro explicativo se alguma estiver faltando.
- [ ] **Repositório Limpo**: O arquivo `.gitignore` inclui categoricamente `.env`, `.env.local`, `.env.production` e `.env*.local`.

---

## 🛠️ 2. Validação Pré-Deploy

- [ ] **Typecheck Estrito**: O comando `tsc --noEmit` executa sem nenhum erro de tipo.
- [ ] **Linter Limpo**: `npm run lint` ou equivalente passa com zero advertências bloqueantes.
- [ ] **Build de Produção**: O comando `npm run build` completa localmente sem avisos de código não encontrado ou falhas de empacotamento.
- [ ] **Scripts de Health Check**: Existe um endpoint simples (`/api/health` ou `/api/ping`) retornando status 200 para monitoramento externo de uptime.

---

## 📊 3. Observabilidade e Monitoramento de Erros

- [ ] **Captura de Exceções**: Sentry (ou ferramenta equivalente) configurado para registrar erros não tratados com stacktrace completo.
- [ ] **Mascaramento de PII**: Dados pessoais de clientes (e-mails, números de cartão, senhas) são anonimizados antes do envio para a plataforma de logs.
- [ ] **Rastreamento de Versão**: Cada erro reportado traz a tag com a versão do software ou o hash do commit git (`release: process.env.VERCEL_GIT_COMMIT_SHA`).
