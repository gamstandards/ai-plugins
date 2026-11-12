# GAMS Publisher

![GAMS Publisher](assets/icon.png)

Run your GAMS working groups and collections from your AI assistant with your own GAMS permissions: read groups, files, actions, discussions, collections and analytics, and make changes only if you allow them.

## Install

1. Download the zip; no need to unzip it
2. In the Claude desktop app open **Customise → Plugins**, click **Add plugin** and upload the zip, then make sure it is enabled
3. The first time Claude uses a GAMS Publisher tool, a Connect card asks you to sign in with your GAMS account and allow access
4. Claude Code instead: run `claude --plugin-dir ~/Downloads/gams-publisher-claude-1.3.1.zip`, then `/mcp` to sign in

Or add the MCP server yourself: https://publishers.gamstandards.com/mcp (setup steps for each assistant: https://publishers.gamstandards.com/settings/ai).

## Access

- You sign in with your own GAMS account; the assistant sees exactly what you can see in the Publisher Console.
- The connection reads analytics and your groups, collections and documents. It can make changes only if you tick "Also allow changes" when you connect, and only changes you could make yourself.
- See and remove connected assistants at https://publishers.gamstandards.com/settings/ai.

## What it sends, and where

- It runs no code on your device: the package is instructions, a reference sheet, manifests and an icon.
- Your assistant calls one server, https://publishers.gamstandards.com/mcp (HTTPS), after you sign in through the GAMS sign-in service it names. It sends the tool calls your assistant makes: group, collection, file and thread ids, search queries, and the text of any change you approve.
- Your own local files are never uploaded by this package. Privacy notice: https://open.gamstandards.com/privacy.
