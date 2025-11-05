# LangGraph AI Chatbot with Tool Integration and Persistent Memory
[![Ask DeepWiki](https://devin.ai/assets/askdeepwiki.png)](https://deepwiki.com/ayushmankr/vishwajeet-project)

This repository contains a conversational AI chatbot built with LangGraph, Google's Gemini model, and a Streamlit frontend. The chatbot is capable of using external tools (web search, calculator, stock price lookup) and maintains a persistent memory of conversations using a SQLite database. This allows users to manage multiple conversations that are saved across sessions.

## ✨ Features

*   **Tool Integration**: The agent can autonomously decide to use tools like DuckDuckGo Search, a calculator, and a stock price API to answer user queries.
*   **Persistent & Stateful Conversations**: Leverages LangGraph's `SqliteSaver` to store conversation state in a `chatbot.db` file, ensuring no loss of history between sessions.
*   **Multi-Conversation UI**: The Streamlit interface allows users to create, manage, and switch between multiple independent chat threads.
*   **Streaming Responses**: Delivers a fluid user experience by streaming the AI's responses in real-time.
*   **LLM Powered**: Utilizes Google's powerful and efficient `gemini-2.5-flash` model for conversational intelligence.
*   **Modular Architecture**: Clean separation between the backend graph logic (`langgraph_tool_backend.py`) and the frontend UI (`streamlit_frontend_tool.py`).

## ⚙️ How It Works

The application is composed of a backend and a frontend:

1.  **Backend (`langgraph_tool_backend.py`)**:
    *   A `StateGraph` is defined to manage the flow of the conversation.
    *   The graph is connected to the Gemini LLM, which is bound to a set of tools (search, calculator, stock price).
    *   When a user query is received, the LLM can either respond directly or invoke one or more tools. A `ToolNode` executes the tool calls and returns the results to the LLM.
    *   The LLM then uses the tool's output to formulate a final answer.
    *   All conversation states are automatically saved to a SQLite database (`chatbot.db`) via the `SqliteSaver` checkpointer.

2.  **Frontend (`streamlit_frontend_tool.py`)**:
    *   A Streamlit application provides the chat interface.
    *   On startup, it queries the database to load and display a list of all previously saved conversation threads in the sidebar.
    *   Users can start a new chat or select an existing one to continue the conversation.
    *   User input is sent to the LangGraph backend, and the application streams the AI's response, including real-time status updates when a tool is being used.

## 🗂️ Project Structure

```
.
├── chatbot.db
├── chatbot.db-shm
├── chatbot.db-wal
├── langgraph_tool_backend.py
├── requirements.txt
└── streamlit_frontend_tool.py
```

## 🛠️ Setup and Usage

Follow these steps to run the chatbot on your local machine.

### 1. Prerequisites

*   Python 3.8+
*   A Google AI API Key
*   An AlphaVantage API Key (for the stock price tool)

### 2. Clone the Repository

```bash
git clone https://github.com/ayushmankr/vishwajeet-project.git
cd vishwajeet-project
```

### 3. Install Dependencies

It is recommended to use a virtual environment.

```bash
# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`

# Install the required packages
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the root of the project directory and add your API keys:

```
GOOGLE_API_KEY="YOUR_GOOGLE_AI_API_KEY"
ALPHAVANTAGE_API_KEY="YOUR_ALPHAVANTAGE_API_KEY"
```

You can get your keys from:
*   [Google AI Studio](https://aistudio.google.com/app/apikey)
*   [AlphaVantage](https://www.alphavantage.co/support/#api-key)

### 5. Run the Application

Start the Streamlit frontend:

```bash
streamlit run streamlit_frontend_tool.py
```

Open your web browser and navigate to the local URL provided by Streamlit (usually `http://localhost:8501`) to start chatting.

## 🚀 Technology Stack

*   **Backend**: LangGraph, LangChain
*   **LLM**: Google Gemini 2.5 Flash
*   **Frontend**: Streamlit
*   **Database**: SQLite
*   **Tools**: DuckDuckGo Search, AlphaVantage API