# SN Commands

A Firefox browser extension that adds a customisable backslash command palette to ServiceNow, letting you store and run JavaScript snippets on any ServiceNow tab with a single keystroke.

---

## Features

- **Command palette** — trigger with `\`, `Ctrl+Shift+Space`, or `F2` on any ServiceNow tab
- **Custom scripts** — store named JavaScript snippets with optional hint text
- **Inline editor** — create, edit, and run commands directly from the extension popup
- **Fullscreen editor** — expand the script editor for longer scripts
- **Import / Export** — back up and restore your commands as JSON
- **Bulk delete** — select multiple commands with checkboxes and delete in one go
- **Clear all** — wipe all commands at once (with confirmation)
- **Light / Dark theme** — toggle in the popup or settings page; the palette follows your theme
- **ServiceNow & Mercedes-Benz domains** — works on `*.service-now.com` and `*.mercedes-benz.com`

---

## Installation

### Manual / Developer install
1. Download the latest `.xpi` from [Releases](../../releases).
2. In Firefox, go to `about:addons` → click the ⚙️ gear → **Install Add-on From File…**
3. Select the `.xpi` file.

### Load unpacked (for development)
1. Clone this repo.
2. Go to `about:debugging` → **This Firefox** → **Load Temporary Add-on…**
3. Select any file inside the repo folder (e.g. `manifest.json`).

---

## Usage

### Opening the palette
| Shortcut | Action |
|---|---|
| `\` | Open palette (when not in a text field) |
| `Ctrl+Shift+Space` | Toggle palette open/closed |
| `F2` | Open palette |

### Inside the palette
| Key | Action |
|---|---|
| Type | Filter commands by name or hint |
| `↑` / `↓` | Navigate commands |
| `Enter` | Run selected command |
| `Esc` | Close palette |

### Managing commands
Click the extension icon in the toolbar to open the **popup**, where you can:
- Create, edit, and delete individual commands
- Run a command directly on the active ServiceNow tab
- Toggle light/dark theme

Click the **⚙️ Settings** icon to open the full-page settings view, which also has:
- **Import** — load commands from a `.json` file (merge or replace)
- **Export** — save all commands to a `.json` file
- **Clear All** — delete every command at once
- **Bulk select** — tick checkboxes next to commands and delete multiple at once

---

## Command JSON format

Commands are stored in `browser.storage.local` under the key `snCommands`. You can import/export them as JSON:

```json
{
  "version": 1,
  "commands": [
    {
      "name": "mycommand",
      "hint": "Short description shown in the palette",
      "script": "(function() {\n    // your ServiceNow JavaScript here\n})();"
    }
  ]
}
```

---

## File structure

```
├── manifest.json       Extension manifest (MV2)
├── content.js          Injected into ServiceNow tabs — renders the palette
├── popup.html          Toolbar popup UI
├── popup.js            Popup logic
├── settings.html       Full-page settings UI
├── settings.js         Settings logic
└── icons/
    ├── icon16.png
    ├── icon48.png
    └── icon128.png
```

---

## Permissions

| Permission | Reason |
|---|---|
| `activeTab` | Run scripts on the current tab |
| `tabs` | Find open ServiceNow tabs to run commands on |
| `storage` | Save commands and theme preference locally |
| `*://*.service-now.com/*` | Inject the palette on ServiceNow instances |

---

## Development notes

- Built with plain JavaScript (no build step, no dependencies).
- Uses **Manifest V2** for Firefox compatibility.
- The palette is injected into **all frames** (`all_frames: true`) to work inside ServiceNow's iframes.
- All palette variables are scoped inside an IIFE to avoid polluting `window`.

---

## License

MIT
