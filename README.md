# Local Commander for Cursor, DeepSeek Harness, Antigravity, Kiro and LM Studio

Connects Cursor, DeepSeek Harness, Antigravity, Kiro and LM Studio to [Local Commander](https://localcmd.app) at `https://localcmd.app/mcp`. Your AI gets hands on every computer you own: it can run commands and scripts, change settings, edit files and check local services on any of them, from one chat. Your workspace at localcmd.app shows which computer ran each request and how it ended.

This repository holds configuration only: a Cursor plugin, a DeepSeek Harness bundle, an Antigravity plugin, a Kiro Power and LM Studio settings. The service runs at localcmd.app.

## Requirements

- A Local Commander account at [localcmd.app](https://localcmd.app).
- A Windows 11 x64 computer with Node.js 20 or later and the paired agent: run `npx local-commander-agent@latest` on it, then choose full access or selected folders.
- The computer must be online for work and for reading results.

Once you approve a computer and connect your AI, it can do what your Windows account can do, just like you at the keyboard. You can limit it to certain folders, block commands, stop running work, or switch a computer off from the site at any time. Every run shows which computer did it. Whatever the AI asks for goes back to the AI.

## Cursor

1. Install the plugin, or add this to `~/.cursor/mcp.json`:

   ```json
   {
     "mcpServers": {
       "local-commander": { "url": "https://localcmd.app/mcp" }
     }
   }
   ```

2. Cursor opens localcmd.app to sign in and approve access (OAuth 2.0). Sign-in happens on localcmd.app; Cursor never sees your password.
3. Ask the agent to list your computers, then work on an approved project.

Troubleshooting:

- **The agent reports no computers:** start the agent on the Windows computer and check that it shows as online at localcmd.app.
- **Sign-in loops or fails:** open Output, choose "MCP Logs", and check the OAuth error; then remove and re-add the server.
- **A result is unknown:** the computer dropped during the task. The work is not re-run automatically; check the computer and ask again.

## DeepSeek Harness

A configuration-only [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) bundle. The tools appear as `mcp__local-commander__<tool>`.

The harness MCP client sends static headers only, so the bundle starts [`mcp-remote`](https://github.com/punkpeye/mcp-remote) over stdio. On first use it opens localcmd.app in your browser to sign in and approve access (OAuth 2.0), then keeps the token refreshed. In your localcmd.app workspace these requests show the client name "MCP CLI Proxy", the name mcp-remote registers.

DeepSeek Harness asks for Node.js 22.19 or later on the machine where you chat.

Install: download `local-commander-dsh-1.0.0.tgz` from the [latest release](https://github.com/netbka/local-commander-plugins/releases/latest), then run:

```sh
dsh plugin --profile web add ./local-commander-dsh-1.0.0.tgz
dsh web
```

You can also build the tarball from this repository with `npm pack`.

Remove:

```sh
dsh plugin --profile web remove local-commander-dsh
```

## Antigravity

This repository is also an Antigravity plugin (`.antigravity-plugin/plugin.json` plus `mcp_config.json`). Like the DeepSeek Harness bundle, it starts [`mcp-remote`](https://github.com/punkpeye/mcp-remote) over stdio, which opens localcmd.app in your browser to sign in and approve access (OAuth 2.0) and keeps the token refreshed. Requests show the client name "MCP CLI Proxy" in your localcmd.app workspace.

Install:

```sh
agy plugin install https://github.com/netbka/local-commander-plugins
```

Or clone the repository and run `agy plugin install <folder>`. Remove with `agy plugin uninstall local-commander`.

## Kiro

The `kiro-power/` folder is a Kiro Power in the Agent Plugins format (`plugin.json` and `mcp.json`). It points Kiro at the remote server `https://localcmd.app/mcp` (Streamable HTTP), which uses OAuth 2.0 sign-in at localcmd.app.

Install: in Kiro, open the Powers panel, choose **Add Custom Power**, then **Import power from GitHub**, and enter `https://github.com/netbka/local-commander-plugins/tree/main/kiro-power`.

## LM Studio

[LM Studio](https://lmstudio.ai) 0.3.17 or later runs MCP servers listed in its `mcp.json`. Like the DeepSeek Harness bundle, the entry starts [`mcp-remote`](https://github.com/punkpeye/mcp-remote) over stdio, which opens localcmd.app in your browser to sign in and approve access (OAuth 2.0) and keeps the token refreshed. Requests show the client name "MCP CLI Proxy" in your localcmd.app workspace. The computer running LM Studio needs Node.js 20 or later.

1. In LM Studio, open the **Program** tab in the right sidebar, choose **Install > Edit mcp.json**, and add:

   ```json
   {
     "mcpServers": {
       "local-commander": {
         "command": "npx",
         "args": ["-y", "mcp-remote@0.15.1", "https://localcmd.app/mcp"]
       }
     }
   }
   ```

   Or paste this link into your browser's address bar on that computer, and LM Studio adds the same entry:

   ```text
   lmstudio://add_mcp?name=local-commander&config=eyJjb21tYW5kIjoibnB4IiwiYXJncyI6WyIteSIsIm1jcC1yZW1vdGVAMC4xNS4xIiwiaHR0cHM6Ly9sb2NhbGNtZC5hcHAvbWNwIl19
   ```

2. In a chat, turn on `mcp/local-commander` and choose a model that supports tool calls. On first use your browser opens localcmd.app to sign in and approve access.
3. Ask the model to list your computers, then work on an approved project.

## Links

Privacy: https://localcmd.app/privacy. Terms: https://localcmd.app/terms. Support: peter.mamrukov@localcmd.app (https://localcmd.app/support).

## License

MIT, for the files in this repository.
