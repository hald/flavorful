# Flavorful

Recipe management and cooking assistance for Claude Cowork.

## Installation

1. In Claude Desktop (Mac OS), go to the Cowork section and open "Plugins" (lower left sidebar).
2. Click + (Add Plugin), then Browse Plugins.
3. Click the marketplace dropdown (defaults to `By Anthropic`) and select `Add marketplace from GitHub`.
4. In the URL field, enter `hald/flavorful`


## Available Plugins

| Plugin | Description |
|--------|-------------|
| `flavorful` | Recipe management: find, save, organize recipes |

## What It Does

- **Recipe management** — Find, save, and organize recipes
- **Context awareness** — Knows your dietary restrictions, equipment, preferences
- **Web import** — Save recipes from URLs, convert to markdown format

## Skills

| Skill | Description |
|-------|-------------|
| `find` | Search recipes by ingredient, tag, time, or dietary needs |
| `save` | Import a recipe from a URL |
| `start` | Initialize your cookbook structure |

These work conversationally — just tell Claude what you need.
Or use slash commands: `/flavorful:find`, `/flavorful:save`, etc.

## Getting Started

Flavorful runs in [Cowork](https://support.claude.com/en/articles/13345190-getting-started-with-cowork), Claude Desktop's task mode where Claude can read and write files on your computer.

**Important:** When you start a Cowork session, you must select your cookbook folder first. This is the folder where Flavorful will store your recipes, preferences, and cookbook collections.

1. Switch to the **Cowork** tab in Claude Desktop
2. When prompted to select a folder, **choose or create your cookbook folder** (e.g., `~/cookbooks` or `~/Documents/my-recipes`)
3. Run `/flavorful:start` to initialize your cookbook structure
4. Start saving and finding recipes

Cowork needs folder access to read and write your recipes. If you skip this step or select the wrong folder, the plugin won't be able to find or save your data.

### Example Session

```
You: /flavorful:start

Claude: [Creates cookbook structure in your folder]
        [Asks about dietary restrictions, equipment, etc.]

You: /flavorful:save https://www.kingarthurbaking.com/recipes/pizza-crust-recipe

Claude: [Imports recipe, saves to your cookbook]

You: /flavorful:find pizza dough

Claude: [Searches your recipes, returns matches]
```

## Data Location

Your recipes live in the **folder you select** on your computer. The plugin looks for:

- `COOK.md` — Your profile (dietary restrictions, preferences)
- `LIBRARY.md` — Your cookbook registry
- `cookbooks/` — Your recipe collections

```
your-folder/
├── COOK.md
├── LIBRARY.md
└── cookbooks/
    ├── recipes/
    └── ai-generated/
```
