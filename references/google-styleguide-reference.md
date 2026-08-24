# Public Google Styleguide Reference Map

Use this reference when the Material 3 skill changes implementation code,
tests, scripts, or documentation.

## Scope And Authority

Source snapshot inspected for this skill:

`/Users/wang/Downloads/styleguide-gh-pages/`

This is a public `gh-pages` snapshot of `google/styleguide`. Its README says
the guides are, with few exceptions, copies of Google's internal language style
guides published for Google-owned or Google-originated open-source projects.
Treat it as a strong public reference, not a complete or guaranteed-current
description of all internal Google engineering practice.

It is an engineering and documentation style-guide collection. It does not
define Material tokens, visual components, product interaction patterns, or a
brand system. Those come from Material 3 and the host product.

## Agent Decision Rule

1. Inspect the host repository and use its formatter, linter, test setup, and
   established conventions first.
2. Identify the file language or deliverable.
3. Use the matching source below for language-specific choices.
4. Apply only rules that fit the host project and task; do not perform an
   unrelated style migration.
5. If no language guide applies, use the conservative cross-language principles
   in this file.

## Original Source Routing

| Target | Primary source in the supplied snapshot | Use it for |
|---|---|---|
| C++ | `cppguide.html` | headers, ownership, classes, functions, naming, comments, formatting |
| Java | `javaguide.html` | source-file structure, formatting, practices, Javadoc |
| JavaScript | `jsguide.html` | modules, source structure, language features, naming, JSDoc, policies |
| TypeScript | `tsguide.html` | source structure, language and type-system choices, naming, documentation, toolchain |
| Python | `pyguide.md` | imports, exceptions, comments/docstrings, naming, main-program structure |
| Go | `go/index.md`, `go/guide.md`, `go/decisions.md`, `go/best-practices.md` | readable Go, clarity, simplicity, API and test decisions |
| Shell | `shellguide.md` | shell selection, quoting, arrays, functions, error handling, ShellCheck |
| HTML/CSS | `htmlcssguide.html` | document and stylesheet style |
| C# | `csharp-style.md` | naming, whitespace, organization, C# language choices |
| Objective-C | `objcguide.md` | Objective-C source and API style |
| R | `Rguide.md` | syntax, namespaces, returns, package documentation |
| JSON / JSONC | `jsoncstyleguide.html` | JSON format and conventions |
| XML formats | `xmlstyle.html` | deciding XML vs alternatives, elements/attributes, format style |
| Markdown | `docguide/style.md` | readable source, headings, links, code blocks, tables |
| Documentation strategy | `docguide/philosophy.md`, `docguide/best_practices.md` | concise, fresh, maintainable documentation |
| README | `docguide/READMEs.md` | package intent, contacts, status, usage, deeper documentation |
| AngularJS | `angularjs-google-style.html` | legacy AngularJS-specific conventions |
| Common Lisp / Vim script | `lispguide.xml`, `vimscriptguide.xml` | language-specific style |

Kotlin and Dart are linked by the snapshot but live outside this repository.
For those languages, use current platform guidance plus local project rules.

## Conservative Cross-Language Principles

These are safe generalizations supported most clearly by the Go and
documentation guides; they are defaults, not replacements for the source above.

- Optimize for a future reader: code purpose and rationale should be clear.
- Prefer simple, direct implementations over needless abstraction or cleverness.
- Use names to convey meaning; comments explain rationale, constraints, and
  safe API use rather than narrating obvious code.
- Keep errors and test failures useful and observable.
- Keep documentation short, source-readable, portable, and updated with code.
- Delete stale or duplicate documentation instead of preserving misleading text.
- Add tests, examples, checks, or focused validation in proportion to risk.
- Maintain local consistency. A project-local convention wins unless the user
  explicitly asks for a style migration.

## Documentation Rules Relevant To This Skill

For generated Markdown such as design-system files, prefer:

- One clear H1, a brief introduction, and descriptive H2 sections.
- Plain Markdown over embedded HTML when possible.
- Informative links and fenced code blocks with a language identifier.
- Readable source text; line wrapping is useful but headings, tables, links, and
  code blocks may be longer where needed.
- A small, accurate document over exhaustive but stale material.
- README files that explain what a package contains, its use, status/contacts
  when relevant, commands or examples, and deeper links.

## UI Implementation Overlay

When this reference is used alongside Material 3:

- Material 3 decides the semantic UI system: tokens, components, states,
  layouts, and accessibility behavior.
- The host project decides its framework, libraries, and final implementation
  conventions.
- This public styleguide helps make the resulting code and documentation clear,
  consistent, and maintainable.

Never derive a button variant, color token, breakpoint, or interaction rule from
this engineering-only source.
