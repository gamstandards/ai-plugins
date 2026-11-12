---
name: gams-publisher
description: "Use when the user works on GAMS (Global Asset Management Standards) working groups or collections as a publisher, chair, secretary or member: preparing a working group meeting, chasing actions, catching up on discussions, reading or quoting working-group documents, reviewing a collection or its drafts before publishing, looking for FAQ gaps, triaging and replying to discussions, updating collections, FAQs, terms and actions, or reporting engagement analytics. Uses the GAMS Publisher MCP tools, which act with the user's own GAMS permissions; changes need the user's consent and are shown as a dry run first."
license: CC0-1.0
compatibility: "Needs the GAMS Publisher MCP server (publishers.gamstandards.com/mcp) with the user signed in to GAMS. Works in any assistant that supports remote MCP servers with OAuth."
metadata:
  version: "1.4.1"
  homepage: "https://publishers.gamstandards.com/settings/ai"
  publisher: "Global Asset Management Standards"
---

# GAMS Publisher

GAMS working groups write industry standards (templates, questionnaires, data schemas,
reference documents, FAQs and glossaries) and publish them as collections on
https://open.gamstandards.com. The Publisher Console (https://publishers.gamstandards.com) is where they run their
groups and collections. These tools give you the same view of the console as the signed-in
user: their groups, files, actions, discussions, collections and drafts, and analytics. If the
user allowed changes when connecting, you can also make the changes they could make themselves.

## Rules

1. **You act as the user, with their permissions.** If a tool says something was not found
   or the user has no access, say so plainly; membership of the working group (or a GAMS
   admin) is needed. Never try other names to get around it.
2. **Change only what the user asked for, and show it first.** Change tools exist only if the
   user ticked "Also allow changes" when connecting. Before any change: read the item (that gives
   you its `etag`), call the change tool with `dry_run: true`, show the user the field-by-field
   result, and make the real call only after they agree. Pass an `idempotency_key` so a retry
   cannot apply a change twice. If a change is refused because the item changed (etag), read it
   again and redo the change on the current version; never overwrite someone else's edit blindly.
   Without change tools, draft the change and give the user the console link.
3. **Never publish unasked.** New FAQs, updates and drafts stay drafts. `publish_collection` and
   `rollback_collection` affect what the public sees: use them only when the user explicitly asks,
   after a dry run. Only a working group's admins may publish, unpublish or archive.
4. **Discussions are people.** Before replying in a thread, read it (`get_thread`), and show the
   user your reply and who will be notified (dry run). Posts are labelled as written with AI assistance.
5. **Quote exactly.** `read_document` returns a document's own text. Quote it word for word and
   name the document; never present a paraphrase as its wording. Do not summarise a document
   you have not read in full: read every part.
6. **Unpublished content is private.** Drafts, unpublished collections, group files and
   discussions belong to the working group. Use them in this conversation; do not post,
   email or publish them unless the user explicitly asks.
7. **Preview links do not expire.** Share a `previewUrl` only with people the user says may see
   unpublished content.
8. **Numbers.** Read `list_metric_definitions` before explaining analytics. "Visits" are browsing
   sessions, not people. Quote numbers exactly and state the date window.
9. **Link back.** Results carry `consoleUrl` links; include them so the user can check and act.
10. **Documents: suggest, don't overwrite.** `edit_document` makes tracked changes and comments
   in the user's own name by default, for the group to accept or reject. Use `mode: direct` only
   when the user asks (`recommended_mode` says when only they work on the file). Never edit while
   someone has the file open: say who, and offer to try later or post in the group's discussion.

## Workflows

**Prepare a working group meeting.** `list_working_groups` → `list_actions` (open actions,
owners, due dates) → `list_threads` for the group (recent discussions) → `get_thread` for the
ones that matter → `list_resources` and `read_document` for the last minutes or agenda. Produce an
agenda with open actions, decisions needed and links.

**Review a collection before publishing.** `get_collection` (state, counts) →
`list_collection_items` with `state: draft` for each type → `get_collection_item` or
`read_document` to check content → `get_faq_opportunities` for questions readers ask that the
content does not answer → `get_preview_link` so reviewers can see it. List what is missing or
inconsistent, item by item, with console links.

**Catch up on discussions.** `list_threads` with `needs_attention: true` and
`include_unread_messages: true` (one call: threads with unread messages or mentions of the user,
new messages inline) → `get_thread` with `unread_only` where more context is needed → draft
replies → `reply_to_thread` with `dry_run`, then for real once the user agrees →
`mark_threads_read` for the threads handled.

**Fill FAQ gaps.** `get_faq_opportunities` → draft answers from the collection's own content
(quote it) → `add_faq_opportunity_as_draft` or `upsert_faqs` (drafts) with `dry_run` first →
tell the user the drafts are ready for review in the console.

**Update a collection.** `get_collection` → `update_collection` / `set_collection_license` /
`upsert_terms` with `dry_run` → show the diff → apply.

**Chase actions after a meeting.** `list_actions` → `update_action` (status, owners, due
dates) and `create_action` for new ones, each with `dry_run` first.

**Edit a working-group document.** `get_document_outline` (anchors, hashes, comments, tracked
changes, version, who has it open) → `read_document` if you need the full text → `edit_document`
with `base_version`, an `expect_hash` per paragraph and `dry_run: true` → show the user the
report → apply. If someone has the document open in the editor, the edit is queued and added when
they close it: tell the user it is queued, not saved (they can add it at once with "Add them now" in the
editor), and check it with `list_pending_edits`. Reviewing: list the comments and tracked changes from the outline, reply or resolve
comments, and accept or reject changes only as the user decides. Undo with
`list_document_versions` and `restore_document_version`.

**Start a document.** `create_document` with Markdown (Word) or rows (Excel), `dry_run` first;
then edit it like any other. `export_pdf` gives a short-lived PDF link to share.

**Fill a questionnaire template.** `get_document_outline` with `content_control_tags` (or all
content controls) → `edit_document` with `set_content_control` per field (tag or id, the value,
its `expect_hash`), `dry_run` first. Dropdowns take a listed option, dates YYYY-MM-DD.

**Summarise what changed.** `list_document_versions` → `compare_document_versions` from the
version the user last saw → summarise by section, quoting changed text exactly; name who saved each
version from the version list.

**Report engagement.** `list_metric_definitions` → `get_group_metrics` or
`get_collection_metrics` (or `get_platform_metrics` for Analytics Admins) → `get_metrics_report`
for monthly tables. State the window and what the metrics measure.

**Find something.** `search` across groups, files, discussions and collections; then the
matching `get_` tool.

## Tools

| Tool | What it does |
|---|---|
| `whoami` | Shows the GAMS account this connection acts as, its platform roles, the access this connection was granted, and whether it can read platform-wide analytics. |
| `list_working_groups` | Lists the GAMS working groups you can see, with their ids for the group analytics tools. |
| `list_metric_definitions` | Explains every analytics column in plain language, with the notes that apply to every figure and aiCountingStart, the day AI assistant use began to be counted. |
| `get_platform_metrics` | Engagement totals for all of GAMS and one row per collection with activity, labelled with its working group. |
| `get_group_metrics` | Engagement totals for each collection of a working group, including collections with no activity. |
| `get_collection_metrics` | Engagement totals for one collection, visitor and AI assistant figures apart; with include_report, also its monthly, weekly, per-country and AI Use tables. |
| `get_metrics_report` | The full analytics report as tables, the same data as the console's workbook download: visits per month, per month and country, per week, and per collection per month and per week, then AI assistant use per month, per assistant per month and per collection per month. |
| `get_group` | Details of one working group you belong to: description, state and console link. |
| `list_resources` | Lists the files and bookmarks in a working group's Resources tab, with names to pass to read_document. |
| `get_resource` | Details of one working-group file or bookmark: type, size and whether read_document can read it. |
| `list_actions` | Lists a working group's action items (reference, description, owners by name, due date, status) with the etag update_action needs. |
| `list_collections` | Lists the collections you can manage (your working groups'), including unpublished ones, with state and links. |
| `get_collection` | One collection's details, state, licence and links, with item counts by type. |
| `list_collection_items` | Lists a collection's templates, references, schemas, terms, FAQs or updates, including drafts, with each item's state. |
| `get_collection_item` | One collection item in full: a template's file details, a reference's source, a schema's JSON Schema, a term, a FAQ or an update's contents. |
| `get_faq_opportunities` | Questions people asked the GAMS assistant about a working group's published collections that the content answers poorly, ranked by priority, with suggested FAQ wording. |
| `search` | Searches your working groups, their files and discussions, and your collections and their items by keyword and meaning. |
| `get_preview_link` | Links to a collection on the public site: the public link once it is published, and a private preview link (for drafts) if you can edit it. |
| `list_threads` | The person's discussion inbox across their working groups, most recent activity first. |
| `get_thread` | One discussion with its messages, oldest first: author names, the full Markdown text as written (@-mentions shown as @Name), mentions, references, attachment details and aiAssisted. |
| `create_thread` | Start a discussion in a working group as the signed-in person; the first message is marked as written with AI assistance. |
| `reply_to_thread` | Post a reply in a discussion as the signed-in person, marked as written with AI assistance. |
| `update_thread` | Close, reopen or archive a discussion, or change its subject. |
| `mark_threads_read` | Mark discussions as read for the signed-in person. |
| `read_document` | The text of a working-group file, collection template or document reference (docx, xlsx, pdf, csv, txt, md, json), rendered as Markdown with headings, tables and sheet names: the content itself, never a summary. |
| `update_collection` | Changes a collection's descriptive details: display_name, description, benefits (replaces the whole list), logo_tile_hex_color_code, last_validated and slug. |
| `set_collection_license` | Sets a collection's content licence, shown on its public page and to AI tools. |
| `publish_collection` | Publishes, unpublishes or archives a collection. |
| `list_collection_revisions` | Lists a collection's saved revisions of its details (name, description, benefits, logo, licence, state, slug), newest first: revision name, time and which fields changed from the revision before. |
| `rollback_collection` | Restores a collection's details (display name, description, benefits, logo and colour, last-validated date, licence) to an earlier revision from list_collection_revisions. |
| `upsert_terms` | Adds and edits a collection's glossary terms in one call (up to 50). |
| `upsert_faqs` | Adds and edits a collection's FAQs in one call (up to 50). |
| `create_update` | Adds an update (news item or release note) to a collection: title, optional subtitle, markdown body and publication date. |
| `add_faq_opportunity_as_draft` | Turns a FAQ opportunity from get_faq_opportunities into a draft FAQ in its collection and marks the opportunity as added, optionally with an edited question or answer. |
| `create_action` | Add an action item to a working group's Actions list, as the signed-in person. |
| `update_action` | Change an action item: status, owners, due date, description or workstream. |
| `add_bookmark` | Add a link to a working group's Resources tab, as the signed-in person. |
| `update_resource_details` | Rename a working-group file or bookmark, or change its description. |
| `get_document_outline` | The structure of a Word (.docx) or Excel (.xlsx) document, for editing it with edit_document. |
| `edit_document` | Change a Word (.docx) or Excel (.xlsx) document with structured operations, as the signed-in person. |
| `list_pending_edits` | Edits made with edit_document while the document was open in the editor wait in a queue until the editor session ends. |
| `create_document` | Create a new Word (.docx) or Excel (.xlsx) document in a working group's files, as the signed-in person, ready to open and co-edit in the editor. |
| `export_pdf` | Convert the current version of a Word, Excel or PowerPoint document to PDF and return a download link for the person. |
| `list_document_versions` | A document's saved versions, newest first: when, by whom and how (in the editor, through an AI assistant, or a restore). |
| `compare_document_versions` | Compare two versions of a Word or Excel document and list what changed, for summaries such as "what changed since last week". |
| `restore_document_version` | Make an earlier version of a document current again, as the signed-in person: for example to undo an edit. |

Full arguments: [references/tools.md](references/tools.md).
