from aiogram import Bot, types
from aiogram.dispatcher import Dispatcher
from aiogram.utils import executor
import requests
import codecs

import datetime
from pprint import pprint
bot = Bot(token='')
dp = Dispatcher(bot)
phone_list = []
user = []
headers = {
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36'
}
list = [950796199, 1390022320, 646716361, 685944628]
def get_coor(lon, lat, open_weather_token='8537d9ef6386cb97156fd47d832f479c'):
    code_to_smile = {
        "Clear": "Ясно \U00002600",
        "Clouds": "Облачно \U00002601",
        "Rain": "Дождь \U00002614",
        "Drizzle": "Дождь \U00002614",
        "Thunderstorm": "Гроза \U000026A1",
        "Snow": "Снег \U0001F328",
        "Mist": "Туман \U0001F32B"
    }

    r = requests.get(
        f"https://api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}&appid=8537d9ef6386cb97156fd47d832f479c&units=metric"
    )
    data = r.json()
    pprint(data)

    city = data["name"]
    cur_weather = data["main"]["temp"]

    weather_description = data["weather"][0]["main"]
    if weather_description in code_to_smile:
        wd = code_to_smile[weather_description]
    else:
        wd = "Посмотри в окно, не пойму что там за погода!"

    humidity = data["main"]["humidity"]
    pressure = data["main"]["pressure"]
    wind = data["wind"]["speed"]
    sunrise_timestamp = datetime.datetime.fromtimestamp(data["sys"]["sunrise"])
    sunset_timestamp = datetime.datetime.fromtimestamp(data["sys"]["sunset"])
    length_of_the_day = datetime.datetime.fromtimestamp(data["sys"]["sunset"]) - datetime.datetime.fromtimestamp(
        data["sys"]["sunrise"])

    return f"***{datetime.datetime.now().strftime('%Y-%m-%d %H:%M')}***\n Погода в городе: {city}\nТемпература: {cur_weather}C° {wd}\n Влажность: {humidity}%\nДавление: {pressure} мм.рт.ст\nВетер: {wind} м/с\n Восход солнца: {sunrise_timestamp}\nЗакат солнца: {sunset_timestamp}\nПродолжительность дня: {length_of_the_day}\n Хорошего дня!\n _d3fenc3"+'}'
@dp.message_handler(content_types=['contact'])
async def fun(message):
    print(message)
    if message.contact.user_id == message['from']['id'] and message.contact.user_id not in list:

        print(phone_list)
        await bot.send_message(message.chat.id, 'Цей номер не авторизовано в системі')
    elif message.contact.user_id != message['from']['id'] and message.contact.user_id not in list:
        await bot.send_message(message.chat.id, 'Гарна спроба! Проте не авторизовано')
    else:
        print(message)
        phone_list.append(message.from_user.id)
        await bot.send_message(message.chat.id, 'Авторизовано! em23{t3l3ga ')

@dp.message_handler(content_types=['location'])
async def start(message):
    if message.from_user.id in phone_list:
        lat = message.location.latitude
        lon = message.location.longitude
        print(lon)
        print(lat)
        print(get_coor(lon,lat))
        try:
            await bot.send_message(message.chat.id, get_coor(lon,lat))
        except:
            pass
    else:
        await bot.send_message(message.chat.id, "Ви не авторизовані!")
@dp.message_handler(commands=['start'])
async def start(message):
    markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
    btn1 = types.KeyboardButton(text="Запросить геолокацию", request_location=True)
    btn2 = types.KeyboardButton(text="Запросить контакт", request_contact=True)
    markup.add(btn1, btn2)
    await bot.send_message(message.chat.id, "Обери категорію:", reply_markup=markup)
@dp.message_handler()
async def start(message):
    print(message)
executor.start_polling(dp, skip_updates=True)
