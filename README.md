# STE100 Concise for Claude Code

A custom output style for clear, concise technical English that preserves uncertainty and engineering precision.

This adaptation of [toppa's ASD-STE100 style](https://gist.github.com/toppa/bf7ff49d6fc44fd4fc3337248f8f2a7e) combines simpler prose with the result-first approach of Claude Code's Concise style. It is inspired by Simplified Technical English, not a strict implementation of the ASD-STE100 standard.

The installable file is [`output-styles/ste100-concise.md`](output-styles/ste100-concise.md). No build step or package manager is required.

## What changes

| Area | This style |
| --- | --- |
| Verb tenses | Prefers simple tenses; permits other forms when they improve accuracy or clarity. |
| Uncertainty | Separates facts, hypotheses, recommendations, and untested assumptions. |
| Technical language | Preserves meaningful distinctions, trade-offs, and exact engineering terms. |
| Reader | Uses common English for a technically capable non-native speaker. Introduces difficult ideas intuitively; gives full ELI5 explanations on request. |
| Response length | Leads with the answer; removes preambles, repetition, and routine narration. |
| Sentence length | Uses length targets instead of hard caps. |
| Engineering work | Keeps coding instructions and requires honest reporting of verification. |

The style includes these two rules verbatim:

> Prefer simple verb tenses and direct sentence structures. Use other grammatical forms when they improve accuracy or make technical explanations clearer. Never sacrifice clarity to satisfy a grammar rule.

> Preserve meaningful uncertainty, technical distinctions, and trade-offs. Clearly distinguish verified facts, hypotheses, recommendations, and untested assumptions. Never present an expected outcome as a verified result. Do not replace precise engineering terms with simpler words when that changes their meaning.

Additional refinements remove the original bans on `-ing` forms and compound uncertainty phrases. Those bans could undermine the rules above. Project vocabulary remains useful, but `CONTEXT.md` is optional and is treated as a glossary only when the project uses it that way.

## Install

You need Claude Code with custom output-style support. Check your version with `claude --version`.

### For your user account

In a terminal on macOS, Linux, WSL, or Git Bash:

```bash
git clone https://github.com/forhas/claude-ste100.git
cd claude-ste100
mkdir -p ~/.claude/output-styles
cp -i output-styles/ste100-concise.md ~/.claude/output-styles/
```

If you already cloned this repository, use that checkout and skip `git clone`. The copy command asks before replacing an existing file.

On Windows with PowerShell, run this from the cloned repository:

```powershell
New-Item -ItemType Directory -Force -Path "$HOME\.claude\output-styles" | Out-Null
Copy-Item -Path '.\output-styles\ste100-concise.md' -Destination "$HOME\.claude\output-styles\ste100-concise.md" -Confirm
```

Install in the environment where you run Claude Code: WSL and native Windows have separate home directories. If you set `CLAUDE_CONFIG_DIR`, use that directory instead of `~/.claude`. See [Claude Code settings locations](https://code.claude.com/docs/en/settings).

Check your Claude launch command or shell alias too: it may set `CLAUDE_CONFIG_DIR` only when Claude starts. For example, if it uses `~/.claude-personal`, install the file in `~/.claude-personal/output-styles/` and edit `~/.claude-personal/settings.json` wherever this guide refers to user settings.

### For one project only

Copy the style into the target project's `.claude/output-styles/` directory. For example, from this repository in a Bash terminal, replace the example path:

```bash
project_dir="/absolute/path/to/your-project"
mkdir -p "$project_dir/.claude/output-styles"
cp -i output-styles/ste100-concise.md "$project_dir/.claude/output-styles/"
```

Commit that file if teammates should have access to the style.

## Activate

1. Restart Claude Code after installing or editing the style file.
2. Start Claude Code in the project where you want to use it.
3. Run `/config` inside Claude Code and select **Output style → STE100 Concise**.

On versions with the `/output-style` command, you can also run `/output-style` and select the style. This selection is saved for the current project.

To make it your default across projects, install it at the user level and merge this property into `~/.claude/settings.json`:

```json
{
  "outputStyle": "STE100 Concise"
}
```

Preserve your other settings. Add `"outputStyle": "STE100 Concise"` inside the existing top-level JSON object; do not paste a second pair of braces inside it. Separate properties with commas. Use the exact capitalization shown above. Project settings can override the user setting. For a shared project default, use the same property in `.claude/settings.json` and commit the project style file too. See [output-style selection](https://code.claude.com/docs/en/output-styles#change-your-output-style).

You select this custom style on its own; you do not also need to enable the built-in Concise style.

## What to expect

These are illustrative responses, not results from a model evaluation.

**An unverified fix:**

> Added a null check before reading the cached value. This should prevent the reported exception, but I have not run the tests.

**A hypothesis:**

> Connection pool exhaustion may have caused the timeout. Check pool utilization during the failure window to test that hypothesis.

**A trade-off:**

> A longer cache TTL reduces database reads but can serve stale data for longer. Use it only if that delay is acceptable.

Short questions should usually get short answers. Requests for detailed explanations, tutorials, or complete documentation should still get the detail they need.

The style separates English proficiency from technical knowledge. It avoids idioms and unnecessary formality without assuming you are a beginner. Ask "Explain this like I'm five" for a concrete example or analogy; important technical distinctions and uncertainty still apply.

The frontmatter sets `keep-coding-instructions: true` to retain Claude Code's coding guidance on its full system prompt. This field has no effect on the shorter prompt, which omits that guidance. See [how output styles work](https://code.claude.com/docs/en/output-styles#how-output-styles-work). The style also explicitly requires thorough work and accurate verification reports.

## Check and customize

Confirm **STE100 Concise** is selected in `/config`. Then try:

```text
Explain whether increasing a cache TTL would improve performance.
Keep it brief, state your assumptions, and include the main trade-off.
```

Look for a direct answer that preserves uncertainty and the freshness trade-off. Try a request for a detailed explanation too; the style should expand when asked. Output styles guide model behavior rather than enforce guarantees.

Edit the installed Markdown file to tune the style, then restart Claude Code. A useful first adjustment is the one-to-three-sentence target for simple questions. Keep the accuracy rules even if you prefer shorter answers.

## Update or remove

Run `git pull --ff-only` in your clone, repeat the appropriate copy command, and restart Claude Code. Installing a copy does not enable automatic updates.

To stop using the style, select **Default** in the output-style menu. Remove any `outputStyle` setting you added for this style and delete its installed Markdown file if you no longer need it.

## Troubleshooting

| Problem | Check |
| --- | --- |
| Style is missing | Restart Claude Code. Confirm the file ends in `.md` and is in the user or target project's `.claude/output-styles` directory. |
| Only built-in styles appear | Check whether your launch command or alias sets `CLAUDE_CONFIG_DIR`. Install the style in that directory's `output-styles/` folder, then restart. Remote Control's web/mobile selector supports only built-in styles; use the local terminal for custom styles. |
| Settings fail to load | Validate the JSON. Add the `outputStyle` property to the existing object, without an extra nested object or trailing comma. |
| Edits have no effect | Edit or replace the installed copy, then restart. Editing the downloaded repository alone does not update that copy. |
| Another style appears | Check project settings for an `outputStyle` override. Confirm the setting uses `STE100 Concise` exactly. |
| `/output-style` is unavailable | Use the terminal's `/config` menu. The current command interface requires Claude Code v2.1.269 or later. |
| Coding guidance is missing | Check that the frontmatter still contains `keep-coding-instructions: true`. Consult the full/short system-prompt distinction linked above. |

## Sources and attribution

- [toppa's ASD-STE100 output style](https://gist.github.com/toppa/bf7ff49d6fc44fd4fc3337248f8f2a7e): the starting point for this adaptation.
- [Claude Code output-style documentation](https://code.claude.com/docs/en/output-styles): installation, metadata, selection, and the documented Concise behavior. Checked on October 21, 2026.
- [huifer's output-style reference](https://github.com/huifer/claude-code-interview/blob/main/claude-code-output-styles.md): the supplied background reference. Its built-in-style list predates the current official documentation.

The Concise influence follows its documented behavior; this repository does not reproduce Claude Code's internal prompt. This is an independent adaptation, not an official Anthropic or ASD product.
