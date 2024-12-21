from telethon import TelegramClient, events
from telethon.errors import RPCError, FloodWaitError
from colorama import Fore, Style, init
import logging
import random
import asyncio
import time

# Инициализация colorama
init(autoreset=True)

# Твои данные
API_ID = 29432999  # Укажите ваш API_ID
API_HASH = "0641784fe55901bbf031dbbd3f0db961"  # Укажите ваш API_HASH
SESSION_NAME = "userbot_session"  # Название сессии

# Настройка прокси (пример с SOCKS5)
proxy = ('socks5', 'proxy_address', proxy_port)  # Укажите адрес и порт прокси

# Создаём клиент с прокси
client = TelegramClient(SESSION_NAME, API_ID, API_HASH, proxy=proxy)

# Настройка логирования
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s",
    handlers=[
        logging.FileHandler("userbot.log"),
        logging.StreamHandler()
    ]
)

# Новый список фраз для комментариев
comment_phrases = [
    """Грузоперевозки по городу и области!\n\nИщете надёжные грузоперевозки? Мы готовы помочь вам в любое время! Рассчитаем стоимость вашей перевозки, учитывая все детали: маршрут, тип груза, вес и габариты.\n\nНаши тарифы:\n\nГАЗель (по городу): от 900 руб./час (минимум 2 часа).\nМежгород (более 200 км): от 40 руб./км.\nНовые территории: от 46 руб./км от места погрузки.\nУслуги грузчиков: от 500 руб./час.\n\nНаши преимущества:\n✔ Аккуратная погрузка и разгрузка.\n✔ Квалифицированные грузчики.\n✔ Чистый и вместительный кузов.\n✔ Работаем по городу, области и в других регионах.\n✔ Срочная подача автомобиля — круглосуточно.\n✔ Доступные цены.\n\nХарактеристики автомобиля:\n\nДлина: 4 м.\nВысота: 1,76 м.\nШирина: 1,80 м.\nГрузоподъёмность: до 2 тонн.\nЗвоните или пишите, и мы обсудим вашу заявку!""",
    """Профессиональные грузоперевозки для вас!\n\nМы предлагаем качественные и доступные услуги по перевозке грузов по городу и области. Готовы оперативно рассчитать стоимость и учесть все ваши пожелания.\n\nТарифы на наши услуги:\n\nГАЗель (по городу): от 900 руб./час, минимум 2 часа.\nМежгород (более 200 км): от 40 руб./км.\nНовые территории: от 46 руб./км (с места погрузки).\nГрузчики: от 500 руб./час.\n\nПочему выбирают нас:\n\nНадёжность: аккуратно перевезём ваши вещи, мебель или технику.\nКруглосуточная подача авто.\nПрофессиональная команда грузчиков.\nРаботаем не только по городу, но и в других регионах.\nГотовы к сотрудничеству с ИП.\n\nТехнические характеристики:\n\nДлина кузова: 4 м.\nВысота: 1,76 м.\nШирина: 1,80 м.\nГрузоподъёмность: до 2 тонн.\nСвяжитесь с нами удобным способом! Мы поможем с переездом и другими задачами.""",
]

# Задержка перед отправкой комментария
async def random_delay():
    delay = random.uniform(60, 300)  # Пауза от 1 до 5 минут с плавающей задержкой
    logging.info(f"Задержка перед отправкой комментария: {round(delay, 2)} секунд.")
    await asyncio.sleep(delay)

# Имитация набора текста
async def typing_simulation(chat_id):
    typing_duration = random.uniform(3, 15)  # Имитировать набор текста от 3 до 15 секунд
    logging.info(f"Имитируем набор текста в чате {chat_id} в течение {round(typing_duration, 2)} секунд.")
    await client.send_message(chat_id, ".", parse_mode="markdown")  # Отправить временное сообщение для имитации набора текста
    await asyncio.sleep(typing_duration)

# Отправка отчёта
async def send_report(success: bool, chat_title: str, message: str):
    report = "📝 **Отчёт по комментариям:**\n\n"
    if success:
        report += f"✅ Успешно добавлен комментарий в: {chat_title}\n"
    else:
        report += f"❌ Ошибка при добавлении комментария в: {chat_title}\n"
        report += f"Причина: {message}\n"
    try:
        logging.info(Fore.GREEN + "Отчёт: " + report)
    except Exception as e:
        logging.error(Fore.RED + f"Ошибка при отправке отчёта: {e}")# -
