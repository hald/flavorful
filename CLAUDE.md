# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Flavorful is a **Claude Cowork plugin marketplace** that provides recipe management and cooking assistance. It is not a traditional software project — there is no build step, no tests, and no runtime code. The entire plugin is markdown skill files and JSON configuration.

The plugin installs as `cookbook@flavorful` in Cowork.

## Architecture

Two-level structure: **marketplace** → **plugin** → **skills**

```
flavorful/                          # Marketplace root
├── .claude-plugin/
│   └── marketplace.json            # Marketplace registry (name: "flavorful")
├── cookbook/                        # Plugin directory
│   ├── .claude-plugin/
│   │   └── plugin.json             # Plugin config (name: "cookbook")
│   └── skills/
│       ├── start/SKILL.md          # Initialize cookbook structure
│       ├── cookbook/SKILL.md        # Internal skill: recipe format reference (not user-invocable)
│       ├── find/SKILL.md           # Search recipes by query
│       └── save/SKILL.md           # Import recipe from URL or text
```

## Skills

Skills are SKILL.md files with YAML frontmatter (`name`, `description`, optional `user-invocable: false`) followed by markdown instructions that teach Claude how to perform the task.

- **start** — Creates COOK.md (user profile), directory structure with COOKBOOK.md descriptions, and a CLAUDE.md in the user's cookbook folder
- **cookbook** — Internal reference skill (not user-invocable) defining the recipe markdown format: YAML frontmatter (title, source, time, tags, dietary) + markdown body (ingredients, instructions, notes)
- **find** — Searches recipes using grep with expanded search terms, filters by user dietary restrictions from COOK.md
- **save** — Fetches URL, extracts recipe data, infers tags/dietary flags, formats to recipe markdown, saves to `cookbooks/recipes/`

## User's Cookbook Structure (created by start skill)

The plugin operates on files in the user's selected Cowork folder:
- `COOK.md` — User profile (dietary restrictions, equipment, preferences)
- `cookbooks/` — Recipe collections, each with a `COOKBOOK.md` description and recipe markdown files

## Reference Docs

For current plugin/skill/marketplace specs, fetch these URLs:
- https://code.claude.com/docs/en/plugins
- https://code.claude.com/docs/en/skills
- https://code.claude.com/docs/en/hooks-guide
- https://code.claude.com/docs/en/plugin-marketplaces
