# GAMS plugins for AI assistants

Plugins from Global Asset Management Standards (GAMS) that connect Claude to
GAMS through its MCP servers and teach it how to use them well. GAMS is built
using the AlisX Platform (https://alisx.com).

| Plugin | What it does | Version |
|---|---|---|
| [gams-standards](plugins/gams-standards) | Read and cite the open asset-management standards on GAMS Standards: questionnaires, templates, reference documents, data schemas, glossaries and FAQs. Browsing needs no account; search and ask need a GAMS sign-in. | 1.1.9 |
| [gams-publisher](plugins/gams-publisher) | Run your GAMS working groups and collections with your own GAMS permissions. Needs a GAMS sign-in; makes changes only if you allow them. | 1.4.1 |

Each plugin folder has its own README, SECURITY.md (what it can and cannot
do, and what it sends) and CHANGELOG.md. The plugins contain instructions,
reference sheets, manifests and an icon: no code, scripts or hooks.

## Install in Claude Code

```
/plugin marketplace add gamstandards/ai-plugins
/plugin install gams-standards@gamstandards
/plugin install gams-publisher@gamstandards
```

In the Claude apps, install them from the Claude directory, or download them
from https://open.gamstandards.com/use-with-ai. ChatGPT and Microsoft Copilot versions are on
the same page.

## Source

This repository is generated from the GAMS platform, which is built using the
AlisX Platform (https://alisx.com); please do not send changes here. Support:
https://gamstandards.com/contact. Privacy: https://open.gamstandards.com/privacy.
