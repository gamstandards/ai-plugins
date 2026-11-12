# GAMS Standards REST API (no sign-in)

Use these when MCP is not connected — any tool that can fetch a URL works. All
responses are JSON unless the path ends in `/markdown`. Index: GET https://open.gamstandards.com/api/ai

| Purpose | Endpoint |
| --- | --- |
| Catalog of published collections (ids, names, counts, licenses, URLs) | GET https://open.gamstandards.com/api/ai/collections |
| Public working groups | GET https://open.gamstandards.com/api/ai/groups |
| One public group and available public roster entries | GET https://open.gamstandards.com/api/ai/groups/{groupId} |
| One collection as JSON (templates, schemas, references, FAQs, terms, hints) | GET https://open.gamstandards.com/api/ai/collections/{collectionId} |
| The same as one Markdown document | GET https://open.gamstandards.com/api/ai/collections/{collectionId}/markdown |
| Contents outline of every template and reference in a collection | GET https://open.gamstandards.com/api/ai/collections/{collectionId}/outlines |
| Full text of a template (JSON with markdown, outline, stats) | GET https://open.gamstandards.com/api/ai/collections/{collectionId}/templates/{templateId}/contents |
| … as Markdown | GET https://open.gamstandards.com/api/ai/collections/{collectionId}/templates/{templateId}/contents/markdown |
| Full text of a reference document or web link | GET https://open.gamstandards.com/api/ai/collections/{collectionId}/references/{referenceId}/contents |
| … as Markdown | GET https://open.gamstandards.com/api/ai/collections/{collectionId}/references/{referenceId}/contents/markdown |
| JSON Schema of a data schema | GET https://open.gamstandards.com/api/ai/collections/{collectionId}/schema/{schemaId} |
| Flat field table of a data schema | GET https://open.gamstandards.com/api/ai/collections/{collectionId}/schema/{schemaId}/fields |
| Original template file (Excel, Word, PDF …) | GET https://open.gamstandards.com/api/collections/{collectionId}/templates/{templateId}/download?inline=true |
| Schema in another format (json, excel, xsd, proto, xbrl) | GET https://open.gamstandards.com/api/collections/{collectionId}/schemas/{schemaId}/download?format={format} |
| Original reference document | GET https://open.gamstandards.com/api/collections/{collectionId}/references/{referenceId}/download?inline=true |

## Promotion drafts
For resource announcements and working group posts, use the public group and
collection endpoints above, then draft in the calling assistant using
[promotion.md](references/promotion.md). Collect contributor confirmations, go-live details and
published style examples locally in the conversation. No dedicated prompt-building
endpoint, server-side AI call or sign-in is needed. David must review and approve
the final post before publication.

## Pagination
Contents are split into small parts of roughly 16,000 characters. All contents endpoints
accept `?part=n` (1-based). JSON responses carry `part`, `total_parts` and
`next_part_url` (null on the last part); Markdown responses put **Content part** and
**Next part** above the body. MCP contents tools accept `part` and return explicit
**Next MCP call** arguments above the body. Read every part before claiming full-document
coverage; if any part is unavailable, state the gap. The outline and stats always describe
the whole document. Markdown/MCP outlines and descriptions may be explicitly abridged
with a link to the full JSON metadata so they cannot crowd out the document text.
`truncated` means extraction loss, not pagination.

## Page URLs
Collection pages live at https://open.gamstandards.com/collections/{slug} (a UUID also resolves).
Public group pages live at https://open.gamstandards.com/groups/{groupId}.
Append `?format=json` or `?format=markdown` to any collection page URL to be redirected
to the API rendition. Section anchors: #templates, #schemas, #resources, #faqs, #terms.

## Licensing
Every JSON response and Markdown rendition carries the collection's content license
(`license` object with `spdx_id`, `name`, `url`, `attribution`, `usage_note`). Collections
without an explicit license carry the open GAMS default (CC0-1.0, `default: true`).

Plain-text guide for LLM tools: https://open.gamstandards.com/llms.txt · full catalog: https://open.gamstandards.com/llms-full.txt
