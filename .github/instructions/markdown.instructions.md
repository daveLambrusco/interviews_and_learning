---
applyTo: "**/*.md"
---

## Markdown Formatting Rules

### Tables

Always use properly aligned markdown tables with a header row and a separator row. Every column must be padded so that all cells in that column have the same width. Calculate the column width as the length of the longest cell in that column (including the header), then pad all other cells with trailing spaces to match. The separator row must use dashes that fill the full column width, not minimal `---`.

```markdown
| Column A   | Column B | Column C     |
|------------|----------|--------------|
| short      | value    | longer value |
| longer val | value    | x            |
```

- Every column must have a header.
- Never use `|---|` or any unpadded separator. Always pad separators to column width.
- Do not omit the separator row.

### Fenced Code Blocks

Every fenced code block must declare a language identifier.

```markdown
```java
// Java code here
```
```

Never use a bare triple-backtick block without a language. If the content is plain text or shell output with no clear language, use `text`. For command-line instructions use `bash`.

### Writing Tone

Write in a clear, direct, conversational style. Avoid bullet-point-heavy, schematic layouts when prose flows better. Prefer full sentences over terse fragments. Do not over-structure with nested lists, use short paragraphs instead. Reserve bullet points for genuinely enumerable items (steps, options, properties), not for every piece of information.

### Post-Write Verification

After writing or editing any `.md` file, re-read the affected sections and fix any violations before finishing:

- All tables must have properly padded, aligned columns. No `|---|` minimal separators.
- All fenced code blocks must have a language identifier.
- No em dash characters (`—`) may remain. Replace with `:` for explanations, `-` in lists, `,` in conversational sentences, '(' and ')' for parenthetical phrases, etc.
- No emojis may remain.
