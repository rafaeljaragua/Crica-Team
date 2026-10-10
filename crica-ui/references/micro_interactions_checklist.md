# Checklist de Micro-Interações & Fluidez Visual (60 FPS)

Use este checklist para validar se a interface possui resposta cinética viva e elegante sem causar jank de renderização.

---

## ⚡ 1. Princípios de Animação de Alta Performance

1. **Animar Apenas Propriedades da GPU**:
   - ✅ Permitido: `transform` (`translate`, `scale`, `rotate`) e `opacity`.
   - ❌ Evite: Animar `height`, `width`, `top`, `left`, `margin` ou `padding` (causam reflow de layout caro na CPU).
2. **Duração Cirúrgica** (tokens de movimento em `design_system_tokens.md`):
   - Hover e mudanças de cor: **`--duration-fast` (120ms)**.
   - Botões, toggles, active/focus: **`--duration-base` (180ms)**.
   - Abertura de modais, drawers e transição de páginas: **`--duration-slow` (260ms)**.
   - Qualquer animação acima de 400ms começa a parecer lenta e irritante para o usuário diário.
3. **Curvas de Aceleração (Easing)**:
   - Use `--ease-out` (`cubic-bezier(0.16, 1, 0.3, 1)`) para elementos que entram em cena.
   - Use `ease-in` para elementos que saem de cena.
   - Nunca use animações lineares (`linear`) para movimentos de interface humana.
4. **Movimento Reduzido**: respeite `prefers-reduced-motion: reduce`, desativando transformações e mantendo apenas mudanças de opacidade/cor instantâneas.

---

## 📋 2. Checklist por Componente

### Botões e Links de Ação
- [ ] Possui transição de cor suave no hover (`transition: background-color var(--duration-fast) var(--ease-out)`).
- [ ] Possui compressão sutil no clique (`active:scale-[0.98]`).
- [ ] O cursor muda explicitamente para `pointer` ao passar sobre o elemento.
- [ ] Botão desabilitado possui `opacity-50`, cursor `not-allowed` e desativa efeitos de hover/active.

### Modais e Diálogos
- [ ] O backdrop (fundo escurecido) aparece com fade-in suave (`opacity: 0 -> 1`).
- [ ] O modal central surge com sutil zoom-in (`scale: 0.95 -> 1.0`) e opacidade progressiva.
- [ ] O fechamento ocorre sem solavancos visuais.

### Listas e Tabelas Interativas
- [ ] Linhas clicáveis destacam a cor de fundo sutilmente no hover.
- [ ] Skeletons de carregamento utilizam animação de gradiente pulsante (*shimmer*) em vez de spinners estáticos centralizados.
- [ ] Tooltips surgem com delay intencional (300ms a 500ms) para não poluir a tela em movimentos rápidos do mouse.
