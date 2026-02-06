---
name: save
description: "Import a recipe from a URL or save a recipe to the user's Flavorful cookbook. Use when the user shares a recipe link or asks to save, store, or import a recipe."
---

# Save

Import a recipe from a website and save it to your cookbook.

## First-Run Check

Before proceeding, verify the cookbook is initialized:
1. Check if `COOK.md` exists in current folder
2. If missing, inform user and suggest starting setup:

   "Your cookbook isn't set up yet. Would you like me to initialize it first?"

   If yes, run the start workflow. If no, explain they need to set up before using this feature.

## Usage

```
/flavorful:save [url]
/flavorful:save [url] --to ai-generated
/flavorful:save [url] --tags quick,favorites
```

## Instructions

### 1. Fetch the URL

Use web fetch to get the page content.

If the URL is inaccessible:
```
I couldn't access that URL. A few things to try:
- Check if the link is correct
- Some sites block automated access
- You can paste the recipe text directly and I'll format it
```

### 2. Extract Recipe Data

Parse the page for recipe content. Look for:

**Required:**
- Title
- Ingredients list
- Instructions/steps

**If available:**
- Total time (prep + cook)
- Servings/yield
- Source attribution (author name)
- Description/intro paragraph

**Common recipe schema locations:**
- JSON-LD structured data (`<script type="application/ld+json">`)
- Microdata (schema.org/Recipe)
- Standard HTML patterns (ingredient lists, numbered steps)

### 3. Infer Metadata

**Tags** — infer from content:
- Cuisine: look for cuisine-specific ingredients or techniques
- Meal type: breakfast, lunch, dinner, dessert, snack
- Speed: "quick" if under 30 min, "weeknight" if under 45 min
- Method: grilled, baked, one-pot, no-cook, instant-pot

**Dietary flags** — scan ingredients:
- `dairy-free`: no milk, cream, butter, cheese, yogurt
- `gluten-free`: no flour, bread, pasta, soy sauce (unless GF noted)
- `vegetarian`: no meat or fish
- `vegan`: no animal products
- `pescatarian`: fish okay, no meat

Be conservative — only add flags you're confident about.

### 4. Format Recipe

Create the recipe file:

```markdown
---
title: [Recipe Title]
source: [original URL]
time: [X min]
tags: [inferred tags]
dietary: [detected dietary flags]
---

# [Recipe Title]

[Description paragraph if available]

## Ingredients

- [ingredient 1]
- [ingredient 2]
...

## Instructions

1. [Step 1]
2. [Step 2]
...

## Notes

- [Any tips or notes from original]
```

### 5. Determine Save Location

**Default:** `cookbooks/recipes/`
**AI-generated recipes:** `cookbooks/ai-generated/`

Use `--to [cookbook]` to save to a specific cookbook.

### 6. Generate Filename

Lowercase, hyphenated, from title:
- "Miso-Glazed Salmon" → `miso-glazed-salmon.md`
- "The Best Chocolate Chip Cookies" → `chocolate-chip-cookies.md`
- "Mom's Famous Pot Roast" → `moms-famous-pot-roast.md`

Check for conflicts — if file exists, append `-2`, `-3`, etc.

### 7. Save and Confirm

Write the file and confirm:

```
Saved: Miso-Glazed Salmon

📍 cookbooks/recipes/miso-glazed-salmon.md
⏱️ 27 min
🏷️ weeknight, seafood, japanese
🥗 dairy-free, pescatarian

Want me to add or change any tags, or show the full recipe?
```

## Handling Poor Extraction

If the page is hard to parse (paywalled, heavy JavaScript, etc.):

```
I found a recipe but couldn't extract all the details cleanly.

Here's what I got:
- Title: [title]
- Ingredients: [partial list or "couldn't extract"]
- Instructions: [partial or "couldn't extract"]

Options:
1. I can save what I have and you can fill in the gaps
2. Paste the recipe text and I'll format it properly
3. Try a different URL
```

## Manual Recipe Entry

If user pastes recipe text instead of URL:

```
User: /flavorful:save

Here's my grandma's recipe:
[recipe text]
```

Parse the text, format it, and save. Ask for any missing info:

```
I've formatted Grandma's Pot Roast. A few quick questions:
- About how long does it take?
- Any dietary notes? (looks dairy-free to me)

[Shows preview]

Save this to your cookbook?
```

## Options

- `--to [cookbook]` — Save to a specific cookbook (e.g., `--to ai-generated`, `--to family-recipes`)
- `--tags [tag1,tag2]` — Add specific tags
