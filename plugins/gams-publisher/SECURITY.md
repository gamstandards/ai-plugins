# What this package can and cannot do

**Version 1.3.1** · published by Global Asset Management Standards at https://publishers.gamstandards.com/settings/ai
and in the GAMS repository https://github.com/gamstandards/ai-plugins, which the Claude directory installs from.
Only install copies from one of those two places; for a downloaded zip, check the SHA-256 shown on
that page against your file.

## Contents

- Instructions (a "skill") that tell your assistant how to use the GAMS Publisher tools.
- A reference sheet describing the tools.
- Platform manifests that point your assistant at the GAMS Publisher server.
- No code, scripts, hooks, binaries or hidden files. Every file is plain text or an icon.

## Network access

- The only server contacted is **https://publishers.gamstandards.com/mcp** (HTTPS), plus the GAMS sign-in service it names
  at https://publishers.gamstandards.com/.well-known/oauth-protected-resource/mcp.
- What is sent: the assistant's tool calls (group, collection, file and thread ids, search queries).
  Your own local files are never uploaded by this package.

## Permissions

- **Sign-in is required.** It uses OAuth 2.1 (authorisation code with PKCE) with your GAMS account,
  and you approve the access on a GAMS consent screen. Tokens are bound to this server only.
- **Read-only unless you allow changes.** Change tools exist only if you tick "Also allow changes" on the
  consent screen. They change only what you could change yourself in the Publisher Console, show a dry run
  first, never delete, and are labelled "via" your assistant in notifications. Untick it by reconnecting.
- **Your permissions, nothing more.** Every call runs as you; GAMS applies the same access checks as the
  Publisher Console.
- **No secrets.** The package contains no keys, tokens or credentials.
- **Revoke at any time** at https://publishers.gamstandards.com/settings/ai.

## Reporting a problem

Security or privacy concerns: see https://open.gamstandards.com/gdpr.md for the data-protection contact.
