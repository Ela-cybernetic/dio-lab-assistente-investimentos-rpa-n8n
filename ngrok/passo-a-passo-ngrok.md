
# Como integrar Google Colab com n8n local usando ngrok

Neste projeto, o script Python foi executado no Google Colab enquanto o n8n estava rodando localmente através do Docker Desktop.

Como o Colab não consegue acessar localhost, foi necessário utilizar o ngrok para criar um túnel público para o Webhook do n8n.

## Passo 1 - Criar uma conta no ngrok

Acesse:

(https://ngrok.com)

Crie uma conta gratuita.

## Passo 2 - Instalar o ngrok

No Windows, a instalação pode ser feita pela Microsoft Store.

Verifique se tudo está funcionando:

ngrok version

## Passo 3 - Configurar o Authtoken

No painel do ngrok, copie o seu token e execute:

ngrok config add-authtoken SEU_TOKEN

## Passo 4 - Verificar o n8n

Confirme que o n8n abre normalmente:

http://localhost:5678

## Passo 5 - Criar o túnel

Execute:

ngrok http 5678


Exemplo de saída:

Forwarding
https://abc123.ngrok-free.app -> http://localhost:5678

## Passo 6 - Configurar o Webhook

No n8n:

Webhook
↓
POST
↓
Path: Clientes


Clique em:

Listen for Test Event

## Passo 7 - Atualizar o Python 

Troque:

http://localhost:5678/webhook/Clientes


por:

https://abc123.ngrok-free.app/webhook-test/Clientes

## Problemas Comuns
### Erro 404

Verifique se o caminho do webhook está correto. Atente-se que ao copiar a URL_NGROK você tem que adicionar o caminho do webhook.
Exemplo:
https://abc123.ngrok-free.app/webhook-test/Clientes
URL_NGROK = https://abc123.ngrok-free.app
caminho para o webhook = /webhook-test/Clientes

### ERR_NGROK_3200

O túnel foi encerrado.

Execute novamente:

ngrok http 5678

O túnel parou de funcionar

Verifique se a janela de comando com o ngrok ativo foi fechada.

🤣 Sim, eu descobri isso da maneira mais difícil.


