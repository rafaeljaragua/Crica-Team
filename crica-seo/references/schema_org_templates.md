# Modelos Prontos: Schema.org (JSON-LD), llms.txt & robots.txt

Este documento reúne templates de dados estruturados prontos para inserção em aplicações web, otimizados para motores de busca e modelos de linguagem generativos.

---

## 💻 1. Schema para SaaS / Aplicação Web (`SoftwareApplication`)

Insira dentro da tag `<head>` no layout principal da landing page ou página de planos:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "NomeDoProduto",
  "operatingSystem": "Web, Cloud",
  "applicationCategory": "BusinessApplication",
  "description": "Descrição objetiva de até duas frases sobre o mecanismo e resultado do software.",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD",
    "availability": "https://schema.org/OnlineOnly"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.9",
    "reviewCount": "128"
  }
}
</script>
```

---

## 🏢 2. Schema para Organização / Empresa (`Organization`)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Nome da Empresa",
  "url": "https://seudominio.com",
  "logo": "https://seudominio.com/logo.png",
  "sameAs": [
    "https://github.com/sua-empresa",
    "https://linkedin.com/company/sua-empresa",
    "https://twitter.com/sua-empresa"
  ]
}
</script>
```

---

## ❓ 3. Schema para Seção de Dúvidas Frequentes (`FAQPage`)

Útil para alimentar Knowledge Graphs e ser citado por Perplexity, ChatGPT Search e AI Overviews. 

> ⚠️ Desde agosto de 2023 o Google exibe rich results de FAQ **apenas para sites governamentais e de saúde reconhecidos**. Em SaaS e landing pages, o schema continua válido e útil para GEO, mas não gera snippet expandido na SERP.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "Como o produto se integra à infraestrutura existente?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "A integração ocorre via webhook ou cliente SDK nativo em Node.js e Python, com tempo médio de setup inferior a 5 minutos."
      }
    },
    {
      "@type": "Question",
      "name": "Os dados armazenados atendem aos requisitos de conformidade da LGPD e GDPR?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sim. Todos os dados são criptografados em repouso com AES-256 e em trânsito com TLS 1.3, com servidores alocados em regiões isoladas."
      }
    }
  ]
}
</script>
```

---

## 🤖 4. Modelo de Arquivo `llms.txt`

Salve na raiz pública do seu projeto (`public/llms.txt` no Next.js/Vite ou raiz do servidor):

```markdown
# NomeDoProduto

> Plataforma de infraestrutura de dados em tempo real para aplicações críticas.

## Visão Geral
NomeDoProduto é uma solução que sincroniza bancos de dados distribuídos com latência inferior a 20ms, eliminando gargalos de leitura em alta concorrência.

## Casos de Uso Principais
- Sincronização multi-região para bases PostgreSQL.
- Camada de cache inteligente sem complexidade de Redis.
- Auditoria de dados com réplicas imutáveis.

## Documentação Técnica
- [Guia Rápido de Instalação](https://seudominio.com/docs/quickstart): Passos via CLI e Docker.
- [Referência de API](https://seudominio.com/docs/api): Endpoints REST e GraphQL disponíveis.
- [Preços e Limites](https://seudominio.com/pricing): Detalhamento de cotas dos planos Free, Pro e Enterprise.
```

---

## 🛡️ 5. Modelo de `robots.txt` Amigável a Motores de IA

Configuração que permite indexadores legítimos e motores de busca com IA, mas protege rotas privadas e autenticadas:

```txt
User-agent: *
Allow: /
Disallow: /admin
Disallow: /api/private/
Disallow: /dashboard/
Disallow: /settings/

# Permitir crawlers de motores generativos para indexação pública
User-agent: GPTBot
Allow: /
Disallow: /admin
Disallow: /dashboard

User-agent: PerplexityBot
Allow: /
Disallow: /admin
Disallow: /dashboard

User-agent: ClaudeBot
Allow: /
Disallow: /admin
Disallow: /dashboard

Sitemap: https://seudominio.com/sitemap.xml
```
