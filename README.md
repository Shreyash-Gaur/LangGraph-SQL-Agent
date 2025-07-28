# 💬 LangGraph SQL Agent

*Conversational Natural Language to SQL with Llama 3.2*

## 🎯 Overview

The **LangGraph SQL Agent** is an advanced project showcasing a conversational agent that can understand natural language, convert it into SQL queries, interact with a database, and deliver human-readable answers. Built with LangGraph, this agent features a sophisticated, multi-step workflow that includes relevance checking, query generation, execution, and a self-correction loop to handle errors, making database interactions as simple as having a conversation.

## ✨ Key Features

  - 🗣️ **Natural Language to SQL**: Translates user questions like "show me my orders" into precise SQL queries.
  - 🤔 **Relevance Checking**: First determines if a question is related to the database schema, providing witty responses for off-topic queries.
  - ⚙️ **Dynamic SQL Execution**: Executes generated queries against a live SQLite database using SQLAlchemy.
  - 📖 **Human-Readable Responses**: Converts raw SQL results into clear, friendly, and easy-to-understand natural language.
  - 🔄 **Self-Correction Loop**: If a generated SQL query fails, the agent automatically attempts to rewrite the original question and regenerate the query, retrying up to 5 times.
  - 🤖 **Stateful Graph Architecture**: Manages the entire multi-step process using a robust and easy-to-visualize state graph built with LangGraph.

## 🛠️ Technology Stack

  - **Agent Framework**: LangGraph for creating the stateful agent graph.
  - **LLM Orchestration**: LangChain for core abstractions (prompts, parsers, messages).
  - **Database**: SQLAlchemy with a SQLite backend.
  - **LLM Integration**: Ollama (Llama 3.2) for all reasoning and generation tasks.
  - **Core Language**: Python 3.10+
  - **Environment**: Jupyter Notebook

## 📁 Project Structure

```
langgraph-sql-agent/
├── assets/
│   ├── workflow.png          # Image of the agent's graph workflow
│   └── setup.png             # Image of the setup process
├── sql-agent-llama.ipynb   # Main notebook with agent implementation
├── setup.py                # Script to initialize and populate the database
├── example.db              # SQLite database file (created by setup.py)
└── README.md               # This file
```
## 🚀 Quick Start

### Prerequisites

  - Ollama running locally.
  - Python 3.10+ with Jupyter Notebook installed.


## 🎮 How It Works

The agent operates as a state graph where each node is a specific function, and edges determine the flow based on the current state.

### 1\. **User Identification**

The workflow begins at the `get_current_user` node, which identifies the active user based on a `current_user_id` passed in the configuration.

### 2\. **Relevance Checking**

The user's question is passed to the `check_relevance` node. The LLM determines if the question is related to the database schema. The `relevance_router` then directs the flow: `relevant` questions go to the SQL generation step, while `not_relevant` questions are sent to the `generate_funny_response` node.

### 3\. **NL-to-SQL Conversion**

The `convert_nl_to_sql` node takes the user's question and schema information and uses the LLM to generate a valid SQL query.

### 4\. **SQL Execution & Routing**

The `execute_sql` node runs the generated query against the database. The `execute_sql_router` then checks for errors.

  - **On Success**: The flow moves to `generate_human_readable_answer`.
  - **On Failure**: The flow moves to the self-correction loop.

### 5\. **Response Generation / Self-Correction**

  - **Success Path**: The `generate_human_readable_answer` node converts the raw SQL result into a friendly, natural language response and the workflow ends.
  - **Failure Path**: The `regenerate_query` node rewrites the user's original question to be more precise. The `check_attempts_router` sends it back to the `convert_to_sql` node to try again. If it fails 5 times, the workflow ends.

## 📊 Sample Queries

You can test the agent's capabilities with the following queries from the notebook:

  - **Create a new record**:
    `"Create a new order for Spaghetti Carbonara."`
  - **Query existing records**:
    `"Show me my orders"`
  - **Handle an irrelevant question**:
    `"Tell me a joke."`

## 🎨 Visualization Features

The project includes a built-in method to visualize the agent's complex graph structure. The notebook uses `app.get_graph(xray=True).draw_mermaid_png()` to generate a detailed diagram of all the nodes, edges, and potential state transitions, making the agent's logic easy to understand and debug.

## 🔧 Customization

### Changing the LLM

You can easily switch the underlying language model by modifying the `model` string in the `ChatOllama` initialization within the notebook's functions.

```python
llm = ChatOllama(model="your-ollama-model:latest", temperature=0)
```

### Modifying Agent Behavior

The agent's logic is defined by the system prompts in each node function (`convert_nl_to_sql`, `check_relevance`, etc.). Customize its behavior by editing these prompts to handle different database schemas or to change its personality.

### Switching the Database

The current implementation uses SQLite. You can switch to any database supported by SQLAlchemy by changing the `DATABASE_URL` environment variable and ensuring the appropriate database drivers are installed.

```python
DATABASE_URL = os.getenv("DATABASE_URL", "postgresql://user:password@host/dbname")
```

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

## 📝 Requirements

The project requires the following Python packages:

```
sqlalchemy
langchain-core
langchain-community
langchain-ollama
langgraph
pydantic
python-dotenv
ipython
# Note: Other dependencies may be required by langchain libraries.
```

## 🔍 Troubleshooting

### Common Issues

  - **Ollama Connection**: Ensure the Ollama application is running on your machine before starting the notebook.
  - **Model Not Found**: Verify that you have pulled the required model (`llama3.2:latest`) using the `ollama pull` command.
  - **SQL Errors**: If the agent repeatedly fails to generate a correct query, try refining the system prompt in the `convert_nl_to_sql` function to provide more specific examples or constraints for your database schema.

## 🤝 Contributing

We welcome contributions\! Please feel free to:

  - **Report Issues**: Submit bug reports and suggest new features.
  - **Code Contributions**: Fork the repository and submit pull requests with your enhancements.
  - **Improve Documentation**: Help us make the documentation clearer and more comprehensive.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

  - **LangChain & LangGraph Teams** for creating the powerful frameworks that make building complex agents possible.
  - **Ollama** for providing an incredible platform for running local LLMs with ease.
  - **The Open Source Community** for their continuous innovation and support.

*Ready to chat with your database? Let's get started\!* 🚀

**Star this repository if you found it helpful\!** ⭐