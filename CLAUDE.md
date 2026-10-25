# 2Brain — Schema and AI Behavior Rules

This file governs all AI behavior in this knowledge base. Read it before every session.

---

## What This Is

A general-purpose personal knowledge base (Second Brain) serving as shared memory for:
- **OpenClaw** — autonomous AI agent framework
- **ClawCode** — Command Center coding agent
- **PaperClip** — agent orchestration control plane

2Brain holds persistent knowledge that all three systems read from and write to.

---

## Folder Rules

| Folder | Rule |
|--------|------|
| `raw/` | Source material only. **Read-only for AI** — never modify raw files. Drop anything here; it will be processed into wiki. |
| `wiki/` | AI-maintained knowledge articles. Create, update, cross-link. Never delete. |
| `outputs/` | Generated answers, reports, analyses. Date-prefix all filenames: `YYYY-MM-DD-slug.md` |
| `scripts/` | Prompt-as-script workflow files. Do not modify — these are invocation templates. |
| `integrations/` | Integration contracts. Update when interfaces change. |

---

## Wiki Rules

- **One file per topic** — each distinct topic gets its own `.md` file in `wiki/`
- **Every wiki file starts with a one-paragraph summary** — written for someone who has never seen it
- **Cross-link related topics** using `[[topic-name]]` format
- **Maintain `wiki/INDEX.md`** — every topic listed with a one-line description, kept alphabetical by domain
- **Never delete wiki content** — mark outdated information with `[OUTDATED as of YYYY-MM-DD]` and add corrections below
- **When new raw sources arrive** — update relevant wiki articles, add new articles if new topics emerge
- **Source citations** — every factual claim should reference a source file from `raw/` if one exists

---

## Ingest Workflow

When a new source arrives in `raw/` and an ingest is requested:

1. **Read** the full source document end to end
2. **Discuss** key takeaways with the user before writing anything — in autonomous mode, log takeaways to `wiki/log.md` as a discussion entry
3. **Create a summary page** in `wiki/` named after the source (e.g., `raw/2026-04-10-article.md` → `wiki/article.md`)
4. **Create or update concept pages** for each major idea or entity — a single source may touch 10–15 wiki pages; that is normal
5. **Add `[[wiki-links]]`** throughout all new and updated pages to connect related concepts
6. **Update `wiki/INDEX.md`** — add new pages with one-line descriptions, update existing descriptions
7. **Append to `wiki/log.md`** — date, source filename, pages created, pages updated

A single ingest should leave multiple pages richer and better connected.

---

## Wiki Page Format

Every wiki page must follow this structure:

```markdown
# Page Title

**Summary**: One to two sentences describing this page.

**Sources**: List of `raw/` files this page draws from.

**Last updated**: YYYY-MM-DD

---

Main content with clear headings and short paragraphs.
Link related concepts using [[wiki-links]] throughout.

## Related pages

- [[related-concept-1]]
- [[related-concept-2]]
```

---

## Citation Rules

- Every factual claim should reference its source file: `(source: filename.md)`
- If two sources disagree, note the contradiction explicitly and keep both views
- If a claim has no source in `raw/`, mark it as `[needs verification]`
- When a newer source supersedes an older claim, update the claim and note the change date

---

## Question Answering Workflow

When answering a question from the knowledge base:

1. Read `wiki/INDEX.md` first to find relevant pages
2. Read those pages and synthesize an answer
3. Cite specific wiki pages in your response: `(see: [[page-name]])`
4. If the answer is not in the wiki, say so clearly — do not fabricate
5. If the answer is valuable, offer to save it as a new wiki page

Good answers should be filed back into the wiki so knowledge compounds.

---

## Wiki Log

The file `wiki/log.md` is an append-only operation log. Rules:
- Never modify or delete past entries
- Every ingest, page creation, page update, and health check gets a log entry
- Format: `YYYY-MM-DD | operation | details`
- Create the file on first use if it does not exist

---

## Output Rules

- Save all generated answers and reports to `outputs/`
- Filename format: `YYYY-MM-DD-<short-slug>.md`
- Every output file must include: date, original query, sources referenced, answer/report, follow-up questions, wiki articles updated
- Use `outputs/_TEMPLATE.md` as the base

---

## Health Check Rules

Run `scripts/health-check.md` when prompted. Check for:
- Contradictions between wiki articles
- Topics mentioned in articles but never given their own file
- Claims not backed by any source in `raw/`
- Compounding errors (wrong facts propagated to multiple articles)
- Suggest 3 new articles that would fill gaps
- Verify all pages follow the Wiki Page Format above
- Find orphan pages (no inbound `[[links]]` from other pages)
- Report findings as a numbered list with suggested fixes

---

## Integration Points

**OpenClaw agents:**
- Before acting, query `wiki/INDEX.md` for relevant context
- After completing tasks, write summaries to `raw/` as new source material
- Format: `raw/YYYY-MM-DD-openclaw-<agent>-<task>.md`

**ClawCode:**
- Load this file (`CLAUDE.md`) and `wiki/INDEX.md` at session start
- Check `wiki/codebase-patterns.md` before writing code
- Log architecture decisions to `raw/` after implementation

**PaperClip:**
- Schedules `compile-wiki` and `health-check` on a recurring basis
- Routes knowledge queries from any agent to the `ask-question` workflow
- Routes agent outputs back as raw sources

**Composio MCP:**
- Ingests content from GitHub repos, Google Drive documents, Slack threads into `raw/`
- Format filenames clearly: `raw/YYYY-MM-DD-<source>-<slug>.md`

---

## Compounding Loop

Every interaction should leave 2Brain better:

1. **Query** → answer is grounded in wiki → saved to `outputs/` → wiki updated with new insights
2. **Ingest** → new source in `raw/` → wiki articles updated or created → INDEX.md updated
3. **Health check** → contradictions flagged → wiki corrected → sources verified

The knowledge base should be smarter after every session than before it.

## Coding standards

Coding standards: follow [CODING_STANDARDS.md](CODING_STANDARDS.md) (from ScottLabs-DevStudio/engineering-standards). Answer every review thread before ship.
