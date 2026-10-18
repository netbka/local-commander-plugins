# Local Commander for Cursor and DeepSeek Harness

Connects Cursor and DeepSeek Harness to [Local Commander](https://localcmd.app) at `https://localcmd.app/mcp`. Your AI can inspect approved project files and Git state, run the checks and commands you authorize, and diagnose approved localhost services on Windows 11 computers you own. Your workspace at localcmd.app shows which computer ran each request and how it ended.

This repository holds configuration only: a Cursor plugin and a DeepSeek Harness bundle. The service runs at localcmd.app.

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

## Links

Privacy: https://localcmd.app/privacy. Terms: https://localcmd.app/terms. Support: peter.mamrukov@localcmd.app (https://localcmd.app/support).

## License

MIT, for the files in this repository.
