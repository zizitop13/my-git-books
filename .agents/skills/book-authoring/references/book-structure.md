# Book folder specification

This specification matches `github-book-reader/src/book.ts` and `github-book-reader/README.md` in this repository.

## Example tree

```text
navigation-handbook/
├── README.md
├── 01-foundations/
│   ├── 01-overview.md
│   └── 02-orbits.md
├── 02-planning/
│   ├── 01-transfers.md
│   └── 02-maneuvers.md
└── assets/
    └── transfer.svg
```

The book URL points to the folder, for example `https://github.com/owner/repo/tree/main/navigation-handbook`.

## Root entry and metadata

Only the book-root `README.md` or `index.md` is the metadata entry point. The reader prefers `README.md` if both exist; avoid keeping both at the root.

Supported optional frontmatter:

```yaml
---
title: Navigation Handbook
toc:
  - 01-foundations/
  - 02-planning/
glossary:
  Periapsis: Closest point in an orbit to the primary body
references:
  Orbital transfer: 02-planning/01-transfers.md
---
```

- `title`: string, display title.
- `toc`: list of strings, used to influence chapter ordering. **Current implementation compares entries with Markdown file paths; directory-only entries do not recursively order all descendants.** For reliable explicit order, list individual chapter paths or use naturally sorted filenames.
- `glossary`: mapping of term to definition.
- `references`: mapping of label to relative Markdown file path.

Metadata is optional. The root entry is also rendered as a chapter. The reader currently parses these metadata fields but does not yet implement all glossary/reference presentation features.

## Markdown conventions

- Use `#` for the chapter title, `##` for sections and `###` for subsections.
- GitHub-flavored Markdown tables, fenced code, task lists, and relative links are supported by the Markdown renderer.
- Mathematical notation may use `$...$` and `$$...$$`.
- Mermaid diagrams may be authored as fenced `mermaid` blocks, but live Mermaid rendering is not yet guaranteed in the reader.
- Use `<!-- pagebreak -->` on its own line to request a deliberate segment boundary.
- Markdown `---` remains a horizontal rule, not a page break.
- Keep diagrams and images in book-local asset folders and link with relative paths. Verify that the reader can display each asset before claiming full support.

## Editing for future inline comments

Annotations may eventually reference a file, heading path, selected quote, surrounding text, and Git revision. Therefore:
- Preserve file paths and heading wording when possible.
- Make narrow edits instead of wholesale reformatting.
- Avoid identical boilerplate paragraphs that are hard to distinguish.
- Never embed issue IDs or comments into prose unless the user explicitly requests that.
- Do not invent stable annotation ID syntax: it is not part of the current book format.

## Validation

Inspect the Markdown tree and test relative links and referenced files. Run project checks where available. This is a content format, not a build system; do not require `npm` in a standalone book folder.
