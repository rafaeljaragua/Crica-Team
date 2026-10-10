# Guia de Configuração: Search Console, Bing, GA4, GTM & Microsoft Clarity

Este guia reúne snippets prontos de alta performance para conectar sua aplicação aos mecanismos de busca e ferramentas de telemetria comportamental sem degradar os Core Web Vitals (LCP, INP, TBT).

---

## 🔍 1. Verificação de Propriedade (Google Search Console & Bing)

Insira as meta tags de verificação dentro do `<head>` do layout principal.

### Next.js (App Router - `app/layout.tsx` via Metadata API):
```typescript
import type { Metadata } from 'next';

export const metadata: Metadata = {
  verification: {
    google: 'SEU_CODIGO_DE_VERIFICACAO_GOOGLE',
    other: {
      'msvalidate.01': 'SEU_CODIGO_DE_VERIFICACAO_BING',
    },
  },
};
```

### HTML Estático / Vite:
```html
<meta name="google-site-verification" content="SEU_CODIGO_DE_VERIFICACAO_GOOGLE" />
<meta name="msvalidate.01" content="SEU_CODIGO_DE_VERIFICACAO_BING" />
```

---

## 🛡️ 2. Consent Mode v2 (LGPD / GDPR) — antes de qualquer tag Google

Obrigatório para tráfego do EEE desde março/2024 e recomendado para LGPD. O estado padrão **negado** deve ser definido **antes** do GA4/GTM carregar; o banner de cookies atualiza o estado após a escolha do usuário. A política e a base legal são definidas com o `crica-seg`.

```tsx
import Script from 'next/script';

export function ConsentDefaults() {
  return (
    <Script
      id="consent-defaults"
      strategy="beforeInteractive"
      dangerouslySetInnerHTML={{
        __html: `
          window.dataLayer = window.dataLayer || [];
          function gtag(){dataLayer.push(arguments);}
          gtag('consent', 'default', {
            ad_storage: 'denied',
            ad_user_data: 'denied',
            ad_personalization: 'denied',
            analytics_storage: 'denied',
            wait_for_update: 500
          });
        `,
      }}
    />
  );
}

// No banner, após o aceite:
// gtag('consent', 'update', { analytics_storage: 'granted', ad_storage: 'granted', ad_user_data: 'granted', ad_personalization: 'granted' });
```

- Microsoft Clarity: só inicialize após consentimento de analytics, ou use a API de consentimento do Clarity (`window.clarity('consentv2', {...})`).
- Persistir a escolha do usuário e reaplicar o `update` em cada carregamento.

---

## 📊 3. Google Analytics 4 (GA4) de Alta Performance

Use a estratégia `afterInteractive` no Next.js para garantir que o script não bloqueie o First Contentful Paint (FCP) nem o LCP:

### Next.js (App Router):
```tsx
import Script from 'next/script';

export function GoogleAnalytics({ gaId }: { gaId: string }) {
  if (!gaId) return null;

  return (
    <>
      <Script
        strategy="afterInteractive"
        src={`https://www.googletagmanager.com/gtag/js?id=${gaId}`}
      />
      <Script
        id="google-analytics-init"
        strategy="afterInteractive"
        dangerouslySetInnerHTML={{
          __html: `
            window.dataLayer = window.dataLayer || [];
            function gtag(){dataLayer.push(arguments);}
            gtag('js', new Date());
            gtag('config', '${gaId}', {
              page_path: window.location.pathname,
            });
          `,
        }}
      />
    </>
  );
}
```

---

## 🏷️ 4. Google Tag Manager (GTM)

Se optar pelo GTM para gerenciar múltiplos pixels em um único container:

### Next.js (App Router):
```tsx
import Script from 'next/script';

export function GoogleTagManager({ gtmId }: { gtmId: string }) {
  if (!gtmId) return null;

  return (
    <>
      <Script
        id="gtm-script"
        strategy="afterInteractive"
        dangerouslySetInnerHTML={{
          __html: `
            (function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
            new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
            j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
            'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
            })(window,document,'script','dataLayer','${gtmId}');
          `,
        }}
      />
      <noscript>
        <iframe
          src={`https://www.googletagmanager.com/ns.html?id=${gtmId}`}
          height="0"
          width="0"
          style={{ display: 'none', visibility: 'hidden' }}
        />
      </noscript>
    </>
  );
}
```

---

## 👁️ 5. Microsoft Clarity (Heatmaps & Rage Clicks)

O Microsoft Clarity grava sessões e gera mapas de calor sem custo. Para proteger a privacidade dos usuários e atender à LGPD, o mascaramento de texto sensível deve ser mantido no padrão rígido.

### Next.js (App Router):
```tsx
import Script from 'next/script';

export function MicrosoftClarity({ clarityId }: { clarityId: string }) {
  if (!clarityId) return null;

  return (
    <Script
      id="microsoft-clarity-init"
      strategy="afterInteractive"
      dangerouslySetInnerHTML={{
        __html: `
          (function(c,l,a,r,i,t,y){
              c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
              t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/"+i;
              y=l.getElementsByTagName(r)[0];y.parentNode.insertBefore(t,y);
          })(window, document, "clarity", "script", "${clarityId}");
        `,
      }}
    />
  );
}
```

---

## ⚡ 6. Regras de Ouro de Performance & Privacidade

1. **Nunca use scripts síncronos**: Scripts colocados diretamente em `<script src="...">` sem `async`, `defer` ou `strategy="afterInteractive"` congelam a renderização da tela.
2. **Consentimento primeiro**: os defaults do Consent Mode v2 são o único script `beforeInteractive`; nenhuma tag de analytics/ads grava cookies antes do aceite.
3. **Mascaramento de Dados no Clarity**: Garanta que campos com senhas, dados de cartão e documentos recebam a classe `clarity-mask` ou mantenha a configuração de privacidade "Strict" ativa no painel do Clarity.
4. **Variáveis de Ambiente**: Sempre armazene os IDs em variáveis públicas no `.env.example`:
   ```env
   NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX
   NEXT_PUBLIC_GTM_ID=GTM-XXXXXXX
   NEXT_PUBLIC_CLARITY_ID=XXXXXXXXXX
   ```
