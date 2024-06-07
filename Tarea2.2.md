# Bot de Telegram para Álbumes de Kanye West

Este bot de Telegram permite a los usuarios obtener la lista de canciones de un álbum específico de Kanye West al enviar el nombre del álbum. El bot responde con la lista de canciones correspondientes.

## Evidencias

![Texto alternativo](https://github.com/VGabriel31/Lenguajes_Automatas_1/blob/main/img/telegram1.png)


# Codigo
```python
import logging
from telegram import Update
from telegram.ext import Application, CommandHandler, MessageHandler, filters, ContextTypes

# Diccionario con álbumes y canciones de Kanye West
kanye_albums = {
    "The College Dropout": ["Jesus Walks", "All Falls Down", "Through the Wire", "Slow Jamz"],
    "Late Registration": ["Gold Digger", "Touch the Sky", "Heard 'Em Say", "Diamonds from Sierra Leone"],
    "Graduation": ["Stronger", "Good Life", "Can't Tell Me Nothing", "Flashing Lights"],
    "808s & Heartbreak": ["Heartless", "Love Lockdown", "Paranoid", "Amazing"],
    "My Beautiful Dark Twisted Fantasy": ["Power", "All of the Lights", "Monster", "Runaway", "Devil in a New Dress"],
    "Yeezus": ["Black Skinhead", "New Slaves", "Bound 2", "Blood on the Leaves"],
    "The Life of Pablo": ["Ultralight Beam", "Famous", "Father Stretch My Hands Pt. 1", "Fade"],
    "Ye": ["Yikes", "All Mine", "Ghost Town", "Wouldn't Leave"],
    "Jesus Is King": ["Follow God", "Selah", "Closed on Sunday", "Use This Gospel"],
    "Donda": ["Hurricane", "Jail", "Off the Grid", "Moon"],
    "Donda 2": ["Pablo", "Louie Bags", "Security", "Happy"]
}

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Envía un mensaje de bienvenida cuando se emite el comando /start."""
    user = update.effective_user
    await update.message.reply_html(
        rf"¡Hola {user.first_name}! Soy un bot que puede mostrar las canciones de un álbum de Kanye West. Envíame un mensaje con el nombre del álbum."
    )

async def help_command(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Envía un mensaje con información de ayuda cuando se emite el comando /help."""
    await update.message.reply_text("Puedes escribir el nombre de un álbum de Kanye West para obtener la lista de canciones.")

async def album_songs(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Muestra las canciones de un álbum de Kanye West."""
    message_text = update.message.text.strip()

    if message_text in kanye_albums:
        album_songs = kanye_albums[message_text]
        response = f"Las canciones del álbum '{message_text}' son:\n" + "\n".join(album_songs)
    else:
        response = "No reconozco ese álbum. Por favor, verifica el nombre e inténtalo de nuevo."

    await context.bot.send_message(chat_id=update.effective_chat.id, text=response)

def main() -> None:
    """Inicio del bot."""
    application = Application.builder().token("TU_TOKEN_DE_BOT").build()  # Reemplaza "TU_TOKEN_DE_BOT" con tu token de bot de Telegram

    application.add_handler(CommandHandler("start", start))
    application.add_handler(CommandHandler("help", help_command))
    application.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, album_songs))

    application.run_polling(allowed_updates=Update.ALL_TYPES)

if __name__ == "__main__":
    main()



