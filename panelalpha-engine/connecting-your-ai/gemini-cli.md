---
title: Connecting Gemini CLI
description: Connect Gemini CLI to your engine. Get the command on your VPS, run it on your computer, then check the connection.
nav:
  group: Connecting Your AI
  order: 8
  title: Gemini CLI
---

# Connecting Gemini CLI

To connect Gemini CLI, you get a command on your VPS and run it on your computer.

## 1. Get the command on your VPS

```bash
pae connect gemini
```

Run this on your VPS. It creates a token and prints the exact command, with your address and token already filled in.

## 2. Run that command on your computer

Paste what your VPS printed. It looks like this:

```bash
gemini mcp add --transport http --header "Authorization: Bearer <token>" panelalpha-engine https://<host>:2011/mcp
```

Gemini CLI expects every option before the name and address. Keep the quotes around the header, and keep the word `Bearer` with a single space after it.

## 3. Confirm it works

```bash
gemini mcp list
```

`panelalpha-engine` should be listed. Then start Gemini CLI and ask it something read-only:

```text
List the projects on this engine.
```

An empty list on a new engine is a success.

## Troubleshooting

**It registered, then will not connect.**
Your engine has a self-signed certificate that your computer does not trust. Gemini CLI runs on Node, so it needs the same fix as Claude Code: [Trust a self-signed certificate](claude-code.md#trust-a-self-signed-engine-certificate). The lasting fix is giving the engine a real certificate: [Install](../getting-started/install.md#if-connecting-asks-you-to-trust-a-certificate).

**The command is rejected as invalid.**
The options are in the wrong place. Everything starting with `--` goes before `panelalpha-engine` and before the address. See step 2.

**401 Unauthorized.**
The token was cut off when you copied it, or it is the wrong type. Assistants need a token from `pae connect`, not `pae api:token:create`.

**405 Method Not Allowed.**
`--transport http` is missing or was set to something else.

**It connects but can barely do anything.**
The permission settings on your VPS are hiding some of its abilities. See [Decide what the assistant may do](your-assistant.md#decide-what-the-assistant-may-do).

**I want to start over.**

```bash
gemini mcp remove panelalpha-engine
```
