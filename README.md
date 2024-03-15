import telebot
from telebot import types
import datetime
import schedule

# Инициализация бота
bot = telebot.TeleBot("6935776483:AAGO9LLhju23y_tS1BDTCMHUNp2-01zjDM8")

# Словарь с днями рождения сотрудников (фамилия и имя в качестве ключа, дата рождения в формате 'dd-mm')
birthdays = {
    'Проводникова Татьяна': '01-23',
    'Пичков Максим': '02-14',
    'Джагарян Диана': '02-15',
    'Трунькина Валерия': '02-19',
    'Иванов Григорий': '02-22',
    'Кузнецова Алена': '02-22',
    'Соловьев Максим': '02-24',
    'Орлов Георгий': '03-05',
    'Темченко София': '03-06',
    'Ефименко Ксения': '03-08',
    'Дорошенко Александр': '03-10',
    'Савостина Алина': '03-21',
    'Хуеплет Залупин': '03-22',
    'Петрова Анастасия': '03-24',
    'Сарафанова Юлия': '04-04',
    'Карлова Ангелина': '04-07',
    'Пантина Ольга': '04-20',
    'Папуця Анна': '05-21',
    'Зайцев Леонид': '05-25',
    'Евсюкова Мария': '06-06',
    'Матвеева Яна': '06-07',
    'Аваков Илья': '06-08',
    'Мороз Дарья': '06-14',
    'Бондарева Дарья': '06-16',
    'Алексеева Алина': '06-24',
    'Чугай Владимир': '06-26',
    'Каржан Юлия': '07-07',
    'Дворникова Екатерина': '07-26',
    'Плюснина Полина': '07-30',
    'Голованова Кристина': '08-06',
    'Макеева Дарья': '08-25',
    'Венцель Алена': '08-26',
    'Ролдугина Марьяна': '08-27',
    'Новикова Ирина': '08-27',
    'Козенцов Владимир': '08-29',
    'Мамина Инга': '08-30',
    'Михалицина Анастасия': '08-31',
    'Садекова Юлия': '09-04',
    'Карась Елизавета': '09-17',
    'Лемешкина Екатерина': '09-26',
    'Кочебин Владислав': '10-02',
    'Пашкова Дарья': '10-26',
    'Пугачев Максим': '10-31',
    'Яковлева Ольга': '12-05',
    'Климова Виктория': '12-08',

}

# Список chat_id для отправки напоминаний
reminder_chat_ids = [5445191637, 243886935]  # Добавьте сюда нужные chat_id

# Словарь для хранения винительных форм имен и фамилий сотрудников
accusatives = {

    'Проводникова Татьяна': 'Проводниковой Татьяны',
    'Пичков Максим': 'Пичкова Максима',
    'Джагарян Диана': 'Джагарян Дианы',
    'Трунькина Валерия': 'Трунькиной Валерии',
    'Иванов Григорий': 'Иванова Григория',
    'Кузнецова Алена': 'Кузнецовой Алены',
    'Соловьев Максим': 'Соловьева Максима',
    'Орлов Георгий': 'Орлова Георгия',
    'Темченко София': 'Темченко Софии',
    'Ефименко Ксения': 'Ефименко Ксении',
    'Дорошенко Александр': 'Дорошенко Александра',
    'Савостина Алина': 'Савостиной Алины',
    'Хуеплет Залупин': 'Хуеплета Залупина',
    'Петрова Анастасия': 'Петровой Анастасии',
    'Сарафанова Юлия': 'Сарафановой Юлии',
    'Карлова Ангелина': 'Карловой Ангелины',
    'Пантина Ольга': 'Пантиной Ольги',
    'Папуця Анна': 'Папуця Анны',
    'Зайцев Леонид': 'Зайцева Леонида',
    'Евсюкова Мария': 'Евсюковой Марии',
    'Матвеева Яна': 'Матвеевой Яны',
    'Аваков Илья': 'Авакова Ильи',
    'Мороз Дарья': 'Мороз Дарьи',
    'Бондарева Дарья': 'Бондаревой Дарьи',
    'Алексеева Алина': 'Алексеевой Алины',
    'Чугай Владимир': 'Чугай Владимира',
    'Каржан Юлия': 'Каржан Юлии',
    'Дворникова Екатерина': 'Дворниковой Екатерины',
    'Плюснина Полина': 'Плюсниной Полины',
    'Голованова Кристина': 'Головановой Кристины',
    'Макеева Дарья': 'Макеевой Дарьи',
    'Венцель Алена': 'Венцель Алены',
    'Ролдугина Марьяна': 'Ролдугиной Марьяны',
    'Новикова Ирина': 'Новиковой Ирины',
    'Козенцов Владимир': 'Козенцова Владимира',
    'Мамина Инга': 'Маминой Инги',
    'Михалицина Анастасия': 'Михалициной Анастасии',
    'Садекова Юлия': 'Садековой Юлии',
    'Карась Елизавета': 'Карась Елизаветы',
    'Лемешкина Екатерина': 'Лемешкиной Екатерины',
    'Кочебин Владислав': 'Кочебина Владислава',
    'Пашкова Дарья': 'Пашковой Дарьи',
    'Пугачев Максим': 'Пугачева Максима',
    'Яковлева Ольга': 'Яковлевой Ольги',
    'Климова Виктория': 'Климовой Виктории',

}

# Функция для преобразования даты рождения в текстовое представление
def birthday_to_words(birthday_str):
    birthday = datetime.datetime.strptime(birthday_str, '%m-%d')
    month_name = birthday.strftime('%B')  # Получаем название месяца
    month_name = month_name.lower().capitalize()  # Преобразуем первую букву в верхний регистр
    month_name = month_name.replace("January", "января").replace("February", "февраля").replace("March", "марта") \
        .replace("April", "апреля").replace("May", "мая").replace("June", "июня").replace("July", "июля") \
        .replace("August", "августа").replace("September", "сентября").replace("October", "октября") \
        .replace("November", "ноября").replace("December", "декабря")  # Переводим на русский язык
    return f'{birthday.day} {month_name}'


# Обработчик команды /start
@bot.message_handler(commands=['start'])
def handle_start(message):
    # Создаем клавиатуру с двумя кнопками
    keyboard = types.ReplyKeyboardMarkup(row_width=1, resize_keyboard=True)
    button1 = types.KeyboardButton('День рождения коллеги')
    button2 = types.KeyboardButton('Дни рождения в этом месяце')
    keyboard.add(button1, button2)

    # Текст с описанием команд и их функционала
    text = ("Привет! Я бот, который поможет тебе узнать информацию о днях рождения твоих коллег.\n\n"
            "Вот список доступных команд:\n\n"
            "<b>День рождения коллеги</b> - узнать день рождения конкретного сотрудника\n"
            "<b>Дни рождения в этом месяце</b> - получить список именинников в текущем месяце\n\n"
            "А также я напомню заранее о дне рождения сотрудника, поэтому не блокируй меня и не убирай звук!\n\n"
            "Просто выбери интересующую тебя команду из списка ниже:")

    # Отправляем приветственное сообщение с клавиатурой и описанием команд
    bot.send_message(message.chat.id, text, reply_markup=keyboard, parse_mode='HTML')

# Обработчик кнопки "День рождения коллеги"
@bot.message_handler(func=lambda message: message.text.lower() == 'день рождения коллеги')
def handle_birthday_colleague(message):
    # Спрашиваем у пользователя фамилию и имя сотрудника
    bot.send_message(message.chat.id, "Введите фамилию и имя коллеги:")


# Обработчик кнопки "Дни рождения в этом месяце"
@bot.message_handler(func=lambda message: message.text.lower() == 'дни рождения в этом месяце')
def handle_birthday_month(message):
    current_month = datetime.datetime.now().strftime('%m')
    birthday_list = [name for name, birthday in birthdays.items() if birthday.startswith(current_month)]

    if birthday_list:
        reply = "В этом месяце день рождения празднуют:\n"
        for name in birthday_list:
            birthday_text = birthday_to_words(birthdays[name])
            reply += f"<b>{name}</b> — {birthday_text}\n"
        bot.send_message(message.chat.id, reply, parse_mode='HTML')
    else:
        bot.send_message(message.chat.id, "В этом месяце у нас нет именинников.")


# Обработчик текстовых сообщений для запроса дня рождения коллеги
@bot.message_handler(func=lambda message: True)
def handle_birthday_request(message):
    if message.text.lower() == 'день рождения коллеги':
        return  # Игнорируем сообщение, если оно было отправлено в ответ на команду "День рождения коллеги"

    # Получаем введенную фамилию и имя
    full_name = message.text.strip().title()  # Приводим к формату "Имя Фамилия"

    # Ищем день рождения сотрудника
    if full_name in birthdays:
        birthday_text = birthday_to_words(birthdays[full_name])
        # Преобразуем фамилию и имя в винительный падеж
        accusative_full_name = accusatives.get(full_name, full_name)
        reply = f"День рождения у <b>{accusative_full_name}</b> — {birthday_text}"
        bot.send_message(message.chat.id, reply, parse_mode='HTML')
    else:
        bot.send_message(message.chat.id, "Извините, я не нашел информацию о таком сотруднике.")


# Функция для преобразования даты рождения в текстовое представление
def birthday_to_words(birthday_str):
    birthday = datetime.datetime.strptime(birthday_str, '%m-%d')
    month_name = birthday.strftime('%B')  # Получаем название месяца
    month_name = month_name.lower().capitalize()  # Преобразуем первую букву в верхний регистр
    month_name = month_name.replace("January", "января").replace("February", "февраля").replace("March", "марта") \
        .replace("April", "апреля").replace("May", "мая").replace("June", "июня").replace("July", "июля") \
        .replace("August", "августа").replace("September", "сентября").replace("October", "октября") \
        .replace("November", "ноября").replace("December", "декабря")  # Переводим на русский язык
    return f'{birthday.day} {month_name}'


# Функция для преобразования даты рождения в текстовое представление
def birthday_to_words(birthday_str):
    birthday = datetime.datetime.strptime(birthday_str, '%m-%d')
    month_name = birthday.strftime('%B')  # Получаем название месяца
    month_name = month_name.lower().capitalize()  # Преобразуем первую букву в верхний регистр
    month_name = month_name.replace("January", "января").replace("February", "февраля").replace("March", "марта") \
        .replace("April", "апреля").replace("May", "мая").replace("June", "июня").replace("July", "июля") \
        .replace("August", "августа").replace("September", "сентября").replace("October", "октября") \
        .replace("November", "ноября").replace("December", "декабря")  # Переводим на русский язык
    return f'{birthday.day} {month_name}'

# Функция для отправки напоминания о дне рождения
def send_birthday_reminder():
    today = datetime.datetime.now()
    print("Checking birthday reminders at:", today)  # Добавим эту строку для отладки
    # Проверяем дни рождения сотрудников
    for name, birthday in birthdays.items():
        birth_date = datetime.datetime.strptime(birthday, '%m-%d').replace(year=today.year)
        # Вычисляем дату напоминания (за неделю до дня рождения)
        reminder_date = birth_date - datetime.timedelta(days=7)
        # Проверяем, что напоминание должно быть отправлено сегодня в 10:30
        if today.date() == reminder_date.date() and today.hour == 10 and today.minute == 30:
            print("Sending reminder for", name)  # Добавим эту строку для отладки
            # Преобразуем фамилию и имя в винительный падеж
            accusative_full_name = accusatives.get(name, name)
            # Формируем текст напоминания с новым форматом времени
            reminder_message = (f"Привет! {accusative_full_name} отмечает день рождения через неделю, "
                                f"не забудь начать сбор на подарок! 🎁🎉🎈"
                                f"\n\nНапоминание отправлено в {today.strftime('%H:%M')}.")
            # Отправляем напоминание каждому chat_id из списка
            for chat_id in reminder_chat_ids:
                bot.send_message(chat_id, reminder_message)

# Регулярная проверка напоминаний о днях рождения
schedule.every().hour.do(send_birthday_reminder)

# Запускаем бота
bot.polling(none_stop=True)
