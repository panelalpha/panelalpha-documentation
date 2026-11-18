---
title: Connecting Codex
description: Connect Codex to your engine. Get the command on your VPS, run it on your computer, then check the connection.
nav:
  group: Connecting Your AI
  order: 6
  title: Codex
---

# Connecting Codex

To connect Codex, you get a command on your VPS and run it on your computer.

## 1. Get the command on your VPS

```bash
pae connect codex
```

Run this on your VPS. It creates a token and prints the exact command, with your address and token already filled in.

## 2. Run that command on your computer

Paste what your VPS printed. Skills first (two commands), then MCP. It looks like this:

```bash
codex plugin marketplace add panelalpha/agent-skills
codex plugin add engine@panelalpha
PANELALPHA_MCP_TOKEN='<token>' codex mcp add panelalpha-engine --url https://<host>:2011/mcp --bearer-token-env-var PANELALPHA_MCP_TOKEN
```

The first two commands install create and debug skills from `panelalpha/agent-skills`. The third registers this engine. The token goes in front of that command, so Codex stores the *name* of the variable and not the token itself. The first two commands are separate lines so they paste in both bash and PowerShell. The third line sets the variable the bash way (`VAR=value command`). On PowerShell, set `PANELALPHA_MCP_TOKEN` first, then run `codex mcp add`.

Because Codex looks the token up by name, **the `PANELALPHA_MCP_TOKEN` variable has to be set whenever you run Codex**, not only when you registered the engine. Add it to your shell startup file:

```bash
echo "export PANELALPHA_MCP_TOKEN='<token>'" >> ~/.bashrc
```

Use `~/.zshrc` instead if you use zsh. Then open a new terminal.

## 3. Confirm it works

```bash
codex mcp list
```

`panelalpha-engine` should be listed. Then ask it something read-only:

```text
List the projects on this engine.
```

An empty list on a new engine is a success.

## Troubleshooting

**It registered, then will not connect.**
Your engine has a self-signed certificate that your computer does not trust. The lasting fix is giving the engine a real certificate: [Install](../getting-started/install.md#if-connecting-asks-you-to-trust-a-certificate). If you cannot do that yet, copy the certificate to your computer and add it to your operating system's trust store:

```bash
scp root@203.0.113.10:/opt/panelalpha/shared-hosting/crt/server.cert ~/panelalpha-engine.cert
```

**It worked when I registered it, then stopped in a new terminal.**
The `PANELALPHA_MCP_TOKEN` variable is missing. Codex stored the variable's name, not the token, so the variable must be present every time you run it. Check with:

```bash
echo "$PANELALPHA_MCP_TOKEN"
```

Empty output is the problem. See step 2.

**401 Unauthorized.**
Either the variable holds the wrong value, or it holds an API token rather than an MCP token. Assistants need one from `pae connect`, not `pae api:token:create`.

**405 Method Not Allowed.**
The connection is set up as something other than HTTP.

**It connects but can barely do anything.**
The permission settings on your VPS are hiding some of its abilities. See [Decide what the assistant may do](your-assistant.md#decide-what-the-assistant-may-do).

**I want to start over.**

```bash
codex mcp remove panelalpha-engine
```
