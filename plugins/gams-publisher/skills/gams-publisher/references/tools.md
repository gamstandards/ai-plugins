# GAMS Publisher tools

Server: https://publishers.gamstandards.com/mcp (sign-in with your GAMS account required). Tools needing publisher.write or documents.write make changes; they appear only when the user allowed changes, and support dry_run.

## `whoami`: Who am I

Shows the GAMS account this connection acts as, its platform roles, the access this connection was granted, and whether it can read platform-wide analytics.

No arguments.

## `list_working_groups`: List working groups

Lists the GAMS working groups you can see, with their ids for the group analytics tools. Sorted active first, then establishing, inactive and archived, with a count per state. Set state to list only one state.

Access needed: analytics.read or publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `state` | string |  | Only groups in this state. Default: all states. One of: active, establishing, inactive, archived. |

## `list_metric_definitions`: Metric definitions

Explains every analytics column in plain language, with the notes that apply to every figure and aiCountingStart, the day AI assistant use began to be counted. Read this before interpreting or reporting any metric: visits count browsing sessions, not people, and AI assistant use is counted separately from them.

Access needed: analytics.read.

No arguments.

## `get_platform_metrics`: Platform metrics

Engagement totals for all of GAMS and one row per collection with activity, labelled with its working group. Needs the Analytics Admin role or GAMS admin.

Access needed: analytics.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `end` | string |  | Last day of the window, YYYY-MM-DD. Default: today. |
| `start` | string |  | First day of the window, YYYY-MM-DD. Default: 12 months before end. |

## `get_group_metrics`: Working group metrics

Engagement totals for each collection of a working group, including collections with no activity.

Access needed: analytics.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `end` | string |  | Last day of the window, YYYY-MM-DD. Default: today. |
| `group` | string | yes | Working group id, "groups/{id}" or a Publisher Console group URL. |
| `start` | string |  | First day of the window, YYYY-MM-DD. Default: 12 months before end. |

## `get_collection_metrics`: Collection metrics

Engagement totals for one collection, visitor and AI assistant figures apart; with include_report, also its monthly, weekly, per-country and AI Use tables.

Access needed: analytics.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `collection` | string | yes | Collection id, "collections/{id}" or a Publisher Console collection URL. |
| `end` | string |  | Last day of the window, YYYY-MM-DD. Default: today. |
| `include_report` | boolean |  | Also return the report tables for this collection. |
| `max_rows` | integer |  | Maximum rows per sheet (default 200, at most 2000). |
| `sheet` | string |  | Only this worksheet: "Visits per Month", "Visits per Month per Country", "Visits per Week", "per Collection per Month", "per Collection per Week", "AI Use per Month", "AI Use per Assistant per Month" or "AI Use per Collection per Month". |
| `start` | string |  | First day of the window, YYYY-MM-DD. Default: 12 months before end. |

## `get_metrics_report`: Metrics report

The full analytics report as tables, the same data as the console's workbook download: visits per month, per month and country, per week, and per collection per month and per week, then AI assistant use per month, per assistant per month and per collection per month. scope is platform (Analytics Admins), group or collection.

Access needed: analytics.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `collection` | string |  | Collection id, "collections/{id}" or a Publisher Console collection URL. |
| `end` | string |  | Last day of the window, YYYY-MM-DD. Default: today. |
| `group` | string |  | Working group id, "groups/{id}" or a Publisher Console group URL. |
| `max_rows` | integer |  | Maximum rows per sheet (default 200, at most 2000). |
| `scope` | string | yes | Report scope. One of: platform, group, collection. |
| `sheet` | string |  | Only this worksheet: "Visits per Month", "Visits per Month per Country", "Visits per Week", "per Collection per Month", "per Collection per Week", "AI Use per Month", "AI Use per Assistant per Month" or "AI Use per Collection per Month". |
| `start` | string |  | First day of the window, YYYY-MM-DD. Default: 12 months before end. |

## `get_group`: Get working group

Details of one working group you belong to: description, state and console link.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `group` | string | yes | Working group: id, "groups/{id}" or a Publisher Console group URL. |

## `list_resources`: List group files

Lists the files and bookmarks in a working group's Resources tab, with names to pass to read_document.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `group` | string | yes | Working group: id, "groups/{id}" or a Publisher Console group URL. |
| `max_results` | integer |  | Maximum results (default 50, at most 200). |

## `get_resource`: Get group file

Details of one working-group file or bookmark: type, size and whether read_document can read it.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `resource` | string | yes | "groups/{g}/resources/{r}" or a Publisher Console resource URL. |

## `list_actions`: List group actions

Lists a working group's action items (reference, description, owners by name, due date, status) with the etag update_action needs. Filter by status.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `group` | string | yes | Working group: id, "groups/{id}" or a Publisher Console group URL. |
| `max_results` | integer |  | Maximum results (default 50, at most 200). |
| `status` | string |  | open (not started or ongoing, the default), one status, or all. One of: open, not_started, ongoing, done, closed, all. |

## `list_collections`: List collections

Lists the collections you can manage (your working groups'), including unpublished ones, with state and links.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `group` | string |  | Only this working group's collections (optional). |
| `max_results` | integer |  | Maximum results (default 50, at most 200). |

## `get_collection`: Get collection

One collection's details, state, licence and links, with item counts by type.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `collection` | string | yes | Collection: id, "collections/{id}" or a Publisher Console collection URL. |

## `list_collection_items`: List collection items

Lists a collection's templates, references, schemas, terms, FAQs or updates, including drafts, with each item's state. Terms and FAQs include their full text.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `collection` | string | yes | Collection: id, "collections/{id}" or a Publisher Console collection URL. |
| `max_results` | integer |  | Maximum results (default 50, at most 200). |
| `state` | string |  | Only items in this state (default all). One of: all, active, draft, proposed, archived. |
| `type` | string | yes | Which items to list. One of: templates, references, schemas, terms, faqs, updates. |

## `get_collection_item`: Get collection item

One collection item in full: a template's file details, a reference's source, a schema's JSON Schema, a term, a FAQ or an update's contents.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `item` | string | yes | Item name such as "collections/{c}/templates/{t}", "collections/{c}/schemata/{s}", "collections/{c}/questions/{q}" (FAQs), or a console URL. |

## `get_faq_opportunities`: FAQ opportunities

Questions people asked the GAMS assistant about a working group's published collections that the content answers poorly, ranked by priority, with suggested FAQ wording.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `group` | string | yes | Working group: id, "groups/{id}" or a Publisher Console group URL. |
| `max_results` | integer |  | Maximum results (default 50, at most 200). |
| `state` | string |  | open (not yet turned into a FAQ, the default), added, or all. One of: open, added, all. |

## `search`: Search

Searches your working groups, their files and discussions, and your collections and their items by keyword and meaning. Returns names to pass to the get_ tools.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `max_results` | integer |  | Maximum results (default 20, at most 50). |
| `query` | string | yes | What to look for. |
| `scope` | string |  | Only within this working group (groups/{id}) or collection (collections/{id}) (optional). |
| `types` | array |  | Only these result types (optional). |

## `get_preview_link`: Collection links

Links to a collection on the public site: the public link once it is published, and a private preview link (for drafts) if you can edit it. Preview links do not expire, so share them only with people who may see unpublished content.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `collection` | string | yes | Collection: id, "collections/{id}" or a Publisher Console collection URL. |

## `list_threads`: List discussions

The person's discussion inbox across their working groups, most recent activity first. For "what needs my attention", call it once with needs_attention=true and include_unread_messages=true: you get every open discussion with unread messages or an @-mention of the person they have not answered, with the unread messages inline. Each row has the unread count, last message (time, author, snippet), participants, mentionsMe, etag and consoleUrl; people are listed once with their ids. Next: get_thread for a whole conversation (or since/unread_only for just the new part), reply_to_thread to answer (dry_run first), and mark_threads_read once the person has seen the messages. Pass nextCursor back as cursor for more.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `cursor` | string |  | nextCursor from the previous call, for the next rows. |
| `group` | string |  | Only this working group's discussions: id, "groups/{id}" or console URL (optional). |
| `include_unread_messages` | boolean |  | Include the unread messages in each row (at most 10 per discussion and 60 in all, each up to 2,000 characters). |
| `max_results` | integer |  | Rows per call (default 20, at most 50). |
| `needs_attention` | boolean |  | Only discussions with unread messages, or where someone @-mentioned the person after their last message. |
| `query` | string |  | Full-text search in subjects and messages (optional); results then come by relevance. |
| `state` | string |  | open (the default), closed, archived or all. One of: open, closed, archived, all. |
| `type` | string |  | Only this kind of discussion (optional). One of: discussion, question, suggestion, proposal, feature_request, support, feedback. |
| `updated_since` | string |  | Only discussions with activity since this time (RFC 3339, e.g. 2026-11-10T09:00:00Z, or a date). |

## `get_thread`: Read discussion

One discussion with its messages, oldest first: author names, the full Markdown text as written (@-mentions shown as @Name), mentions, references, attachment details and aiAssisted. To fetch only what is new, pass since (the nextSince of your last read) or unread_only=true instead of re-reading everything; before (the before value returned) pages back through older messages. Returns the etag update_thread needs and the people involved with their ids.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `before` | string |  | Only messages before this time: the before value of an earlier call, to page back. |
| `max_messages` | integer |  | Most recent messages to include (default 50, at most 200). |
| `since` | string |  | Only messages after this time (RFC 3339; use nextSince from an earlier call). |
| `thread` | string | yes | "threads/{id}", a thread id or a Publisher Console thread URL (from list_threads). |
| `unread_only` | boolean |  | Only the messages the person has not read yet. |

## `create_thread`: Start discussion

Start a discussion in a working group as the signed-in person; the first message is marked as written with AI assistance. The group's admins get an email, other members a daily digest, and @-mentioned people an email at once. Always call it first with dry_run=true, show the person the preview and who will be notified, and only after they agree repeat the call without dry_run. To continue an existing discussion use reply_to_thread instead. GAMS allows one new discussion per minute per person.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `group` | string | yes | Working group: id, "groups/{id}" or console URL. Only groups the person belongs to. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `mentions` | array |  | People to @-mention: names, email addresses or ids (users/{id}) of the working group's members (or people already in the discussion). They get an email at once. Write "@Name" in the text where the mention belongs; anyone not written there is addressed at the start. |
| `references` | array |  | GAMS content the message is about (optional): collections/{id}, collections/{id}/templates/{id} (or schemas, references, terms, faqs, updates), groups/{id}/resources/{id}. |
| `subject` | string | yes | Subject line (at most 200 characters). |
| `text` | string | yes | The message, in Markdown, written as the person would post it. |
| `type` | string |  | Kind of discussion (default discussion). One of: discussion, question, suggestion, proposal, feature_request, support. |

## `reply_to_thread`: Reply in discussion

Post a reply in a discussion as the signed-in person, marked as written with AI assistance. @-mentioned people get an email at once, active participants an email, other participants a daily digest. Always call it first with dry_run=true, show the person the preview and who will be notified, and post only after they agree. Repeating the same text within 10 minutes returns the earlier post instead of posting twice. Closed discussions take no replies.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `mentions` | array |  | People to @-mention: names, email addresses or ids (users/{id}) of the working group's members (or people already in the discussion). They get an email at once. Write "@Name" in the text where the mention belongs; anyone not written there is addressed at the start. |
| `references` | array |  | GAMS content the message is about (optional): collections/{id}, collections/{id}/templates/{id} (or schemas, references, terms, faqs, updates), groups/{id}/resources/{id}. |
| `text` | string | yes | The message, in Markdown, written as the person would post it. |
| `thread` | string | yes | "threads/{id}", a thread id or a Publisher Console thread URL (from list_threads). |

## `update_thread`: Close, reopen, archive or rename discussion

Close, reopen or archive a discussion, or change its subject. Only the working group's admins (and GAMS admins) can. Pass the etag from list_threads or get_thread; dry_run=true shows the change first. New posts change a discussion's etag, so a refusal for a changed etag returns the current one to retry with.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `etag` | string | yes | The etag from the read this change is based on (get_* tools return it). The change is refused if the item changed since; read it again and retry. |
| `state` | string |  | New state: open (reopen), closed or archived. One of: open, closed, archived. |
| `subject` | string |  | New subject (at most 200 characters). |
| `thread` | string | yes | "threads/{id}", a thread id or a Publisher Console thread URL (from list_threads). |

## `mark_threads_read`: Mark discussions read

Mark discussions as read for the signed-in person. It changes only their own unread counts; nobody is notified and no content changes. Use it after you have shown the person the unread messages from list_threads or get_thread, passing those thread names.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `threads` | array | yes | Thread names (threads/{id}), at most 50. |

## `read_document`: Read document

The text of a working-group file, collection template or document reference (docx, xlsx, pdf, csv, txt, md, json), rendered as Markdown with headings, tables and sheet names: the content itself, never a summary. Long documents come in parts of about 16,000 characters; ask for the next part until parts is reached. Quote it exactly; do not paraphrase it as the document's wording.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `document` | string | yes | groups/{g}/resources/{r}, collections/{c}/templates/{t} or collections/{c}/references/{r}, or a console URL. |
| `part` | integer |  | Which part to return (default 1). |

## `update_collection`: Update collection details

Changes a collection's descriptive details: display_name, description, benefits (replaces the whole list), logo_tile_hex_color_code, last_validated and slug. Only the fields you pass change; state, working group and licence never do (use publish_collection and set_collection_license). Any working-group member can do this. Read the collection with get_collection first and pass its etag; call with dry_run: true, show the person the diff, then repeat without dry_run.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `benefits` | array |  | The complete new list of benefits. It replaces the current list; [] removes them all. |
| `collection` | string | yes | Collection: id, "collections/{id}" or a Publisher Console collection URL. |
| `description` | string |  | New description, as shown on the public page. |
| `display_name` | string |  | New display name (1 to 200 characters). |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `etag` | string | yes | The etag from the read this change is based on (get_* tools return it). The change is refused if the item changed since; read it again and retry. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `last_validated` | string |  | When the content was last checked, YYYY-MM-DD. Today's date marks it validated now. |
| `logo_tile_hex_color_code` | string |  | Logo tile colour, #RRGGBB. |
| `slug` | string |  | Public address keyword, e.g. "due-diligence-questionnaires": lowercase letters and digits joined by single hyphens, at most 80. Changing it moves the public URL; the old one keeps redirecting. |

## `set_collection_license`: Set collection licence

Sets a collection's content licence, shown on its public page and to AI tools. Pass an SPDX id, or an empty spdx_id with display_name and url for the publisher's own terms. The licence is replaced as a whole; for CC0-1.0, CC-BY-4.0 and CC-BY-ND-4.0, fields you leave out get the console's wording. A licence labels content; it never changes who can see it. Read the collection with get_collection for its etag, call with dry_run: true, show the person the diff, then repeat without dry_run.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `attribution` | string |  | Credit line the licence requires, e.g. "www.openfunds.org". |
| `collection` | string | yes | Collection: id, "collections/{id}" or a Publisher Console collection URL. |
| `description` | string |  | One or two friendly sentences on what people may do with the content. |
| `display_name` | string |  | Friendly name on the public page, e.g. "Free to use". Required for the publisher's own terms. |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `etag` | string | yes | The etag from the read this change is based on (get_* tools return it). The change is refused if the item changed since; read it again and retry. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `spdx_id` | string | yes | SPDX licence identifier, e.g. CC0-1.0 (free to use), CC-BY-4.0 (free to use with credit), CC-BY-ND-4.0 (free to use, share as published). "" for the publisher's own terms. |
| `url` | string |  | Link to the licence deed or the publisher's terms. |

## `publish_collection`: Publish or unpublish collection

Publishes, unpublishes or archives a collection. Only the working group's admins (and GAMS admins) can do this. Publishing makes the collection public at its public URL on the GAMS public site, where anyone and any AI assistant can read its active items; unpublishing or archiving takes that page down. Read the collection with get_collection for its etag, call with dry_run: true to see the URL that will appear or disappear and whether you are allowed, get the person's explicit go-ahead, then repeat without dry_run. New working groups cannot publish during their first day.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `action` | string | yes | publish (make it public), unpublish (back to in development) or archive (retire it). One of: publish, unpublish, archive. |
| `collection` | string | yes | Collection: id, "collections/{id}" or a Publisher Console collection URL. |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `etag` | string | yes | The etag from the read this change is based on (get_* tools return it). The change is refused if the item changed since; read it again and retry. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |

## `list_collection_revisions`: List collection revisions

Lists a collection's saved revisions of its details (name, description, benefits, logo, licence, state, slug), newest first: revision name, time and which fields changed from the revision before. Use it to find a revision to restore with rollback_collection. Revisions do not record who made a change, and items (FAQs, terms, files) have their own history.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `collection` | string | yes | Collection: id, "collections/{id}" or a Publisher Console collection URL. |
| `max_results` | integer |  | Most recent revisions to list (default 20, at most 100). |

## `rollback_collection`: Roll back collection details

Restores a collection's details (display name, description, benefits, logo and colour, last-validated date, licence) to an earlier revision from list_collection_revisions. Its state (published or not), working group, slug and items (FAQs, terms, files) stay as they are. Only the working group's admins can do this. Read the collection with get_collection for its etag, call with dry_run: true to see what will change back, get the person's go-ahead, then repeat without dry_run.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `collection` | string | yes | Collection: id, "collections/{id}" or a Publisher Console collection URL. |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `etag` | string | yes | The etag from the read this change is based on (get_* tools return it). The change is refused if the item changed since; read it again and retry. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `revision` | string | yes | Revision to restore: its name from list_collection_revisions (collections/{c}/revisions/{r}) or id. |

## `upsert_terms`: Add or edit glossary terms

Adds and edits a collection's glossary terms in one call (up to 50). Items without name are added; items with name and etag (from list_collection_items type terms, which also shows what exists, so you avoid duplicates) are changed, only in the fields given. New terms are drafts unless publish: true. The public page of a published collection shows every term, drafts included, so adding terms to a published collection needs publish: true. Each item gets its own result (created, updated, unchanged or failed with the reason), so one bad item does not block the rest. Call with dry_run: true, show the person the plan, then repeat without dry_run.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `collection` | string | yes | Collection: id, "collections/{id}" or a Publisher Console collection URL. |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `publish` | boolean |  | Make every term in this call active. Without it, new terms are drafts and existing ones keep their state. |
| `terms` | array | yes | 1 to 50 terms. Each gets its own result. |

## `upsert_faqs`: Add or edit FAQs

Adds and edits a collection's FAQs in one call (up to 50). Items without name are added as drafts that only the working group sees; items with name and etag (from list_collection_items type faqs, which also shows what exists, so you avoid duplicates) are changed, only in the fields given, and keep their state. publish: true makes every FAQ in the call active, which shows it on the public page once the collection is published; pass it only when the person asked to publish. Each item gets its own result (created, updated, unchanged or failed with the reason), so one bad item does not block the rest. Call with dry_run: true, show the person the plan, then repeat without dry_run.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `collection` | string | yes | Collection: id, "collections/{id}" or a Publisher Console collection URL. |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `faqs` | array | yes | 1 to 50 FAQs. Each gets its own result. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `publish` | boolean |  | Make every FAQ in this call active (public once the collection is published). Only when the person asked to publish; without it, new FAQs are drafts and existing ones keep their state. |

## `create_update`: Add collection update

Adds an update (news item or release note) to a collection: title, optional subtitle, markdown body and publication date. Updates have no draft state: on an unpublished collection only the working group sees it until the collection is published, but on a published collection it is public at once, so there the call needs publish_now: true. Call with dry_run: true, show the person the update and where it will appear, then repeat without dry_run.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `body` | string |  | The update text, markdown (optional). |
| `collection` | string | yes | Collection: id, "collections/{id}" or a Publisher Console collection URL. |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `publication_date` | string |  | YYYY-MM-DD (default today). |
| `publish_now` | boolean |  | Required on a published collection, where a new update is public at once. Pass it only when the person wants the update live now. |
| `subtitle` | string |  | One-line summary (optional). |
| `title` | string | yes | Headline, e.g. "Version 2.1 released". |

## `add_faq_opportunity_as_draft`: Add FAQ opportunity as draft

Turns a FAQ opportunity from get_faq_opportunities into a draft FAQ in its collection and marks the opportunity as added, optionally with an edited question or answer. The draft is private to the working group until someone publishes it (upsert_faqs with publish: true, or the console). Check the suggested answer against the collection first; call with dry_run: true, show the person the FAQ, then repeat without dry_run.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `etag` | string |  | The opportunity's etag, if you have it (optional). |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `opportunity` | string | yes | The opportunity's name from get_faq_opportunities (groups/{g}/faqOpportunities/{id}). |
| `question` | string |  | Edited question wording (optional; default the suggested question). |
| `response` | string |  | Edited answer, markdown (optional; default the suggested response). Needed when the opportunity has needsAnswer. |

## `create_action`: Add action

Add an action item to a working group's Actions list, as the signed-in person. Owners are matched to the group's members by name or email; newly assigned owners get an email and the rest of the group sees it in the daily digest. Call it first with dry_run=true, show the person the preview, then repeat without dry_run. To change an existing action use update_action.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `description` | string | yes | What needs to be done. |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `due_date` | string |  | Due date, YYYY-MM-DD (optional). |
| `group` | string | yes | Working group: id, "groups/{id}" or console URL. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `owners` | array |  | Owners: names, email addresses or ids (users/{id}) of the working group's members. Newly assigned owners get an email. |
| `reference` | string |  | The group's reference for the action, e.g. ddq-12 (optional). |
| `workstream` | string |  | Workstream the action belongs to (optional). |

## `update_action`: Update action

Change an action item: status, owners, due date, description or workstream. Pass the action's name and etag from list_actions; only the fields you pass change (owners replaces the whole list). dry_run=true returns the field-by-field change to show the person first. Newly assigned owners get an email.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `action` | string | yes | groups/{g}/actions/{a}, from list_actions. |
| `description` | string |  | New description. |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `due_date` | string |  | New due date, YYYY-MM-DD, or "none" to clear it. |
| `etag` | string | yes | The etag from the read this change is based on (get_* tools return it). The change is refused if the item changed since; read it again and retry. |
| `owners` | array |  | Owners: names, email addresses or ids (users/{id}) of the working group's members. Newly assigned owners get an email. |
| `status` | string |  | New status. One of: not_started, ongoing, done, closed. |
| `workstream` | string |  | New workstream ("" to clear). |

## `add_bookmark`: Add bookmark

Add a link to a working group's Resources tab, as the signed-in person. If the group already has a bookmark for the same URL, that one is returned and nothing is added. Group members see new bookmarks in their daily digest. dry_run=true previews it.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `description` | string |  | What the link is and why it matters (optional). |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `group` | string | yes | Working group: id, "groups/{id}" or console URL. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `title` | string | yes | Title shown in the group's resources. |
| `url` | string | yes | The link (http or https). |

## `update_resource_details`: Update file details

Rename a working-group file or bookmark, or change its description. Details only: it never changes a file's content. Pass the name and etag from list_resources or get_resource; dry_run=true shows the change first.

Access needed: publisher.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `description` | string |  | New description ("" to clear). |
| `display_name` | string |  | New name shown in the group's resources. |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `etag` | string | yes | The etag from the read this change is based on (get_* tools return it). The change is refused if the item changed since; read it again and retry. |
| `resource` | string | yes | groups/{g}/resources/{r} or a console resource URL, from list_resources. |

## `get_document_outline`: Document outline for editing

The structure of a Word (.docx) or Excel (.xlsx) document, for editing it with edit_document. Word: paragraphs and tables in order, each paragraph with an anchor and a hash; content controls (form fields: tag, type, value, options); comments (with replies and resolved state) and tracked changes with their authors. Excel: sheets with used ranges, then the cells of one sheet or range with values and formulas; defined names; comments. Also returns the version (pass it to edit_document as base_version), who has the document open in the editor now (edits are refused while anyone does), and recommended_mode: "direct" only when the person is the only one who has saved the document. Long documents are paged: pass next_cursor as cursor. To read a document's text use read_document.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `content_control_tags` | array |  | Word: list only the content controls (form fields) with these tags, e.g. the fields of a questionnaire template. |
| `cursor` | string |  | next_cursor from the previous page. |
| `document` | string | yes | groups/{g}/resources/{r}, collections/{c}/templates/{t} or collections/{c}/references/{r}, or a console URL. |
| `max_items` | integer |  | Paragraphs/tables or cells per page (default 400; Word at most 2000, Excel at most 5000). |
| `range` | string |  | Excel: an A1 range of that sheet, e.g. "A1:H200" (default its used range). |
| `sheet` | string |  | Excel: the sheet whose cells to list (default the first). |

## `edit_document`: Edit document

Change a Word (.docx) or Excel (.xlsx) document with structured operations, as the signed-in person. Read it first with get_document_outline. mode "suggest" (the default) makes tracked changes and comments in the person's own name for the group to accept or reject; in Excel, which has no tracked changes, each suggested cell value becomes a comment on the cell. mode "direct" edits in place: use it only when the person asks, typically for documents only they work on (recommended_mode "direct"). All operations apply or none do. Refused while anyone has the document open in the editor. Excel: after value changes the whole workbook's formula results are recalculated, which takes a few seconds (up to about 30 for very large workbooks), so put all changes to a workbook in ONE call rather than one call per cell; edits to the same document run one at a time. Call with dry_run=true first and show the person the report; every saved change can be undone with restore_document_version.

Word operations (each may carry expect_hash, the paragraph's hash from the outline; a mismatch refuses the whole call):
- {"kind":"replace_text","anchor":"…","find":"old text","replace":"new text","occurrence":1}  (replace "" deletes; occurrence needed only when the text appears more than once)
- {"kind":"insert_paragraph","after":"anchor" | "before":"anchor","text":"…","style":"Heading2"}  (copies the anchor's formatting unless style is given)
- {"kind":"delete_paragraph","anchor":"…"}
- {"kind":"set_table_cell","table":0,"row":1,"column":2,"text":"…"}
- {"kind":"add_comment","anchor":"…","quote":"text to comment on","text":"comment"}
- {"kind":"reply_comment","comment_id":"…","text":"…"} / {"kind":"resolve_comment","comment_id":"…"}
- {"kind":"accept_revisions" | "reject_revisions","revision_ids":["…"]} or "author":"Name" or "all":true
- {"kind":"set_checkbox","anchor":"…","checked":true}
- {"kind":"set_content_control","tag":"fund_name" | "id":"…","value":"…","occurrence":1,"expect_hash":"…"}  (form fields: text, dropdown option, YYYY-MM-DD date, true/false checkbox)
Excel operations:
- {"kind":"set_cells","sheet":"Sheet1","cells":{"B4":42,"C4":"text","D4":"=SUM(B2:B4)","E4":{"text":"=literal"}},"expect":{"B4":40}}
- {"kind":"append_rows","sheet":"Sheet1","rows":[["a",1],["b",2]]}  (direct mode only)
- {"kind":"add_cell_comment","sheet":"Sheet1","cell":"B4","text":"…"}

Access needed: documents.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `base_version` | string |  | The version from get_document_outline. If the document changed since, the edit applies only when every paragraph it touches carries an unchanged expect_hash (Excel: expect values). |
| `document` | string | yes | groups/{g}/resources/{r}, collections/{c}/templates/{t} or collections/{c}/references/{r}, or a console URL. |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `mode` | string |  | "suggest" (default) or "direct". One of: suggest, direct. |
| `operations` | array | yes | The operations, in order (see the tool description). |

## `create_document`: Create document

Create a new Word (.docx) or Excel (.xlsx) document in a working group's files, as the signed-in person, ready to open and co-edit in the editor. Word: give the content as Markdown (headings, paragraphs, bulleted and numbered lists, tables, quotes, links, bold, italic). Excel: give sheets as [{"name":"Actions","header":true,"rows":[["Item","Owner"],["DDQ","Mark"]]}]; values are text, numbers, true/false or null, a text starting with "=" is a formula (start it with ' to keep it as text). The group sees the new file in its daily digest. Call with dry_run=true first and show the person the plan.

Access needed: documents.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `description` | string |  | A short description shown with the file (optional). |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `group` | string | yes | Working group: id, "groups/{id}" or console URL. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `kind` | string | yes | docx (Word) or xlsx (Excel). One of: docx, xlsx. |
| `markdown` | string |  | Word: the content as Markdown. |
| `name` | string | yes | The document's name in the group's files, e.g. "March meeting pack". |
| `sheets` | array |  | Excel: the sheets (see the tool description). |

## `export_pdf`: Export to PDF

Convert the current version of a Word, Excel or PowerPoint document to PDF and return a download link for the person. The link works for a short time (see expires) and needs no sign-in, so share it only with people who may see the document.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `document` | string | yes | groups/{g}/resources/{r}, collections/{c}/templates/{t} or collections/{c}/references/{r}, or a console URL. |

## `list_document_versions`: Document versions

A document's saved versions, newest first: when, by whom and how (in the editor, through an AI assistant, or a restore). Use it to find the version to restore with restore_document_version.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `document` | string | yes | groups/{g}/resources/{r}, collections/{c}/templates/{t} or collections/{c}/references/{r}, or a console URL. |
| `page_size` | integer |  | Versions per page (default 20, at most 100). |
| `page_token` | string |  | next_page_token from the previous page. |

## `compare_document_versions`: What changed in a document

Compare two versions of a Word or Excel document and list what changed, for summaries such as "what changed since last week". Word: paragraphs inserted, deleted, changed (with a word-level diff) or moved, table rows and cells, form-field values, comments and tracked changes. Excel: input values and formulas changed per sheet, separated from results that changed only because their inputs did. Take versions from list_document_versions; to_version defaults to the current one. Quote the changed text exactly.

Access needed: publisher.read.

| Argument | Type | Required | Description |
|---|---|---|---|
| `document` | string | yes | groups/{g}/resources/{r}, collections/{c}/templates/{t} or collections/{c}/references/{r}, or a console URL. |
| `from_version` | string | yes | The earlier version, from list_document_versions. |
| `max_changes` | integer |  | Changes to list at most (default 200, at most 2000); summary counts are always complete. |
| `to_version` | string |  | The later version (default: the current version). |

## `restore_document_version`: Restore document version

Make an earlier version of a document current again, as the signed-in person: for example to undo an edit. The version being replaced stays in the history, so a restore can itself be undone. Refused while anyone has the document open in the editor. Pass the version to restore and current_version from list_document_versions. Call with dry_run=true first and confirm with the person.

Access needed: documents.write.

| Argument | Type | Required | Description |
|---|---|---|---|
| `current_version` | string | yes | currentVersion from list_document_versions; the restore is refused if the document changed since. |
| `document` | string | yes | groups/{g}/resources/{r}, collections/{c}/templates/{t} or collections/{c}/references/{r}, or a console URL. |
| `dry_run` | boolean |  | Return exactly what would change, field by field, without changing anything. Use it first and show the result before the real call. |
| `idempotency_key` | string |  | Any unique string for this change (e.g. a UUID). Repeating a call with the same key returns the first result instead of acting twice. |
| `version` | string | yes | The version to restore, from list_document_versions. |

