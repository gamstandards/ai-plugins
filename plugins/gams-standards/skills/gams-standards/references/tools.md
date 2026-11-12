# GAMS Standards MCP tools

Server: `https://open.gamstandards.com/mcp` (Model Context Protocol, Streamable HTTP, read-only).
Discovery manifest: https://open.gamstandards.com/.well-known/mcp.json

Anonymous tools need no sign-in. Tools marked **sign-in** return an OAuth challenge
(HTTP 401 + WWW-Authenticate) the first time; MCP clients such as Claude and ChatGPT
prompt the user to connect their GAMS account and retry automatically. Sign-in only
grants the `ask` scope: search and grounded Q&A under a per-user rate limit.

Collection-scoped tools accept an optional `preview_token` for unpublished collections
(the `?preview=` value of a preview URL a publisher shared with the user).

## list_collections

List all published resource collections on GAMS Standards. Returns collection IDs, names, descriptions, status, template/schema counts, and API URLs. The full text of every template and reference document is available via the get_template_contents and get_reference_contents tools.

Inputs: none.

## list_groups

List the active and establishing GAMS working groups in the public directory. Returns group IDs, names, aims, status and public page URLs. Use this to find the group when preparing a LinkedIn post with the gams-promotion skill. No sign-in required.

Inputs: none.

## get_group

Get a public GAMS working group’s aims and public roster (names, firms and roles only). The roster is a list of potential credits: confirm who contributed to the specific work before thanking or tagging anyone. Private groups are not returned. No sign-in required.

Inputs:
- `group_id` (string, required) — The group ID returned by list_groups.

## get_collection

Get full details for a specific collection including all templates (with download URLs), data schemas (with parsed JSON Schema), references, FAQs, and glossary terms. To read the full text of a template or reference (e.g. the complete question bank of a questionnaire), use get_template_contents or get_reference_contents.

Inputs:
- `collection_id` (string, required) — The collection: its UUID, its URL slug (e.g. "term-sheet") or its page link (a URL ending /collections/term-sheet). A link the user shared can be passed as it is.

## get_schema

Get a JSON Schema definition for a specific data schema. Returns the full parsed JSON Schema object with all field definitions, types, and constraints, plus download URLs for alternative formats (Excel, XSD, Protobuf, XBRL). Use this to understand the data structure for extraction workflows.

Inputs:
- `collection_id` (string, required) — The collection: its UUID, its URL slug (e.g. "term-sheet") or its page link (a URL ending /collections/term-sheet). A link the user shared can be passed as it is.
- `schema_id` (string, required) — The schema UUID

## get_schema_fields

Get a flattened table of all fields in a data schema with their paths, types, whether they are required, descriptions, and validation constraints. Useful for understanding what data to extract from a source document.

Inputs:
- `collection_id` (string, required) — The collection: its UUID, its URL slug (e.g. "term-sheet") or its page link (a URL ending /collections/term-sheet). A link the user shared can be passed as it is.
- `schema_id` (string, required) — The schema UUID

## get_template_contents

Read a template file (Excel, Word, PDF, text) as Markdown with an outline. Long documents are returned in parts with a parts list saying which sections each part holds: follow Next MCP call to the final part to read the full contents or complete question bank, or pass section to go straight to the part you need. Use download_template_url for the original file bytes.

Inputs:
- `collection_id` (string, required) — The collection: its UUID, its URL slug (e.g. "term-sheet") or its page link (a URL ending /collections/term-sheet). A link the user shared can be passed as it is.
- `template_id` (string, required) — The template UUID
- `part` (integer) — 1-based content part (default 1). Read all parts using the next tool arguments returned before the body; the outline and the parts list describe the entire document.
- `section` (string) — Jump to the part holding this section: a section title from the parts list, or any text such as a field ID, a defined term or a question number. Returns that part and names the other parts where it appears. Takes precedence over part.
- `part_chars` (integer) — Characters per part (default 16000, up to 64000). Larger parts mean fewer calls for long documents; keep the same value for every part of one document.

## get_reference_contents

Read a reference as Markdown with an outline. Long documents are returned in parts with a parts list saying which sections each part holds: follow Next MCP call to the final part to read the full contents, or pass section to go straight to the part you need. Document references (PDF, Word, Excel) are parsed from the file GAMS publishes. A bookmark reference is a link a working group added to the collection; it returns the text of that public web page, which is third-party content GAMS does not author.

Inputs:
- `collection_id` (string, required) — The collection: its UUID, its URL slug (e.g. "term-sheet") or its page link (a URL ending /collections/term-sheet). A link the user shared can be passed as it is.
- `reference_id` (string, required) — The reference UUID
- `part` (integer) — 1-based content part (default 1). Read all parts using the next tool arguments returned before the body; the outline and the parts list describe the entire document.
- `section` (string) — Jump to the part holding this section: a section title from the parts list, or any text such as a field ID, a defined term or a question number. Returns that part and names the other parts where it appears. Takes precedence over part.
- `part_chars` (integer) — Characters per part (default 16000, up to 64000). Larger parts mean fewer calls for long documents; keep the same value for every part of one document.

## download_template_url

Get a download link for the original template file (Excel, PDF, Word). The link is for a person: assistants usually cannot fetch binary files. To read the template, use get_template_contents. If the user needs the file itself, for example to populate or submit it, share this link and ask them to download it and attach it to the conversation.

Inputs:
- `collection_id` (string, required) — The collection: its UUID, its URL slug (e.g. "term-sheet") or its page link (a URL ending /collections/term-sheet). A link the user shared can be passed as it is.
- `template_id` (string, required) — The template UUID

## get_terms

Get all glossary terms and definitions for a collection. Use this to understand domain-specific terminology when extracting or interpreting data.

Inputs:
- `collection_id` (string, required) — The collection: its UUID, its URL slug (e.g. "term-sheet") or its page link (a URL ending /collections/term-sheet). A link the user shared can be passed as it is.

## get_faqs

Get all frequently asked questions and answers for a collection.

Inputs:
- `collection_id` (string, required) — The collection: its UUID, its URL slug (e.g. "term-sheet") or its page link (a URL ending /collections/term-sheet). A link the user shared can be passed as it is.

## search — sign-in

Search across published GAMS content — templates, schemas, references, documents, FAQs, and glossary terms — with relevance-ranked results, snippets, and extractive answers. Scope to specific collections or search everything. Requires sign-in; without it this tool returns an authentication challenge.

Inputs:
- `query` (string, required) — Free-text search query
- `collections` (array of string) — Optional scope: up to 20 collections, each as a UUID, "collections/{uuid}" resource name, URL slug, or collection page URL. Omit to cover all published collections.
- `content_types` (array of string) — Optional filter to specific content types.
- `page_size` (integer) — Maximum results (default 10).
- `page_token` (string) — Pagination token from a previous response.

## ask — sign-in

Ask a question answered from published GAMS content, with citations. Scope to specific collections or ask across everything. Pass the returned session id with a follow-up question to keep context. Requires sign-in; without it this tool returns an authentication challenge.

Inputs:
- `question` (string, required) — The question to answer (up to 2000 characters)
- `collections` (array of string) — Optional scope: up to 20 collections, each as a UUID, "collections/{uuid}" resource name, URL slug, or collection page URL. Omit to cover all published collections.
- `session` (string) — Session id from a previous ask response, for follow-up questions.
