import openai
from telegram import Update
from telegram.ext import Updater, CommandHandler, MessageHandler, Filters, CallbackContext
from queue import Queue

# Устанавливаем ваш OpenAI API ключ
openai.api_key = 'ваш-ключ-api'

# Очередь для обработки запросов
request_queue = Queue()

# Обработчик команды /start
def start(update: Update, context: CallbackContext) -> None:
    update.message.reply_text('Привет! Отправь мне описание, и я создам изображение для тебя.')

# Обработчик текстовых сообщений
def handle_text(update: Update, context: CallbackContext) -> None:
    # Получаем текст сообщения
    user_input = update.message.text

    # Добавляем запрос в очередь
    request_queue.put((update.message.chat_id, user_input))

# Функция обработки очереди
def process_queue():
    while True:
        if not request_queue.empty():
            # Извлекаем запрос из очереди
            chat_id, user_input = request_queue.get()

            # Используем OpenAI API для генерации изображения на основе текста
            response = openai.Completion.create(
                engine="text-davinci-002",
                prompt=user_input,
                n=1,
                stop=None,
                temperature=0.7,
            )

            # Получаем URL сгенерированного изображения
            image_url = response['choices'][0]['text']

            # Отправляем изображение пользователю
            updater.bot.send_photo(chat_id=chat_id, photo=image_url)

# Главная функция
def main():
    # Создаем Updater и передаем ему токен вашего бота
    updater = Updater("ваш-токен-бота")

    # Получаем диспетчер для регистрации обработчиков
    dp = updater.dispatcher

    # Регистрируем обработчики
    dp.add_handler(CommandHandler("start", start))
    dp.add_handler(MessageHandler(Filters.text & ~Filters.command, handle_text))

    # Запускаем бота
    updater.start_polling()

    # Запускаем цикл обработки сообщений
    updater.idle()

if __name__ == '__main__':
    main()
