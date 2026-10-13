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

---

## 7. ⚖️ Os 11 Riscos Jurídicos Globais & Defesa do Consumidor (Brasil / EUA / UE)

Estes 11 itens são os maiores alvos de processos judiciais, ações civis públicas (*class actions*) e sanções da ANPD, Senacon, Procons, FTC, lojas de aplicativos e autoridades europeias de proteção de dados:

- [ ] **1. Verificação de Idade no Cadastro (ECA Digital, LGPD Art. 14, COPPA, GDPR Art. 8)**:
  - Se o produto tiver acesso por menores ou conteúdo restrito, a mera autodeclaração (*"Tenho 18 anos"*) é insuficiente.
  - Exigir data de nascimento verificável ou consentimento formal de responsáveis para menores de idade; produtos para crianças exigem fluxo COPPA estrito.
- [ ] **2. Fontes Auto-Hospedadas no Próprio Servidor (Eliminação de Google Fonts CDN)**:
  - Proibido carregar fontes via CDN externo (`fonts.googleapis.com`) em runtime para não transferir IP de visitantes para servidores dos EUA sem consentimento prévio (Precedente do Tribunal de Munique / GDPR / Schrems II).
  - Usar fontes locais empacotadas (`@font-face` com `.woff2` locais ou `next/font/local`).
- [ ] **3. Gravação de Sessão Mascarada & Sob Consentimento (CIPA, Hotjar, Clarity, FullStory)**:
  - Todo gravador de sessão deve rodar apenas após aceite expresso no banner de cookies.
  - Mascaramento rigoroso ativado em **todos** os campos de entrada (`mask-all-inputs`, `data-clarity-mask`), bloqueando gravação de senhas, cartões, CPF, nomes e dados sensíveis para evitar vazamento ativo de PII e processos de interceptação não autorizada (*wiretapping*).
- [ ] **4. E-mails Transacionais e de Marketing com Identificação e Unsubscribe (CAN-SPAM, Decreto 7.962/2013, CDC)**:
  - Todo e-mail enviado deve conter no rodapé: Razão Social, CNPJ (ou endereço físico postal válido) e link funcional de cancelamento de inscrição (*unsubscribe em 1 clique*).
- [ ] **5. Transparência Radical em Assinaturas e Renovação Automática (FTC Click-to-Cancel, CDC Art. 39/51)**:
  - Abaixo ou ao lado imediato do botão de assinar: destacar preço exato, periodicidade (mensal/anual), regra de renovação automática e facilidade de cancelamento.
  - Enviar e-mail de aviso prévio (7 a 15 dias antes) em renovações periódicas (especialmente anuais).
  - Cancelamento self-service descomplicado na interface da conta (proibido reter o usuário ou obrigar contato telefônico/chat com atendente).
- [ ] **6. Canal de Denúncia de Direitos Autorais & Notice-and-Takedown (DMCA Safe Harbor § 512, Marco Civil Art. 19, DSA)**:
  - Para plataformas que permitem upload ou publicação de conteúdo por usuários (UGC), manter página `/copyright` ou canal público para denúncias de infração de direitos autorais.
  - Possuir procedimento operacional para suspender/remover o conteúdo infrator mediante notificação, resguardando a isenção de responsabilidade da plataforma.
- [ ] **7. Acessibilidade Digital & Prevenção de Litígios (ADA Title III, European Accessibility Act - EAA, LBI Art. 63)**:
  - Alvo massivo de ações judiciais nos EUA e exigência legal na UE e Brasil.
  - Conformidade WCAG AA obrigatória: botões de ícone com `aria-label`, inputs associados a `<label>`, foco visível para teclado (`:focus-visible`), hierarquia semântica de headings e contraste mínimo de 4.5:1.
- [ ] **8. Simetria de Consentimento de Cookies sem Dark Patterns (GDPR, CNIL, ANPD)**:
  - Proibido esconder a recusa atrás de várias telas de preferências.
  - O banner inicial de cookies deve oferecer a opção de recusar cookies não essenciais com a mesma facilidade e destaque do aceite: botões `[Aceitar]` e `[Rejeitar Não Essenciais]` no primeiro nível.
- [ ] **9. Direito de Arrependimento e Reembolso em 7 Dias (CDC Art. 49) e 14 Dias na UE**:
  - No Brasil, o prazo de reflexão de 7 dias para compras online é direito irrenunciável. Proibida cláusula de "não fazemos reembolso de forma alguma" para o mercado brasileiro.
  - O sistema de billing e a política de termos de uso devem prever fluxo ágil de reembolso dentro do prazo legal.
- [ ] **10. Exclusão Efetiva de Conta e Dados Self-Service (Apple App Store, Google Play, LGPD Art. 18, GDPR Art. 17)**:
  - Obrigatório para aprovação em lojas de aplicativos e exigência de leis de privacidade.
  - Nas configurações de perfil/conta, deve existir botão explícito: *"Excluir minha conta e dados"*. Proibido obrigar o usuário a enviar e-mail para suporte.
  - A ação deve disparar a exclusão real (ou anonimização) no banco de dados e remoção de arquivos pessoais nos storages (S3/R2).
- [ ] **11. Transparência de IA & Governança de Dados com LLMs (EU AI Act)**:
  - Informar de forma transparente quando um conteúdo for gerado ou quando o usuário estiver interagindo com um assistente de IA.
  - Garantir nos Termos de Uso e DPA (Data Processing Addendum) que os dados privados do usuário não são compartilhados para re-treinamento de modelos públicos por provedores terceiros (OpenAI/Anthropic/Google).


