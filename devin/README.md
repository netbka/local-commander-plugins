# Local Commander

Give your AI hands on every computer you own. One chat, all your machines — even from your phone.

Local Commander connects your AI to Windows 11 computers you own. Your workspace at https://localcmd.app shows which computer ran each request and how it ended.

## Setup

Create a Local Commander account, pair a Windows 11 x64 computer using Node.js 20+ and `npx local-commander-agent@latest`, and keep the agent online. Connect this plugin to https://localcmd.app/mcp and complete the browser OAuth consent. Select the intended computer and workspace before work begins.

Once you approve a computer and connect your AI, it can do what your Windows account can do. You can limit it to folders, block commands, stop running work or switch a computer off. Full Access is not an OS sandbox. Files, command output and other requested results return through Local Commander to your AI provider.

## First task

Ask: "List my available workspaces, let me choose the computer and project, then inspect Git status and summarize the changed paths." Use synthetic files for evaluation. Keep operation IDs if a request is still running; never repeat an uncertain write merely because a response was interrupted.

## Support and policies

Operator: Peter Mamrukov (Israel). Contact: peter.mamrukov@localcmd.app.
[Support](https://localcmd.app/support) · [Privacy](https://localcmd.app/privacy) · [Terms](https://localcmd.app/terms).
The service includes file writes and command execution; this is not a read-only connector. Service terms apply separately from this configuration's MIT license.

Use a personal installation or per-member Personal OAuth access. Do not share one person's Local Commander grant as organization-wide credentials. Start a new Devin session after changing plugin configuration.
