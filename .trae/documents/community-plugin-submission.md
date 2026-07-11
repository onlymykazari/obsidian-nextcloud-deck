# 上架 Obsidian Community Plugins — PR 模板

## PR 标题

```
Add plugin: NextDeck
```

## PR 描述（Markdown）

复制以下内容到 [obsidian-releases](https://github.com/obsidianmd/obsidian-releases) PR 描述框：

---

### I have read and adhered to the developer policies

- [x] I have read the [Developer Policies](https://docs.obsidian.md/Developer+policies)
- [x] I have read the [tips and tricks](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines) for making a good plugin

### Plugin info

- **Repo**: https://github.com/onlymykazari/obsidian-nextcloud-deck
- **Release**: https://github.com/onlymykazari/obsidian-nextcloud-deck/releases/tag/1.0.0
- **Manifest**: https://github.com/onlymykazari/obsidian-nextcloud-deck/blob/main/manifest.json

### Description

NextDeck adds Kanban boards to Obsidian where every card is a real Markdown note in the vault, and offers optional two-way sync against a self-hosted [Nextcloud Deck](https://github.com/nextcloud/deck) instance. Cards keep their frontmatter (labels / dates / checklist / list assignment) in sync across devices; attachments live under the vault's board folder and mirror to Nextcloud through the official Deck attachment API. All credentials are AES-GCM encrypted at rest.

Forked from [Task Deck](https://github.com/ismailivanov/task-deck) and refocused on Nextcloud as the sync backend.

### Compliance checklist

- [x] `manifest.json` at repo root with required fields; matching copy in the release
- [x] Release tag equals version (`1.0.0`, no `v` prefix)
- [x] Release attaches `main.js`, `manifest.json`, `styles.css` as assets
- [x] No `innerHTML` / `outerHTML` / `insertAdjacentHTML`
- [x] No `eval` / `new Function()` / `document.write`
- [x] All network I/O goes through Obsidian's `requestUrl` (no raw `fetch` / `XMLHttpRequest`)
- [x] `setInterval` and `setTimeout` are registered via `registerInterval` where they outlive a modal / view lifecycle
- [x] `onunload` cleans up its own state and does NOT `detachLeavesOfType`
- [x] Plugin id (`obsidian-nextcloud-deck`) is kept for install-in-place upgrades; display name (`NextDeck`) has no "Obsidian" / "Plugin" prefix
- [x] `isDesktopOnly: false` — no Node.js API dependencies

### Known style notes

A handful of semantic accent colours in `styles.css` (`#f59e0b` lock, `#8b5cf6` progress marker, `#8cc63f` completed checkbox) are hard-coded rather than pulled from Obsidian CSS variables. These represent domain-specific status colours the theme system does not expose. They render correctly against both default light and dark themes; theme authors can still override via the plugin's public class names.

### How to test

1. Enable the plugin.
2. Run **Open board** from the command palette.
3. Create a board, add a list, add a card.
4. Open **Settings → NextDeck → Nextcloud sync** and either **Sign in with browser** against a Nextcloud instance with Deck installed, or leave sync disabled — the plugin fully works standalone.

---

## Steps to submit

```bash
# 1. Fork obsidianmd/obsidian-releases on the web
# 2. Clone your fork locally
git clone https://github.com/<your-username>/obsidian-releases.git
cd obsidian-releases

# 3. Edit community-plugins.json — append this entry as the LAST array item
#    (keep alphabetical / append-at-end depending on current convention;
#    the maintainers will move it if needed)
```

Append to `community-plugins.json`:

```json
{
  "id": "obsidian-nextcloud-deck",
  "name": "NextDeck",
  "author": "onlymykazari",
  "description": "Markdown-backed kanban boards with two-way Nextcloud Deck sync.",
  "repo": "onlymykazari/obsidian-nextcloud-deck"
}
```

Then:

```bash
git checkout -b add-nextdeck
git add community-plugins.json
git commit -m "Add plugin: NextDeck"
git push origin add-nextdeck
# 4. Open a PR on GitHub against obsidianmd/obsidian-releases:main
#    Paste the PR description from the "PR 描述" section above.
```

## After the PR is merged

- Users can install **NextDeck** from Settings → Community plugins → Browse
- Future releases auto-distribute: just tag GitHub release with the new semver (e.g. `1.0.1`) and attach the 3 files
- Do **NOT** re-open a PR to `obsidian-releases` for version bumps
- Update `manifest.json` in repo AND in the release asset — they must match

## Common review feedback (in case reviewer pushes back)

1. **"Please remove `console.error` calls"** — usually fine to keep for error paths, but if pushed, wrap in a debug flag.
2. **"Use Notice for user-facing errors instead of console"** — we already do (`new Notice(...)`); console is only for devtools debugging.
3. **"vault.adapter.write is discouraged"** — we fall back to it only after `vault.getAbstractFileByPath` returns null despite `adapter.exists === true`. The reviewer usually accepts this justification if you cite it.
4. **"You use `setTimeout` in modals.js line 1002 / 1713 without registerInterval"** — modals are short-lived; setTimeout cleaned on close via closure. If pressed, we can move to `this.registerDomEvent` equivalents.
5. **Style hard-coded colours** — see "Known style notes" above; acceptable if justified.
