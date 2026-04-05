# LLM-Wiki: Personal Exocortex

A template and architecture for maintaining a knowledge base in Obsidian, where AI acts as an editor and writer rather than just a search engine.

This approach synthesizes ideas from three authors: the core concept of an LLM-wiki in plain Markdown belongs to Andrej Karpathy. The semantic directory structure is borrowed from Farzaa. The "Lineage" rule (mandatory linking of all facts to source files in `raw/`) is adopted from Brad Groux's methodology.

No complex RAG pipelines, vector databases, or embeddings. The entire wiki is a set of plain text files. Modern models with massive context windows (Gemini 1.5/3.1 Pro, Claude 3.5/4.6 Sonnet) can easily ingest millions of tokens at once. It's simpler and more reliable to feed them the entire text.

## How it works physically

1. **Capture:** You drop any file (a quick note, a log, a whiteboard photo, a voice memo) into the `raw/` folder.
2. **Synthesis:** An AI agent (e.g., OpenClaw, a local script, or Claude Code) reads `raw/`, reviews the current wiki structure, and organically updates or creates relevant articles.
3. **Result:** The agent makes a `git commit`, and polished, fully rewritten pages with links to the original sources appear in your Obsidian vault.

## Sync Topology

Since personal knowledge management involves mobile devices, PCs, and an AI agent (server/cloud), setting up the right data flow is crucial:

**Option A: Pure Git (Free)**
All devices and the AI agent sync via a single Git repository.
*   **Pros:** Completely free. Single source of truth.
*   **Cons:** Git on mobile devices (especially iOS) relies on third-party apps (like Working Copy) and can be clunky. There is a risk of merge conflicts if the AI and user edit files simultaneously.

**Option B: Hybrid (Obsidian Sync + Git Bridge)**
Mobile devices and PCs sync via native Obsidian Sync. The PC is additionally configured to auto-push to a private Git repository (e.g., via the Obsidian Git plugin), from which the AI agent pulls and pushes files.
*   **Pros:** Seamless mobile experience. The AI operates in an isolated loop (Git) and doesn't break native phone synchronization.
*   **Cons:** Requires a paid Obsidian Sync subscription.

## Directory Structure (Digital Garden)

Categorization here is based on *meaning*, not data types. Recommended base structure for a personal wiki:

- `raw/` — raw sources (read-only, the AI never modifies these).
- `wiki/philosophies/` — work and life principles.
- `wiki/patterns/` — recurring behavioral loops (trigger -> reaction -> result).
- `wiki/decisions/` — important decisions (context and why they were made).
- `wiki/tensions/` — unresolvable value conflicts or trade-offs.
- `wiki/eras/` — chapters of life.
- `wiki/projects/` — projects (from idea to completion).
- `wiki/people/` — people and their impact.

## Three Core Rules for the AI (AGENTS.md)

The system prompt for the model (see `AGENTS.md`) rests on three pillars:
1. **No editorializing.** Articles are written dryly, like Wikipedia. Emotions are conveyed *only* through direct quotes from the raw notes.
2. **Lineage.** Any added fact must contain a `[[wikilink]]` to the source file in `raw/`. You should always be able to click and verify where the AI got the information.
3. **Writer, not a clerk.** The AI shouldn't just append new events to the end of a file (like a chronological log). It must organically weave new data into the existing text so the article reads as a cohesive narrative.