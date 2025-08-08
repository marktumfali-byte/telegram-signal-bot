import random
import schedule
import time
import datetime
from telegram import Bot

# ==== Настройки бота ====
TOKEN = "8162965975:AAHclXgLYZ-nKTxi2VB-ihfFNbozYuCin6A"
CHANNEL_ID = "@Pvtbinary_bott"

bot = Bot(token=TOKEN)

def random_direction():
    return random.choice(["🟢 ВВЕРХ", "🔴 ВНИЗ"])

def random_expiration():
    return f"{random.randint(1, 5)} минут"

def send_signals():
    now = datetime.datetime.now()
    day_of_week = now.weekday()

    if day_of_week < 5:
        eurusd_message = (
            f"🔹 Актив: EUR/USD\n"
            f"🔹 Направление: {random_direction()}\n"
            f"⏱️ Экспирация: {random_expiration()}"
        )
        bot.send_message(chat_id=CHANNEL_ID, text=eurusd_message)

    binidx_message = (
        f"🔹 Актив: BIN/IDX\n"
        f"🔹 Направление: {random_direction()}\n"
        f"⏱️ Экспирация: {random_expiration()}"
    )
    bot.send_message(chat_id=CHANNEL_ID, text=binidx_message)

    print(f"[{now.strftime('%H:%M')}] Сигналы отправлены")

for hour in range(10, 20):
    schedule.every().day.at(f"{hour:02d}:00").do(send_signals)
    schedule.every().day.at(f"{hour:02d}:30").do(send_signals)

if __name__ == "__main__":
    print("✅ Бот запущен. Ожидает время сигналов...")
    while True:
        schedule.run_pending()
        time.sleep(1)

