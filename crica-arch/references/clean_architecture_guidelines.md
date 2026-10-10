# Diretrizes de Arquitetura Limpa & Organização Modular (Feature-Based)

Use este guia para organizar codebases modernos em TypeScript/Next.js/React, evitando pastas desorganizadas com dezenas de arquivos desconexos.

---

## 📁 1. Estrutura Recomendada: Por Funcionalidade (*Feature-Based*)

Em vez de agrupar tudo em pastas genéricas (`/components`, `/hooks`, `/services`), agrupe o código ao redor do domínio de negócio:

```
src/
├── app/                      # Roteamento e layouts de página (apenas casca/composição)
│   ├── (auth)/login/page.tsx
│   └── (dashboard)/invoices/page.tsx
├── features/                 # Módulos de domínio autocontidos
│   ├── auth/
│   │   ├── components/       # Componentes exclusivos de auth (LoginForm.tsx)
│   │   ├── services/         # Chamadas ao backend / SDK de auth
│   │   ├── hooks/            # Hooks de sessão e login
│   │   ├── types.ts          # Tipos e interfaces específicas de auth
│   │   └── schemas.ts        # Schemas de validação Zod
│   └── invoices/
│       ├── components/       # InvoiceTable.tsx, InvoiceStatusBadge.tsx
│       ├── services/         # invoiceService.ts
│       ├── types.ts          # Invoice, InvoiceItem
│       └── utils.ts          # formatCurrency.ts
├── shared/                   # Código verdadeiramente compartilhado por toda a aplicação
│   ├── components/ui/        # Componentes base de UI (Button, Modal, Input)
│   ├── lib/                  # Clientes de terceiros inicializados (supabase.ts, db.ts)
│   └── hooks/                # Hooks utilitários globais (useDebounce, useMediaQuery)
```

---

## 🛡️ 2. A Regra do Desacoplamento de UI

### Exemplo de Código Ruim (Acoplado) ❌
```tsx
// Componente misturando visual, SQL e tratamento de erro
export function InvoicesList() {
  const [data, setData] = useState([]);
  
  useEffect(() => {
    supabase.from('invoices').select('*').then(res => setData(res.data));
  }, []);

  return <div>{data.map(i => <div key={i.id}>{i.total}</div>)}</div>;
}
```

### Exemplo de Código Arquitetado Corretamente ✅
```tsx
// 1. Service / Hook cuida da chamada e tipagem
export function useInvoices() {
  return useQuery({
    queryKey: ['invoices'],
    queryFn: async () => invoiceService.listAll(),
  });
}

// 2. Componente de UI foca apenas em estados visuais
export function InvoicesList() {
  const { data: invoices, isLoading, isError } = useInvoices();

  if (isLoading) return <InvoicesTableSkeleton />;
  if (isError) return <ErrorMessage message="Erro ao carregar faturas." />;
  if (!invoices?.length) return <EmptyStateInvoices />;

  return <InvoicesTable items={invoices} />;
}
```
