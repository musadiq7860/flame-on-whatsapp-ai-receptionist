# Flame On! — WhatsApp AI Receptionist 🍕

An AI-powered WhatsApp receptionist for **Flame On!**, a restaurant in Pakistan. It handles the full order flow — greeting, menu Q&A, price calculation, order confirmation, and delivery address collection — entirely in **Roman Urdu**, then logs confirmed orders straight to Google Sheets.

Built as an n8n workflow using an AI agent node backed by Gemini, with Redis-based conversation memory so the bot remembers context per customer.

## How it works

1. **WhatsApp Trigger** — receives incoming customer messages via the WhatsApp Cloud API.
2. **AI Agent (Gemini 2.0 Flash)** — interprets the message against a system prompt containing the full menu, pricing, and order-flow rules.
3. **Redis Chat Memory** — keeps a rolling conversation window (15 messages) per customer, keyed by their WhatsApp number.
4. **Google Sheets Tool** — once an order is confirmed and the address is provided, the agent logs Phone Number, Order Details, Total Price, Address, and Status to a Google Sheet.
5. **Send Message** — replies to the customer over WhatsApp.

## Order Flow

1. Customer asks about the menu or places an order.
2. Bot calculates the total and asks for confirmation.
3. On confirmation, bot asks for the delivery address.
4. Bot saves the order to Google Sheets and confirms it's been sent to the kitchen.

## Tech Stack

- **[n8n](https://n8n.io/)** — workflow orchestration (self-hosted on Railway)
- **Google Gemini 2.0 Flash** — LLM powering the AI agent
- **WhatsApp Cloud API (Meta)** — messaging channel
- **Redis** — per-customer conversation memory
- **Google Sheets** — order storage/backend

## Setup

1. Import `whatsapp_chat_bot_flame_on.json` into your n8n instance.
2. Configure credentials:
   - WhatsApp Cloud API (Meta Business)
   - Google Gemini API key
   - Redis connection
   - Google Sheets (Service Account) with access to your orders sheet
3. Update the `phoneNumberId` in the **Send message** node to your own WhatsApp Business number.
4. Replace the Google Sheets `documentId` with your own sheet, matching the columns: `Phone Number`, `Order Details`, `Total Price`, `Status`, `Address`.
5. Update the menu and prices in the AI Agent's system prompt to match your restaurant.
6. Activate the workflow.

## Customization

The entire menu, pricing, and conversation rules live in the **AI Agent** node's system prompt — no code changes needed to update prices or add items.

## Contributors

- [Muhammad Musaddaq Qaysir](https://github.com/musadiq7860)
- [Faraz shoukat](https://github.com/farazshoukat)


## License

MIT
