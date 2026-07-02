# Chat Session Exporter

A small Manifest V3 Chrome extension that copies the currently open AI chat page as JSON, Markdown, or both.

## Install locally

1. Open `chrome://extensions`.
2. Enable **Developer mode**.
3. Click **Load unpacked**.
4. Select this folder: `chat-session-exporter`.
5. Open a chat page, click the extension icon, choose a format, then click **Copy current chat**.

## Supported extraction

The extension has first-class extraction for ChatGPT-style pages via `[data-message-author-role]`, with adapters/fallback heuristics for Claude, Gemini, Copilot, and generic message-like pages. Modern AI chat UIs change their markup often, so the extractor intentionally uses multiple selectors and falls back to visible message-like containers.

## Privacy model

The extension does not send conversation data to a server. It reads the active tab only after the user clicks the extension and writes the generated export text to your clipboard.

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
      "content": "Plain text",
      "content_markdown": "Markdown-ish content"
    }
  ],
  "warnings": []
}
```
