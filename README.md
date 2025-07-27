
# Conversational SQL Agent with LangGraph & Ollama

This project demonstrates a sophisticated, conversational Text-to-SQL agent built using Python, LangGraph, and Ollama. The agent can understand natural language questions, convert them into complex SQL queries (including `INSERT` statements with subqueries and `SELECT` statements with `JOIN`s), execute them against a database, and provide human-readable responses.

The entire workflow runs locally without requiring API keys for the core language model, thanks to Ollama.

## ✨ Key Features

  - **Natural Language to SQL:** Translates plain English questions into precise SQL queries.
  - **Handles Complex Queries:** Capable of generating both data retrieval (`SELECT` with `JOIN`s) and data creation (`INSERT` with subqueries) statements.
  - **Relevance Checking:** Intelligently determines if a question is relevant to the database schema and provides conversational fallbacks for off-topic queries.
  - **Robust Workflow:** Built as a state machine using LangGraph, allowing for complex logic, error handling, and retries.
  - **Local & Private:** Runs entirely on your local machine using Ollama, ensuring data privacy and no LLM API costs.
  - **Human-Friendly Responses:** Formats raw database outputs into clear, easy-to-understand sentences.

## ⚙️ Architecture & Workflow

The agent operates as a cyclical graph, or a state machine, built with LangGraph. At each step, the agent decides on the next action based on the current state.

The primary nodes in the graph include:

1.  **`get_current_user`**: Identifies the user making the request.
2.  **`check_relevance`**: Uses the LLM to determine if the question is database-related.
3.  **`convert_to_sql`**: The core node that translates the user's question into a SQL query. This is guided by a robust prompt with examples.
4.  **`execute_sql`**: Runs the generated query against the database.
5.  **`generate_human_readable_answer`**: Formats the result for the user.
6.  **`generate_funny_response`**: Handles irrelevant questions.

The graph below visualizes the agent's decision-making process:

## 🛠️ Tech Stack

  - **Core Logic:** LangChain & LangGraph
  - **Language Model:** Ollama (with `llama3.2`)
  - **Database:** SQLAlchemy (with SQLite)
  - **Language:** Python

## 🚀 Getting Started

Follow these steps to set up and run the project on your local machine.

### 1\. Prerequisites

  - Python 3.8+
  - [Ollama](https://ollama.com/) installed and running.

### 2\. Clone the Repository

```bash
git clone https://github.com/Shreyash-Gaur/LangGraph-SQL-Agent.git
cd LangGraph-SQL-Agent
```

### 3\. Set Up the Environment

It's recommended to use a virtual environment.

```bash
# Create a virtual environment
python -m venv .venv

# Activate it (on macOS/Linux)
source .venv/bin/activate

# Or on Windows
# .venv\Scripts\activate
```

### 4\. Install Dependencies

Install the required packages from the `requirements.txt` file.

```bash
pip install -r requirements.txt
```

### 5\. Download the Language Model

Pull the `llama3.2` model using Ollama.

```bash
ollama pull llama3.2
```

### 6\. Initialize the Database

Run the setup script from your terminal to create the `example.db` file and populate it with sample data.

```bash
python setup_db.py
```

You should see the message: `Datenbank wurde erfolgreich erweitert und mit Beispieldaten gefüllt.`

### 7\. Run the Agent

Now you are ready to run the agent.

1.  Launch Jupyter Lab or Jupyter Notebook:
    ```bash
    jupyter lab
    ```
2.  Open the `ollama-sql-agent.ipynb` notebook.
3.  Run the cells sequentially from top to bottom.

## Usage

You can interact with the agent by modifying the last few cells in the notebook. Change the `user_question_*` variables to ask your own questions.

**Example Invocation:**

```python
# The configuration to identify the current user (user_id: "2" is "Bob")
fake_config = {"configurable": {"current_user_id": "2"}}

# Ask a question to see your orders
user_question_3 = "Show me my orders"
result_3 = app.invoke({"question": user_question_3, "attempts": 0}, config=fake_config)
print(result_3["query_result"])

# Expected Output:
# Hello Bob, you have ordered Lasagne for $14.0 and Spaghetti Carbonara for $15.0.
```

## 📄 License

This project is licensed under the MIT License.