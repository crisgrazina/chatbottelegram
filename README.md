# chatbottelegram

# Como Criar um Chatbot no Telegram com Respostas Personalizadas

Este tutorial mostrará como criar um chatbot simples no Telegram que responde a comandos específicos e palavras-chave, além de enviar imagens automaticamente.

---

## 1️⃣ Pré-requisitos

Antes de começar, certifique-se de ter:
- Conta no **Telegram**.
- Conta no **Replit** (para hospedar o bot).
- Conhecimento básico de Python (opcional, mas ajuda).
- **BotFather** no Telegram (bot oficial para gerenciar outros bots).

---

## 2️⃣ Criando o Bot no BotFather

1. No Telegram, procure por **BotFather** e clique em **START**.
2. Envie o comando:
/newbot

3. Siga as instruções para configurar nome e username do bot.
4. Após a configuração, o BotFather fornecerá um **token de API**. **Guarde esse token**, pois ele será usado no código.

---

## 3️⃣ Configurando o Replit

1. Acesse o [Replit](https://replit.com) e faça login.
2. Crie um novo projeto:
- Clique em **Create**.
- Selecione **Python** como template.
- Nomeie o projeto como desejar (ex.: `telegram-bot`).
3. Configure o token como variável de ambiente:
- Clique em **Tools > Secrets**.
- Adicione:
  - **KEY**: `BOT_TOKEN`
  - **VALUE**: Cole o token fornecido pelo BotFather.

---

## 4️⃣ Instalando a Biblioteca Necessária

1. No Shell do Replit, execute o seguinte comando:
```bash
pip install python-telegram-bot==20.3
Aguarde a instalação ser concluída.

5️⃣ Criando o Código Inicial
No arquivo principal do projeto (main.py), cole o seguinte código básico:

python

from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes
import os

# Obter o token de ambiente
TOKEN = os.getenv("BOT_TOKEN")

# Função para o comando /start
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text("Olá! Eu sou o ChatBot do Cris Grazina!")

# Configuração do bot
app = ApplicationBuilder().token(TOKEN).build()

# Adicionar o comando /start ao bot
app.add_handler(CommandHandler("start", start))

# Iniciar o bot

if __name__ == "__main__":
    app.run_polling()
6️⃣ Testando o Bot
Clique em Run no Replit.
No Telegram, abra o bot pelo username configurado no BotFather.
Envie o comando /start. O bot deve responder com:
arduino
Copiar código
Olá! Eu sou o ChatBot do Cris Grazina!
7️⃣ Adicionando Funcionalidades
Agora vamos adicionar respostas personalizadas para comandos e palavras-chave.

7.1 Comando /sobre
Adicione a função para o comando /sobre:

python
Copiar código
async def sobre(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text("Este é um ChatBot para teste do vídeo no Canal Graziverso")
Registre o comando no bot:

python
Copiar código
app.add_handler(CommandHandler("sobre", sobre))
7.2 Responder à Palavra-chave "Olá"
Adicione uma função para capturar mensagens com "Olá":

python
Copiar código
async def responder_ola(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    mensagem = update.message.text.lower()
    if "olá tudo bem" in mensagem or mensagem == "olá":
        await update.message.reply_text("Olá, como vai você? Seja bem-vindo(a) ao nosso ChatBot")
Registre um MessageHandler:

python
Copiar código
app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND & filters.Regex(".*olá.*"), responder_ola))
7.3 Resposta Padrão
Adicione uma função para mensagens não reconhecidas:

python
Copiar código
async def resposta_padrao(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text(
        "Desculpe, estou ainda em treinamento. Não entendi seu pedido. Algo mais no que eu possa ajudar?"
    )
Registre o MessageHandler para mensagens genéricas:

python
Copiar código
app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, resposta_padrao))
7.4 Enviar Imagem ao Detectar "imagem"
Adicione uma função para enviar imagens:

python
Copiar código
async def enviar_imagem(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    url_imagem = "https://via.placeholder.com/512"
    await context.bot.send_photo(chat_id=update.effective_chat.id, photo=url_imagem)
    await update.message.reply_text("Aqui está a imagem que você pediu!")
Registre o handler para "imagem":

python
Copiar código
app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND & filters.Regex(".*imagem.*"), enviar_imagem))
8️⃣ Código Final
Aqui está o código completo com todas as funcionalidades:

python
Copiar código
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, filters, ContextTypes
import os

# Obter o token de ambiente
TOKEN = os.getenv("BOT_TOKEN")

# Função para o comando /start
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text("Olá! Eu sou o ChatBot do Cris Grazina!")

# Função para o comando /sobre
async def sobre(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text("Este é um ChatBot para teste do vídeo no Canal Graziverso")

# Função para responder à palavra-chave "Olá" ou "Olá tudo bem"
async def responder_ola(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    mensagem = update.message.text.lower()
    if "olá tudo bem" in mensagem or mensagem == "olá":
        await update.message.reply_text("Olá, como vai você? Seja bem-vindo(a) ao nosso ChatBot")

# Função para enviar uma imagem
async def enviar_imagem(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    url_imagem = "https://via.placeholder.com/512"
    await context.bot.send_photo(chat_id=update.effective_chat.id, photo=url_imagem)
    await update.message.reply_text("Aqui está a imagem que você pediu!")

# Função para lidar com mensagens não reconhecidas
async def resposta_padrao(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text(
        "Desculpe, estou ainda em treinamento. Não entendi seu pedido. Algo mais no que eu possa ajudar?"
    )

# Configuração do bot
app = ApplicationBuilder().token(TOKEN).build()

# Adicionar os comandos e handlers
app.add_handler(CommandHandler("start", start))
app.add_handler(CommandHandler("sobre", sobre))
app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND & filters.Regex(".*olá.*"), responder_ola))
app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND & filters.Regex(".*imagem.*"), enviar_imagem))
app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, resposta_padrao))

# Iniciar o bot
if __name__ == "__main__":
    app.run_polling()
