# Padrões de Documentação de API & Exemplo OpenAPI 3.0

Diretrizes para documentar rotas de API REST de forma padronizada e legível para clientes, integrações e agentes autônomos.

---

## 📐 1. Princípios de Documentação de Endpoints

Para cada endpoint documentado, é obrigatório especificar:
1. **Método e Rota**: Ex: `POST /api/v1/invoices`
2. **Autenticação**: Ex: `Bearer Token (JWT)` ou `ApiKey`
3. **Parâmetros**: URL params, Query params e corpo JSON da requisição (*Request Body*).
4. **Respostas Padronizadas**:
   - `200/201`: Sucesso com schema e exemplo real.
   - `400 Bad Request`: Erro de validação de campos.
   - `401 Unauthorized / 403 Forbidden`: Problemas de credencial ou permissão.
   - `404 Not Found`: Recurso não localizado.
   - `500 Internal Server Error`: Falha inesperada do servidor.

---

## 📄 2. Exemplo OpenAPI 3.0 (YAML)

```yaml
openapi: 3.0.3
info:
  title: API do Produto
  description: Endpoints para gestão de faturas e workspaces.
  version: 1.0.0
paths:
  /api/v1/invoices:
    post:
      summary: Criar nova fatura
      description: Emite uma nova fatura vinculada ao workspace ativo.
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - customer_id
                - amount_cents
                - due_date
              properties:
                customer_id:
                  type: string
                  format: uuid
                  example: "123e4567-e89b-12d3-a456-426614174000"
                amount_cents:
                  type: integer
                  minimum: 100
                  example: 4990
                due_date:
                  type: string
                  format: date
                  example: "2026-11-01"
      responses:
        '201':
          description: Fatura criada com sucesso
          content:
            application/json:
              schema:
                type: object
                properties:
                  id:
                    type: string
                    format: uuid
                  status:
                    type: string
                    example: "pending"
                  total:
                    type: number
                    example: 49.90
        '400':
          description: Erro de validação dos campos obrigatórios
        '401':
          description: Token de autenticação inválido ou ausente
```
