import telebot
import random
import os
from telebot import types

# توکن ربات خود را اینجا قرار دهید
BOT_TOKEN = os.getenv('BOT_TOKEN', 'YOUR_BOT_TOKEN_HERE')

bot = telebot.TeleBot(BOT_TOKEN)

# کارت های تاروت و معنی آنها
tarot_cards = {
    "احمق (The Fool)": "شروع جدید، ماجراجویی، بی‌پروایی مثبت، آزادی از ترس",
    "جادوگر (The Magician)": "قدرت اراده، خلاقیت، مهارت، توانایی تحقق آرزوها",
    "کاهنه عالی (High Priestess)": "شهود، حکمت درونی، اسرار، قدرت زنانه",
    "امپراتورس (The Empress)": "مادری، حاصلخیزی، طبیعت، عشق بی‌قید و شرط",
    "امپراتور (The Emperor)": "اقتدار، رهبری، نظم، پدر سالار",
    "کشیش (The Hierophant)": "سنت، آموزش، راهنمایی معنوی، دانش مذهبی",
    "عاشقان (The Lovers)": "عشق، انتخاب، رابطه، هماهنگی",
    "ارابه (The Chariot)": "پیروزی، اراده قوی، کنترل، حرکت به جلو",
    "قدرت (Strength)": "شجاعت درونی، کنترل خویشتن، صبر، قدرت ملایم",
    "درونگرا (The Hermit)": "جستجوی درونی، خلوت، حکمت، راهنمایی",
    "چرخ بخت (Wheel of Fortune)": "تغییر، شانس، سرنوشت، چرخه زندگی",
    "عدالت (Justice)": "توازن، انصاف، قانون، عواقب اعمال",
    "آویخته (The Hanged Man)": "قربانی، نگاه متفاوت، صبر، رهایی",
    "مرگ (Death)": "تحول، پایان و شروع، تغییر بنیادی، تولد دوباره",
    "اعتدال (Temperance)": "تعادل، میانه‌روی، آرامش، هماهنگی",
    "شیطان (The Devil)": "وابستگی، محدودیت، وسوسه، مادی‌گرایی",
    "برج (The Tower)": "تغییر ناگهانی، ویرانی، آشفتگی، بیداری",
    "ستاره (The Star)": "امید، الهام، آرزو، شفا",
    "ماه (The Moon)": "توهم، ترس، ناآگاهی، رؤیا",
    "خورشید (The Sun)": "شادی، موفقیت، حیاتی، مثبت اندیشی",
    "قضاوت (Judgement)": "رستگاری، بخشش، بیداری، شروع دوباره",
    "جهان (The World)": "تکمیل، موفقیت، تحقق هدف، کمال"
}

# پیام‌های انگیزشی
motivational_messages = [
    "✨ یادت باشه که هر پایانی، شروع جدیدی هست",
    "🌟 قدرت تغییر زندگیت توی دستان خودته",
    "💫 به شهودت اعتماد کن، راه درست رو نشونت میده",
    "🌙 گاهی باید صبر کنی تا زمان مناسب فرا برسه",
    "☀️ هر چالشی فرصتی برای رشد و یادگیری است"
]

@bot.message_handler(commands=['start'])
def send_welcome(message):
    welcome_text = """
🔮 سلام! به ربات فال تاروت خوش اومدی! 

برای گرفتن فال تک نیت، کافیه سوالت رو بپرسی یا از دستورات زیر استفاده کنی:

/tarot - فال تک نیت تاروت
/help - راهنما

✨ سوالت رو با تمرکز بپرس تا بهترین پاسخ رو بگیری
    """
    bot.reply_to(message, welcome_text)

@bot.message_handler(commands=['help'])
def send_help(message):
    help_text = """
📚 راهنمای استفاده:

🔮 /tarot - دریافت فال تک نیت
❓ /help - نمایش این راهنما

نحوه استفاده:
1️⃣ سوالت رو با دقت فکر کن
2️⃣ دستور /tarot رو بزن
3️⃣ کارت تاروت و تفسیرش رو دریافت کن

💡 نکته: سوالات واضح و مشخص بهترین پاسخ رو میدن!
    """
    bot.reply_to(message, help_text)

@bot.message_handler(commands=['tarot'])
def tarot_reading(message):
    # انتخاب تصادفی کارت
    card_name = random.choice(list(tarot_cards.keys()))
    card_meaning = tarot_cards[card_name]
    motivational_msg = random.choice(motivational_messages)
    
    # ساخت پیام فال
    reading_text = f"""
🔮 **فال تک نیت شما:**

🎴 **کارت:** {card_name}

📖 **تفسیر:** 
{card_meaning}

{motivational_msg}

━━━━━━━━━━━━━━━━
💫 این فال راهنمایی کلی است و تصمیمات مهم باید با فکر و بررسی گرفته شود
    """
    
    # ایجاد کیبورد برای فال جدید
    markup = types.InlineKeyboardMarkup()
    new_reading_btn = types.InlineKeyboardButton("🔄 فال جدید", callback_data="new_tarot")
    markup.add(new_reading_btn)
    
    bot.reply_to(message, reading_text, parse_mode='Markdown', reply_markup=markup)

@bot.callback_query_handler(func=lambda call: call.data == "new_tarot")
def new_tarot_reading(call):
    # انتخاب کارت جدید
    card_name = random.choice(list(tarot_cards.keys()))
    card_meaning = tarot_cards[card_name]
    motivational_msg = random.choice(motivational_messages)
    
    reading_text = f"""
🔮 **فال جدید شما:**

🎴 **کارت:** {card_name}

📖 **تفسیر:** 
{card_meaning}

{motivational_msg}

━━━━━━━━━━━━━━━━
💫 این فال راهنمایی کلی است و تصمیمات مهم باید با فکر و بررسی گرفته شود
    """
    
    markup = types.InlineKeyboardMarkup()
    new_reading_btn = types.InlineKeyboardButton("🔄 فال جدید", callback_data="new_tarot")
    markup.add(new_reading_btn)
    
    bot.edit_message_text(reading_text, call.message.chat.id, call.message.message_id, 
                         parse_mode='Markdown', reply_markup=markup)

@bot.message_handler(func=lambda message: True)
def handle_message(message):
    # پاسخ به پیام‌های عادی
    responses = [
        "🔮 برای دریافت فال تاروت، دستور /tarot رو بزن!",
        "✨ آماده‌ای برای کشف آینده؟ /tarot رو امتحان کن!",
        "🌟 فال تاروت منتظرته! فقط /tarot بزن"
    ]
    
    bot.reply_to(message, random.choice(responses))

if __name__ == "__main__":
    print("ربات تاروت شروع شد...")
    bot.infinity_polling()