---
data: 2026-04-10T10:10:00
tags:
  - api
status: rascunho
references:
  - https://green-api.com/en/waba/docs/api/
relacionados: []
---

## Tópicos Principais

### 1. Número de Telefone Empresarial
- O número oficial precisa ser registrado na WABA
- não pode ser ativo no aplicativo do Whatsapp nem Whatsapp business
### 2. Business Manager
- Plataforma para configurar os números, templates, permissões e webhooks
- Nessa plataforma você também faz o Token OAuth para autenticação de chamadas na API
### 3. Envio de Mensagens

Você faz uma requisição HTTP para o endpoint da API:
````javascript
POST /v18.0/{phone-number-id}/messages  
Authorization: Bearer {ACCESS_TOKEN}
````

#### Tipos de mensagens:

#### 1. Sessão (24h)

- Quando o usuário inicia conversa
- Você pode responder livremente por 24h

####  2. Template (HSM)

- Para iniciar conversa
- Precisa ser aprovado pela Meta
- Ex: confirmação, alerta, cobrança
#### <font color="#f79646">O mais ideal para os clientes seria Template, por conta da janela de tempo dos atendimentos ser grande</font>

#### 4. Recebimento de Mensagens (Webhooks) - Fluxo de Envio
- 1. Whatsapp envia um *POST* para o seu Webhook
- 2. O payload JSON inclui:
	- Número de usuário
	- Conteúdo da mensagem
	- Timestamp - (horário do envio)
- Exemplo:
```JSON
{  
	"messages": [  
		{  
			"from": "551199999999",  
			"text": {  
			"body": "Olá"  
			}  
		}  
	]  
}
```

### 5. Modos De uso
#### CloudAPI - (recomendado):
 - #### Hospedado pela MetaPlataforms
 - ##### Simplicidade na aplicação
 - Escalabilidade automatica
 
#### On-Premises API:
- #### Self-Hosted
- #### Complexidade na implementação - Instalação via Portainer

### 6. Regras Importantes
- #### Spans não são permitidos na WABA
- #### Os templates precisam da aprovação da META
- #### As mensagens DEVEM ter contexto com o usuário
	 OBS:****<font color="#f79646"> Qualidade do número impacta no envio - (AQUECIMENTO DO NÚMERO)</font>
### 7. Status e Eventos - Webhooks
- Sent
- delivered
- read
- failed


---
## 1. Preços

### 1. Se o usuário manda mensagem primeiro:
- Você tem uma janela de **24 horas**
- Pode responder **quantas vezes quiser**
- **Custo: ZERO**
<font color="#f79646">Service conversation / atendimento</font>
### 2. Inicia conversa (template obrigatório)

Exemplos:

- “Seu pedido foi enviado”
- “Promoção de hoje”
- “Seu boleto vence amanhã”
<font color="#f79646">Aqui você precisa usar templates aprovados</font>
### Tabela de custos da WABA (2025–2026)

| Tipo de mensagem      | Quem inicia | Uso típico                   | Preço estimado (USD) | Preço estimado (BRL) |
| --------------------- | ----------- | ---------------------------- | -------------------- | -------------------- |
| Atendimento (Service) | Cliente     | Suporte, dúvidas, chat       | **Grátis**           | **Grátis**           |
| Utilidade (Utility)   | Empresa     | Pedido, status, lembrete     | $0.004 – $0.045      | R$ 0,02 – R$ 0,25    |
| Autenticação (OTP)    | Empresa     | Código de login, verificação | $0.003 – $0.030      | R$ 0,02 – R$ 0,20    |
| Marketing             | Empresa     | Promoções, campanhas         | $0.020 – $0.130      | R$ 0,10 – R$ 0,65    |
### Custos adicionais (fora da Meta Platforms)

|Item|Descrição|Faixa de custo|
|---|---|---|
|BSP (ex: Twilio, Zenvia)|Provedor da API|R$ 50 – R$ 500+/mês + markup|
|Infraestrutura|Apenas se usar On-Premises|Variável|
|Chatbot / IA|Integração com automação|Variável|
### Exemplo prático de custo mensal

|Cenário|Volume|Tipo|Custo estimado|
|---|---|---|---|
|Campanha marketing|2.000 msgs|Marketing|~R$ 800|
|Notificações|3.000 msgs|Utilidade|~R$ 450|
|Atendimento|ilimitado|Service|R$ 0|
