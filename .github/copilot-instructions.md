# Copilot Instructions for javascript-notes

A Jekyll-based technical documentation site covering JavaScript, TypeScript, React, and related web development topics.

## Code Style

**Markdown Format:**
- Use ATX-style headings (`#`, `##`, `###`, etc.) for document structure
- Wrap code blocks with triple backticks and language identifiers (e.g., ` ```js `, ` ```typescript `, ` ```yaml `)
- Use inline code with single backticks for technical terms, function names, and code snippets
- Maintain consistent indentation (2 spaces for nested lists and code blocks)
- Link external references with [text](url) format, including MDN documentation links

**File Organization:**
- Documentation files are in [`docs/`](docs/) directory as individual markdown files
- Each file focuses on a specific technology or concept (e.g., [javascript.md](docs/javascript.md), [typescript.md](docs/typescript.md))
- File names should be lowercase with hyphens (e.g., `github-actions.md`, `design-patterns.md`)

## Architecture

**Documentation Structure:**
The site uses Jekyll with the "just-the-docs" theme ([_config.yml](_config.yml)). Content is organized as:
- **Topic files in [`docs/`](docs/)**: Each markdown file (`*.md`) represents a major topic
- **Navigation**: Just-the-docs automatically generates sidebar navigation from file structure
- **Frontmatter**: Each doc file requires Jekyll frontmatter with `title`, `layout: default`

**File Relationships:**
- [index.md](index.md) serves as the site homepage
- [_config.yml](_config.yml) configures Jekyll, theme, and site metadata
- [Gemfile](Gemfile) and [Gemfile.lock](Gemfile.lock) manage Ruby dependencies (Jekyll, just-the-docs theme)

**Content Patterns:**
- Use section headings (`## Section Name`) to organize topics within files
- Include code examples with language-specific syntax highlighting
- Reference external documentation (especially MDN) for web APIs
- Group related concepts under appropriate heading levels

## Build and Test

**Prerequisites:**
- Ruby with Bundler (required for Jekyll)
- Node.js for optional TypeScript/JavaScript tooling

**Commands:**
```bash
# Install dependencies
bundle install

# Build the site locally
bundle exec jekyll build

# Serve locally with live reload (default: http://localhost:4000)
bundle exec jekyll serve

# Build for production (GitHub Pages deployment)
bundle exec jekyll build --source . --destination _site
```

**Validation:**
- Check that markdown files have proper frontmatter (`title` and `layout: default`)
- Verify code blocks use language identifiers for syntax highlighting
- Test local build with `bundle exec jekyll serve` before committing significant changes

## Project Conventions

**Frontmatter Template:**
Every markdown file in [`docs/`](docs/) must include YAML frontmatter:
```yaml
---
title: Topic Name
layout: default
---
```

**Content Guidelines:**
- Open with `# {Title}` matching the `title` field in frontmatter
- Use consistent terminology across related documents
- When explaining concepts, progress from simple to complex
- Include practical code examples where applicable
- Link to related topics using internal references: `[topic-name](topic-name.md)`

**Examples of Well-Structured Topics:**
- [javascript.md](docs/javascript.md): Organized by language features (data types, scope, functions)
- [typescript.md](docs/typescript.md): May follow similar hierarchical pattern
- Each has clear section headings and code examples

## Integration Points

**Jekyll Processing:**
- The site automatically builds via Jekyll when files in `docs/` are updated
- "just-the-docs" theme processes markdown and generates HTML
- Site is deployed via GitHub Pages from the `main` branch

**GitHub Pages:**
- Triggered automatically on push to main branch via Actions workflow
- Check [.github/workflows/](.github/workflows/) for build and deploy configuration
- Build artifacts are generated in `_site/` directory (excluded from version control)

**Dependencies:**
- [Gemfile](Gemfile): Specifies Jekyll and just-the-docs theme
- [package.json](package.json): Optional Node.js dependencies (RxJS ^7.8.2, ts-node ^10.9.2)
- [tsconfig.json](tsconfig.json): TypeScript configuration (target: ES2016, module: commonjs)

## Working with Documentation

**When Adding New Topics:**
1. Create a new file in [`docs/`](docs/) with descriptive name
2. Add required frontmatter with title and layout
3. Structure with clear section headings and examples
4. Test locally with `bundle exec jekyll serve`
5. Commit and push to trigger GitHub Pages build

**When Editing Existing Topics:**
- Maintain consistent heading hierarchy within files
- Preserve existing code examples and explanations
- Update related cross-references if topic scope changes
- Run local build to verify no formatting issues
