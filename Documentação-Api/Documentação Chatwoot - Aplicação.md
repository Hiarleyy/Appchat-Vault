---
data: 2026-03-03T10:55:00
tags:
  - "#api"
status: rascunho
references:
  - https://developers.chatwoot.com/api-reference/introduction
relacionados: []
---

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

## [Coletar Conversas de um Contato ](https://asaa-4968020.postman.co/workspace/My-Workspace~024cedf2-663d-4b70-8296-163caa4011d8/request/43702588-5ad876c6-32c8-4353-8902-417122d8362a?action=share&source=copy-link&creator=43702588)

- Coleta todas as conversas associadas aquele contato, incluindo todos os agentes que o contato já conversou.
```javascript
const options = {method: 'GET', headers: {api_access_token: '<api-key>'}};

fetch('https://app.chatwoot.com/api/v1/accounts/{account_id}/contacts/{id}/conversations', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```

## [Busca nos contatos por Palavra-Chave](https://asaa-4968020.postman.co/workspace/My-Workspace~024cedf2-663d-4b70-8296-163caa4011d8/request/43702588-4610f2c1-9c64-4380-bae7-b2cf2fbf4f75?action=share&source=copy-link&creator=43702588)

- Busca informações nos contatos com palavra-chave, atualmente suportando pesquisas de email. Encontra contatos que possuem um identificador sendo email ou telefone.
```javascript
const options = {method: 'GET', headers: {api_access_token: '<api-key>'}};

fetch('https://app.chatwoot.com/api/v1/accounts/{account_id}/contacts/search?page=1', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```

## [Filtro de Contatos]()

- Filtra os contatos com um filtro customizado e paginação

```Javascript
const options = {
  method: 'POST',
  headers: {api_access_token: '<api-key>', 'Content-Type': 'application/json'},
  body: JSON.stringify({
    payload: [
      {
        attribute_key: 'name',
        filter_operator: 'equal_to',
        values: ['en'],
        query_operator: 'AND'
      },
      {
        attribute_key: 'country_code',
        filter_operator: 'equal_to',
        values: ['us'],
        query_operator: null
      }
    ]
  })
};

fetch('https://app.chatwoot.com/api/v1/accounts/{account_id}/contacts/filter', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```

## [Criar uma caixa de contatos]()

- Criar um registro de caixa de entrada de contato para uma caixa de entrada
```javascript
const options = {
  method: 'POST',
  headers: {api_access_token: '<api-key>', 'Content-Type': 'application/json'},
  body: JSON.stringify({inbox_id: 1, source_id: '<string>'})
};

fetch('https://app.chatwoot.com/api/v1/accounts/{account_id}/contacts/{id}/contact_inboxes', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```

## [Obtenha caixas de entrada acessíveis]()

 - Obtenha uma lista de caixas de entrada de contatos

```javascript
const options = {method: 'GET', headers: {api_access_token: '<api-key>'}};

fetch('https://app.chatwoot.com/api/v1/accounts/{account_id}/contacts/{id}/contactable_inboxes', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```


## [Unir Contatos]()

- Mesclar dois contatos em um único contato. O contato base permanece e recebe todos os dados do contato mesclado. Após a mesclagem, o contato mesclado é excluído permanentemente. Essa ação é irreversível. Todas as conversas, marcadores e atributos personalizados do contato mesclado serão transferidos para o contato base.

```javascript
const options = {
  method: 'POST',
  headers: {api_access_token: '<api-key>', 'Content-Type': 'application/json'},
  body: JSON.stringify({base_contact_id: 1, mergee_contact_id: 2})
};

fetch('https://app.chatwoot.com/api/v1/accounts/{account_id}/actions/contact_merge', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
```

