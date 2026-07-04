[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/osdeibi-mcp-typebot-badge.png)](https://mseep.ai/app/osdeibi-mcp-typebot)

# MCP-Typebot

A small MCP server that exposes Typebot’s REST API as callable tools in Claude Desktop (via STDIO).
You can create, list, get, update, delete, publish/unpublish Typebots, list results, and start chats—using natural-language commands.

---

## Features

- **createBot**  
  Create a new Typebot in your workspace.  
  **Required:** `name`  
  **Optional:** `workspaceId`, `description`

- **listBots**  
  List all Typebots in your workspace.  
  **Optional:** `workspaceId`

- **getBot**  
  Fetch a Typebot by its ID.  
  **Required:** `botId`

- **updateBot**  
  Patch an existing Typebot (e.g. rename).  
  **Required:** `botId`, `typebot` (object with fields to change)  
  **Optional:** `overwrite`

- **deleteBot**  
  Delete a Typebot by its ID.  
  **Required:** `botId`

- **publishBot** / **unpublishBot**  
  Toggle a Typebot’s published state.  
  **Required:** `botId`

- **listResults**  
  Retrieve conversation results for a Typebot.  
  **Required:** `botId`  
  **Optional:** `limit`, `cursor`, `timeFilter`, `timeZone`

- **startChat**  
  Begin a new chat session with a Typebot.  
  **Required:** `botId`  
  **Optional:** `chat.context`

---

## Prerequisites

1. **Node.js 18+**  
2. A valid **Typebot API token** and **workspace ID**  
3. Claude Desktop connected to your local MCP server
---

## Installation

```bash
git clone <repo-url>
cd mcp-typebot
npm install
npm run build
```

---

## You can also install the published package directly via npm:

```bash
npm install mcp-typebot
npm start
```

---

## Running

```bash
npm start
```

This starts the MCP server on STDIO. Claude Desktop (or any MCP client) will connect to it automatically.

---

## Usage in Claude Desktop

Simply write natural commands like:

> **User**: “Create me a new typebot”  
> **Claude**: “Sure—what name?”  
> **User**: “MyDemoBot”  
> **Claude** (internally invokes):
> ```
> @createBot {"name":"MyDemoBot"}
> ```

Or, explicitly:

```
@updateBot {"botId":"<your_bot_id>","typebot":{"name":"NewName"},"overwrite":true}
```

You can also start a chat:

```
@startChat {"botId":"<your_bot_id>"}
```

---

## Extending

- Add new tools by implementing them in `src/tools/bots.ts` and registering them in `src/index.ts`.  
- Define a Zod schema for each tool to get automatic prompting and validation.

---

## License



---

## Configuring Claude Desktop

To connect Claude Desktop to this MCP server, add the following to your Claude configuration (e.g. `claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "mcp-typebot": {
      "command": "node",
      "args": [
        "path/to/project/dist/index.js"
      ],
      "env": {
        "TYPEBOT_TOKEN": "YOUR_TOKEN_HERE",
        "TYPEBOT_WORKSPACE_ID": "YOUR_WORKSPACE_ID"
      }
    }
  }
}
```

Make sure the `command` and `args` point to your local built `index.js`, and that your `.env` values match those in `env`.
