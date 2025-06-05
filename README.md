# MCP Terminal Server with SSE

A Model Context Protocol (MCP) server that enables AI models to execute terminal commands remotely via Server-Sent Events (SSE). This project creates a bridge between AI programs and your computer's terminal, allowing AI assistants to perform system-level operations with real-time feedback.

## What This Project Does

This server allows AI models (or any MCP-compatible client) to:
- Execute terminal/command-line commands remotely
- Receive real-time output streaming via Server-Sent Events
- Perform system-level tasks through a web interface
- Interact with your computer's file system and tools

Think of it as giving an AI assistant the ability to use your command line remotely - powerful for automation, development assistance, and system management.

## Key Features

- **Real-time Communication**: Uses Server-Sent Events for live command output streaming
- **MCP Protocol**: Implements Model Context Protocol for standardized AI tool integration
- **Dockerized Deployment**: Containerized for easy setup and security isolation
- **Python-based**: Built with FastMCP, Starlette, and Uvicorn
- **Multiple Tools**: Includes command execution and utility functions

## Project Structure

```
mcp_server/
├── terminal_server_sse.py    # Main MCP server implementation
├── main.py                   # Simple entry point
├── Dockerfile               # Container configuration
├── pyproject.toml          # Python project configuration
├── requirements.txt        # Python dependencies
├── uv.lock                # Dependency lock file
├── browser_mcp.json       # MCP client configuration
└── README.md              # This file
```

## Installation & Setup

### Prerequisites
- Python 3.13+
- Docker (optional, for containerized deployment)

### Local Installation

1. **Clone the repository** (or create the project directory)
```bash
mkdir mcp_terminal_server
cd mcp_terminal_server
```

2. **Create a virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

### Docker Installation

1. **Build the Docker image**
```bash
docker build -t terminal_server_sse .
```

2. **Run the container**
```bash
docker run --rm -p 8081:8081 -v /your/workspace:/root/mcp/workspace terminal_server_sse
```

## Usage

### Starting the Server

**Local Development:**
```bash
python terminal_server_sse.py
```

**With Custom Configuration:**
```bash
python terminal_server_sse.py --host 0.0.0.0 --port 8081
```

**Docker Deployment:**
```bash
docker run --rm -p 8081:8081 terminal_server_sse
```

### Expected Output
```
INFO:     Started server process [5868]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8081 (Press CTRL+C to quit)
```

## Available Tools

The server exposes the following MCP tools:

### `run_command`
Execute shell commands and get real-time output.
```json
{
  "tool": "run_command",
  "arguments": {"command": "ls -la"}
}
```

### `add_numbers`
Simple utility function for mathematical operations.
```json
{
  "tool": "add_numbers",
  "arguments": {"a": 5, "b": 3}
}
```

## API Endpoints

- `GET /sse` - Server-Sent Events connection endpoint
- `POST /messages/` - Send tool calls and commands

## Practical Use Cases

### 1. AI Development Assistant
```bash
# AI can help with:
git status                    # Check repository status
npm install                   # Install dependencies  
python -m pytest            # Run tests
docker build -t myapp .      # Build containers
```

### 2. System Administration
```bash
# AI can manage:
ps aux | grep python         # Monitor processes
df -h                        # Check disk space
mkdir -p projects/new-app    # Create directory structures
find . -name "*.py"          # Search for files
```

### 3. File Operations
```bash
# AI can organize:
ls ~/Downloads               # List files
mkdir ~/Downloads/organized  # Create folders
mv *.pdf documents/          # Move files
```

### 4. Development Workflows
```bash
# AI can automate:
git add .                    # Stage changes
git commit -m "Update"       # Commit changes
npm run build               # Build projects
python setup.py install    # Install packages
```

## Testing the Server

### Basic Connection Test
```bash
curl http://localhost:8081/sse
```

### Tool Call Test
```bash
curl -X POST http://localhost:8081/messages/ \
     -H "Content-Type: application/json" \
     -d '{"tool": "add_numbers", "arguments": {"a": 5, "b": 3}}'
```

## Security Considerations

**Important Security Notes:**

- This server executes arbitrary shell commands
- Docker provides isolation but isn't foolproof
- Consider implementing command whitelisting for production
- Use proper authentication mechanisms
- Be cautious with workspace volume mounts
- Monitor executed commands for security

### Recommended Security Practices:
1. Run in Docker containers for isolation
2. Limit workspace directory access
3. Implement command filtering/whitelisting
4. Use read-only mounts where possible
5. Monitor and log all command executions

## Configuration

### Default Settings
- **Port**: 8081
- **Host**: 0.0.0.0
- **Workspace**: `~/mcp/workspace`
- **Container Workspace**: `/root/mcp/workspace`

### Environment Variables
You can customize the server by modifying `terminal_server_sse.py` or using command-line arguments.

## Integration with AI Models

This server is designed to work with MCP-compatible AI clients. Popular integrations include:

- Claude with MCP support
- Custom AI agents
- Automation scripts
- Development tools

### Example AI Interaction:
```
User: "Help me organize my Downloads folder"
AI: *connects to MCP server*
AI: *executes* ls ~/Downloads
AI: *executes* mkdir ~/Downloads/organized  
AI: *executes* mv ~/Downloads/*.pdf ~/Downloads/organized/
AI: "I've organized your PDF files into the organized folder!"
```

## Dependencies

- `mcp>=1.3.0` - Model Context Protocol framework
- `starlette>=0.46.0` - Web framework
- `uvicorn>=0.34.0` - ASGI server

##  Development

### Adding New Tools
To add new MCP tools, modify `terminal_server_sse.py`:

```python
@mcp.tool()
def your_new_tool(parameter: str) -> str:
    """Description of your tool."""
    # Your implementation here
    return result
```

### Customizing the Server
- Modify port and host settings
- Add authentication middleware
- Implement command filtering
- Add logging and monitoring

##  License

Distributed under the MIT License. See [LICENSE](https://opensource.org/licenses/MIT) for more information.

---

**Note**: This server provides powerful capabilities by allowing AI models to execute system commands. Always use appropriate security measures and run in controlled environments.
