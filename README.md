# Documentação API Contrans

Documentação estática (Swagger UI) hospedada no GitHub Pages.

## Estrutura

- `index.html` — página que renderiza o Swagger UI
- `api.yaml` — especificação OpenAPI editável (ponto de partida: cópia da Escalasoft)
- `api-escalasoft-original.yaml` — backup intocado do original, só pra referência

## Editar

1. Mexa em `api.yaml` (remover endpoints que não usamos, ajustar campos adaptados, adicionar novos).
2. Commit + push na branch `main`.
3. GitHub Pages publica automaticamente.

## Publicar no GitHub Pages (primeira vez)

1. Criar repo no GitHub e fazer push deste diretório.
2. No repo: **Settings → Pages → Source: Deploy from branch → Branch: `main` / root**.
3. URL ficará em `https://<usuario>.github.io/<repo>/`.

## Testar localmente

Abrir `index.html` direto no navegador costuma falhar por CORS no fetch do YAML. Rodar um servidor estático:

```powershell
python -m http.server 8000
```

E acessar `http://localhost:8000/`.
