---
name: start
description: "Initialize a new Flavorful cookbook with the required file structure, preferences file, and library index. Use when the user wants to set up their cookbook for the first time or mentions getting started with Flavorful."
---

# Start

Initialize Flavorful and set up your cooking profile.

## Instructions

### 1. Determine Cookbook Path

Check Claude's memory (userMemories) for a saved cookbook path.
- If found, use that path.
- If not found, ask the user where they'd like their cookbook folder to live (suggest `~/Documents/cookbook` as a default).
- Save the chosen path to memory using `memory_user_edits` so it persists across conversations.

### 2. Check What Exists

Check the cookbook path for existing setup:
- `CLAUDE.md` — project instructions
- `COOK.md` — user profile
- `cookbooks/` — recipe collections

### 3. Create Directory Structure

If not already set up, create the structure in the cookbook folder:

```
[cookbook path]/
├── cookbooks/
│   ├── recipes/
│   └── ai-generated/
```

### 4. Copy Template Files

Read and copy the bundled templates to their destinations:

| Template file | Destination |
|---|---|
| [templates/CLAUDE.md](templates/CLAUDE.md) | `./CLAUDE.md` |
| [templates/recipes-COOKBOOK.md](templates/recipes-COOKBOOK.md) | `./cookbooks/recipes/COOKBOOK.md` |
| [templates/ai-generated-COOKBOOK.md](templates/ai-generated-COOKBOOK.md) | `./cookbooks/ai-generated/COOKBOOK.md` |
| [templates/COOK.md](templates/COOK.md) | `./COOK.md` |

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

### 6. Update COOK.md

Based on their answers, update the COOK.md that was copied in step 4. Fill in their name, dietary info, equipment, and any interests mentioned. If they didn't provide much info, leave the defaults from the template.

### 7. Confirm Setup

```
Your cookbook is ready!

What's next:
- /cookbook:save [url] — import your first recipe
- /cookbook:find [query] — search your recipes

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
- /cookbook:find [query] — search recipes
- /cookbook:save [url] — import a recipe

Want me to show your current profile or update anything?
```

## Notes

- Don't overwhelm with questions — keep it quick and use interactive tools when appropriate
- If they seem technical, mention they can edit COOK.md directly
