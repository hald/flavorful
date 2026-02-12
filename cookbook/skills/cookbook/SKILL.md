---
name: cookbook
description: "Understand the Flavorful cookbook file structure, recipe markdown format, and user preferences in COOK.md. Use when you need to read, interpret, or work with cookbook files but no specific action like find, save, or start is needed."
user-invocable: false
---

# Cookbook

This skill teaches you to understand and work with Flavorful's recipe format.

## Format Overview

A simple, markdown-based format for cooking knowledge:

- **User files** (COOK.md) — prose and lists, easy to edit
- **Recipe files** — minimal YAML frontmatter + markdown body
- **Cookbook descriptions** (COOKBOOK.md per collection) — what each cookbook is for

## File Locations

Files live in the **user's selected folder** (the current working directory). Structure:

```
[user's folder]/
├── COOK.md               # User profile
└── cookbooks/
    ├── recipes/          # All user recipes
    │   └── COOKBOOK.md    # Collection description
    └── ai-generated/     # Created by AI
        └── COOKBOOK.md    # Collection description
```

**Important:** Do NOT use hardcoded paths. Work with whatever folder the user has selected.

## Reading User Context

**Always read COOK.md first** when helping with cooking tasks. It contains:

- Dietary restrictions and preferences
- Equipment available
- Time constraints
- Current interests

Example COOK.md:
```markdown
# About Me

Hal, cooking for 2. Intermediate home cook.
Weeknight max: 45 minutes.

## Dietary

- Restrictions: dairy-free
- Preferences: pescatarian
- Avoid: blue cheese, raw tomatoes

## Equipment

instant pot, cast iron, dutch oven, food processor
```

**How to parse:** Look for the Dietary section. Extract restrictions, preferences, and items to avoid. Use these to filter recipe suggestions.

## Discovering Cookbooks

Each cookbook is a subfolder under `cookbooks/` with a `COOKBOOK.md` describing the collection.

**How to discover:** Scan `cookbooks/*/COOKBOOK.md` to find all available cookbooks. Don't hardcode cookbook names — users may create custom cookbooks or import shared ones.

## Recipe Format

Recipes use minimal YAML frontmatter for searchable metadata:

```markdown
---
title: Miso-Glazed Salmon
source: https://cooking.nytimes.com/recipes/1234
time: 27 min
tags: [weeknight, seafood, japanese]
dietary: [dairy-free, pescatarian]
---

# Miso-Glazed Salmon

A weeknight favorite...

## Ingredients
...

## Instructions
...

## Notes

- Tips or substitutions
```

### Frontmatter Fields

| Field | Type | Purpose |
|-------|------|---------|
| `title` | string | Recipe name (required) |
| `source` | string | URL, "Grandma's recipe", "AI Generated (Claude)" |
| `time` | string | Total time ("27 min", "1 hr") |
| `tags` | array | Searchable keywords |
| `dietary` | array | Dietary flags for filtering |

### Body Sections

Standard order:
1. Description (no heading, first paragraph)
2. `## Ingredients` — bulleted list
3. `## Instructions` — numbered steps
4. `## Notes` — tips, substitutions (optional)

## File Naming

Recipe files: lowercase, hyphens, `.md` extension
- `miso-glazed-salmon.md`
- `chocolate-chip-cookies.md`
- `quick-weeknight-pasta.md`

## Error Handling

If COOK.md doesn't exist:
- Suggest running `/cookbook:start` to set up the cookbook

If no `cookbooks/` directory or no COOKBOOK.md files found:
- Suggest running `/cookbook:start` to set up the cookbook

If a cookbook is empty:
- Note that no recipes were found
- Suggest `/cookbook:save [url]` to import some
