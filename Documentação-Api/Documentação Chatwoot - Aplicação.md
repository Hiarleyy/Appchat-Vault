---
data: 2026-03-03T10:55:00
tags:
  - "#api"
status: rascunho
references:
  - https://developers.chatwoot.com/api-reference/introduction
relacionados: []
---
---
# Api de aplicação

As APis de aplicação são projetadas para interagir com uma conta do ChatWoot da perspectiva de um ***agente/administrador***. Utilize para criar ferramentas internas de importação e exportação dos dados.

## Autenticação
Requer o um nome de usuário ==**(acess_token)**==, gerado nas Configurações do Perfil, após fazer login na conta do ChatWoot.
## Disponibilidade
Compatível com instalações do chatwoot na nuvem e em servidores próprios.

# Conta

## [Obter detalhes da conta (Get)](https://asaa-4968020.postman.co/workspace/My-Workspace~024cedf2-663d-4b70-8296-163caa4011d8/request/43702588-c1c35a14-5573-48ae-ae06-f8663d9eba1d?action=share&source=copy-link&creator=43702588&ctx=documentation)
- Obtém dados da Conta atual - (ADMINISTRADOR)

```Javascript
 curl --request GET \
  --url https://app.chatwoot.com/api/v1/accounts/{id} \
  --header 'api_access_token: EVa5C6mYHAGcJ18uctU7ENdg'
```

## [Registro de Auditoria (Get)]()

- Obtenha detalhes das entradas do log de auditoria de uma conta. Este endpoint está disponível apenas nas edições Enterprise e requer que o recurso ==audit_logs== esteja habilitado.

``` javascript
curl --request GET \
  --url 'https://app.chatwoot.com/api/v1/accounts/{account_id}/audit_logs?page=1' \
  --header 'api_access_token: XkrhDJKaaBGKcP2uNDFHSPsA'
```
