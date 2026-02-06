---
name: find
description: "Search the user's Flavorful recipe collection by ingredients, cuisine, technique, or dietary needs. Use when the user asks what they can cook, searches for recipes, or mentions available ingredients."
---

# Find

Search across all your cookbooks for recipes matching a query.

## First-Run Check

Before proceeding, verify the cookbook is initialized:
1. Check if `COOK.md` exists in current folder
2. If missing, inform user and suggest starting setup:

   "Your cookbook isn't set up yet. Would you like me to initialize it first?"

   If yes, run the start workflow. If no, explain they need to set up before using this feature.

## Usage

```
/cookbook:find [query]
```

## Query Types

The command supports natural language queries. Parse the intent:

| Query | What to search |
|-------|---------------|
| `salmon` | Ingredient in title or ingredients list |
| `weeknight` | Tag match |
| `dairy-free` | Dietary flag |
| `under 30 minutes` | Time constraint |
| `quick seafood dairy-free` | Combined: time + ingredient + dietary |
| `japanese` | Tag (cuisine) |
| `something with chicken` | Ingredient search |

## Instructions

### 1. Load User Context

Read `COOK.md` (in current folder) to understand:
- Dietary restrictions (filter out incompatible recipes)
- Preferences (rank matching recipes higher)
- Time constraints (default weeknight limit)

### 2. Expand Search Terms

Before searching, expand the query to related terms. This improves recall. For example:

| User searches for | Also search for |
|-------------------|-----------------|
| `salmon` | fish, seafood |
| `chicken` | poultry |
| `pasta` | noodles, spaghetti |
| `quick` | weeknight, fast, "time: [12][0-9] min" |
| `japanese` | miso, teriyaki, soy |
| `mexican` | taco, burrito, salsa |
| `italian` | pasta, basil, parmesan |
| `healthy` | vegetable, salad |
| `vegetarian` | vegan, meatless |

**Example:** User searches "salmon" → search for `salmon|fish|seafood`

### 3. Search with Grep (Programmatic)

**Do NOT load all recipes into context.** Use grep to find matches efficiently.

**Search pattern for ingredients/title:**
```bash
grep -ril "salmon\|fish\|seafood" cookbooks/*/
```

**Search pattern for tags:**
```bash
grep -l "tags:.*seafood" cookbooks/**/*.md
```

**Search pattern for dietary:**
```bash
grep -l "dietary:.*dairy-free" cookbooks/**/*.md
```

**Search pattern for time (quick = under 30 min):**
```bash
grep -l "time: [12][0-9] min" cookbooks/**/*.md
```

**Combined search example ("quick seafood"):**
```bash
# Find seafood recipes, then filter for quick ones
grep -ril "salmon\|fish\|seafood\|shrimp" cookbooks/*/ | \
  xargs grep -l "time: [12][0-9] min\|tags:.*quick\|tags:.*weeknight"
```

### 4. Read Matching Recipes

Once grep returns a list of files, **then** read those specific files to extract details for display.

Only read the recipes that matched — not all recipes.

### 5. Filter by User Restrictions

Remove recipes that conflict with user's dietary restrictions from COOK.md.

Exception: If user explicitly searches for something that violates their restrictions (e.g., a pescatarian searching for "chicken"), include those results but note the conflict.

### 6. Present Results

Show top 5-10 matches:

```
Found 3 recipes matching "quick seafood":

1. **Miso-Glazed Salmon**
   27 min · dairy-free · [weeknight, japanese, seafood]

2. **Garlic Butter Shrimp**
   15 min · gluten-free · [quick, seafood]

3. **Instant Pot Salmon**
   20 min · dairy-free · [instant-pot, healthy]

Say "show 1" or the recipe name for details.
```

Format: **Title** / time · dietary flags · [tags]

## Search Strategy Summary

```
1. Parse query → identify ingredients, tags, dietary, time constraints
2. Expand terms → add related search terms
3. Grep for matches → get list of matching files
4. Read matched files only → extract title, time, tags, dietary
5. Filter by user restrictions → remove incompatible
6. Present ranked results
```

## Edge Cases

### No Results

```
No recipes found matching "thai curry".

Suggestions:
- /cookbook:save [url] to import a recipe
- Try a broader search like "curry" or "asian"
```

### No Cookbooks

```
Your cookbook is empty! Let's add some recipes.

- /cookbook:save [url] — import from a website
- Or tell me a recipe and I'll help you write it down
```

### Ambiguous Query

If the query is very broad (e.g., "dinner"):

```
That's pretty broad! Here are some options:

By cuisine: italian, japanese, mexican, indian
By protein: chicken, fish, vegetarian
By time: quick (under 30 min), weeknight

What sounds good?
```

## Examples

```
User: /cookbook:find salmon
→ grep for "salmon|fish|seafood" across recipes

User: /cookbook:find quick weeknight
→ grep for "quick|weeknight" in tags AND time < 30 min

User: /cookbook:find dairy-free pasta
→ grep for "dairy-free" in dietary AND "pasta|noodle" in content

User: /cookbook:find something japanese
→ grep for "japanese" in tags OR "miso|soy|teriyaki" in content
```
