# ZAI Web Search MCP Server for Zed

This extension integrates [z.ai Web Search](https://z.ai) as a Model Context Protocol (MCP) server for Zed's Assistant, providing web search capabilities directly in your prompts.

## What is z.ai Web Search?

z.ai Web Search provides real-time web search results that can be used by the LLM to answer questions with up-to-date information from the internet.

### ✅ With Web Search

- Up-to-date information from the web
- Real-time search results in your prompts
- No hallucinated or outdated information

## Installation

This extension can be installed from the Zed extension registry.

## Agent Mode Configuration

If you're using Zed's agent mode, you need to enable this context server for your assistant:

1. Open Zed's assistant settings
2. Enable the ZAI Web Search MCP server. If you see that the status of the tool is a red dot, make sure you toggle it so that becomes green.
3. Enable the ZAI Web Search MCP Server in the active assistant profile. In the chat section, click on the `Write | Ask` button, then click on `tools`, then enable the ZAI Web Search MCP Server.

## API Key Configuration

You need a z.ai API key to use this extension.

Add your API key in the extension settings:

```json
{
  "context_server": {
    "mcp-server-zai-web-search": {
      "source": "extension",
      "enabled": true,
      "settings": {
        "zai_api_key": "YOUR_ZAI_API_KEY"
      }
    }
  }
}
```

## How It Works

The extension uses [supergateway](https://github.com/supercorp-ai/supergateway) to bridge the remote Streamable HTTP MCP server at `https://api.z.ai/api/mcp/web_search_prime/mcp` to a local stdio connection that Zed can communicate with.

## Lighter alternative: connect over HTTP, no bridge

Zed can talk to the remote endpoint directly. Declare the server under the same
id in your `settings.json` and the extension's bridge is not started:

```json
{
  "context_servers": {
    "mcp-server-zai-web-search": {
      "url": "https://api.z.ai/api/mcp/web_search_prime/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_ZAI_API_KEY"
      }
    }
  }
}
```

Tool names stay the same. This saves one Node.js process per open project,
and one per external agent session (for example Claude via ACP), because Zed
hands agents the HTTP server instead of the bridge command.

## Development

Clone the project and build:

```bash
cargo build
```

## License

Apache-2.0