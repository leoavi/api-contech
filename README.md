# Documentação API Contrans

Documentação estática (Swagger UI) hospedada no GitHub Pages.

## Estrutura

- `index.html` — página que renderiza o Swagger UI com **seletor de definições** no topo (dropdown "Select a definition")
- `api-producao.yaml` — **aba "Produção"** — espelho fiel dos endpoints realmente cadastrados em `IN_WEBSERVICE` (handle=1, "1 - Web service Escalasoft") no banco ESCALASOFT. Aba padrão (abre primeiro).
- `api-exemplo.yaml` — **aba "Exemplo"** — contrato idealizado baseado na documentação Contech padrão (cópia da Escalasoft). Mantido como referência do "como deveria ser".

## Diferença entre as abas

| Aspecto | Produção | Exemplo |
|---|---|---|
| Origem | Dump direto do banco ESCALASOFT (`IN_WEBSERVICE handle=1`) | Doc Contech padrão, importada manualmente |
| Operações ativas | 59 | 50 |
| Schemas de payload | `type: object` vazio (banco não armazena schema) | Preenchidos no YAML |
| Autenticação cadastrada | `Nenhum` em 100% dos endpoints | `BearerAuth` / `BasicAuth` |
| Convenção de URL | Inconsistente (Title Case, lowercase, segmentos variáveis) | Lowercase + camelCase padronizado |
| Módulos (tags) | 15 produtos do ERP (Administração, Armazém, Operacional, Pessoa, RH, Tecnologia, etc.) | 6 módulos (WMS, ERP-Fiscal, ERP-Materiais, ERP-Financeiro, TMS, OMS) |

Os dois mundos hoje **não compartilham nenhuma operação** — a aba Exemplo é o contrato que se quer expor a terceiros via gateway, a aba Produção é o que está cadastrado internamente no ERP.

## Atualizar a aba "Produção" a partir do banco

A aba Produção é gerada por um script Python que lê o banco ESCALASOFT direto. Para regenerar:

```bash
# 1) Dump das 5 tabelas IN_WEBSERVICE* via Invoke-Escalasoft.ps1
# 2) Conversão em api-producao.yaml via generate_producao.py
# (scripts vivem em outro repo de automação local)
```

> Não editar `api-producao.yaml` à mão — o conteúdo será sobrescrito na próxima regeneração.

## Editar a aba "Exemplo"

1. Mexa em `api-exemplo.yaml` (remover endpoints que não usamos, ajustar campos adaptados, adicionar novos)
2. Commit + push na branch `main`
3. GitHub Pages publica automaticamente

## Publicar no GitHub Pages (primeira vez)

1. Criar repo no GitHub e fazer push deste diretório
2. No repo: **Settings → Pages → Source: Deploy from branch → Branch: `main` / root**
3. URL ficará em `https://<usuario>.github.io/<repo>/`

## Testar localmente

Abrir `index.html` direto no navegador costuma falhar por CORS no fetch do YAML. Rodar um servidor estático:

```powershell
python -m http.server 8000
```

E acessar `http://localhost:8000/`.
