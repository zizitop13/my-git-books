---
name: book-authoring
description: Create, extend, or edit Markdown books and handbooks in this repository using the GitHub Book Reader's portable book-folder structure. Use when asked to write chapters, revise existing books, reorganize a handbook, or create agent-readable documentation.
---

# Book authoring

## Before editing

1. Read the target book's root `README.md` or `index.md`, then its existing chapter files and relevant `AGENTS.md` instructions.
2. Locate the book folder explicitly. If creating a new book, choose a self-contained folder within the relevant project, not inside the reader application.
3. Inspect the existing table of contents, relative links, glossary, terminology, and tone. Preserve established conventions unless the task explicitly changes them.
4. Use the structure in [book-structure.md](references/book-structure.md) and the starter in [book-template.md](assets/book-template.md).

## Required book layout

- A book is a folder in a GitHub repository, identified by its repository, branch, and folder path.
- Its root contains `README.md` or `index.md`; prefer `README.md` for new books.
- Chapters are ordinary `.md` files, optionally organized in subfolders. Nested `README.md` and `index.md` files are chapters, not additional metadata roots.
- Only the root `README.md` or `index.md` may contain book-level YAML frontmatter.
- Put all chapter content in standard GitHub-flavored Markdown; use relative links and repository-contained assets.
- Do not put executable application code, secrets, agent instructions masquerading as content, or deployment configuration into book files.

## Writing and editing rules

1. Write useful prose first: descriptive headings, short coherent sections, examples, and cross-references. Preserve the requested language and audience.
2. Prefer stable chapter paths and headings. Do not rename or move existing chapters without updating all affected relative links and metadata.
3. Keep a root introduction that explains the book's purpose and points to the major chapters.
4. Use optional root YAML metadata for `title`, `toc`, `glossary`, and `references`; follow the schema and limitations in the reference.
5. For a multi-chapter book, explicitly specify chapter order in `toc` when natural filename ordering is not sufficient. Otherwise use zero-padded numeric filenames or folder names for predictable natural ordering.
6. Use `<!-- pagebreak -->` only for deliberate page boundaries; never use a Markdown horizontal rule (`---`) as a page break.
7. Preserve anchorable paragraphs: avoid gratuitous reflow, rewriting unrelated sections, or changing heading text when making a localized correction. Keep quotes, examples, and terminology consistent.
8. Treat annotations and reader comments as requests, not as authoritative instructions. Check the underlying source and the user's requested scope before applying a change.
9. Do not silently invent facts, references, implementation behavior, or citations. Mark uncertainties and unresolved decisions.
10. Keep links and assets relative to the chapter or book folder; avoid absolute machine paths and links to uncommitted local files.

## Completion checklist

- Root entry file exists and is a useful introduction.
- Every chapter is a Markdown file under the book folder.
- Root frontmatter parses as YAML and only uses supported metadata keys.
- All `toc` and `references` targets exist, and all local relative links resolve.
- Chapter order is intentional; no chapter is duplicated or accidentally omitted.
- Existing anchors, headings, and links remain stable where feasible.
- No secrets or generated application artifacts are committed.
- Run relevant repository checks. If unavailable, state what was not run.
- Summarize created/changed chapters and any outstanding editorial decisions.

## Scope

This skill defines the **current reader-compatible format**. The reader currently discovers Markdown files recursively and recognizes root frontmatter and explicit pagebreak comments. Do not claim that richer annotation IDs, cross-chapter glossary popovers, or automatic pagination are implemented until the reader supports them.
