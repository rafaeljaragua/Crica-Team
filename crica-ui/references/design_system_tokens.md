# Tokens de Design System Semântico (CSS / Tailwind)

Referência oficial de tokens da `crica-ui`. Os nomes seguem a convenção semântica usada no `SKILL.md` (compatível com shadcn/ui e Tailwind v4 `@theme`), com **temas claro e escuro** e cores em **OKLCH** (perceptualmente uniforme, facilita calibrar contraste e variações).

> ⚠️ **Os valores de matiz são exemplos.** Derive `--primary` e `--accent` da identidade da marca. Nunca entregue o azul padrão de framework (`#3b82f6` / `blue-500`) como cor primária.

---

## 🎨 1. Cores Semânticas (Claro + Escuro)

Componentes **nunca** usam cores absolutas (`bg-blue-500`, `text-gray-900`, hex soltos) — apenas estes tokens.

```css
:root {
  /* Superfícies */
  --background: oklch(0.985 0.003 285);
  --foreground: oklch(0.21 0.02 285);
  --card: oklch(1 0 0);
  --card-foreground: oklch(0.21 0.02 285);
  --popover: oklch(1 0 0);
  --popover-foreground: oklch(0.21 0.02 285);

  /* Ação */
  --primary: oklch(0.52 0.17 285);            /* contraste ≥ 4.5:1 com --primary-foreground */
  --primary-foreground: oklch(0.985 0.005 285);
  --secondary: oklch(0.95 0.01 285);
  --secondary-foreground: oklch(0.27 0.03 285);
  --accent: oklch(0.94 0.03 200);
  --accent-foreground: oklch(0.30 0.06 200);

  /* Neutros de apoio */
  --muted: oklch(0.96 0.005 285);
  --muted-foreground: oklch(0.50 0.015 285);  /* contraste ≥ 4.5:1 com --background */
  --border: oklch(0.91 0.008 285);
  --input: oklch(0.91 0.008 285);
  --ring: oklch(0.52 0.17 285);

  /* Feedback */
  --destructive: oklch(0.55 0.21 25);
  --destructive-foreground: oklch(0.985 0 0);
  --success: oklch(0.55 0.14 155);
  --success-foreground: oklch(0.985 0 0);
  --warning: oklch(0.75 0.15 75);
  --warning-foreground: oklch(0.25 0.05 75);
}

.dark {
  --background: oklch(0.15 0.012 285);
  --foreground: oklch(0.96 0.005 285);
  --card: oklch(0.19 0.014 285);
  --card-foreground: oklch(0.96 0.005 285);
  --popover: oklch(0.21 0.015 285);
  --popover-foreground: oklch(0.96 0.005 285);

  --primary: oklch(0.72 0.14 285);
  --primary-foreground: oklch(0.17 0.03 285);
  --secondary: oklch(0.25 0.015 285);
  --secondary-foreground: oklch(0.94 0.005 285);
  --accent: oklch(0.30 0.05 200);
  --accent-foreground: oklch(0.92 0.04 200);

  --muted: oklch(0.24 0.012 285);
  --muted-foreground: oklch(0.70 0.015 285);
  --border: oklch(1 0 0 / 0.09);
  --input: oklch(1 0 0 / 0.12);
  --ring: oklch(0.72 0.14 285);

  --destructive: oklch(0.65 0.19 25);
  --destructive-foreground: oklch(0.15 0.02 25);
  --success: oklch(0.70 0.14 155);
  --success-foreground: oklch(0.15 0.03 155);
  --warning: oklch(0.80 0.14 75);
  --warning-foreground: oklch(0.20 0.04 75);
}
```

### Regras de calibração
- Todo par `--x` / `--x-foreground` deve passar **WCAG AA** (4.5:1 texto normal; 3:1 texto grande e componentes de UI).
- Variações de hover: ajuste apenas a **luminância** (L) em ±0.04–0.06, mantendo matiz e croma.
- No escuro, reduza o croma da primária (cores saturadas "vibram" em fundos escuros).

---

## 🔤 2. Tipografia

```css
:root {
  --font-sans: "Inter", "Geist", ui-sans-serif, system-ui, sans-serif;
  --font-mono: "Geist Mono", "JetBrains Mono", ui-monospace, monospace;

  --text-display: 3rem;     /* 48px — line-height 1.1, peso 700 */
  --text-h1: 2.25rem;       /* 36px — line-height 1.15, peso 600/700 */
  --text-h2: 1.75rem;       /* 28px — line-height 1.2, peso 600 */
  --text-h3: 1.25rem;       /* 20px — line-height 1.3, peso 600 */
  --text-body: 1rem;        /* 16px — line-height 1.55, peso 400 */
  --text-body-sm: 0.875rem; /* 14px — line-height 1.5, peso 400/500 */
  --text-caption: 0.75rem;  /* 12px — line-height 1.4, peso 500 */

  --tracking-tight: -0.02em;  /* Display e H1 */
  --tracking-normal: 0;
  --tracking-wide: 0.06em;    /* Eyebrows em caixa alta */
}
```

| Token | Tamanho | Peso | Line-height | Uso |
| :--- | :--- | :--- | :--- | :--- |
| `display` | 48px (32px mobile) | 700 | 1.1 | Hero |
| `h1` | 36px | 600–700 | 1.15 | Título de página |
| `h2` | 28px | 600 | 1.2 | Seção |
| `h3` | 20px | 600 | 1.3 | Título de card |
| `body` | 16px | 400 | 1.55 | Texto corrido |
| `body-sm` | 14px | 400–500 | 1.5 | Tabelas, UI densa |
| `caption` | 12px | 500 | 1.4 | Microcopy, labels |

---

## 📏 3. Espaçamento (múltiplos de 4px)

| Token | Valor | Aplicação Típica |
| :--- | :--- | :--- |
| `space-1` | 4px | Ícone ↔ texto |
| `space-2` | 8px | Padding de badges, gap compacto |
| `space-3` | 12px | Padding de inputs e botões |
| `space-4` | 16px | Padding de cards pequenos, gap de listas |
| `space-6` | 24px | Padding padrão de cards e modais |
| `space-8` | 32px | Distância entre blocos de formulário/seções internas |
| `space-12` | 48px | Separação de seções em páginas de app |
| `space-16` | 64px | Margem vertical de seções de landing page |

---

## ⬜ 4. Raio de Borda

```css
:root {
  --radius-sm: 4px;      /* Checkboxes, badges técnicos */
  --radius-md: 8px;      /* Inputs, botões, selects */
  --radius-lg: 12px;     /* Cards, dropdowns */
  --radius-xl: 16px;     /* Modais, sheets, containers de destaque */
  --radius-full: 9999px; /* Avatares, pills de status */
}
```

---

## 🌫️ 5. Elevação e Sombras

Sombras suaves de duas camadas. No tema escuro, a elevação é comunicada principalmente por **superfícies mais claras** (`--card`, `--popover`) e bordas sutis — sombras pretas quase não aparecem.

```css
:root {
  --shadow-sm: 0 1px 2px oklch(0 0 0 / 0.06), 0 1px 3px oklch(0 0 0 / 0.04);
  --shadow-md: 0 2px 4px oklch(0 0 0 / 0.06), 0 6px 16px oklch(0 0 0 / 0.06);
  --shadow-lg: 0 4px 12px oklch(0 0 0 / 0.10), 0 16px 40px oklch(0 0 0 / 0.10);
  --shadow-glow: 0 0 0 1px oklch(from var(--primary) l c h / 0.25), 0 0 24px oklch(from var(--primary) l c h / 0.20);
}

.dark {
  --shadow-sm: 0 1px 2px oklch(0 0 0 / 0.40);
  --shadow-md: 0 4px 12px oklch(0 0 0 / 0.45);
  --shadow-lg: 0 12px 32px oklch(0 0 0 / 0.55);
}
```

> `oklch(from ...)` (relative color syntax) é suportado nos navegadores modernos; para suporte legado, defina o glow com valores fixos.

---

## 🎞️ 6. Movimento

```css
:root {
  --duration-fast: 120ms;   /* hover, cor */
  --duration-base: 180ms;   /* botões, toggles */
  --duration-slow: 260ms;   /* modais, drawers */
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```
