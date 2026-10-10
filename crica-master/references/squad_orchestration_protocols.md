# Protocolos de Orquestração da Squad Crica & Handoffs

Este documento estabelece as regras de governança e passagem de contexto entre as 10 skills da squad Crica, coordenadas pelo `crica-master`.

> **Fonte da verdade**: a matriz de acionamento abaixo é a referência oficial. As tabelas "Acionamento Cruzado" de cada `SKILL.md` são derivadas dela. Ao alterar um gatilho, atualize **os dois lados** (quem convoca e quem recebe) e esta matriz.

---

## 🚦 Regras de Ouro de Handoff Inter-Skills

1. **Entrada e Saída Estruturadas**: Toda skill encerra sua etapa com um bloco no formato de [handoff_template.md](handoff_template.md), servindo de insumo direto para a próxima skill da esteira.
2. **Sem Suposições Implícitas**: Se o `crica-po` não especificou um comportamento de borda, o `crica-arch` não deve adivinhar; ele deve solicitar ao `crica-master` um refinamento rápido de requisito.
3. **Pausa em Falhas Críticas**: Se `crica-seg` identificar uma brecha de autorização ou `crica-tester` encontrar quebra de fluxo impeditiva, a esteira de deploy (`crica-ops`) é imediatamente bloqueada.
4. **Carregar antes de executar**: "Convocar" uma skill significa ler `../crica-X/SKILL.md` e seguir suas fases (ver Protocolo de Acionamento no `crica-master`).

---

## 🔄 Esteira Principal (Feature de Ponta a Ponta)

```
[crica-po]      ──(User Stories & Regras)──>   [crica-copy] & [crica-arch]
[crica-copy]    ──(Microcopy & Mensagens)──>   [crica-ui]
[crica-arch]    ──(Schemas, Types & APIs)──>   [crica-ui], [crica-fast] & [crica-seg]
[crica-ui]      ──(Interface Completa)─────>   [crica-tester]
[crica-fast]    ──(Queries Otimizadas)─────>   [crica-tester]
[crica-seg]     ──(Rotas Blindadas)────────>   [crica-tester]
[crica-tester]  ──(Fluxos Aprovados)───────>   [crica-seo] & [crica-ops]
[crica-ops]     ──(Build & Deploy Pronto)──>   [crica-docs]
[crica-docs]    ──(Manual & Changelog)─────>   [crica-master] (Entrega Final)
```

---

## 🧮 Matriz Oficial de Acionamento (Quem convoca → Quem recebe)

| Convoca ↓ / Recebe → | po | copy | ui | arch | fast | seg | tester | seo | ops | docs | master |
| :--- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| **crica-po** | | ● | | ● | ● | ● | ● | | | | |
| **crica-copy** | | | ● | | | | | ● | | | ● |
| **crica-ui** | | ● | | | ● | | ● | | | | |
| **crica-arch** | | | ● | | ● | ● | | | | ● | |
| **crica-fast** | | | | ● | | | ● | | ● | | |
| **crica-seg** | | | | ● | | | ● | | ● | | ● |
| **crica-tester** | | ● | | ● | | | | ● | ● | | ● |
| **crica-seo** | | | | ● | ● | ● | | | ● | | |
| **crica-ops** | | | | ● | | ● | | | | ● | |
| **crica-docs** | | ● | | | | | | | | | ● |
| **crica-master** | ● | ● | ● | ● | ● | ● | ● | ● | ● | ● | |

### Leitura por skill (Recebe de)

| Skill | Recebe de |
| :--- | :--- |
| `crica-po` | master |
| `crica-copy` | po, ui, tester, docs, master |
| `crica-ui` | copy, arch, master |
| `crica-arch` | po, fast, seg, tester, seo, ops, master |
| `crica-fast` | po, ui, arch, seo, master |
| `crica-seg` | po, arch, seo, ops, master |
| `crica-tester` | po, ui, arch, fast, seg, master |
| `crica-seo` | copy, tester, master |
| `crica-ops` | fast, seg, tester, seo, master |
| `crica-docs` | arch, ops, master |
| `crica-master` | copy, seg, tester, docs (+ escalonamentos de qualquer skill) |

---

## 🛠️ Comandos Rápidos de Invocação da Squad

O usuário pode invocar missões inteiras com comandos executivos simples direcionados ao `crica-master` (convenção de linguagem natural, não slash commands):

- `@crica-master nova-feature <descrição>`: Dispara o pipeline completo de criação de ponta a ponta.
- `@crica-master auditoria-geral`: Dispara varredura 360° em segurança, performance, SEO, QA e textos.
- `@crica-master preparar-lancamento`: Validação pré-deploy, SEO/GEO, blindagem de segredos e geração de documentação.
- `@crica-master corrigir-bug <descrição>`: Dispara diagnóstico pelo Tester, correção pelo Arch/Fast/Seg e teste de regressão.
