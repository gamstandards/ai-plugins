# What this package can and cannot do

**Version 1.1.8** · published by Global Asset Management Standards at
https://open.gamstandards.com/use-with-ai and in the GAMS repository https://github.com/gamstandards/ai-plugins, which the
Claude directory installs from. Only install copies from one of those two places; for a
downloaded zip, check the SHA-256 shown on that page against your file.

## Contents
- Instructions (a "skill") that tell your assistant how to use GAMS Standards well:
  which tool to call, how to cite, how to respect content licenses.
- Reference sheets describing the GAMS MCP tools and REST endpoints.
- Platform manifests that point your assistant at the GAMS server.
- No code, scripts, hooks, binaries or hidden files. Every file is plain text or an icon.

## Network access
- The only server contacted is **https://open.gamstandards.com** (HTTPS), plus its sign-in service
  **the authorization server listed at https://open.gamstandards.com/.well-known/oauth-protected-resource/mcp** if you choose to sign in.
- What is sent: the assistant's tool calls — collection, group and item ids, search
  queries and questions you ask about the standards. Promotion drafting and pasted
  style examples stay in the calling assistant; GAMS does not receive these local
  inputs. Your own documents are never uploaded by this package.

## Permissions
- **Read-only.** Every GAMS tool only reads published content; none creates, changes or
  deletes anything on GAMS. Promotion drafts are written by the calling assistant;
  GAMS does not call an AI or publish a post. David’s review and approval are required
  before publication.
- **No secrets.** The package contains no API keys, tokens or credentials, and never
  asks you to type any.
- **Sign-in is optional** and only needed for the `search` and `ask` tools. It uses
  standard OAuth 2.1 (authorisation code with PKCE) with your GAMS account, and grants
  only the `ask` scope. You can revoke it from your assistant's connector settings at
  any time.
- **Rate limits** apply to signed-in tools so the service stays available to everyone.

## Removing it
Delete the plugin or agent from your assistant's plugin/agent settings. Nothing is left
behind on your device or on GAMS.

## Reporting a problem
Security or privacy concerns: see https://open.gamstandards.com/gdpr.md for the data-protection contact.
