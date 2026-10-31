# Local Commander for Claude Code

Run tasks on your own computers from this chat. Pair a Windows, macOS or Linux machine once, and Claude Code can run commands, read, write and search files, use Git, check localhost services, follow long jobs and take screenshots on it — from your desk or from your phone. The computer you work on needs no AI client installed.

## Set up

1. On each computer you want to reach, run:

   ```
   npx local-commander-agent@latest
   ```

   and approve the pairing in the browser that opens. Choose full access to your user account or only the folders you pick.

2. Install this plugin in Claude Code:

   ```
   /plugin install local-commander --marketplace netbka/local-commander-plugins
   ```

   It adds the `local-commander` MCP server (`https://localcmd.app/mcp`); sign in with your Local Commander account when Claude Code asks.

3. Ask. For example: "On HomePc, run the tests in D:\Git\myapp and show me the failures" or "Which of my computers is low on disk?"

## What it does and does not do

Your AI works through commands, files and scripts. If a program has a command line, a script or an API — Git, ffmpeg, Blender, Python, a build tool — Claude Code can run it on your computer and read the result. It does not press buttons in windows. Every action is a command you can see in the Local Commander dashboard and cancel; you can revoke a computer's access at any time.

The computer must be switched on, online and running the paired agent.

## Open source

A single-machine edition that runs over stdio, MIT-licensed: https://github.com/netbka/local-commander-mcp-agent

## Links

- Website: https://localcmd.app
- Security: https://localcmd.app/security
- Privacy: https://localcmd.app/privacy
- Support: https://localcmd.app/support
