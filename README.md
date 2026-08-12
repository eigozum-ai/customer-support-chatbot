***

# 🤖 Smart Customer Support Chatbot

A fully open-source, production-grade AI Chatbot for customer support—made for **Future Interns ML Internship Task 3**.

***

## 🗂️ Folder Structure

```
customer-support-chatbot/
│
├── actions/                      # Custom actions for Rasa
│   ├── action_fallback_gpt.py
│   ├── action_order_status.py
│   └── actions.py
│
├── analytics/                    # Analytics dashboard and resources
│   ├── dashboard.py
│   └── Dockerfile.analytics
│
├── backend/                      # FastAPI backend and business logic
│   ├── chatbot.py
│   ├── config.py
│   ├── main.py
│   ├── memory.py
│   └── utils.py
│
├── data/                         # NLU training data and example conversations
│   ├── nlu.yml
│   ├── sample_conversations.csv
│   ├── stories.yml
│
├── database/                     # MongoDB/Redis connections and logging
│   ├── mongo_setup.py
│   └── save_chat.py
│
├── deployment/                   # Deployment configurations
│   ├── Dockerfile
│   └── render.yaml
│
├── frontend/                     # Streamlit app (UI)
│   ├── app.py
│   ├── Dockerfile.frontend
│   └── styles.css
│
├── knowledge_base/               # Knowledge base and FAQ resources
│   ├── faqs.csv
│   └── kb_loader.py
│
├── models/                       # (Optional) Store only latest/active .tar.gz models
│   └── 20250904-174035-greedy-network.tar.gz
│
├── rasa_bot/                     # Main Rasa configs, pipeline & actions Docker
│   ├── actions.py                # (If using Rasa's internal actions, else keep in /actions)
│   ├── Dockerfile.actions
│   ├── events.db
│   └── packages.txt
│
├── screenshots/                  # For documentation/demo
│   ├── 01_ui_homepage.jpg
│   ├── 02_chat_demo_output.jpg
│   ├── 03_rasa_server_log.jpg
│   ├── 04_streamlit_server_log.jpg
│   └── 05_backend_server_log.jpg
│
├── tests/                        # Rasa test stories
│   └── test_stories.yml
│
├── .gitignore                    # Standard ignores for Python, secrets, venv, models
├── .env.example                  # Template for all required environment variables
├── config.yml                    # Rasa config pipeline
├── credentials.yml               # Messaging channel credentials (do not commit real secrets)
├── docker-compose.yml            # Infra orchestration
├── domain.yml                    # Rasa domain (intents, slots, actions, responses)
├── endpoints.yml                 # Rasa endpoints (action, tracker store etc.)
├── README.md                     # Project overview and setup
├── requirements.txt              # All Python package requirements
├── rules.yml                     # Rasa rules (fallbacks etc.)
└── (venv/)                       # (Not pushed to repo, should be gitignored)
```

***

## 📋 Project Overview

**Goal:**  
Automate customer support for a retail/ecommerce setting using a multilingual AI chatbot. Uses Rasa for NLP/dialogue, FastAPI backend, Streamlit UI and analytics, MongoDB for storage, and smart fallback via APIs like OpenAI/Hugging Face.

**ML Task 3 (Future Interns 2025):**  
- Intent recognition: orders, refunds, FAQs, escalation.
- Human fallback, logging, analytics.
- Modular code, Docker deployment, and best practice structure.

***

## 🚀 Features

- **Order Tracking:** Get status by order ID.
- **Refunds:** Automated refund/return policy info.
- **FAQ System:** Query a knowledge base using csv data (LangChain supported).
- **Talk to Human:** Human agent intent/escalation.
- **AI Fallback:** Smart fallback (OpenAI/Gemini/HF) for unknown/complex queries.
- **Analytics Dashboard:** Track usage, trends, session logs.
- **Professional Deployment:** Easy Docker Compose or local setup.

***

## 🛠️ Tech Stack

- Rasa 3.x (core NLP, dialogue)
- Python 3.9+ (all custom logic)
- FastAPI (integration/backend)
- Streamlit (frontend, dashboard)
- MongoDB/Redis (DB, cache)
- Hugging Face/OpenAI API (fallback/LLM)
- Docker, Docker Compose (infra)

***

## 🔑 Environment Setup

### 1. Clone & Install
```bash
git clone https://github.com/eigozum-ai/customer-support-chatbot
cd customer-support-chatbot
python -m venv venv
source venv/bin/activate      # (Windows: venv\Scripts\activate)
pip install -r requirements.txt
```

### 2. Configure Environment
```bash
cp .env.example .env
# Insert your API & DB vars in .env
```

### 3. Train Bot & Run
```bash
rasa train
# Then (in separate terminals):
rasa run --enable-api --cors "*"
rasa run actions
uvicorn backend.main:app --reload
streamlit run frontend/app.py
```
or, for **all-in-one deployment:**
```bash
docker-compose up --build
```

***

## 📊 Analytics

Launch analytics dashboard:
```bash
streamlit run analytics/dashboard.py
```
**Features:** See intent distributions, session stats, and user trends.

***

## 📝 Sample Data

**data/sample_conversations.csv**  
```csv
user,bot
Hi,Hello! How can I help you today?
Where is my order?,Please provide your order ID, I’ll check the status for you.
12345,Your order #12345 is being processed and will be delivered in 3 days.
Refund policy?,Our refund policy allows refunds within 7 days of purchase.
Talk to human,Sure, connecting you to a human agent.
Blah,Sorry, I didn’t understand. Would you like to rephrase?
```

**knowledge_base/faqs.csv**  
```csv
question,answer
How to reset password?,Please click on 'Forgot Password' at login.
How to check order status?,Provide your order ID to track your order.
What is your return policy?,Returns accepted within 30 days.
```

***

## 🗣️ Supported Intents & Flows

- **greet**: “hi”, “hello”, “hey”
- **track_order**: “track my order”, “where is order #12345”
- **refund_policy**: “what is your refund/return policy?”
- **faq**: “how to reset password?” etc. (FAQs via CSV)
- **talk_to_human**: “I want to talk to a human”
- **goodbye**: “bye”, “good night”
- **fallback**: unknown, gibberish, complex
- **bot_challenge**: “are you a bot?”

***

## 🖼️ Demo Screenshots
check in my screenshots folder
| UI Homepage                    | Chat In Action                        |
|------------------------------- |---------------------------------------|
|  |  |

| Rasa/Backend Logs              | Streamlit/Analytics                   |
|------------------------------- |---------------------------------------|
|  |  |

***

## 🔒 Environment Variables

Create `.env` from `.env.example`:
```
OPENAI_API_KEY=your-openai-key
HF_API_TOKEN=your-huggingface-key
GEMINI_API_KEY=your-gemini-key
MONGO_URI=mongodb://admin:password123@localhost:27017/chatbot
```

***

## 🧩 Testing

- Use stories in `/tests/test_stories.yml` for Rasa’s `rasa test`.
- Custom test convos: update data/sample_conversations.csv as you want.

***

## 🗄️ Deployment

- **Docker Compose:** Easiest for production/local demo.
- **Cloud (Render, etc):** Use `deployment/render.yaml` as needed.
- **Port mapping:** Default ports - Rasa (5005), Backend API (8000), Frontend (8501), Analytics (8502).

***

