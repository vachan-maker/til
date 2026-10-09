# AGENTS.md

## About

- Personal wiki / "second brain" for quick reference: commonly used commands, programming concepts, and other topics.
- Goal: look things up fast instead of searching the web repeatedly.
- Built with [mdbook](https://rust-lang.github.io/mdBook/).

## Commands

| Task            | Command       |
| --------------- | ------------- |
| Serve locally   | `mdbook serve` |
| Build the book  | `mdbook build` |

## Writing Guidelines

- Write in Markdown.
- Be **precise, concise, and accurate**. No filler, no long preambles.
- Prefer short explanations, tables, and code snippets over prose.
- Verify commands and facts before adding them.

## AI-Generated Content

Every page whose content is AI-generated **must** start with this warning, placed at the very top of the page (before any other content):

```md
> [!WARNING]
> This content is AI-generated. Verify before relying on it.
```

## Admonitions

Use these mdbook admonitions where appropriate:

```md
> [!NOTE]
> General information or additional context.

> [!TIP]
> A helpful suggestion or best practice.

> [!IMPORTANT]
> Key information that shouldn't be missed.

> [!WARNING]
> Critical information that highlights a potential risk.

> [!CAUTION]
> Information about potential issues that require caution.
```