# Checklist Detalhado de Auditoria (Vibe Code)

Use este documento como referência rápida durante as inspeções de código.

---

## 1. Segredos & Repositório
- [ ] Chaves de API privadas (OpenAI, Stripe secret, AWS/Cloudflare, Supabase `service_role`) não estão em código cliente nem no Git.
- [ ] O arquivo `.env` está no `.gitignore`. Existe um `.env.example` sem valores reais.
- [ ] Não há chaves ou tokens sensíveis em logs ou `console.log`.

---

## 2. Banco de Dados & RLS
- [ ] RLS (Row Level Security) está habilitado em **todas** as tabelas acessíveis por chaves públicas/anon.
- [ ] Políticas de RLS não usam regras cegas como `using (true)` para `UPDATE` ou `DELETE`, e `INSERT`/`UPDATE` possuem `WITH CHECK`.
- [ ] Policies não confiam em `raw_user_meta_data` / `user_metadata` (editável pelo próprio usuário) para decisões de autorização; usar `app_metadata` ou tabela de papéis.
- [ ] Views no schema exposto usam `WITH (security_invoker = true)` — por padrão, views executam com os privilégios do dono e **ignoram RLS**.
- [ ] Funções `SECURITY DEFINER` chamáveis via RPC validam `auth.uid()` e definem `SET search_path = ''`.
- [ ] Buckets do Storage são privados por padrão, com policies em `storage.objects` por dono/pasta.
- [ ] Supabase: `get_advisors` (type `security`) executado sem alertas críticos pendentes.
- [ ] Consultas usam parâmetros / ORM seguro (sem concatenação de strings em queries SQL).
- [ ] Senhas nunca são salvas em texto puro (usam hash bcrypt/argon2).

---

## 3. Autenticação & Autorização
- [ ] Rotas e ações sensíveis validam autenticação no **servidor/backend**, nunca confiando apenas em validação visual de frontend.
- [ ] Permissões de registro (RBAC / Ownership): o backend verifica se o `user_id` autenticado é dono do registro que está sendo alterado.
- [ ] Cookies de sessão usam flags `HttpOnly`, `Secure` e `SameSite`.
- [ ] **CSRF**: mutações autenticadas por cookie em route handlers/APIs customizadas verificam `Origin` ou token CSRF.
- [ ] **Open Redirect**: parâmetros `next`, `redirect_to`, `callbackUrl` aceitam apenas caminhos relativos ou domínios em allowlist.
- [ ] **Rate Limiting em Autenticação**: Endpoints de login, registro, recuperação de senha e verificação de OTP possuem limites rígidos contra força bruta e enumeração.

---

## 4. Integridade & Validação de Entrada
- [ ] Todas as mutações e endpoints validam payloads com schema (ex: Zod).
- [ ] Proteção contra adulteração de campos (*mass-assignment*): campos como `role`, `is_admin`, `price`, `user_id` são controlados estritamente pelo servidor.
- [ ] Sanitização contra XSS em conteúdos ricos enviados por usuários.
- [ ] **SSRF**: funcionalidades que fazem fetch de URLs fornecidas pelo usuário bloqueiam IPs privados, loopback e endpoints de metadata de nuvem (`169.254.169.254`).
- [ ] Uploads de arquivos:
  - Se o projeto não usa uploads, o endpoint deve estar desabilitado.
  - Se usa (ex: avatar), valida extensão, tamanho máximo e tipo MIME real, salvando em storage isolado com URLs assinadas quando privado.

---

## 5. Rede, APIs, Rate Limiting & Resiliência
- [ ] As respostas de API retornam apenas campos necessários (evita retornar colunas desnecessárias ou dados pessoais de outros usuários).
- [ ] Webhooks de terceiros (Stripe, Clerk, etc.) validam assinatura criptográfica antes de processar.
- [ ] **FinOps Rate Limiting**: Endpoints de APIs pagas (LLMs como OpenAI/Claude, e-mails como Resend, SMS como Twilio) possuem rate limit e teto de custo por usuário/IP para evitar exaustão financeira.
- [ ] **Proteção de Formulários Públicos & Anti-Spam**: Endpoints abertos (contato, geração de leads, feedback) possuem rate limiting por IP ou desafio de bot (Cloudflare Turnstile/hCaptcha).
- [ ] **Proteção contra DoS em Tarefas Pesadas**: Operações computacionalmente intensas (geração de PDF, redimensionamento de imagens, exportação em lote) contam com estrangulamento (throttling) e limite de concorrência.
- [ ] **Persistência de Rate Limit em Serverless**: Rate limiters em ambientes Serverless/Edge utilizam armazenamento compartilhado/persistente (ex: Upstash Redis) em vez de contadores locais em memória volátil.
- [ ] Cabeçalhos de segurança básicos (CSP, X-Frame-Options, HSTS) e forçar HTTPS.
- [ ] Dependências verificadas sem vulnerabilidades críticas (`npm audit`).

---

## 6. Privacidade & Conformidade (LGPD / GDPR)
- [ ] Inventário de dados pessoais coletados, local de armazenamento e base legal de cada um.
- [ ] Scripts não essenciais (analytics, pixels, gravação de sessão) só carregam após consentimento (Consent Mode v2 via `crica-seo`).
- [ ] Usuário consegue exportar e excluir seus dados; exclusão de conta remove/anonimiza registros e arquivos de storage.
- [ ] PII mascarada em logs, Sentry e gravações de sessão.
- [ ] Política de privacidade publicada e coerente com os dados realmente coletados.
