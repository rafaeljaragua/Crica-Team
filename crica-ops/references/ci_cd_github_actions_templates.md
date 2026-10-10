# Templates de Workflows CI/CD (GitHub Actions)

Templates prontos para automação de testes, validação estática e deploy de aplicações fullstack.

---

## 🧪 1. Pipeline de Validação Contínua (CI)

Salve em `.github/workflows/ci.yml`. Executa em todo `push` ou `pull_request` direcionado às branches `main` ou `develop`:

```yaml
name: CI Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout do Código
        uses: actions/checkout@v4

      - name: Configurar Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'

      - name: Instalar Dependências
        run: npm ci

      - name: Checagem de Tipos (TypeScript)
        run: npm run typecheck || npx tsc --noEmit

      - name: Executar Linter
        run: npm run lint

      - name: Executar Testes Automatizados
        run: npm test --if-present

      - name: Validar Build de Produção
        run: npm run build
        env:
          NEXT_TELEMETRY_DISABLED: 1
          NODE_ENV: production
```

---

## 🚀 2. Deploy Seguro com Migração de Banco Supabase

Salve em `.github/workflows/deploy.yml`. Executa apenas após aprovação ou merge na branch `main`:

```yaml
name: Deploy Produção

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'

      - name: Instalar Dependências
        run: npm ci

      - name: Build
        run: npm run build

      - name: Notificar Sucesso
        run: echo "Build concluído com sucesso. Pronto para deploy na plataforma de hospedagem."
```
