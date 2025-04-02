# Simple MCP Client / Server 
https://modelcontextprotocol.io/quickstart/server


Install [uv](https://docs.astral.sh/uv/getting-started/installation/), on macOS and Linux.

```sh
$ curl -LsSf https://astral.sh/uv/install.sh | sh
```

Then git clone the repo.


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
mcp_server$ uv run weather_stdio.py
```


### Test with VSCode + Cline
Add MCP server to Cline.

```
Cline -> MCP Servers (icon close to '+') -> Installed -> Add
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
                "/home/users/tikoehle/mcp_server",
                "run",
                "weather_stdio.py"
            ]
        }
  }
}
```

=> Cline shows the 2 get_* Tools functions. Click 'Done' to return to Cline Task input trying the new server.



### Test
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
uv run client.py path/to/server.py

```
mcp_client$ uv run client_stdio.py ../mcp_server/weather_stdio.py
```


## MCP Inspector

### Installation Node.js
On the MCP server compute, get ```Node.js``` (Current) for ```Linux``` using ```nvm``` with ```npm```. https://nodejs.org/en/download

```
cd $HOME

# Download and install nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash

# in lieu of restarting the shell
\. "$HOME/.nvm/nvm.sh"

# Download and install Node.js:
nvm install 23

# Verify the Node.js version:
node -v # Should print "v23.10.0".
nvm current # Should print "v23.10.0".

# Verify npm version:
npm -v # Should print "10.9.2".
```

### Inspecting a locally developed server

```
mcp_server$ uv run mcp dev ./weather_stdio.py
Starting MCP inspector...
Proxy server listening on port 3000

🔍 MCP Inspector is up and running at http://localhost:5173 🚀
```

**Note:**

Another method to install the latest version and start the Inspector.
```
mcp_server$ npx @modelcontextprotocol/inspector@latest
```

### Open the MCP Inspector client UI in the Browser

```
http://comp9:5173   --> Connect
```
**Note:**

If the MCP dev server runs on a remote compute, for example comp9, then the MCP Inspector Client UI needs to connect to this machine.

Transport Type, Command and Arguments appeared with the correct parameters when launching the UI in the browser.

```
Transport Type: STDIO
Command: uv
Arguments: run --with mcp mcp run ./weather_stdio.py
```
or

```
Transport Type: SSE
URL: http://comp9:8001/sse
```

**Note:**

MCP development tools
```
mcp_server$ uv run mcp
```



## SSE Client / Server

Server
```
mcp_server$ uv run weather_sse.py      # port 8001
```

Client
```
mcp_client$ uv run client_sse.py http://comp9:8001/sse
```
