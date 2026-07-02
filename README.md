# ChatGPT Session Exporter Chrome

A small Manifest V3 Chrome/Chromium extension that copies the current ChatGPT conversation to your clipboard as **JSON** or **Markdown**.

Repository: <https://github.com/tunnckoCore/chatgpt-session-exporter-chrome>

## What it does

- Reads the active chat tab only when you click the extension button.
- Extracts conversation metadata: title, URL, platform, conversation id, and message count.
- Copies the session to your clipboard as either:
  - JSON with `content` and formatted `content_markdown` fields
  - Markdown rendered from the JSON payload
- Preserves fenced code blocks and code-block newlines.
- Avoids the browser downloads API entirely, so there is no save-dialog crash path in Helium.

## Install locally

```bash
git clone git@github.com:tunnckoCore/chatgpt-session-exporter-chrome.git
cd chatgpt-session-exporter-chrome
```

Then in Chrome, Chromium, Brave, or Helium:

1. Open `chrome://extensions`.
2. Enable **Developer mode**.
3. Click **Load unpacked**.
4. Select the cloned `chatgpt-session-exporter-chrome` folder.
5. Open a ChatGPT conversation, click the extension icon, choose **JSON** or **Markdown**, then click **Copy current chat**.

After editing the extension locally, go back to `chrome://extensions` and click reload on the extension card.

## Supported extraction

The extension has first-class extraction for ChatGPT-style pages via `[data-message-author-role]`.

It also includes best-effort adapters/fallback heuristics for Claude, Gemini, Copilot, and generic message-like pages. AI chat UIs change markup often, so non-ChatGPT extraction is intentionally heuristic.

## Privacy model

The extension does not send conversation data to a server. It reads the active tab only after you click the extension, builds the export locally, and writes the generated text to your clipboard.

## JSON shape

```json
{
  "schema": "chat-session-exporter.v1",
  "exported_at": "ISO-8601 timestamp",
  "source": {
    "platform": "chatgpt",
    "title": "Conversation title",
    "url": "https://...",
    "conversation_id": "optional-id"
  },
  "message_count": 2,
  "messages": [
    {
      "index": 0,
      "role": "user",
      "author": "user",
      "content": "Plain text with preserved newlines where possible",
      "content_markdown": "Markdown content with fenced code blocks preserved"
    }
  ],
  "warnings": []
}
```

## Branch

Default branch: `master`.
