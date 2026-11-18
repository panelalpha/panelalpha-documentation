---
title: Connecting ChatGPT Desktop
description: ChatGPT Desktop runs Codex behind its chat. On your VPS it prints a prompt you paste into ChatGPT Desktop. That runs the same Codex setup as Codex.
nav:
  group: Connecting Your AI
  order: 7
  title: ChatGPT Desktop
---

# Connecting ChatGPT Desktop

ChatGPT Desktop runs Codex behind its chat. On your VPS it prints a prompt you paste into ChatGPT Desktop. That runs the same Codex setup as [Codex](codex.md).

## 1. Get the prompt on your VPS

```bash
pae connect chatgpt-desktop
```

Run this on your VPS. It creates a token and prints a prompt, with your address and token already filled in.

## 2. Paste it into ChatGPT Desktop

Copy the prompt your VPS printed. Paste it into ChatGPT Desktop. It looks like this:

```text
Add the GitHub marketplace panelalpha/agent-skills and install engine@panelalpha. Then set PANELALPHA_MCP_TOKEN=<token> and run: codex mcp add panelalpha-engine --url https://203.0.113.10:2011/mcp --bearer-token-env-var PANELALPHA_MCP_TOKEN. Confirm panelalpha-engine is connected, then list the projects on this engine.
```

**Copy the prompt your VPS printed, not the one above.** `203.0.113.10` is a placeholder address used throughout this guide.

What this does: ChatGPT Desktop installs create and debug skills, then registers this engine with Codex.

What you should see: it lists the projects on this engine. An empty list on a new engine is a success.

Because Codex looks the token up by name, **the `PANELALPHA_MCP_TOKEN` variable has to be set whenever you run Codex**, not only when you registered the engine. Details: [Connecting Codex](codex.md).

## Troubleshooting

**It registered, then will not connect.**
Your engine has a self-signed certificate that your computer does not trust. The lasting fix is giving the engine a real certificate: [Install](../getting-started/install.md#if-connecting-asks-you-to-trust-a-certificate). If you cannot do that yet, copy the certificate to your computer and add it to your operating system's trust store, the same way as [Codex](codex.md).

**401 Unauthorized.**
The token is wrong, or it is an API token rather than an MCP token. Assistants need one from `pae connect chatgpt-desktop`, not `pae api:token:create`.

**It worked when I registered it, then stopped in a new terminal.**
The `PANELALPHA_MCP_TOKEN` variable is missing. See [Connecting Codex](codex.md).

**It connects but can barely do anything.**
The permission settings on your VPS are hiding some of its abilities. Check with `pae mcp:tool:list` on your VPS: [Decide what the assistant may do](your-assistant.md#decide-what-the-assistant-may-do).
