# Checklist de Auditoria Estática: Caça a Fluxos Quebrados & Dead Ends

Use este checklist para inspecionar o código-fonte antes de executar a aplicação, identificando armadilhas de interface e botões sem funcionalidade.

---

## 🔎 1. Varredura de Botões Fantasmas & Mockups

| Padrão Suspeito | Risco | Como Localizar | O que Corrigir |
| :--- | :--- | :--- | :--- |
| `onClick={() => {}}` | O botão parece funcional, mas não faz absolutamente nada. | `grep "onClick={\(\)\s*=>\s*{\s*}}"` | Conectar à função real de estado, modal ou requisição de API. |
| `onClick={() => console.log(...)}` | Código de protótipo/teste esquecido em produção. | `grep "onClick={.*console\.log"` | Substituir pelo disparo da ação real do sistema. |
| `onClick={() => null}`, `onClick={noop}`, `onClick={undefined}` | Variações de handler vazio que escapam da busca por `() => {}`. | `grep -E "onClick=\{\s*(\(\)\s*=>\s*(null\|undefined\|false)\|noop\|undefined\|null)\s*\}"` | Conectar à ação real ou remover o botão. |
| `<button>` sem `onClick` e sem `type="submit"` | Elemento clicável que não dispara nenhum evento de clique nem submete formulário. | Inspecionar botões fora de formulários. | Adicionar `onClick` ou converter para link `<Link href="...">` se for apenas navegação. |
| `<button>` dentro de `<form>` sem `type` | O padrão HTML é `type="submit"`: botões auxiliares ("Cancelar", "Mostrar senha", "Adicionar item") submetem o formulário sem querer. | Buscar `<button` sem `type=` dentro de componentes que renderizam `<form>`. | Declarar `type="button"` em todo botão que não seja o submit. |
| `href="#"` ou `href=""` | Clicar no link causa rolagem involuntária para o topo da página sem navegar. | `grep 'href="#"'` ou `grep 'href=""'` | Apontar para rota real ou trocar por elemento `<button>` se for ação na mesma tela. |
| `<Link href="/rota-inexistente">` | Resulta em erro 404 assim que o usuário clica. | Comparar links do menu/rodapé com arquivos de rotas. | Criar a página de destino ou ajustar o link para a rota correta. |

---

## 📝 2. Resiliência de Formulários & Estados Críticos

### Teste de Submissão & Cliques Concorrentes
- [ ] O botão de submissão do formulário possui estado `disabled={isLoading || isSubmitting}` durante a chamada de rede?
- [ ] Há um indicador visual (spinner, texto alternativo como *"Salvando..."*) ativo enquanto a requisição ocorre?
- [ ] Os campos de input são protegidos contra envio acidental ao pressionar a tecla `Enter` antes do preenchimento completo?

### Tratamento de Falhas e Validação de Erros
- [ ] O formulário exibe mensagens de erro específicas abaixo de cada campo quando dados inválidos são fornecidos (ex: e-mail fora de padrão, senha curta)?
- [ ] Se a API ou backend retornar status 400, 409 (conflito) ou 500, a UI apresenta um toast/alerta de erro compreensível ao invés de ficar travada no estado de loading?
- [ ] Os estados de sucesso limpam o formulário e oferecem feedback imediato (ex: toast de confirmação ou redirecionamento suave)?

---

## 🕳️ 3. Resiliência a Dados Vazios (*Empty States*) & Nulos

### Null Checks & Segurança de Arrays
- [ ] Todas as chamadas de iteração (`.map`, `.filter`, `.reduce`) utilizam optional chaining ou fallback seguro:
  - ❌ `data.items.map(...)` (Causa tela branca se `data` ou `items` vierem nulos)
  - ✅ `(data?.items ?? []).map(...)`
- [ ] A aplicação foi testada com banco de dados zerado (zero registros)?
- [ ] Quando uma lista está vazia (`items.length === 0`), a tela exibe um componente de *Empty State* com mensagem clara e botão para cadastrar o primeiro item?

---

## ⌨️ 4. Acessibilidade Operacional (a11y)

- [ ] É possível navegar por todos os botões, links e campos da tela utilizando exclusivamente a tecla `Tab`?
- [ ] O foco visual é visível ao navegar por teclado (outline de foco presente, sem `outline: none` sem substituto)?
- [ ] Modais fecham ao pressionar a tecla `Esc`?
- [ ] Botões que contêm apenas ícones gráficos (ex: botão de lixeira, lápis, fechar) possuem atributo `aria-label` descritivo?
