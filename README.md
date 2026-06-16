🦷 Agentic Dental Appointment Management System

An autonomous, multi-agent scheduling system built to manage dental clinic workflows. Leveraging LangGraph for stateful orchestration and Groq's Llama-3.3-70B for high-speed inference, this system handles dynamic patient inquiries, secure database mutations, and deterministic tool execution with zero hallucinations.

Authors: Muhammad Moosa (2023469)

Muhammad Saim (2023503)

Faculty of Artificial Intelligence, Ghulam Ishaq Khan Institute (GIKI)

🏗️ System Architecture

The project utilizes a Supervisor-Worker Agentic Topology implementing the ReAct (Reason + Act) framework:

Supervisor Agent: Analyzes the conversational state and routes user intent to specialized sub-agents.

Worker Agents: Specialized nodes (Info, Booking, Cancellation) equipped with distinct toolsets.

Data Layer: Flat-file CSV storage acting as the environmental state, interacted with via strict CRUD utility functions.

🛡️ Core Engineering Highlights

Deterministic Execution ($T=0$): Model temperature is strictly set to 0 to ensure mathematical predictability and eliminate hallucinated tool calls during database mutations.

Structural Guardrails: Implements Pydantic schema validation to enforce strict type coercion and validate temporal/categorical data before it hits the local storage layer.

Pre-Model Hooks: Custom sanitization layers to format LangChain message histories and handle API-specific payload constraints seamlessly.

⚙️ Prerequisites

Python 3.10+

A valid Groq API Key (Free tier available at console.groq.com)

MacOS (M-series recommended for optimal local execution) or Windows/Linux.

🚀 Local Setup & Installation

1. Clone the repository and navigate to the directory:

git clone [https://github.com/yourusername/Dental-Appointment-System-using-LangGraph.git](https://github.com/yourusername/Dental-Appointment-System-using-LangGraph.git)
cd Dental-Appointment-System-using-LangGraph


2. Create and activate a virtual environment:

python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate


3. Install the required dependencies:

pip install --upgrade pip
pip install langgraph langchain langchain-groq pandas pydantic python-dotenv


4. Configure the API Key:
Open dental_agent/agent.py (or your .env file) and insert your Groq API key:

llm = ChatGroq(
    api_key="your_groq_api_key_here", 
    model="llama-3.3-70b-versatile", 
    temperature=0
)


💻 Usage

⚠️ CRITICAL: Ensure doctor_availability.csv is completely closed in all external applications (Excel, Numbers, etc.) before running the system to prevent file-lock permission errors during tool execution.

Launch the CLI inference server:

python main.py


Example Prompts to Test Routing & Tool Calling:

Information Retrieval: "Show me the available slots for an orthodontist next week."

Multi-turn Booking: "I need to book an appointment for patient 1000082." (The agent will sequentially request the missing doctor and time details).

Direct Booking: "Book patient 1000082 with Emily Johnson on 5/10/2026 at 9:00."

Cancellation: "Cancel the appointment for patient 1000082 at 5/10/2026 9:00."

📂 Project Structure

├── main.py                     # CLI Entry point and inference loop
├── doctor_availability.csv     # Local State / Feature Store
├── dental_agent/
│   ├── agent.py                # LLM initialization and Graph definition
│   ├── utils.py                # Pre-model hooks and message sanitization
│   ├── config/
│   │   └── settings.py         # Environment variables and hyperparameters
│   ├── models/
│   │   └── state.py            # Pydantic schemas and LangGraph state definitions
│   └── tools/
│       ├── csv_reader.py       # Read operations (Slots, Specializations)
│       └── csv_writer.py       # Write operations (Booking, Cancellations)

