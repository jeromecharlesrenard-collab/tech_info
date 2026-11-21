# Tech Info

## Purpose
Static website of tech reference pages (machinery, materials, fabrication, 3D printing), published with GitHub Pages. It is a duplicate of the JCR Knowledge Bank site (`C:\Users\Jerome\PYTHON_PROJECTS\knowledge_bank`): same style, script, tools, apps, and conventions.
- Live site: https://jeromecharlesrenard-collab.github.io/tech_info/
- Repo: https://github.com/jeromecharlesrenard-collab/tech_info (branch `main`, remote `origin`)

`index.html` is the home page ("TECH INFO") with an alphabetical list of pages. Every other page sits in `html_pages/` (e.g. `html_pages/Materials.html`) and reaches shared files with `../` (see Conventions).

## Pages
- `Fabrication.html`: Main Fabrication Technologies (title boxes Subtractive / Formative / Additive / Joining & Assembly / Composites & Other) and Finishing (Tumbling). Sources: data/Fabrication.pdf, data/Fabrication_Technologies.pdf
- `Machinery_Equipment.html`: 25 shop machines in row cards, grouped Machining / Cutting / Forming / Joining / Grinding & Finishing, plus a laser / plasma / water jet table. Source: data/Machinery & Equipment.pdf
- `Materials.html`: title boxes Stainless Steel / Carbon & Alloy Steel / Aluminum (grades in `tl-list`), plus a Quick Comparison table. Source: data/Materials.pdf
- `Polymer_3D_Print.html`: Manufacturing Processes (Additive vs. Subtractive, 3D Printing Processes, Vacuum Casting) and Plastics (one table). Source: data/Polymer & 3D Print.pdf

## Folder Structure
```
tech_info/
├── index.html               home page (alphabetical list; sorted/grouped by assets/kb_common.js)
├── html_pages/              ALL pages except index.html; links use ../
├── kb_images/               ALL page images, named <page>_<item>.png
├── kb_config.js             page settings: arrow sizes/margins, search placeholder, local preview name
├── assets/
│   ├── kb_style.css         shared stylesheet - ALL page styling lives here
│   ├── kb_common.js         applies kb_config.js, sorts the home page list, expand, recap, search
│   ├── return_arrow.svg     top-left return icon
│   ├── top_arrow.svg        top-right "back to top" icon
│   ├── tech_info.ico        "TI" icon (shortcuts, preview and add-data windows, folder icon)
│   └── tech_info.png        "TI" icon (browser tab)
├── templates/page_template.html   starting point for new pages
├── main.py                  entry point: logging + config + local preview, or --window, or --add-data
├── tools/                   kb_core.py (paths, config, logging), new_page.py, site_check.py, preview.py, add_data.py
├── tests/                   pytest tests (logic only, no GUI)
├── docs/tech_info NOTES.md
├── data/                    private source documents (git-ignored, never published)
├── logs/app.log             rotating log (git-ignored)
├── config.json              tool settings: log_level, preview_port, preview_idle_minutes, add_data_timeout_minutes, site_url, repo_url, default_source
├── requirements.txt         pinned dev dependencies
├── pytest.ini, .editorconfig, .gitignore, .gitattributes, .nojekyll, desktop.ini (folder icon, git-ignored)
└── .vscode/                 settings, recommended extensions, tasks
```
The `kb_` file prefix is kept from the Knowledge Bank so the sites share identical code.

## Environment
- venv: `C:\Users\Jerome\PYTHON_PROJECTS\tech_info\.venv` (Python 3.14 from `C:\Users\Jerome\AppData\Local\Programs\Python\Python314\python.exe`)
- Dependencies (requirements.txt, pinned): pytest, pillow (icon generation). The website itself needs no Python.

## Commands (run from the project folder)
- Shortcuts (in the project folder and in `PYTHON_PROJECTS\1 PYTHON APPS SHORTCUP`; git-ignored, *.lnk):
  - `TECH INFO L.lnk`: open the site locally, no window (GitHub-like server; it stops by itself after `preview_idle_minutes` in config.json, default 60, without page requests; launching again while it runs just reopens the browser). Same as `.venv\Scripts\pythonw.exe main.py`.
  - `TECH INFO W.lnk`: open the live website.
  - `TECH INFO PREVIEW.lnk`: control window (OPEN SITE / CHECK SITE / EXIT), same as `main.py --window` or VS Code task "TI: Preview window".
  - `TECH INFO ADD.lnk`: TECH INFO ADD DATA window (`tools/add_data.py`, same as `main.py --add-data` or task "TI: Add data"). Same app as KNOWLEDGE BANK ADD: pick a target page (or "New Page"), a source file (.docx, .pdf, .txt, .xlsx, .jpg, .png, .gif, .bmp, .html) and/or pasted text, and/or Instructions, "Update GitHub" (checked by default), then PROCESS; Claude (`claude -p` in this folder) places the data in the page style, runs the checks, and publishes. Unchecked: local update only, then a pop-up offers to publish just the files that job changed. "Match GitHub": commits and pushes every local change so GitHub matches the local site exactly (refuses if GitHub has newer commits). Timeout: `add_data_timeout_minutes` in config.json.
  - Shortcut folder copies: `4 TI ADD.lnk` and `4 TI WEB.lnk` (like `4 KB ADD.lnk` / `4 KB WEB.lnk`).
- New page: `.venv\Scripts\python.exe -m tools.new_page --title "Name" --description "One line"`
- Check site (links, images, upper/lowercase, arrow, listing): `.venv\Scripts\python.exe -m tools.site_check`
- Tests: `.venv\Scripts\python.exe -m pytest`
- VS Code: Terminal > Run Task > "TI: Preview site" / "TI: Preview window" / "TI: Add data" / "TI: New page" / "TI: Check site" / "TI: Run tests"
- Publish: commit and push to `origin main`; GitHub Pages updates in 1-2 minutes

## Conventions
- **Every page uses the shared style**: pages live in `html_pages/` and link `../assets/kb_style.css`, `../kb_config.js`, `../assets/kb_common.js` in `<head>` and `../assets/tech_info.png` as icon; return arrow `<a class="return-arrow" href="../index.html">` right after `<body>`; images `../kb_images/...`; home page entries link `html_pages/<Page>.html`. Only index.html stays in the root (GitHub Pages home page). The site check reports any page outside `html_pages/`. Do not put `<style>` blocks in pages; add shared classes to kb_style.css.
- White background, black Arial text, bold where needed; cards, stat boxes, striped tables.
- **No round numbering badges** on cards or boxes.
- **Boxed sections** ("title boxes"): `<article class="card row-card title-box">` with a one-line `<div class="title-line"><h3>Title</h3><span class="origin">...</span></div>` and all content in `<div class="more">`, shown with the expand button (see Materials.html).
- Long lists of items: full-width row cards (`.rows` / `.row-card`, see Machinery_Equipment.html): one header line (`.row-head` with h3 + `.fact`s), max 2 text lines (`p.desc`), extra details in `<div class="more">` behind the expand icon.
- **Top arrow** and **Expand all** button are added automatically by kb_common.js.
- **Recap**: every section has one. Put `<div class="recap" hidden>` with a `<table class="recap-table">` (columns Item | Remember; key words and numbers in `<strong>`, short memory aids in `<em>`) inside a box, or right after a section `<h2>` when the section holds many boxes or a table. kb_common.js adds a round "R" button. Recaps are short: general-knowledge facts only, rounded easy numbers, strong words, about 3 to 8 rows. Search ignores recap text.
- **Search**: same code as the Knowledge Bank (kb_common.js). Placeholder and local-preview name: `searchPlaceholder` / `localPreviewName` in kb_config.js. Needs http(s) (live site or TECH INFO L), not file://. Its sessionStorage cache key includes the site path (`kb-search-v2:/tech_info/`), because all jeromecharlesrenard-collab.github.io sites share one origin.
- Home page entries: name on top, description + "Added" date on one line.
- Create new pages with `tools/new_page.py` (creates them in `html_pages/` and adds the home page entry automatically).
- **All page images go in `kb_images/`** (published), lowercase page prefix. Source files go in `data/` (never published; pages must not link to it). File and folder names are case-sensitive on GitHub.
- Content: digest the source notes into a quick reference; keep every source item, group items logically, correct clear errors (and report them), and add only well-established facts. Footer: "Source: <file>; details from standard references. Created <date>."
- American spelling. Python: PEP 8, type hints, docstrings, pathlib, paths resolved from the project folder; tools must work under pythonw.exe (no print dependency in GUI code).
- After every change, run the site check and tests, then commit and push to `origin main` without asking. Ask only when there is an open question or a check/test fails. Do not start the preview window without the user's authorization.
