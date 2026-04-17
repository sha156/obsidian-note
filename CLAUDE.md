# ADHD Knowledge Vault — LLM Wiki Schema

This vault is a personal knowledge base maintained by an LLM for an ADHD user.
The user curates sources, directs exploration, and asks questions.
The LLM does all writing, cross-referencing, filing, and maintenance.

---

## Architecture

Three layers:

| Layer | Path | Owner | Rule |
|-------|------|-------|------|
| **Raw sources** | `raw/` | User | Immutable. LLM reads but never modifies. |
| **Wiki** | Everything else (01-06 dirs, root pages) | LLM | LLM creates, updates, and maintains all pages. |
| **Schema** | This file (`CLAUDE.md`) | Co-evolved | User and LLM refine together over time. |

Two infrastructure files at the vault root:

| File | Purpose |
|------|---------|
| `index.md` | Content catalog — every wiki page listed with link and summary. LLM reads this first when answering queries. |
| `log.md` | Chronological operation log — append-only record of ingests, queries, lint passes. |

---

## Directory Structure

```
adhd_knowledge/
├── CLAUDE.md              # This schema
├── index.md               # Wiki catalog (LLM-maintained)
├── log.md                 # Operation log (append-only)
├── 00-使用说明书首页.md     # Vault homepage
├── 01-大脑硬件/            # Neurology — attention, energy curves
├── 02-能量系统/            # Energy — charging, draining triggers
├── 03-Bug与补丁/           # Problems & proven solutions
├── 04-日志/                # Daily logs, templates
├── 05-爱好/                # Hobbies (subdirs per hobby)
│   ├── 跑步/
│   ├── 越野跑/
│   └── 战锤数学/
├── 06-恢复拉伸/            # Physical recovery & stretching
└── raw/                   # Immutable source materials
    ├── articles/          # Web clips, saved articles
    ├── transcripts/       # Video/podcast transcripts
    └── assets/            # Images, PDFs, data files
```

New categories follow the `NN-名称/` pattern (e.g., `07-工作/`). Assign the next available number.

---

## Page Conventions

### Language

All wiki content is written in **Chinese**. Use natural, first-person voice for self-knowledge pages. Use neutral informational tone for reference/hobby pages.

### Frontmatter

Every wiki page must have YAML frontmatter:

```yaml
---
tags: [category, topic, context]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

- `tags` — array, category first then specifics. Include `ADHD` tag on self-knowledge pages.
- `created` — date page was first created.
- `updated` — date page was last substantively modified.

### Page Structure

```markdown
---
tags: [...]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# Page Title

> Opening insight or core metaphor (blockquote, one or two lines)

---

## Section Heading
(Content — text, tables, lists)

---

## ... more sections ...

---

*关联页面: [[path/page1|显示名]] · [[path/page2|显示名]]*
```

Rules:
- **Opening blockquote** on every substantive page — a memorable hook or core metaphor.
- **Horizontal rules** (`---`) separate major sections.
- **Related pages footer** at the bottom of every page using `· ` separator.
- **No question marks** in titles or headings. Rephrase as statements.
- **Wiki links** use display text: `[[05-爱好/跑步/总览|跑步总览]]`.

### Content Style

- **Tables over prose** for reference content — parameters, comparisons, decision matrices.
- **ADHD-friendly options** where applicable — provide minimal/standard/complete variants (e.g., 10min / 20min / 40min plans).
- **Common misconceptions** section (`常见误解`) on pages where intuition misleads.
- **Fill-in sections** using HTML comments `<!-- 在这里记录... -->` to invite user personalization.
- **Numbered action codes** for action/exercise pages (e.g., `KB-S1`, `IT-E3`) for cross-file reference.
- **Source attribution** at page bottom for research-based content.

### Linking

- Always link to existing wiki pages when mentioning their topic.
- Use `[[filename|显示名]]` format with readable display text.
- When a concept is mentioned but has no page yet, note it for future creation (don't create empty stubs).

---

## Operations

### Ingest

When the user adds a new source to `raw/` and asks to process it:

1. **Read** the source material completely.
2. **Discuss** key takeaways with the user — what's interesting, what's new, what contradicts existing knowledge.
3. **Create** a source summary page in the appropriate wiki directory.
4. **Update** existing wiki pages that relate to the new information — add cross-references, revise claims, note contradictions.
5. **Update** `index.md` — add the new page, update summaries of modified pages.
6. **Append** to `log.md` — record the ingest with date, source name, and pages touched.

A single source ingest typically touches 5-15 wiki pages.

The user prefers to ingest one source at a time and stay involved. Read summaries aloud, check updates, and ask what to emphasize before filing.

### Query

When the user asks a question:

1. **Read** `index.md` to find relevant wiki pages.
2. **Read** the relevant pages.
3. **Synthesize** an answer with citations to specific wiki pages.
4. **Optionally file** — if the answer is a valuable synthesis (comparison, analysis, new connection), ask the user if it should be filed as a new wiki page. Good answers compound in the wiki.

Answer formats vary by question type:
- Factual lookup → direct answer with page citation
- Comparison → markdown table
- Analysis → new wiki page draft
- Overview → structured summary with links

### Lint

Periodic health check of the wiki. Run when the user asks or when the wiki has grown significantly since the last lint.

Check for:
- **Orphan pages** — no inbound links from other pages
- **Stale content** — claims that newer sources have superseded
- **Missing cross-references** — topics mentioned on a page but not linked
- **Contradictions** — conflicting claims across pages
- **Coverage gaps** — important concepts mentioned but lacking their own page
- **Broken links** — wiki links pointing to non-existent pages
- **Missing footers** — pages without `*关联页面*` section
- **Missing frontmatter** — pages without proper YAML header

Output a lint report with findings organized by severity, and suggest fixes.

---

## Rules

1. **Never modify `raw/`** — source materials are immutable.
2. **Always update `index.md`** after creating or substantially modifying a wiki page.
3. **Always append to `log.md`** after ingest, significant query-to-page filing, or lint operations.
4. **Preserve existing content** — when updating a page, add to it or revise specific claims. Don't rewrite pages from scratch unless the user requests it.
5. **Flag contradictions** — when new information conflicts with existing wiki content, note both positions rather than silently overwriting.
6. **No question marks** in document titles, headings, or section headers.
7. **Chinese content** — all wiki pages are written in Chinese. This schema file is the exception.
8. **Frontmatter required** — every wiki page must have `tags`, `created`, `updated` fields.

---

## Log Format

Each log entry follows this format for parseability:

```markdown
## [YYYY-MM-DD] operation | Subject

Description of what was done.
Pages touched: [[page1]], [[page2]], ...
```

Operations: `ingest`, `query-filed`, `lint`, `update`, `create`, `setup`

---

## Index Format

Organized by category. Each entry is one line:

```markdown
### Category Name

- [[path/page|显示名]] — one-line summary
- [[path/page|显示名]] — one-line summary
```

Include page count and last-updated metadata per category.
