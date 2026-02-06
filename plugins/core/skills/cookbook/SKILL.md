---
name: cookbook
description: "Understand the Flavorful cookbook file structure, recipe markdown format, and user preferences in COOK.md. Use when you need to read, interpret, or work with cookbook files but no specific action like find, save, or start is needed."
user-invocable: false
---

# Cookbook

This skill teaches you to understand and work with Flavorful's recipe format.

## Format Overview

A simple, markdown-based format for cooking knowledge:

- **User files** (COOK.md, LIBRARY.md) — prose and lists, easy to edit
- **Recipe files** — minimal YAML frontmatter + markdown body

## File Locations

Files live in the **user's selected folder** (the current working directory). Structure:

```
[user's folder]/
├── COOK.md               # User profile
├── LIBRARY.md            # Cookbook registry
└── cookbooks/
    ├── recipes/          # All user recipes
    └── ai-generated/     # Created by AI
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

## Reading LIBRARY.md

This tells you which cookbooks exist:

```markdown
# My Cookbooks

## recipes
All my recipes — personal creations and web imports.

## ai-generated
Recipes created by AI. Move favorites to recipes.
```

**How to parse:** Each `## heading` is a cookbook name. The path is `cookbooks/{name}/`.

**Important:** Always read LIBRARY.md to discover which cookbooks exist. Don't hardcode cookbook names — users may create custom cookbooks or import shared ones.

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
- Suggest running `/flavorful:start` to set up the cookbook

If a cookbook is empty:
- Note that no recipes were found
- Suggest `/flavorful:save [url]` to import some
