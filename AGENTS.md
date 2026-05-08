# AGENTS.md

This vault is an LLM-maintained wiki for personal knowledge and research synthesis.

## Purpose

The vault has three layers:

1. Raw sources in `raw/`
2. Compiled wiki pages in `wiki/`
3. Operating files at the vault root

The agent reads raw sources, writes and maintains the wiki, and keeps the index and log current.

## Ownership Rules

- `raw/` is immutable. Read from it, but do not modify, rename, or delete files there unless the user explicitly asks.
- `wiki/` is LLM-owned. Create and update markdown pages there freely as part of normal operation.
- `index.md` and `log.md` are LLM-maintained operational files and should be updated whenever meaningful wiki changes occur.

## Directory Conventions

- `raw/inbox/`: newly added unprocessed sources
- `raw/sources/`: organized source documents that remain immutable
- `raw/assets/`: images and attachments referenced by source material
- `wiki/overview/`: broad entry pages for major areas of the vault
- `wiki/topics/`: subject synthesis pages
- `wiki/entities/`: durable pages for people, organizations, books, tools, places, and similar nodes
- `wiki/personal/`: durable self-knowledge pages
- `wiki/source-notes/`: one canonical note per raw source
- `wiki/analyses/`: durable outputs from valuable questions
- `templates/`: optional helper templates when useful

## Frontmatter Schema

All durable wiki pages should use YAML frontmatter with this shape:

```yaml
id: stable-slug
type: overview | topic | entity | personal | source-note | analysis
status: seed | active | mature | archived
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: []
source_count: 0
sources: []
related: []
confidence: low | medium | high
```

Use lowercase slugs for `id`. Keep `updated` current whenever a page changes.

## Page Rules

- Prefer updating an existing durable page over creating a new one.
- Create a new page only when a concept, entity, or question has enough ongoing value to justify a standalone node.
- Preserve uncertainty and disagreement explicitly. Do not flatten conflicting claims into false certainty.
- Avoid deleting substantial content. Merge, mark obsolete, or archive instead.

## Ingest Workflow

When processing a new source in `raw/inbox/` or `raw/sources/`:

1. Read the source.
2. Create or update a matching page in `wiki/source-notes/`.
3. Update relevant pages in `wiki/topics/`, `wiki/entities/`, and `wiki/personal/`.
4. Update `index.md` if new pages were created or summaries changed materially.
5. Append an entry to `log.md`.
6. Explicitly note contradictions, uncertainty, and worthwhile follow-up questions.

Default behavior is autonomous. Do not wait for confirmation between every edit unless the user asks for a guided workflow.

## Query Workflow

Answer from the wiki first.

1. Read `index.md` to locate relevant material.
2. Read the most relevant wiki pages.
3. Read source-note pages and raw sources only as needed to support or verify the answer.
4. Answer with links to the relevant wiki pages and source notes.
5. If the result has durable value, create or update a page in `wiki/analyses/`.
6. If a new durable page is created, update `index.md` and append a `query` entry to `log.md`.

## Lint Workflow

On request, or when the user asks for maintenance, check for:

- orphan pages
- weak or missing cross-links
- stale claims superseded by newer sources
- repeated concepts that should become their own page
- duplicate pages that should merge
- low-confidence claims with weak sourcing
- research gaps worth investigating

Record meaningful lint work in `log.md`.

## Index Rules

`index.md` is the primary navigation surface.

- Keep it grouped by category.
- Give each durable page a one-line summary.
- Keep summaries concise and factual.
- Update it whenever new durable pages are added or the structure changes materially.

## Log Rules

`log.md` is append-only.

Use headings in this format:

```md
## [YYYY-MM-DD HH:MM] ingest | Title
```

Supported event labels:

- `ingest`
- `query`
- `lint`
- `refactor`

Each entry should state what changed, which pages were touched, and any unresolved questions.

## Style

- Use markdown links for cross-references whenever possible.
- Keep pages concise, navigable, and cumulative.
- Favor synthesis over duplication.
- Treat the wiki as a long-lived knowledge artifact, not a chat transcript.
