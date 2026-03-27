# Keep a Changelog — Deep Dive

## Guiding Principles

1. **Changelogs are for humans, not machines.** Write for the person reading, not for a parser.
2. **Every version should have an entry.** Even if the changes are small.
3. **Group by type of change.** Added, Changed, Deprecated, Removed, Fixed, Security.
4. **Versions should be linkable.** Each version entry should be a navigable anchor.
5. **Show the latest version first.** Most recent changes are most relevant.
6. **Include the release date.** ISO 8601 format (YYYY-MM-DD) for clarity.

## Format Specification

```markdown
## [1.2.0] - 2024-03-15

### Added
- New feature description

### Changed
- Modified behavior description

### Deprecated
- Feature that will be removed

### Removed
- Feature that was removed

### Fixed
- Bug fix description

### Security
- Vulnerability fix description
```

## Common Anti-Patterns

### Auto-Generated from Git Commits

Commit messages serve a different purpose than changelog entries. "fix: typo in README" is a valid commit message but a useless changelog entry. Curate your changelog — it's a communication to users, not a dump of implementation history.

### Dumping Everything in "Changed"

If everything is "Changed," the categorization adds no value. Take the time to properly categorize — it's the reason the system exists.

### No Unreleased Section

Maintain an `[Unreleased]` section at the top for changes that haven't shipped yet. This helps track what's coming in the next release and makes release preparation faster.

### Version Without Date

A version number without a release date loses half its usefulness. Users often need to know "when did this change?" to correlate with issues they observed.

## Yanked Releases

If a release had critical bugs and was pulled:

```markdown
## [1.2.1] - 2024-03-16 [YANKED]
```

Mark it clearly. Don't delete the entry — its existence explains the version gap.

## Linking Versions

At the bottom of the changelog, add comparison links:

```markdown
[1.2.0]: https://github.com/owner/repo/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/owner/repo/compare/v1.0.0...v1.1.0
```

This lets readers see the exact diff for any version, providing complete transparency.
