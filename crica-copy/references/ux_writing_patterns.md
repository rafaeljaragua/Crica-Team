# Padrões de UX Writing & Microcopy de Produto

Este documento reúne padrões de redação de interface para produtos digitais de alta confiabilidade. A premissa central é reduzir a carga cognitiva, orientar o usuário com precisão e eliminar infantilizações no ecossistema do produto.

---

## 🔘 Botões e Chamadas para Ação (CTAs)

### Regra de Ouro: Verbo de Ação + Substantivo do Objeto
Evite botões com textos vagos como "Clique aqui", "Confirmar", "Ok" ou "Avançar". O botão deve descrever exatamente a consequência do clique.

| Contexto | Ruim (Genérico / AI Slop) | Recomendado (crica-copy) |
| :--- | :--- | :--- |
| **Criação de chave de API** | "Gerar agora!" | `Criar chave de API` |
| **Exclusão de recurso** | "Sim, deletar" | `Excluir banco de dados` |
| **Atualização de assinatura** | "Fazer upgrade mágico" | `Mudar para plano Pro` |
| **Envio de convite** | "Convide sua equipe maravilhosa" | `Enviar convite` |
| **Download de dados** | "Baixar tudo!" | `Exportar CSV` |

---

## ⚠️ Mensagens de Erro e Alertas

### A Fórmula dos 3 Elementos:
1. **O que aconteceu**: Declaração factual, sem histeria.
2. **Por que aconteceu**: Razão técnica ou lógica imediata.
3. **O que fazer agora**: Ação corretiva recomendada.

### Exemplos Práticos:

#### Falha de Conexão com Banco de Dados:
- ❌ **Evite**: *"Ops! Houve um errinho ao conectar ao seu banco 😢 Tente de novo daqui a pouco!"*
- ✅ **Prefira**: *"Não foi possível estabelecer conexão com o host db.supabase.co: porta 5432 inacessível. Verifique as regras de firewall ou o status da instância."*

#### Validação de Formulário:
- ❌ **Evite**: *"Parece que esse campo ficou em branco! Preencha para continuar sua jornada!"*
- ✅ **Prefira**: *"O campo 'Nome da Organização' é obrigatório (mínimo de 3 caracteres)."*

#### Limite de Requisições / Rate Limit:
- ❌ **Evite**: *"Calma aí, amigão! Você foi rápido demais!"*
- ✅ **Prefira**: *"Limite de 100 requisições/minuto excedido. Novas chamadas serão liberadas em 28 segundos."*

---

## 🕳️ Estados Vazios (*Empty States*)

Estados vazios não devem ser apenas um "espaço em branco com um desenho triste". Eles são oportunidades de instrução funcional.

### Estrutura Ideal de um Empty State:
1. **Título**: Informa o estado atual sem drama (`Nenhum projeto encontrado`).
2. **Descrição**: Explica brevemente o que pertence a esse espaço e como populá-lo (`Projetos criados ou compartilhados com você aparecerão aqui.`).
3. **CTA Contextual**: Ação direta para resolver o vazio (`Criar primeiro projeto`).

---

## 🪟 Modais de Ação Destrutiva (Confirmação Crítica)

Em ações irreversíveis (deleção de dados, reset de credenciais, cancelamento imediato), nunca use humor ou tom casual.

### Estrutura:
- **Título do Modal**: Específico sobre o impacto (`Excluir repositório 'core-engine'?`).
- **Corpo do Modal**: Deixe claro o que é perdido permanentemente (`Essa ação não pode ser desfeita. Todos os 14 branches, 350 commits e releases associadas serão apagados dos servidores.`).
- **Confirmação Explícita**: Solicitar digitação do nome do recurso para evitar cliques acidentais.
- **Botões**:
  - Cancelar: `Cancelar` (neutro)
  - Ação: `Excluir repositório permanentemente` (estilo destrutivo/vermelho)

---

## 💡 Tooltips e Dicas de Contexto

- **Não repita o label**: Se o campo diz "E-mail de cobrança", o tooltip não deve dizer "Digite aqui o seu e-mail de cobrança".
- **Explique o não-óbvio**: Use tooltips para esclarecer implicações: *"Enviaremos notas fiscais e alertas de expiração do cartão para este endereço, em vez do e-mail do titular da conta."*
- **Limite de tamanho**: No máximo 2 linhas de texto. Se precisa de mais explicação, cabe uma documentação dedicada.
