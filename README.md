from telegram import Bot, Update
from telegram.ext import Updater, MessageHandler, Filters
import logging

TOKEN = "7572434536:AAHreG5ISHqDwLpoe8Wk6TCCRwZf7YsusAw"
ADMIN_CHAT_ID = 5539061712

# Logging setup
logging.basicConfig(format="%(asctime)s - %(name)s - %(levelname)s - %(message)s", level=logging.INFO)

bot = Bot(token=TOKEN)

# Function to send welcome message and notify admin
def welcome(update: Update, context):
    user = update.message.new_chat_members[0]
    name = user.first_name
    user_id = user.id
    chat_id = update.message.chat_id

    welcome_text = f"Welcome {name} to our group! 🎉"
    update.message.reply_text(welcome_text)

    # First personal message
    pm_text = f"""Hello {name}, 👋  
🔹 Features of **25 Rs Mod:**  
🟠 Esp  
🟠 Aimbot  
🟠 Hide Esp  
🟠 No ban  
🟠 No crash  

🔥 **Lifetime Access | Cracked Paid Mod** 🔥  

💰 Want to buy? Price: **₹25**  
Contact: [@Rajeshchaudhary_0](https://t.me/Rajeshchaudhary_0)
"""
    
    # Second personal message explaining price
    reason_text = f"""🤔 **Why is this mod so cheap?**  
🔹 This is a **cracked version of premium paid mods**, so you are getting the same features for just ₹25 instead of ₹1000+!  
🚀 **Grab it now before the price goes up!**  

💰 **Buy Now:** [@Rajeshchaudhary_0](https://t.me/Rajeshchaudhary_0)
"""
    
    try:
        bot.send_message(chat_id=user_id, text=pm_text, parse_mode="Markdown")
        bot.send_message(chat_id=user_id, text=reason_text, parse_mode="Markdown")

        # Notify admin about the new user and sent messages
        admin_msg = f"""🔔 **New User Joined:**  
👤 Name: {name}  
🆔 User ID: {user_id}  

✅ Bot sent messages:  
1️⃣ Mod Features  
2️⃣ Price Explanation
"""
        bot.send_message(chat_id=ADMIN_CHAT_ID, text=admin_msg)

    except Exception as e:
        logging.error(f"Error sending PM: {e}")

# Function to notify admin when user replies to bot
def user_reply(update: Update, context):
    user = update.message.from_user
    user_msg = update.message.text

    # Notify admin about the user's reply
    admin_reply_msg = f"""📩 **User Reply Received:**  
👤 Name: {user.first_name}  
🆔 User ID: {user.id}  
💬 Message: {user_msg}
"""
    bot.send_message(chat_id=ADMIN_CHAT_ID, text=admin_reply_msg)

def main():
    updater = Updater(token=TOKEN, use_context=True)
    dp = updater.dispatcher

    dp.add_handler(MessageHandler(Filters.status_update.new_chat_members, welcome))
    dp.add_handler(MessageHandler(Filters.text & ~Filters.command, user_reply))

    updater.start_polling()
    updater.idle()

if __name__ == "__main__":
    main()
