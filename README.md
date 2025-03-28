# Simple MCP Client / Server 
https://modelcontextprotocol.io/quickstart/server

## MCP Server (stdio)

```
uv init mcp_server
cd mcp_server/
uv run main.py                        # just to create the .venv, then rm the main.py

uv add "mcp[cli]" httpx               # MCP server Python lib

touch weather_stdio.py                # STDIO server implementation
```


### Start the server
```
(base) cisco@vm2agentic:~/tikoehle/mcp_server$ uv run weather_stdio.py
```


### Test with VSCode + Cline plugin
Add to Cline.
```
Cline -> MCP Servers (icon close to '+') -> Installed -> Add ->
```
You get a json template "mcpServers": {} and you add the new server. 
Here is the cline_mcp_settings.json:

```
{
  "mcpServers": {
    "weather": {
            "command": "uv",
            "args": [
                "--directory",
                "/home/cisco/tikoehle/mcp_server",
                "run",
                "weather_stdio.py"
            ]
        }
  }
}
```

=> Cline shows the 2 get_* Tools functions. Click 'Done' to return to Cline Task input trying the new server.



### Test in Cline
Type a task calling the two tools: 
"Can you tell me the weather forecast for San Francisco, CA"
"Can you tell me the weather alerts for San Francisco, CA"

=> uses my two new MCP 'weather' server tools.


Note:
Local MCP servers should not log messages to stdout because this will interfere with protocol operation.


## MCP Client
https://modelcontextprotocol.io/quickstart/client

```
uv init mcp_client
cd mcp_client/
uv run main.py                     # create venv

uv add mcp

touch client_stdio.py              # MCP client impl.
```


### Run the client
uv run client.py path/to/server.py          # python server

```
(base) cisco@vm2agentic:~/tikoehle/mcp_client$ uv run client_stdio.py ../mcp_server/weather_stdio.py
```

## Protocol debugging: MCP Inspector

```
(base) cisco@vm2agentic:~/tikoehle/mcp_server$ uv run mcp
```

To run the MCP server with the MCP Inspector requires to install ```nodejs``` and ``` npm```.

Start the server and the client and then connect to the inspector URL for debugging.

```
(base) cisco@vm2agentic:~/tikoehle/mcp_server$ uv run mcp dev weather_stdio.py
```


## SSE Client / Server

Server
```
(base) cisco@vm2agentic:~/tikoehle/mcp_server$ uv run weather_sse.py      # port 8001
```

Client
```
(base) cisco@vm2agentic:~/tikoehle/mcp_client$ uv run client_sse.py http://localhost:8001/sse
```
