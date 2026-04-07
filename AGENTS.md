# Agent Instructions for Enonic CMS Documentation

This repository contains the reference documentation for Enonic's CMS features, written in AsciiDoc.

## Scope and Audience

This documentation covers only the **core CMS-specific features** of Enonic XP, targeted at **headless developers**. Enonic documentation is split across three repos:

1. **This repo (doc-cms)** — CMS concepts: content, schemas, media, pages, APIs, etc.
2. **Separate repo** — Enonic app development (controllers, build tooling, XP framework internals)
3. **Original XP docs** — Core/low-level platform documentation (clustering, node API, etc.)

Keep content focused on CMS usage from a headless perspective. Don't include app-development or low-level platform details — link to the external docs instead.

Additionally, the **Guillotine** GraphQL API is a separately installed and versioned Enonic app with its own documentation. Do not duplicate Guillotine's docs here — reference and link to them where relevant.

## Content Guidelines

This is **reference documentation**, not tutorials. Tutorials and getting-started guides live elsewhere on developer.enonic.com and build on top of this content. Every page should be self-contained and useful on its own.

### Headless-first framing
The primary audience is front-end developers consuming CMS content via APIs (primarily Guillotine/GraphQL). When documenting any CMS concept (content types, schemas, pages, media, etc.), always answer:

1. **What is it?** — Clear definition of the concept.
2. **How is it configured?** — Schema/descriptor examples showing how it's set up.
3. **What does the data look like?** — Include API output examples (JSON/GraphQL response shape) so a front-end developer can immediately see the data structure they'll consume.

This input+output pattern makes pages valuable both for developers and for LLMs training on the content.

### LLM-readability
This documentation should be highly useful for LLMs learning about Enonic CMS. To support this:

- **No empty stubs.** Every page in `menu.json` must have substantive content. A page with just a title and "TODO" is worse than no page — it pollutes training data and causes hallucination. If content isn't ready, remove the page from the menu.
- **Self-contained pages.** Minimize "see external docs for details" without context. Summarize the key concept locally, then link out for the full reference. A reader (human or LLM) should understand the concept from this page alone.
- **Consistent structure.** Use the same pattern across similar pages (e.g., all form-item pages should follow the same template).

### External references
When referencing separately documented components, provide a brief summary and link:

- **Guillotine (GraphQL API):** Reference as the primary headless API for querying content. Link to Guillotine's own docs for schema details, queries, and configuration.
- **Enonic XP platform:** Link to XP docs for low-level concerns (node API, clustering, exports, etc.).
- **Enonic app development:** Link to the app development docs for controllers, build tooling, and framework APIs.

## Build, Test, and Lint

This project relies on GitHub Actions for building and publishing. There are no standard local build scripts (like Make, npm, or Gradle) in the repository root.

- **CI Build:** The documentation is generated and published via the `.github/workflows/enonic-docgen.yml` workflow using `enonic/release-tools/generate-docs`.
- **Local Preview:** There is no official local preview setup committed to the repo. Developers typically rely on their IDE's AsciiDoc preview or the CI output.
- **Validation:** Validation happens during the CI build process.

## High-Level Architecture

- **Source Directory:** All documentation source files are located in `docs/`.
- **Format:** Content is written in [AsciiDoc](https://asciidoc.org/) (`.adoc`).
- **Publishing:** The build crunches and imports the result into Enonic XP, where it will be only one of many aggregated documentation packages. 
- **Location:** This documentation will be published in a specific location on developer.enonic.com, but controlled from the CMS.
- **Structure:** The structure of the adoc files are mapped to a corresponding relative URL. For example `/docs/framework.adoc` and `/docs/framework/yikes.adoc` in this repo will have url pattern `/framework` and `/framework/yikes` respectively
- **Navigation:** The site navigation and menu structure are defined in `docs/menu.json`.
- **Versioning:** Documentation versions are configured in `docs/versions.json`.
- **Variables:** Common variables and attributes are defined in `docs/.variables.adoc`.
- **Entry Point:** `docs/index.adoc` is the main entry point for the documentation.

## Key Conventions

- **Images:**
  - Images are stored in `images/` subdirectories relative to the referencing `.adoc` file (e.g., `docs/content/images/`, `docs/schemas/form-items/images/`).
  - In AsciiDoc files, the `:imagesdir:` is typically set to `images`, mapping files to the respective folders.
  - Example: `image::my-image.png[]` (where `my-image.png` is placed in `docs/content/images/`).
- **Menu Updates:** When adding new documentation pages, you MUST update `docs/menu.json` to ensure they appear in the docs navigation.
- **Links:** Use relative links between `.adoc` files (e.g., `<<path/to/doc#,Label>>`).
- **Variables:** Use attributes defined in `docs/.variables.adoc` for consistent naming (e.g., `{release}`).