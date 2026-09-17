---
name: markdownlint
description: >-
  Ensure any markdown (.md) file created or edited conforms to the CommonMark
  specification and passes markdownlint-cli2 without errors. Use this skill
  whenever you write, modify, or review Markdown documents, or when a user asks
  to validate/lint Markdown.
---

# Markdown lint & CommonMark compliance skill

This skill guarantees that every Markdown file you create or edit is:

1. **CommonMark-spec compliant** — correct headings, lists, fenced code blocks, emphasis, links, blockquotes, and tables.
2. **markdownlint-clean** — passes `markdownlint-cli2` with **0 issues**.

The linter binary is already installed on the host and available via the command `markdownlint-cli2` (v0.23+).

## When to use

- Any time you write a new `.md` file.
- Any time you edit an existing `.md` file.
- When a user asks to lint, validate, or fix Markdown files.

## Procedure

### 1. Write Markdown correctly from the start

Follow these rules while authoring content so rework is minimal:

- **MD001 / MD003**: Use ATX headings (`#`, `##`, `###`) with a single space after the `#`; increment heading levels by one (no skipping).
- **MD009 / MD010**: No trailing spaces; use spaces (not tabs) for indentation.
- **MD012**: No more than one consecutive blank line.
- **MD013**: Keep lines under 80 characters where reasonable (unless the project config disables it).
- **MD022 / MD031**: Surround headings and fenced code blocks with a blank line.
- **MD024**: Do not repeat identical heading text in the same document (rename or disambiguate).
- **MD025**: Use exactly one top-level H1 heading per document (usually the document title).
- **MD026**: No trailing punctuation (`:;,.!?`) in headings.
- **MD029 / MD030**: Number ordered lists consecutively; use consistent spacing after list markers.
- **MD031**: Blank line around fenced code blocks.
- **MD040**: Fenced code blocks should have an explicit language tag (e.g. ``` ```yaml```), except when used for plain output/mermaid diagrams if the config allows.
- **MD045 / MD046**: Use `![alt](url)` syntax for images and consistent fenced-code style (``` ``` not `~~~```).
- **MD047**: End the file with exactly one newline.
- **MD048**: Use backticks (``` ``` ```) for fenced code blocks consistently.

For CommonMark correctness specifically:
- Use blank lines between block-level elements (paragraphs, lists, headings).
- Indent nested list items correctly (4 spaces for content, sub-list markers aligned with parent text).
- Escape special characters (`*`, `_`, `` ` ``, `#`, `[`, `]`) when used literally.
- Close inline code spans with the same number of backticks used to open them.

### 2. Run the linter on the file(s)

After writing or editing, always validate:

```bash
markdownlint-cli2 "path/to/file.md"
```

To lint the whole project (all `*.md` files):

```bash
markdownlint-cli2 "**/*.md"
```

### 3. Read and fix the reported issues

The linter prints issues as `path:line:col MDxxx rule-name [details]`. For each issue:

1. Open the affected file at the reported line.
2. Apply the fix that matches the rule name (`MD001`–`MD051`). A cross-reference table of the most common rules is included below.
3. Re-run `markdownlint-cli2` on the file.
4. Repeat until output shows **0 issues**.

### 4. Report the result

State the final lint summary (e.g. `0 issues in 1 file`) in your completion message.

## Common rule reference

| Rule | Name | What it checks |
|---|---|---|
| MD032 | blanks-around-lists | Blank line around lists |
| MD033 | no-inline-html | No inline HTML (unless allowed) |
| MD034 | no-bare-urls | No bare URLs without angle brackets |
| MD036 | no-emphasis-as-heading | No bold text used as a heading |
| MD041 | first-line-heading | First line is a top-level heading |
| MD044 | proper-names | Proper capitalization of known names |

## Notes

- If a project rule is intentionally disabled (e.g. line length for long Russian text), respect the project's `.markdownlint-cli2.jsonc` / `.markdownlint.json` config — but do not disable rules casually to make errors disappear.
- Prefer fixing the document over suppressing the rule. Only adjust config for legitimate project-wide needs, and mention it.
- Table support in CommonMark is an extension (GFM); markdownlint handles tables via `MD055`/`MD056`/`MD058`. Keep table pipes aligned consistently when used.