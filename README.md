# Task Deck

Task Deck is a small kanban board for Obsidian. It keeps the board simple, but every card is still a real Markdown note in your vault.

I built it for tracking tasks without leaving Obsidian: lists, cards, labels, dates, and checklists in one view.

Screenshots will be added later.

## Support

If Task Deck is useful for your workflow, you can support the project here:

https://buymeacoffee.com/carbon06

## Features

- Kanban lists with drag-and-drop ordering
- Cards stored as Markdown notes under `Kanban Cards/`
- Inline card creation and renaming
- Global colored labels
- Start and due dates with a compact date picker
- Checklist progress on cards
- Card details rendered as Markdown
- Automatic sync for cards created outside the board

## Usage

- Run `Open Task Deck` from the command palette.
- Use `Add list` to create a new list.
- Use `Add card` under a list, then type the card name inline.
- Click a card to edit labels, details, dates, and checklist items.
- Use `Open note` when you want to work with the card as a normal Markdown file.
- Drag cards between lists and drag list headers to reorder columns.

If you create a Markdown card directly inside `Kanban Cards/`, Task Deck will pick it up and show it on the board.

## Development

Source files live in `src/`. After changing them, run:

```bash
node build.js
```

Obsidian loads the generated `main.js` file.

## Install

Download the release files and place them here:

```text
Your Vault/.obsidian/plugins/obsidian-tasks-kanban/
```

Then enable **Task Deck** from Obsidian's Community plugins settings.
