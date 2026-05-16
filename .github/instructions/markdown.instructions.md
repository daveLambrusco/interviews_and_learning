---
applyTo: "**/*.md"
---

## Markdown Formatting Rules

### Tables

Always use properly aligned markdown tables with a header row and a separator row. Follow these steps to ensure correct formatting:

1. Create a header row with descriptive column names.
2. Calculate the column width as the length of the longest cell in that column (including the header).
3. Pad all cells in that column with trailing spaces to match the column width.
4. Create a separator row using dashes that fill the full column width. Never use minimal `---` separators.
5. Verify that every column has a header and no separator row is omitted.

```markdown
| Column A   | Column B | Column C     |
|------------|----------|--------------|
| short      | value    | longer value |
| longer val | value    | x            |
```

For tables with merged cells or uneven column counts, ensure all rows have the same number of delimiters and maintain consistent cell width across all rows. For merged cells, repeat the content in each column or use a placeholder to maintain alignment.

If a table cannot be formatted to meet these alignment rules (for example, due to extreme cell width disparities or content that renders poorly), use a fallback approach: simplify the table structure, convert to a list format, or move detailed information to accompanying prose.

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
