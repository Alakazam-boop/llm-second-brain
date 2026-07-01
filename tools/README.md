# tools/ — Optional helper scripts

Some command files mention helper scripts such as `researcher.py` and `sbm_catalog.py`.
**These are an optional performance optimization and are NOT bundled** with this template.

You do **not** need them to use the system. Wherever a command says *"run the Researcher"* or
*"refresh the catalog"*, an agent with file access (e.g. Claude Code) achieves the same result by
**searching the vault directly** — grepping note text, following `[[wikilinks]]`, and reading
`index.md`. The scripts simply automate that retrieval for large vaults.

If you want to build your own:
- `researcher.py <vault> "<query>" --json` → returns the most relevant `core`/`related` notes for a query.
- `sbm_catalog.py scan|stats|record-read <vault>` → maintains a small SQLite catalog of the vault.

Until then, the commands fall back to the agent doing the search itself — which works well into the
low thousands of notes.
