---
name: start
description: "Initialize a new Flavorful cookbook with the required file structure, preferences file, and library index. Use when the user wants to set up their cookbook for the first time or mentions getting started with Flavorful."
---

# Start

Initialize Flavorful and set up your cooking profile in the current folder.

## Instructions

### 1. Check What Exists

Check the **current working directory** (the folder the user selected) for existing setup:
- `COOK.md` — user profile
- `LIBRARY.md` — cookbook registry
- `cookbooks/` — recipe collections

### 2. Create Directory Structure

If not already set up, create the structure in the current folder:

```
[current folder]/
├── COOK.md
├── LIBRARY.md
└── cookbooks/
    ├── recipes/
    │   └── COOKBOOK.md
    └── ai-generated/
        └── COOKBOOK.md
```

### 3. Create LIBRARY.md

```markdown
# My Cookbooks

## recipes
All my recipes — personal creations and web imports.

## ai-generated
Recipes created by AI. Move favorites to recipes.
```

### 4. Create COOKBOOK.md Files

For `cookbooks/recipes/COOKBOOK.md`:
```markdown
# My Recipes

Personal recipes, adaptations, and web imports.
```

For `cookbooks/ai-generated/COOKBOOK.md`:
```markdown
# AI-Generated Recipes

Recipes created by Claude. Move favorites to recipes.
```

### 5. Gather User Context

Ask the user a few quick questions to populate COOK.md:

```
Let me set up your cookbook. A few quick questions:

1. Any dietary restrictions? (e.g., dairy-free, gluten-free, vegetarian)
2. Any foods you avoid or dislike?
3. What's your typical weeknight cooking time limit?
4. What equipment do you have? (e.g., instant pot, air fryer, dutch oven)
5. How many people do you usually cook for?
```

Keep it conversational. If they say "none" or seem unsure, that's fine — the profile can be updated later.

### 6. Create COOK.md

Based on their answers, create COOK.md:

```markdown
# About Me

[Name], cooking for [X]. [Skill level if mentioned].
Weeknight max: [time].

## Dietary

- Restrictions: [list or "none"]
- Preferences: [list or "none"]
- Avoid: [list or "none"]

## Equipment

[comma-separated list]

## Current Interests

- [anything they mentioned wanting to learn/try]
```

If they didn't provide much info, keep it minimal:

```markdown
# About Me

Cooking profile — update as needed.

## Dietary

- Restrictions: none noted
- Preferences: none noted

## Equipment

Standard kitchen setup
```

### 7. Confirm Setup

```
Your cookbook is ready!

What's next:
- /flavorful:save [url] — import your first recipe
- /flavorful:find [query] — search your recipes

Your profile is in COOK.md — edit anytime to update preferences.
```

## If Already Initialized

If already set up, count recipes and report:

```bash
find cookbooks -name "*.md" -path "*/recipes/*" | wc -l
```

Then:

```
Your cookbook is already set up.

- [X] recipes across your cookbooks
- Profile: COOK.md

Commands:
- /flavorful:find [query] — search recipes
- /flavorful:save [url] — import a recipe

Want me to show your current profile or update anything?
```

## Notes

- Don't overwhelm with questions — keep it quick and use interactive tools when appropriate
- If they seem technical, mention they can edit COOK.md directly
