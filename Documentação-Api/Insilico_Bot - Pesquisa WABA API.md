---
data: 2026-05-28T19:53:00-03:00
tags:
  - api
  - whatsapp
  - waba
  - meta
status: anotacao
autor: Insilico_Bot
references:
  - https://developers.facebook.com/docs/whatsapp/cloud-api/get-started
  - https://developers.facebook.com/docs/whatsapp/cloud-api/overview
  - https://developers.facebook.com/docs/whatsapp/cloud-api/reference/messages
  - https://developers.facebook.com/docs/whatsapp/cloud-api/webhooks
  - https://developers.facebook.com/docs/whatsapp/business-management-api/message-templates
---

# Insilico_Bot - Pesquisa WABA API

## Ideia central

A WABA API normalmente se refere ao uso da WhatsApp Business Platform, especialmente a Cloud API da Meta, para integrar um numero comercial do WhatsApp a um sistema proprio. Ela permite enviar mensagens, receber mensagens por webhook, gerenciar templates, trabalhar com midias e conectar conversas a CRMs, bots, filas de atendimento ou automacoes internas.

## Componentes principais

- **Meta Business Portfolio / Business Manager:** conta empresarial onde ficam os ativos da empresa.
- **WABA (WhatsApp Business Account):** conta do WhatsApp Business Platform vinculada ao negocio.
- **Phone Number ID:** identificador tecnico do numero usado nas chamadas da API.
- **WABA ID:** identificador da conta WABA, usado para templates, numeros, configuracoes e inscricoes.
- **Access token:** token Bearer usado para autenticar chamadas no Graph API.
- **App da Meta:** aplicacao criada em Meta for Developers, onde o produto WhatsApp e os webhooks sao configurados.
- **Webhook:** endpoint HTTPS do backend que recebe mensagens recebidas, status de entrega, leitura, falhas e eventos da conta.

## Fluxo basico de configuracao

1. Criar ou acessar uma conta no Meta for Developers.
2. Criar um app e adicionar o produto **WhatsApp**.
3. Vincular ou criar uma WABA no Business Manager.
4. Adicionar um numero de telefone ou usar o numero de teste da Meta.
5. Obter:
   - `phone_number_id`
   - `whatsapp_business_account_id`
   - `access_token`
6. Configurar um endpoint HTTPS publico para webhooks.
7. Definir um `verify_token` proprio para validacao inicial do webhook.
8. Assinar o app nos campos/eventos do WhatsApp, principalmente mensagens e status.
9. Criar e aprovar templates para conversas iniciadas pela empresa.
10. Testar envio e recebimento em ambiente controlado antes de ir para producao.

## Envio de mensagens

O envio e feito via Graph API, geralmente no endpoint:

```http
POST https://graph.facebook.com/vXX.X/{PHONE_NUMBER_ID}/messages
Authorization: Bearer {ACCESS_TOKEN}
Content-Type: application/json
```

Exemplo conceitual de mensagem de texto:

```json
{
  "messaging_product": "whatsapp",
  "to": "5599999999999",
  "type": "text",
  "text": {
    "body": "Ola, esta e uma mensagem enviada pela Cloud API."
  }
}
```

Para iniciar conversa fora da janela de atendimento, normalmente e necessario usar **message templates** aprovados. Mensagens livres sao mais adequadas quando o usuario ja abriu uma janela de atendimento ao enviar mensagem para a empresa.

## Webhooks

O webhook e essencial porque a Cloud API nao funciona como um WhatsApp Web visual. As mensagens recebidas e os status chegam no backend por eventos HTTP.

Eventos comuns:

- mensagem recebida do cliente;
- status de mensagem enviada: `sent`, `delivered`, `read`, `failed`;
- eventos relacionados a conta, templates ou qualidade;
- recebimento de midias, com IDs que depois precisam ser consultados/baixados pela API.

Pontos importantes:

- O endpoint precisa ser HTTPS e acessivel publicamente.
- A validacao inicial usa o `verify_token` configurado no painel da Meta.
- O backend deve responder rapidamente com `200 OK`.
- O processamento pesado deve ser assíncrono: receber evento, persistir, responder 200 e processar em fila/job.
- E recomendavel validar assinatura da requisicao quando configurada, para evitar eventos falsos.

## Templates

Templates sao mensagens pre-aprovadas usadas principalmente quando a empresa inicia contato ou quando a janela de atendimento expirou.

Categorias comuns:

- **Utility:** confirmacoes, avisos transacionais, atualizacoes de pedido, lembretes.
- **Authentication:** codigos de verificacao e login.
- **Marketing:** ofertas, campanhas, reengajamento e comunicacoes promocionais.

Boas praticas:

- Criar templates objetivos e com variaveis claras.
- Evitar linguagem promocional em templates transacionais.
- Criar versoes por idioma quando necessario.
- Controlar status de aprovacao, qualidade e possiveis rejeicoes.

## Configuracao recomendada no backend

Variaveis de ambiente tipicas:

```env
WHATSAPP_ACCESS_TOKEN=
WHATSAPP_PHONE_NUMBER_ID=
WHATSAPP_WABA_ID=
WHATSAPP_VERIFY_TOKEN=
META_APP_SECRET=
GRAPH_API_VERSION=vXX.X
```

Estrutura minima:

- rota `GET /webhooks/whatsapp` para verificacao da Meta;
- rota `POST /webhooks/whatsapp` para receber eventos;
- servico de envio de mensagens;
- persistencia de contatos, conversas, mensagens e status;
- fila para processar eventos;
- logs de payloads e erros da Graph API;
- painel/admin para acompanhar falhas, tokens e templates.

## Pontos de atencao

- Numero usado na Cloud API pode ter restricoes em relacao ao uso simultaneo com app WhatsApp/WhatsApp Business, dependendo do modo de onboarding e coexistencia.
- Token temporario serve para teste; producao deve usar token apropriado de longa duracao/sistema.
- Business verification, permissões e configuracao da WABA podem bloquear partes do fluxo.
- Templates precisam de aprovacao e podem falhar por categoria, texto, variaveis ou politica.
- Webhook mal configurado e uma das causas mais comuns de "enviei mas nao recebi resposta/status".
- A API nao oferece uma inbox pronta; normalmente e necessario integrar com Chatwoot, CRM, sistema proprio ou BSP.
- Para clientes finais, Embedded Signup pode simplificar onboarding, mas exige app/configuracao de parceiro ou fluxo OAuth correto.

## Checklist rapido de implementacao

- [ ] Confirmar se sera Cloud API direta da Meta ou via BSP.
- [ ] Definir WABA e numero que sera usado.
- [ ] Gerar token adequado para producao.
- [ ] Configurar webhook publico com verificacao.
- [ ] Persistir mensagens recebidas e enviadas.
- [ ] Implementar envio de texto, template e midia.
- [ ] Criar templates iniciais e aprovar.
- [ ] Registrar logs de erros da Graph API.
- [ ] Testar status `sent`, `delivered`, `read` e `failed`.
- [ ] Documentar limites, responsabilidades e fluxo de suporte.

## Fontes consultadas

- Meta Developers - Cloud API Get Started: https://developers.facebook.com/docs/whatsapp/cloud-api/get-started
- Meta Developers - Cloud API Overview: https://developers.facebook.com/docs/whatsapp/cloud-api/overview
- Meta Developers - Messages endpoint: https://developers.facebook.com/docs/whatsapp/cloud-api/reference/messages
- Meta Developers - Webhooks: https://developers.facebook.com/docs/whatsapp/cloud-api/webhooks
- Meta Developers - Message Templates: https://developers.facebook.com/docs/whatsapp/business-management-api/message-templates
- WhatsApp Business Developer Hub: https://whatsappbusiness.com/developers/developer-hub/

