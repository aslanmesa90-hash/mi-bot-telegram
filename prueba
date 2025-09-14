import os
import requests
from flask import Flask, request

app = Flask(name)

# Configuración - Railway pasa el token por variable de entorno
TOKEN = os.environ.get('TELEGRAM_TOKEN', '8264891324:AAH3O7vWVzOBcujT7eB_lDN1u32IADz4w8Q')
BASE_URL = f"https://api.telegram.org/bot{TOKEN}"

def send_message(chat_id, text, reply_markup=None):
    """Enviar mensaje con teclado"""
    url = f"{BASE_URL}/sendMessage"
    payload = {
        "chat_id": chat_id,
        "text": text,
        "parse_mode": "HTML",
        "reply_markup": reply_markup
    }
    try:
        response = requests.post(url, json=payload)
        return response.json()
    except Exception as e:
        print(f"Error enviando mensaje: {e}")
        return None

def create_main_keyboard():
    """Teclado principal con botones"""
    keyboard = {
        "keyboard": [
            [{"text": "💰 Balance"}, {"text": "📊 Cartera"}],
            [{"text": "💸 Transferir"}, {"text": "📈 Historial"}],
            [{"text": "🆘 Ayuda"}, {"text": "🏠 Inicio"}]
        ],
        "resize_keyboard": True,
        "one_time_keyboard": False
    }
    return keyboard

@app.route('/')
def home():
    return "🤖 ¡Bot Goodbank funcionando en Railway!"

@app.route('/webhook', methods=['POST'])
def webhook():
    try:
        data = request.get_json()
        
        if 'message' in data:
            message = data['message']
            chat_id = message['chat']['id']
            text = message.get('text', '').strip()
            user_name = message['from'].get('first_name', 'Usuario')
            
            # Responder a comandos y botones
            if text.startswith('/start') or text == "🏠 Inicio":
                keyboard = create_main_keyboard()
                welcome_text = f"""
👋 ¡Hola {user_name}! 🤖
<b>Bienvenido a Goodbank</b>

👇 <b>Elige una opción:</b>
💰 Balance - Ver tu saldo
📊 Cartera - Ver tus inversiones
💸 Transferir - Enviar dinero
📈 Historial - Ver transacciones
🆘 Ayuda - Centro de ayuda
                """
                send_message(chat_id, welcome_text, keyboard)
            
            elif text == "💰 Balance":
                balance_msg = f"""
💼 <b>Balance de {user_name}</b>

💰 <b>Saldo disponible:</b> $1,250.00
📈 <b>Inversiones:</b> $3,500.00
💳 <b>Crédito disponible:</b> $2,000.00
🏦 <b>Total:</b> $6,750.00
                """
                send_message(chat_id, balance_msg)
            
            elif text == "📊 Cartera":
                wallet_msg = f"""
📊 <b>Cartera de {user_name}</b>

🔹 <b>Bitcoin (BTC):</b> 0.025 BTC
🔹 <b>Ethereum (ETH):</b> 1.5 ETH  
🔹 <b>USD Coin (USDC):</b> 500 USDC
🔹 <b>Solana (SOL):</b> 8.2 SOL

<b>Valor total:</b> $3,500.00
                """
                send_message(chat_id, wallet_msg)
            
            elif text == "🆘 Ayuda":
                help_text = """
🆘 <b>Centro de Ayuda</b>

<b>Botones disponibles:</b>
💰 Balance - Ver tu saldo
📊 Cartera - Ver tus inversiones  
💸 Transferir - Enviar dinero
📈 Historial - Ver transacciones

<b>Soporte:</b> @soporte_goodbank
                """
                send_message(chat_id, help_text)
            
            else:
                keyboard = create_main_keyboard()
                send_message(chat_id, "🤔 No entiendo ese comando.\nUsa los botones de abajo 👇", keyboard)
        
        return "OK"
    
    except Exception as e:
        print(f"Error: {e}")
        return "Error"

if name == 'main':
    port = int(os.environ.get('PORT', 5000))
    app.run(host='0.0.0.0', port=port)
