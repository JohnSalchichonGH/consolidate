# Consolidate

**Merge an entire project's source files into a single, LLM-ready document.**

One HTML file. No install. No dependencies. No server. Open it in a browser, pick a folder, and download a consolidated snapshot of your codebase — formatted for pasting straight into an LLM's context window.

---

## Why

LLMs work best when they can see your whole project at once — the config, the models, the routes, the utilities — not one file at a time. But preparing that context manually means copy-pasting dozens of files, losing the directory structure, and hoping you didn't miss something important.

Consolidate does this in seconds: scan a folder, pick the files you want, download a single document with a clear structure the LLM can parse immediately.

---

## Features

### Zero-friction setup
- **Single HTML file** — no Node, no Python, no package manager. Double-click to open.
- **100% local** — your code never leaves your machine. No uploads, no server, no network requests.
- **Works offline** — once opened, everything runs in-browser.

### Smart file scanning
- **Folder upload** via browser-native directory picker, or drag-and-drop a folder onto the page.
- **Automatic binary detection** — reads the first 8 KB of each file and checks for null bytes. Images, compiled binaries, fonts, databases, and other non-text files are silently skipped.
- **Smart defaults** — common non-source folders (`node_modules`, `.git`, `__pycache__`, `dist`, `build`, `venv`, `.next`, and others) are automatically deselected on scan. They still appear in the tree so you can opt back in if needed.
- **Oversized file protection** — files larger than 15 MB are excluded automatically.
- **Empty file filtering** — zero-byte files are skipped.

### Interactive file browser
- **Collapsible directory tree** with the full folder hierarchy of your project.
- **Cascading checkboxes** — toggle a folder to select or deselect all files inside it. Partial selection states shown for folders with mixed checked/unchecked children.
- **File search** — real-time text filter that matches against the full file path.
- **Extension filter chips** — auto-generated from the files found in your project, sorted by frequency. Click to toggle visibility for entire file types (e.g. show only `.py` and `.ts` files).
- **Bulk selection** — All, None, and Invert buttons for fast toggling.
- **File preview** — click any file in the tree to view its content in the right panel.

### Output customization
- **Two output formats:**
  - **XML** (default) — optimized for LLM consumption. Clean `<file path="...">` tags with unambiguous boundaries.
  - **Markdown** — fenced code blocks with syntax highlighting tags. Better for human reading or pasting into documents.
- **Directory tree** — optional structural overview at the top of the output, using token-efficient indented format.
- **File index** — optional file listing with language and size metadata.
- **Token estimation** — live count displayed in the header with color-coded badges:
  - 🟢 Green: under 30K tokens
  - 🟠 Orange: 30K–80K tokens
  - 🔴 Red: over 80K tokens
- **Smart output filename** — the downloaded file is named based on context:
  - Single file selected → `config_consolidated.xml`
  - All selected files share one subdirectory → `backend_consolidated.xml`
  - Mixed selection → uses the uploaded folder name, e.g. `my-project_consolidated.xml`

### Syntax awareness
- **60+ file extensions** recognized with correct language tags (Python, TypeScript, Rust, Go, SQL, YAML, HCL, and many more).
- **Special filenames** handled — `Dockerfile`, `Makefile`, `.gitignore`, `.env`, `Jenkinsfile`, `Vagrantfile`, and others get correct language tagging regardless of extension.

---

## Getting started

1. **Download** `consolidate.html` (it's the only file you need).
2. **Open** it in any modern browser (Chrome, Firefox, Edge, Safari).
3. **Click** the drop zone or drag a project folder onto it.
4. **Select** the files you want using the tree, search, and filters.
5. **Choose** your output format and options in the header.
6. **Download** — click the download button to get your consolidated file.

That's it.

---

## Usage guide

### Uploading a project

Click the central drop zone or drag a folder from your file manager onto the page. The browser will read every file recursively and filter out binary/oversized content automatically.

> **Note:** Browser folder upload uses the `webkitdirectory` API. You'll see a confirmation dialog asking permission to upload the folder — this is your browser's standard security prompt. The files are read locally and never sent anywhere.

After scanning, the sidebar populates with your project's file tree. Source files are selected by default, while common non-source folders (`node_modules`, `.git`, `__pycache__`, `dist`, `build`, `venv`, etc.) are automatically deselected to keep the output focused on actual project code. These folders still appear in the tree — just uncheck or re-check them as needed.

### Filtering files

There are three ways to narrow down which files end up in the output:

**Search bar** — type any part of a file path to filter the tree in real time. For example, typing `store` would show only files with "store" in their path.

**Extension chips** — the colored chips below the search bar represent every file extension found in your project, sorted by count. Click one to toggle it — only files with active extensions will appear. Click multiple chips to combine filters (e.g. `.py` + `.yaml` to see only Python and config files).

**Checkboxes** — manually check or uncheck individual files or entire folders. Use the All / None / Invert buttons for bulk operations. Folder checkboxes cascade: unchecking a folder unchecks every file inside it.

All three filters work together. The download button and token estimate update in real time as you adjust the selection.

### Choosing an output format

Use the **Format** dropdown in the header toolbar:

#### XML (recommended for LLMs)

```xml
<project>
<structure>
backend/
  api/
    chat.py
    upload.py
  config.py
  main.py
</structure>

<index>
  <entry path="backend/config.py" language="python" size="685 B" />
  <entry path="backend/main.py" language="python" size="4.2 KB" />
</index>

<file path="backend/config.py" language="python">
from pathlib import Path
...
</file>

<file path="backend/main.py" language="python">
from fastapi import FastAPI
...
</file>
</project>
```

**Why XML is the default:** LLMs are trained extensively on XML. The `<file>` tags create unambiguous start/end boundaries — there's no risk of a triple-backtick inside source code breaking the framing (a real problem with Markdown). The format is also more token-efficient: one `path=` attribute replaces a heading + separator + fence opener.

#### Markdown

```markdown
# Project Structure

​```
backend/
  api/
    chat.py
    upload.py
  config.py
  main.py
​```

---

# File Index

_4 files · 12.3 KB_

- `backend/config.py`
- `backend/main.py`

---

## `backend/config.py`

​```python
from pathlib import Path
...
​```
```

Better for human reading, sharing in documents, or pasting into Markdown-native tools.

### Output options

**Tree** (toggle) — includes or excludes the directory structure overview at the top of the output. Uses simple 2-space indentation, which is ~40% more token-efficient than traditional box-drawing tree characters (`├──`, `└──`) while being equally understandable by LLMs.

**Index** (toggle) — includes or excludes the file listing. In XML mode, this renders as `<entry>` tags with path, language, and size attributes. In Markdown mode, it's a simple bullet list.

**Token estimate** — the badge in the header shows a rough token count based on ~4 characters per token (a standard approximation for code). Use this to check whether your selection fits within your LLM's context window before downloading.

### Previewing files

Click any file in the sidebar tree to view its contents in the right panel. This doesn't affect selection — it's just for inspecting content before deciding whether to include or exclude a file.

---

## How it works

1. **Reading** — The browser's `webkitdirectory` API (or the drag-and-drop `FileSystemEntry` API) provides access to every file in the selected folder. Each file is read as an `ArrayBuffer`.

2. **Binary detection** — The first 8,192 bytes of each file are scanned for null bytes (`0x00`). If any are found, the file is classified as binary and skipped. This is the same heuristic used by Git and most text editors.

3. **Tree building** — File paths are split on `/` and assembled into a nested object structure that mirrors the folder hierarchy. Common non-source directories (`node_modules`, `.git`, `dist`, etc.) are automatically deselected.

4. **Rendering** — The tree is rendered as interactive HTML with checkboxes, expand/collapse arrows, file-type icons, and size labels. Extension chips are generated by counting occurrences of each extension across all files.

5. **Generation** — When you click Download, the selected files are concatenated into a single string in the chosen format (XML or Markdown), wrapped in a `Blob`, and triggered as a browser download. Nothing is sent over the network.

---

## Browser compatibility

| Browser | Support |
|---------|---------|
| Chrome / Edge | ✅ Full support |
| Firefox | ✅ Full support |
| Safari | ✅ Full support (folder upload via click; drag-and-drop may require allowing the permission) |
| Mobile browsers | ⚠️ Limited (most mobile browsers don't support folder selection) |

---

## FAQ

**Does this upload my code anywhere?**
No. Everything runs locally in your browser. No network requests are made. You can verify this by opening DevTools → Network tab while using the tool.

**Can I use this with private/proprietary code?**
Yes. Your code never leaves your machine. The HTML file doesn't even load external resources beyond two Google Fonts (which can be removed for fully air-gapped use).

**Why not just use `find` and `cat`?**
You could, but you'd lose the interactive file selection, binary detection, syntax-aware language tags, directory tree, token estimation, and output format options. Consolidate gives you control over exactly what goes into the context.

**What's the maximum project size it can handle?**
It's limited by browser memory. Projects with a few thousand files and tens of megabytes of source code work well. For monorepos with 50K+ files, consider pre-filtering to a subdirectory.

**Can I customize the ignored file types?**
The tool doesn't use an allowlist — it includes everything that isn't binary. Common junk folders (`node_modules`, `.git`, `dist`, `build`, `venv`, etc.) are deselected by default but still visible in the tree. To exclude specific extensions, use the extension filter chips in the sidebar, or manually uncheck individual files and folders.

**Why are some folders unchecked after scanning?**
Folders like `node_modules`, `.git`, `__pycache__`, `dist`, and `build` are automatically deselected because they rarely contain code you'd want an LLM to see. They still appear in the tree — click their checkbox to include them if needed.

**Which format should I use for ChatGPT / Claude / Gemini?**
XML works well with all of them. Every major LLM handles XML structure reliably. Use Markdown only if you're pasting into a tool that renders Markdown, or if you prefer reading the file yourself.

---

## License

MIT — use it however you want.
