# Docs-as-Code

## The Core Principle

Docs-as-code means applying software development practices to documentation: version control, peer review, automated testing, and continuous deployment. This is not about storing docs in a git repo for its own sake — it is about closing the gap between when a system changes and when its documentation reflects that change.

Why the gap matters: Every hour that documentation is out of sync with the system it describes, someone makes a decision based on wrong information. In a fast-moving project, manual documentation processes create a permanent lag that compounds over time.

## Markdown as the Authoring Format

Markdown is the lingua franca of docs-as-code. It is readable as plain text, renderable as HTML, diffable in version control, and writable by anyone — including AI tools via MCP.

### Why Markdown Over Rich Text

- **Version control friendly.** Markdown diffs show exactly what changed in a document. A WYSIWYG format shows a binary blob changed.
- **Tooling agnostic.** Markdown works in any editor, any platform, any rendering pipeline. Switching tools does not require migrating content.
- **Separation of content and presentation.** The author focuses on structure and words. The rendering system handles typography, layout, and styling. This separation means documentation looks consistent without authors making design decisions per page.
- **AI tool compatible.** MCP-based workflows produce markdown natively. The `save_item` tool accepts markdown content, making the authoring-to-publishing pipeline frictionless.

### Markdown Discipline

Not all markdown is equal. Disciplined markdown is easier to maintain, review, and render consistently.

**One sentence per line.** This is the single most impactful markdown habit. When each sentence is its own line in the source file, diffs show exactly which sentence changed. Paragraph-level diffs (where the entire paragraph is one line) make it impossible to see what actually changed.

**Heading hierarchy is non-negotiable.** H1 for the document title (used once), H2 for major sections, H3 for subsections. Never skip levels. The heading hierarchy is the document's table of contents and its accessibility structure.

**Fenced code blocks with language tags.** Always specify the language. `typescript`, `bash`, `json`, `sql`, `yaml` — the renderer uses this for syntax highlighting, and search tools use it for filtering.

**Consistent list formatting.** Pick either `-` or `*` for unordered lists and stick with it across the entire documentation set. Inconsistency signals carelessness.

## The Review Process

Documentation review is different from code review, but it benefits from the same rigor.

### What Documentation Review Catches

- **Technical inaccuracy.** The most critical failure mode. A reviewer who knows the system can catch statements that are plausible but wrong.
- **Assumption gaps.** The author knows the context. The reviewer represents the reader. "What does this term mean?" and "How do I get to this state?" are review comments that improve the document for everyone.
- **Structural problems.** A reviewer can see when a document buries its most important information three sections deep, or when two sections cover the same ground.
- **Stale references.** Links to other documents, API endpoints, or external resources that have moved or changed.

### Review Checklist for Documentation

1. **Is the document type correct?** — A how-to guide that spends 60% of its length explaining concepts is actually an explanation document masquerading as a guide.
2. **Does the keySummary match the content?** — If the title says "How to configure X" but the document explains the philosophy of X, the title is lying.
3. **Are code examples accurate?** — Run them. If you cannot run them, flag them.
4. **Does the document assume knowledge it should not?** — Check prerequisites. If the document assumes the reader has done something, is that something documented and linked?
5. **Are cross-references valid?** — Follow every link. Broken links are broken promises.

## Testing Documentation

Documentation can be tested. Not with unit tests, but with processes that catch the most common failure modes.

### Link Validation

Broken internal links are the most common documentation defect. They accumulate silently — a document gets renamed, a slug changes, a section gets reorganized — and suddenly cross-references point to nothing.

**Prevention:** Before publishing, verify that every cross-reference points to a document that exists. Use `search_items` to confirm the target document is published.

### Code Example Verification

Code examples that do not work are worse than no examples. They teach the reader something false and waste their time debugging documentation instead of their actual problem.

**Prevention:** Before including a code example, run it. If the example depends on setup the reader must do first, document that setup as a prerequisite.

### Freshness Audits

Documentation does not age gracefully. It ages silently and dangerously.

**Process:** On a regular cadence, review each workspace. For each document, ask: "Is this still accurate?" Documents that describe APIs, configurations, or version-specific behavior are the most likely to have drifted.

**Markers of staleness:**
- Version numbers that are no longer current
- References to features that have been renamed or removed
- Code examples using deprecated syntax or APIs
- Links to external resources that return 404

## Keeping Docs in Sync With Systems

The hardest problem in documentation is not writing — it is maintaining. A perfect document today becomes a misleading document in three months if the system it describes changes.

### Change-Driven Documentation

The ideal: every system change triggers a documentation review. When an API endpoint changes, the reference documentation is updated in the same workflow. When a configuration option is added, the configuration reference gains a new entry.

In practice, this means treating documentation updates as part of the definition of done for any system change. The system is not "done" until its documentation is accurate.

### Documentation Debt

Like technical debt, documentation debt accumulates when the team ships changes without updating docs. Unlike technical debt, documentation debt is invisible until someone reads the wrong information and makes a mistake.

**Signs of documentation debt:**
- Support questions that are answered by information not in the docs
- New team members who struggle despite "comprehensive" documentation
- Documents with last-modified dates far older than the system they describe

**Paying down documentation debt:** Allocate regular time — not leftover time — for documentation maintenance. Review one workspace per cycle. Update what is wrong, deprecate what is obsolete, and create what is missing.

## The Authoring Workflow in Pignal

Pignal's MCP workflow naturally supports docs-as-code principles:

1. **Draft in private** — `save_item` with default `private` visibility. The document exists but is not published.
2. **Validate for accuracy** — `validate_item` to mark the document as reviewed. This is the "review" step of the workflow.
3. **Publish when ready** — `vouch_item` to make the document public with a permanent URL slug.
4. **Iterate** — Use `save_item` to update published documents. The URL slug remains stable. Content evolves.

This maps cleanly to the docs-as-code lifecycle: write, review, publish, maintain.
