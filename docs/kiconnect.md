# Using KI:connect.nrw as a Custom Coding Agent in VS Code

## Overview

KI:connect.nrw exposes an OpenAI-compatible API, so any VS Code extension that accepts a custom OpenAI-compatible provider can use it as a coding agent. You only need three things: the base URL, your API key and the exact model name.

| Setting | Value |
| --- | --- |
| Base URL | `https://chat.kiconnect.nrw/api` |
| Auth | `Authorization: Bearer <your API key>` |
| Endpoints | `chat/completions`, `responses`, `embeddings`, `models` |
| Model name | The model's display name from the KI:connect model overview, e.g. `openai-gpt-oss-120b` |

This tutorial covers three ways to wire it in: Cline (a full autonomous agent), Continue (chat, inline edits, agent mode and autocomplete) and VS Code's built-in chat via its Custom Endpoint provider. Pick one; Cline is the quickest route to a real agent.

## Prerequisites and API key

You need VS Code, an account at an NRW university that offers KI:connect, and a personal API key.

1. Log in at [chat.kiconnect.nrw](https://chat.kiconnect.nrw/app) with your university single sign-on.
2. Click your name in the lower-left corner and open **API Keys Management**.
3. Give it a name (e.g. `vscode`), click **Generate Key**, and copy the key somewhere safe.
4. Open the model overview (top menu → **Learn more**) and note the exact display name of the model you want, e.g. `openai-gpt-oss-120b`. Spelling, spaces and capitalisation must match exactly.

Which models your university offers varies by tenant. For agent work, prefer a model that supports tool/function calling well; large models such as GPT OSS 120B or the GPT-5 family usually handle agent loops better than small ones.

## Step 1: Test the endpoint

Confirm the key and model name work before touching VS Code; it saves a lot of guesswork later.

macOS / Linux:

```bash
export KICONNECT_API_KEY="paste-your-key-here"

# 1. List the models your key can use
curl -s https://chat.kiconnect.nrw/api/v1/models \
  -H "Authorization: Bearer $KICONNECT_API_KEY"

# 2. Send a test prompt (use a name exactly as listed above)
curl -s https://chat.kiconnect.nrw/api/v1/chat/completions \
  -H "Authorization: Bearer $KICONNECT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "openai-gpt-oss-120b",
    "messages": [{"role": "user", "content": "Write a Python hello world."}]
  }'
```

Windows PowerShell:

```powershell
$env:KICONNECT_API_KEY = "paste-your-key-here"
$headers = @{ Authorization = "Bearer $env:KICONNECT_API_KEY" }

Invoke-RestMethod https://chat.kiconnect.nrw/api/v1/models -Headers $headers

$body = @{
  model    = "openai-gpt-oss-120b"
  messages = @(@{ role = "user"; content = "Write a Python hello world." })
} | ConvertTo-Json -Depth 5
Invoke-RestMethod https://chat.kiconnect.nrw/api/v1/chat/completions `
  -Method Post -Headers $headers -ContentType "application/json" -Body $body
```

The `/models` response lists the `id` values your key accepts; copy one of them verbatim into the extension settings below. If the test prompt returns a `choices` array with code in it, you are ready.

## Option A: Cline (recommended for a full agent)

Cline is an open-source agent that reads and edits files, runs terminal commands and iterates on errors, all with your approval at each step.

1. In VS Code, open the Extensions view (`Ctrl+Shift+X` / `Cmd+Shift+X`), search for **Cline** and install it (publisher: `saoudrizwan.claude-dev`).
2. Click the Cline icon in the activity bar, then the gear icon to open its settings.
3. Fill in the provider fields:

| Field | Value |
| --- | --- |
| API Provider | **OpenAI Compatible** (not "OpenAI") |
| Base URL | `https://chat.kiconnect.nrw/api/v1` |
| API Key | Your KI:connect key, without a `Bearer` prefix |
| Model ID | The exact model name, e.g. `openai-gpt-oss-120b` |

4. Under the model configuration options, set the context window and max output tokens to match the model card if you know them, and leave image support off unless the model accepts images.
5. Click **Done**, type a small task such as "Create a `hello.py` that prints the date" and approve the file write when Cline asks.

Keep the Base URL at `/api/v1`; Cline appends `/chat/completions` itself. Start with auto-approve switched off so you see every command before it runs. Roo Code and Kilo Code are Cline forks and use the same four fields under their "OpenAI Compatible" provider.

Source: [Cline docs — OpenAI Compatible](https://docs.cline.bot/provider-config/openai-compatible).

## Option B: Continue (chat, edits and agent mode)

Continue gives you a chat panel, inline edits (`Ctrl+I` / `Cmd+I`) and an Agent mode that uses tools, all configured in one YAML file.

1. Install **Continue** from the Extensions view.
2. Open the Continue panel, click the gear icon and choose to edit the config. This opens `~/.continue/config.yaml`.
3. Add a model entry pointing at KI:connect:

```yaml
name: KI:connect
version: 1.0.0
schema: v1
models:
  - name: KI:connect GPT OSS 120B
    provider: openai                     # Continue's OpenAI-compatible client
    model: openai-gpt-oss-120b           # exact display name from /models
    apiBase: https://chat.kiconnect.nrw/api/v1
    apiKey: ${{ secrets.KICONNECT_API_KEY }}
    capabilities:
      - tool_use                         # needed for Agent mode
    roles:
      - chat
      - edit
      - apply
    defaultCompletionOptions:
      temperature: 0.2

  # Optional: embeddings for @codebase search
  # - name: KI:connect embeddings
  #   provider: openai
  #   model: <embedding model display name>
  #   apiBase: https://chat.kiconnect.nrw/api/v1
  #   apiKey: ${{ secrets.KICONNECT_API_KEY }}
  #   roles:
  #     - embed
```

4. Put the key in `~/.continue/.env` as `KICONNECT_API_KEY=your-key`, or paste it directly into `apiKey` for a quick local test. Never commit either file.
5. Save; Continue reloads the config. Pick **KI:connect GPT OSS 120B** in the model dropdown, switch the mode selector to **Agent**, and give it a task.

Without `apiBase`, Continue sends your key to OpenAI, so double-check that line. Skip the `autocomplete` role: KI:connect lists no fill-in-the-middle endpoint, and autocomplete fires on every typing pause, which quickly hits the per-user parallel-request limit.

Source: [Continue config.yaml reference](https://docs.continue.dev/reference).

## Option C: VS Code's built-in chat (Custom Endpoint)

VS Code's own Chat view and agent mode can use KI:connect through the **Custom Endpoint** provider, with no extra extension. Bring-your-own-key models work without a GitHub account or Copilot plan.

1. Open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`) and run **Chat: Manage Language Models**.
2. Select **Add Models** → **Custom Endpoint**.
3. Enter a group name (e.g. `KI:connect`), a display name and your API key. VS Code stores the key in secret storage, not in the file.
4. Choose API type **Chat Completions**.
5. VS Code opens `chatLanguageModels.json`. Edit the model entry so it looks like this, then save:

```json
[
  {
    "name": "KI:connect",
    "vendor": "customendpoint",
    "apiKey": "${input:kiconnectApiKey}",
    "apiType": "chat-completions",
    "models": [
      {
        "id": "openai-gpt-oss-120b",
        "name": "KI:connect GPT OSS 120B",
        "url": "https://chat.kiconnect.nrw/api/",
        "toolCalling": true,
        "vision": false,
        "maxInputTokens": 110000,
        "maxOutputTokens": 16000,
        "modelOptions": { "temperature": 0.2 }
      }
    ]
  }
]
```

6. Open the Chat view, pick **KI:connect GPT OSS 120B** in the model picker, switch to Agent mode and give it a task. Restart VS Code if the model doesn't appear.

Three things to know:

- `toolCalling: true` is required for agent mode; models without it are hidden from the agent picker. Set the token limits from the model card; their sum must not exceed the model's context window.
- If you are not signed in to Copilot, set **Chat: Byok Utility Model Default** to **Main Agent Model** (or set `chat.utilityModel` and `chat.utilitySmallModel`) so titles and commit messages still work. Every utility call then counts against your KI:connect limits.
- Inline code completions are not covered by BYOK. On a Copilot Business or Enterprise seat, an admin must allow "Bring Your Own Language Model Key in VS Code" first.

Source: [VS Code docs — AI language models](https://code.visualstudio.com/docs/agent-customization/language-models).

## Troubleshooting

Most failures come from the base URL or the model name; rerun the curl test from Step 1 to isolate which.

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| 404 Not Found | Base URL is `https://chat.kiconnect.nrw/api` without `/v1`, or the full `/chat/completions` path was pasted into a base-URL field | Cline/Continue: use `.../api/v1`. VS Code Custom Endpoint: use the full `.../api/v1/chat/completions` |
| 401 Unauthorized | Wrong key, key pasted with a ` Bearer  ` prefix, or key expired | Paste the bare key; log in to the web UI and **Rotate** the key if it expired |
| 400 or "model not found" | Model name doesn't match the display name exactly | Copy an `id` verbatim from the `/models` response, including spaces and capitals |
| 429 or requests hang | Per-user parallel-request limit or your group's token/cost quota | Don't run several agents at once, disable autocomplete, wait and retry |
| Agent chats but never edits files, or loops | Model handles tool calling poorly | Switch to a larger model; in VS Code check `toolCalling: true` |
| Model missing from VS Code picker | `toolCalling` false, config not reloaded, or BYOK disabled by policy | Fix the JSON, restart VS Code, ask your Copilot admin about the BYOK policy |
| "Context length exceeded" | Long agent session or token limits set too high | Start a new task; lower `maxInputTokens` / context window to match the model card |

## Data protection and usage notes

A coding agent sends file contents, diffs and terminal output to the model on every step, so choose the model with your code's sensitivity in mind.

- **Where data goes:** open-weight models marked "Processing exclusively in Germany" (e.g. GPT OSS 120B, Mistral) run on servers in NRW. Commercial models such as GPT-5 may also process data in the US, though providers only see pseudonymised users.
- **Quota:** API calls share the same group, token and cost limits as the web interface; there is no separate API budget. Agents use far more tokens than chat, so watch your usage.
- **Secrets:** keep `.env` files, credentials and personal data out of the agent's context (Cline supports a `.clineignore` file). Never commit your API key or the config files that contain it.
- **Rules:** check your university's KI:connect terms of use before using it on research data, student data or third-party code.

## References

- [KI:connect](https://chat.kiconnect.nrw/app) | [API documentation](https://chat.kiconnect.nrw/app/api-docs/)
- [Cline](https://cline.bot/) | [Documentation](https://docs.cline.bot/cline-overview)
- [Continue](https://continue.dev/) | [Documentation](https://docs.continue.dev/)
- [AI language models in VS Code](https://code.visualstudio.com/docs/agent-customization/language-models)
