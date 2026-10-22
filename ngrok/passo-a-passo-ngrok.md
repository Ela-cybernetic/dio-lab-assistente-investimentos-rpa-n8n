
# Como integrar Google Colab com n8n local usando ngrok

## Contexto

Neste projeto, o script Python foi executado no Google Colab enquanto o n8n estava rodando localmente através do Docker Desktop.

Como o Colab não consegue acessar o `localhost`, foi necessário utilizar o **ngrok** para criar um túnel público para o Webhook do n8n.

---

## Passo 1 - Criar uma conta no ngrok

Acesse:

https://ngrok.com

Crie uma conta gratuita.

---

## Passo 2 - Instalar o ngrok

No Windows, a instalação pode ser feita pela Microsoft Store.

Verifique se tudo está funcionando:

```cmd
ngrok version
```

---

## Passo 3 - Configurar o Authtoken

No painel do ngrok, copie o seu token e execute:

```cmd
ngrok config add-authtoken SEU_TOKEN
```

Exemplo:

```cmd
ngrok config add-authtoken 1a2b3c4d5e6f...
```

---

## Passo 4 - Verificar o n8n

Confirme que o n8n abre normalmente:

```text
http://localhost:5678
```

Se a interface do n8n abrir no navegador, a porta está funcionando corretamente.

---

## Passo 5 - Criar o túnel

Execute:

```cmd
ngrok http 5678
```

Exemplo de saída:

```text
Forwarding https://abc123.ngrok-free.app -> http://localhost:5678
```

Copie a URL pública gerada pelo ngrok.

---

## Passo 6 - Configurar o Webhook

No n8n:

```text
Webhook
↓
POST
↓
Path: Clientes
```

Clique em:

```text
Listen for Test Event
```

O Webhook ficará aguardando requisições.

---

## Passo 7 - Atualizar o Python

Troque:

```python
N8N_WEBHOOK = "http://localhost:5678/webhook/Clientes"
```

por:

```python
N8N_WEBHOOK = "https://abc123.ngrok-free.app/webhook-test/Clientes"
```

Agora o Google Colab conseguirá enviar dados para o n8n local através do túnel criado pelo ngrok.

---

# Problemas Comuns

## Erro 404

Verifique se o caminho do Webhook está correto.

Um erro comum é utilizar apenas a URL gerada pelo ngrok:

```text
https://abc123.ngrok-free.app
```

Sem incluir o endpoint do Webhook:

```text
/webhook-test/Clientes
```

Exemplo correto:

```text
https://abc123.ngrok-free.app/webhook-test/Clientes
```

Onde:

```text
URL_NGROK = https://abc123.ngrok-free.app

CAMINHO_WEBHOOK = /webhook-test/Clientes
```

Resultado:

```text
https://abc123.ngrok-free.app/webhook-test/Clientes
```

---

## ERR_NGROK_3200

O túnel foi encerrado.

Execute novamente:

```cmd
ngrok http 5678
```

---

## O túnel parou de funcionar

Verifique se a janela do Prompt de Comando com o ngrok ativo foi fechada.

Ao fechar a janela, o túnel é encerrado automaticamente e a URL pública deixa de funcionar.

🤣 Sim... eu descobri isso da maneira mais difícil.

---

## Resultado

Com o ngrok configurado corretamente, o fluxo fica assim:

```text
Google Colab
       ↓
      ngrok
       ↓
Webhook do n8n
       ↓
Processamento do Workflow
       ↓
Resposta personalizada
```

Dessa forma, foi possível integrar o ambiente remoto do Google Colab com uma instância local do n8n executada via Docker Desktop.
