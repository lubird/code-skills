---
name: obsidian-load-context
description: Load context from recent coding session notes in an Obsidian vault. Use at the start of a session to catch up on recent work, open items, and next steps. Requires Obsidian to be open and the obsidian:obsidian-cli skill.
allowed-tools: Read Bash(git rev-parse:*) Bash(obsidian:*)
---

Load the last N session notes from an Obsidian vault and synthesize a context briefing for the current session.

Vault and path are configurable per project with `.agents/code-skills.json` at the git root:

```json
{
  "obsidian": {
    "vault": "Second Brain",
    "notesPath": "Projects/MyProject"
  }
}
```

Configuration precedence:
1. `.agents/code-skills.json` fields: `obsidian.vault`, `obsidian.notesPath`
2. Environment variables: `OBSIDIAN_VAULT`, `OBSIDIAN_NOTES_PATH`
3. Defaults: vault `Second Brain`, notes path `Coding Sessions`

## Obsidian CLI access

In sandboxed agents, `obsidian` may report `The CLI is unable to find Obsidian` even when Obsidian is running because the sandbox cannot access the Obsidian CLI Unix socket. If that happens, retry the same `obsidian` command outside the sandbox/escalated before concluding Obsidian is closed. Only fall back to direct filesystem reads if the escalated CLI command also fails or the user explicitly approves the fallback.

The number of sessions to load defaults to 3 but can be overridden at call time.

## Steps

1. **Resolve Obsidian configuration**:
   - Get the git root with `git rev-parse --show-toplevel`.
   - If `<git-root>/.agents/code-skills.json` exists, read it and use `obsidian.vault` and `obsidian.notesPath` from that file.
   - Otherwise use `OBSIDIAN_VAULT` and `OBSIDIAN_NOTES_PATH` from the environment.
   - If neither is set, use vault `Second Brain` and notes path `Coding Sessions`.

2. **Find recent notes** using the `obsidian:obsidian-cli` skill. Search for notes in the configured folder, sorted by recency, limited to N:
   ```bash
   obsidian vault="<resolved vault>" search query="path:\"<resolved notesPath>\"" limit=<N>
   ```

3. **Read each note**:
   ```bash
   obsidian vault="<resolved vault>" read path="<note path>"
   ```

4. **Synthesize a briefing** from the notes and present it to the user. Structure it as:

   ### Recent Work
   <Narrative summary across all loaded sessions — what was built, what changed, key decisions made>

   ### Open Items
   <Consolidated list of open items and deferred work from all sessions, most recent first>

   ### Next Steps
   <Consolidated next steps from all sessions, most recent first. Flag any that appear across multiple sessions.>

5. **Report back**: state how many notes were loaded and their date range.
