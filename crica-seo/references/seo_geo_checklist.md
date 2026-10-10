# Checklist Completo de Auditoria: SEO Técnico & GEO (Generative Engine Optimization)

Use este checklist para validar se uma aplicação web, SaaS ou landing page está pronta para ser rastreada, compreendida e indexada por humanos, crawlers tradicionais (Google, Bing) e motores de resposta com IA (Perplexity, ChatGPT Search, Gemini).

---

## 🔍 1. SEO Técnico & Estrutura de Código

### Metadados Essenciais no `<head>`
- [ ] **Title Tag**: Presente, único por página, entre 50 e 60 caracteres.
- [ ] **Meta Description**: Presente, informativa, entre 140 e 160 caracteres. Sem repetição excessiva de palavras-chave.
- [ ] **Tag Canonical**: Definida explicitamente apontando para a URL canônica preferencial (`<link rel="canonical" href="https://dominio.com/pagina" />`).
- [ ] **Viewport & Charset**: `<meta name="viewport" content="width=device-width, initial-scale=1.0" />` e `<meta charset="UTF-8" />`.
- [ ] **Favicon & Web App Manifest**: Favicon em múltiplos tamanhos (SVG, 32x32, 192x192, 512x512) e `manifest.json` válido.

### Semântica HTML5
- [ ] **H1 Único**: Apenas uma tag `<h1>` por página, contendo o núcleo da proposta de valor.
- [ ] **Hierarquia de Títulos**: Não há saltos de níveis (ex: `<h1>` direto para `<h3>`).
- [ ] **Elementos Semânticos**: Uso correto de `<header>`, `<main>`, `<nav>`, `<article>`, `<section>`, `<aside>` e `<footer>`.
- [ ] **Acessibilidade e Imagens**: Todas as imagens possuem atributo `alt` preenchido contextualmente.
- [ ] **Prevenção de CLS**: Imagens possuem atributos `width` e `height` definidos ou containers com aspect-ratio.

### Rastreabilidade e Infraestrutura
- [ ] **robots.txt**: Acessível na raiz (`/robots.txt`), referenciando o `sitemap.xml` e sem bloquear assets críticos (CSS/JS).
- [ ] **sitemap.xml**: Válido, atualizado dinamicamente, sem URLs redirecionadas (301) ou quebradas (404).
- [ ] **Controle de Noindex**: Rotas de autenticação, painéis administrativos (`/admin`, `/app`) e ambientes de staging com `<meta name="robots" content="noindex, nofollow" />`.
- [ ] **Página 404 Customizada**: Retorna status HTTP 404 real (não soft 404 / 200 OK) e oferece links de retorno para a navegação principal.

---

## 🤖 2. GEO (Generative Engine Optimization) & IA Readiness

### Dados Estruturados (Schema.org / JSON-LD)
- [ ] **Validação Sintática**: Bloco `<script type="application/ld+json">` sem erros de parsing JSON.
- [ ] **SoftwareApplication / WebApplication**: Configurado para SaaS com tipo, preço (`offers`), categoria e sistema operacional suportado.
- [ ] **Organization**: Identifica claramente nome da marca, URL oficial, logotipo e redes sociais vinculadas (`sameAs`).
- [ ] **FAQPage**: Perguntas e respostas objetivas formatadas como schema para citação por LLMs e Knowledge Graphs. ⚠️ Desde ago/2023 o Google só exibe rich result de FAQ para sites governamentais e de saúde reconhecidos — não prometa rich snippet a SaaS/landing pages.

### Otimização para Agentes & Crawlers de IA
- [ ] **Arquivo llms.txt**: Presente em `/llms.txt`, documentando a proposta do produto, endpoints públicos e visão geral em Markdown limpo.
- [ ] **Permissão de AI Bots**: `robots.txt` permite explicitamente o rastreamento das páginas públicas por bots como `GPTBot`, `PerplexityBot` e `ClaudeBot`.
- [ ] **Respostas Autocontidas**: Definições diretas de conceitos no texto (ex: *"O que é X? X é..."*), permitindo que mecanismos de busca baseados em RAG extraiam o trecho como citação com facilidade.
- [ ] **Dados Primários e Métricas**: Inclusão de dados objetivos e estatísticas proprietárias (aumenta a taxa de referência por IAs que buscam evidências).

---

## 📱 3. Social Graph & Compartilhamento

- [ ] **OpenGraph Básico**: `og:title`, `og:description`, `og:url`, `og:type` (`website` ou `article`).
- [ ] **OpenGraph Image**: Imagem com proporção 1.91:1 (ideal: 1200x630 pixels) e tamanho inferior a 1 MB.
- [ ] **Twitter Card**: `<meta name="twitter:card" content="summary_large_image" />` com `twitter:title`, `twitter:description` e `twitter:image`.
- [ ] **Previsão em Mensageiros**: Testado e funcionando no WhatsApp, Telegram e LinkedIn sem links ou imagens quebradas.

---

## 📊 4. Web Analytics & Verificação em Motores de Busca

### Google Search Console & Bing Webmaster
- [ ] **Verificação do Search Console**: Meta tag `google-site-verification` ou registro DNS ativo e validado.
- [ ] **Verificação do Bing Webmaster**: Meta tag `msvalidate.01` ativa (essencial para indexação no ecossistema OpenAI/ChatGPT Search).
- [ ] **Envio de Sitemap**: Sitemap oficial (`/sitemap.xml`) submetido e sem erros de processamento em ambas as plataformas.

### Telemetria e Comportamento (GA4, GTM & Microsoft Clarity)
- [ ] **Injeção Assíncrona**: Scripts de GA4 ou GTM carregados sem bloquear renderização (estratégia `afterInteractive` ou `async`).
- [ ] **Microsoft Clarity**: Instalado com modo de privacidade rígido (mascaramento de senhas e inputs sensíveis ativado).
- [ ] **Validação de Performance**: O carregamento dos scripts de telemetria não degradou o Core Web Vitals (TBT e LCP validados com `crica-fast`).

