# MCP (Model Context Protocol)

A Python project that runs an **MCP client** (OpenAI-backed) and an **MCP weather server**, so you can chat with an LLM and have it call weather tools (alerts and forecasts via the US National Weather Service API).

## Project structure

- **`mcp/client.py`** — MCP client that connects to MCP servers over stdio and uses **OpenAI** (GPT-4o) to answer queries using the server’s tools.
- **`Weather/src/weather.py`** — MCP server that exposes:
  - `get_alerts(state)` — weather alerts for a US state (e.g. `CA`, `NY`).
  - `get_forecast(latitude, longitude)` — forecast for a location.

## Setup

1. **Clone and enter the project**
   ```bash
   cd /path/to/MCP
   ```

2. **Create and activate a virtual environment**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate   # macOS/Linux
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure API key**  
   Create a `.env` in the project root (or export in the shell):
   ```bash
   OPENAI_API_KEY=your-openai-api-key
   ```

## Running the client

Connect the client to the weather server and start the interactive chat:
        Client -> Server Location
```bash
python3 mcp/client.py /path/to/MCP/Weather/src/weather.py
```

Example (from project root):

```bash
python mcp/client.py "$(pwd)/Weather/src/weather.py"
```

At the `Query:` prompt you can ask things like “What are the weather alerts for CA?” or “What’s the forecast for 37.77, -122.42?” — the client will call the MCP tools and return the results.

Type `quit` to exit.

## Using the weather server in Claude Desktop

Add the weather server to your Claude Desktop MCP config (e.g. `claude_desktop_config.json`) so Claude can use the same tools:

```json
{
  "mcpServers": {
    "weather": {
      "command": "/path/to/MCP/.venv/bin/python",
      "args": ["/path/to/MCP/Weather/src/weather.py"]
    }
  }
}
```

Replace `/path/to/MCP` with the absolute path to this project (e.g. `/Users/yourname/Desktop/MCP`).

## Requirements

- Python 3.x
- Dependencies in `requirements.txt`: `mcp`, `mcp[cli]`, `openai`, `python-dotenv`, `httpx`
