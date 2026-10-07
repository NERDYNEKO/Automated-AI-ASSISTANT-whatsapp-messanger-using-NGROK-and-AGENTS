# WhatsApp AI Assistant (n8n + Gemini)

An AI agent that chats with people on WhatsApp in real time. Built with **n8n**, the **WhatsApp Business Cloud API**, and **Google Gemini**, with conversation memory so replies stay in context.

> Message the number on WhatsApp, and the agent answers instantly, no human in the loop.

---

## Features

- **Live WhatsApp replies**: incoming messages are caught by a webhook and answered by an AI agent
- **LLM-powered agent**: Gemini handles understanding and response generation
- **Conversation memory**: remembers earlier messages in the chat, so follow-ups and corrections work
- **Customizable persona**: change the system prompt to turn the bot into a support agent, booking assistant, FAQ bot, or lead-capture bot
- **Low-code workflow**: the whole pipeline is one importable n8n workflow, easy to extend
- **Google Sheets integration (optional)**: log leads or conversations to a sheet (see Roadmap)

---

## Architecture

```
WhatsApp user
     │
     ▼
WhatsApp Cloud API ──► Webhook (n8n WhatsApp Trigger)
                              │
                              ▼
                     AI Agent node (Gemini)
                      ├── System prompt (persona + rules)
                      ├── Simple Memory (chat context)
                      └── Tools (e.g. Google Sheets)
                              │
                              ▼
                  WhatsApp "Send Message" node
                              │
                              ▼
                        WhatsApp user
```

---

## Tech Stack

| Layer | Tool |
|---|---|
| Automation | n8n (self-hosted, local) |
| Messaging | WhatsApp Business Cloud API (Meta) |
| LLM | Google Gemini |
| Memory | n8n Simple Memory |
| Logging (optional) | Google Sheets |
| Tunnel for local dev | ngrok |

---

## Getting Started

### Prerequisites

- Node.js 18+ (or Docker) to run n8n
- A Meta Developer account with a WhatsApp Business app
- A Google AI Studio API key for Gemini
- ngrok (or any public HTTPS tunnel) for the webhook during local development

### 1. Run n8n locally

```bash
npx n8n
```

n8n opens at `http://localhost:5678`.

### 2. Expose n8n with a public URL

```bash
ngrok http 5678
```

Set the ngrok HTTPS URL as n8n's webhook URL:

```bash
WEBHOOK_URL=https://<your-ngrok-id>.ngrok-free.app npx n8n
```

### 3. Set up WhatsApp Cloud API

1. Create an app at [developers.facebook.com](https://developers.facebook.com) and add the **WhatsApp** product.
2. Copy your **Phone Number ID** and a **access token**.
3. In the app's WhatsApp configuration, set the webhook callback URL to the one shown in your n8n WhatsApp Trigger node and subscribe to the `messages` field.

### 4. Import the workflow

1. In n8n, go to **Workflows → Import from file** and select `workflow.json`.
2. Add credentials:
   - **WhatsApp API** (access token + phone number ID)
   - **Google Gemini (PaLM) API** (API key)
   - **Google Sheets** (only if you use logging)
3. Edit the **system prompt** in the AI Agent node to define your assistant's role.
4. Activate the workflow.

### 5. Test it

Send a message from your phone to the WhatsApp test number. The agent should reply within a few seconds.

---

## Customizing the Agent

The agent's behaviour lives in the system prompt of the AI Agent node. Example:

```
You are a friendly assistant for <business name> on WhatsApp.
Keep replies short and clear. Answer only questions related to <business>.
If you don't know something, say so and offer to connect a human.
```

Swap in your own rules, tone, and knowledge to adapt it to any use case.

---

## Roadmap

- [ ] Log leads and conversations to Google Sheets
- [ ] Add tools for the agent (menu lookup, booking, order capture)
- [ ] Support images and voice notes
- [ ] Human handoff for complex queries
- [ ] Deploy n8n to a cloud host (no ngrok)

---

## Project Structure

```
.
├── workflow.json   # Exported n8n workflow
├── README.md
└── assets/         # Screenshots / demo GIF
```

---

## Demo

Add a screenshot or GIF of the chat and the n8n workflow here:

```
![Demo](assets/demo.gif)
```

---

## Notes

- Never commit API keys or access tokens. Export the workflow **without credentials**.
- The WhatsApp test number only messages numbers you have added as recipients in the Meta dashboard.

---

## Author

**Nabonita**
GitHub: [@NERDYNEKO](https://github.com/NERDYNEKO)

If you found this useful, consider giving the repo a star.
