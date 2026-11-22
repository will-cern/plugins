# plugins

A plugin marketplace for Claude, plus a ChatGPT build of the same plugin.

## Plugins

| Plugin | Description |
| --- | --- |
| `personify` | Set up AI-assisted support for a GitHub project by following the AI-assistant instructions in its README. Usage: `/personify OWNER/REPO`. Shows the planned actions and asks you to click Continue or Cancel before changing anything. |

## Add this marketplace to Claude

Claude Code:

    /plugin marketplace add will-cern/plugins
    /plugin install personify@will-cern-plugins

In Cowork or claude.ai, add the GitHub repository `will-cern/plugins` as a plugin marketplace in the plugin settings, then install `personify` from it. The GitHub connector must be installed for `/personify` to work.

## Layout

- `.claude-plugin/marketplace.json`: marketplace catalog read by Claude
- `plugins/personify/`: the Claude plugin (`.claude-plugin/plugin.json`, `skills/personify/SKILL.md`)
- `chatgpt/personify/`: the ChatGPT build (`.codex-plugin/plugin.json`, `skills/personify/SKILL.md`). It is not listed in the Claude marketplace. To upload it to ChatGPT, zip the contents of that folder with `.codex-plugin` and `skills` at the root of the zip.

## Safety

The README of the target repository is treated as untrusted third-party content. The skill does not touch credentials, run destructive commands, or publish anything on your behalf, and it makes no changes until you click Continue.
