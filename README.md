## Implementação do Desafio Completo

### RPA com Python

O notebook `rpa/extrair_clientes.ipynb` utiliza Python, Requests e BeautifulSoup para acessar a página de clientes, extrair nome, e-mail, saldo e perfil de investidor e enviar os dados ao Webhook do N8N via requisição POST.

### Workflow N8N

O workflow está disponível em `n8n/workflow.json` e realiza as seguintes etapas:

1. Recebe os clientes pelo Webhook.
2. Consulta o arquivo `docs/data.csv` com as opções de investimento.
3. Processa e cruza os investimentos com o perfil de cada cliente.
4. Processa cada cliente individualmente com o `Loop Over Items`.
5. Envia os dados para o modelo Google Gemini.
6. Organiza a resposta gerada pela IA.
7. Valida o formato do endereço de e-mail com um nó IF usando expressão regular.
8. Envia a mensagem personalizada pelo Gmail.
9. Retorna ao Loop para processar o próximo cliente e finaliza pelo `Respond to Webhook`.

### Integração com IA Generativa

Foi utilizado o nó Google Gemini do N8N para gerar mensagens personalizadas. O prompt recebe o nome, saldo, perfil e investimentos compatíveis do cliente e orienta o modelo a gerar uma mensagem clara, sem inventar informações e identificando o conteúdo como uma simulação educacional.

### Decisões técnicas

- **BeautifulSoup:** utilizado para extrair os dados da tabela HTML da página de clientes.
- **Webhook:** utilizado como ponto de entrada para integrar o script Python ao N8N.
- **HTTP Request:** utilizado para obter o CSV de investimentos hospedado no GitHub Pages.
- **Merge + Code:** utilizados para combinar os dados dos clientes com os investimentos e preparar os itens para o restante do fluxo.
- **Loop Over Items:** configurado para processar um cliente por vez e controlar o ritmo das chamadas ao modelo de IA.
- **Google Gemini:** utilizado para realizar a geração dinâmica das mensagens no desafio completo.
- **IF + Regex:** utilizado para validar o formato do endereço antes do envio.
- **Gmail:** utilizado para realizar o envio das mensagens personalizadas.

### Validação

O workflow foi testado com os 10 clientes da página. O fluxo recebeu os clientes pelo Webhook, processou-os individualmente, gerou mensagens com o Gemini e executou os envios pelo Gmail.

Em um teste utilizando um endereço de e-mail real como destinatário, as 10 mensagens foram recebidas com sucesso.

> A validação do nó IF verifica o formato do e-mail; ela não confirma a existência de uma caixa postal.

## Entregáveis

### MVP

- [x] Repositório forkado com o workflow N8N implementado
- [x] Workflow N8N exportado em `n8n/workflow.json`
- [x] Script de RPA integrado ao Webhook do N8N
- [x] Fluxo testado de ponta a ponta

### Desafio Completo

- [x] Todos os itens do MVP
- [x] Integração com IA Generativa no N8N
- [x] Mensagens geradas dinamicamente via LLM
- [x] Documentação das decisões técnicas
