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

> [!NOTE]
> As APis de aplicação são projetadas para interagir com uma conta do ChatWoot da perspectiva de um ***agente/administrador***. Utilize para criar ferramentas internas de importação e exportação dos dados.

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

# Contatos

## [Listar Contatos(Get)](https://asaa-4968020.postman.co/workspace/My-Workspace~024cedf2-663d-4b70-8296-163caa4011d8/request/43702588-876d9f45-dae5-4e3a-ae3a-026bf8a3c6a6?action=share&source=copy-link&creator=43702588)

- Lista de todos os contatos resolvidos com paginação (Tamanho da página = 15). Contatos resolvidos são aqueles que possuem um valor para identificador, e-mail ou número de telefone.
  
```javascript
const options = {method: 'GET', headers: {api_access_token: '<api-key>'}};

fetch('https://app.chatwoot.com/api/v1/accounts/{account_id}/contacts?page=1', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```

## [Criar Contato](https://asaa-4968020.postman.co/workspace/My-Workspace~024cedf2-663d-4b70-8296-163caa4011d8/request/43702588-ab79f6c9-e126-4fc3-ad17-14f0b590bd85?action=share&source=copy-link&creator=43702588)

- Criar um novo contato

``` javascript
const options = {
  method: 'POST',
  headers: {api_access_token: '<api-key>', 'Content-Type': 'application/json'},
  body: JSON.stringify({
    inbox_id: 1,
    name: 'Alice',
    email: 'alice@acme.inc',
    blocked: false,
    phone_number: '+123456789',
    avatar: '<string>',
    avatar_url: 'https://example.com/avatar.png',
    identifier: '1234567890',
    additional_attributes: {type: 'customer', age: 30},
    custom_attributes: {}
  })
};

fetch('https://app.chatwoot.com/api/v1/accounts/{account_id}/contacts', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```
## [Mostrar Contato]()

- Mostrar contato utilizando ID

``` Javascript
const options = {method: 'GET', headers: {api_access_token: '<api-key>'}};

fetch('https://app.chatwoot.com/api/v1/accounts/{account_id}/contacts/{id}', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```

## [Atualizar um Contato](https://asaa-4968020.postman.co/workspace/My-Workspace~024cedf2-663d-4b70-8296-163caa4011d8/request/43702588-f7065a43-2239-4755-949c-482b58bed621?action=share&source=copy-link&creator=43702588)

- Atualiza um contato da lista

```javascript
const options = {
  method: 'PUT',
  headers: {api_access_token: '<api-key>', 'Content-Type': 'application/json'},
  body: JSON.stringify({
    name: 'Alice',
    email: 'alice@acme.inc',
    blocked: false,
    phone_number: '+123456789',
    avatar: '<string>',
    avatar_url: 'https://example.com/avatar.png',
    identifier: '1234567890',
    additional_attributes: {type: 'customer', age: 30},
    custom_attributes: {}
  })
};

fetch('https://app.chatwoot.com/api/v1/accounts/{account_id}/contacts/{id}', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```


## [Deletar um Contato](https://asaa-4968020.postman.co/workspace/My-Workspace~024cedf2-663d-4b70-8296-163caa4011d8/request/43702588-6008895e-5828-41e2-aa3f-2eac2196eac6?action=share&source=copy-link&creator=43702588)

- Deleta um contato da lista de contatos via ID

```javascript
const options = {method: 'DELETE', headers: {api_access_token: '<api-key>'}};

fetch('https://app.chatwoot.com/api/v1/accounts/{account_id}/contacts/{id}', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```