# plugins

A plugin marketplace for Claude, plus a ChatGPT build of the same plugin.

## Plugins

| Plugin | Description |
| --- | --- |
| `personify` | Set up AI-assisted support (aka a "Persona") for a GitHub project by following the AI-assistant instructions in its README. Usage: `/personify OWNER/REPO` (Claude) or `@personify OWNER/REPO` (ChatGPT). Shows the planned actions and asks you to click Continue or Cancel before changing anything. |

## Add this marketplace to Claude

Claude Code:

    /plugin marketplace add will-cern/plugins
    /plugin install personify@will-cern-plugins

In Cowork or claude.ai, add the GitHub repository `will-cern/plugins` as a plugin marketplace in the plugin settings, then install `personify` from it. The GitHub connector must be installed for `/personify` to work.

## Layout

- `.claude-plugin/marketplace.json`: marketplace catalog read by Claude
- `plugins/personify/`: the Claude plugin (`.claude-plugin/plugin.json`, `skills/personify/SKILL.md`)
- `chatgpt/personify/`: the ChatGPT build (`.codex-plugin/plugin.json`, `skills/personify/SKILL.md`). It is not listed in the Claude marketplace.

## Create a zip of a single plugin to upload to ChatGPT

ChatGPT plugins live in the `chatgpt/` folder, one folder per plugin. To upload one, zip that plugin's folder contents into a single archive.

**The one rule:** `.codex-plugin` and `skills` must sit at the top level of the zip. Do not zip the plugin folder itself (`personify/`) or the whole repo, or ChatGPT will reject the upload with an error saying the archive must contain `.codex-plugin/plugin.json` or `skills/*/SKILL.md`.

1. Get the repo onto your computer, either by cloning it or by using **Code, Download ZIP** on GitHub and unzipping it:

       git clone https://github.com/will-cern/plugins.git

2. Open a terminal in the folder of the plugin you want. For `personify`:

       cd plugins/chatgpt/personify

3. Create the zip from inside that folder.

   macOS or Linux:

       zip -r ../../personify-chatgpt-plugin.zip .codex-plugin skills

   Windows (PowerShell or Command Prompt, Windows 10 or later):

       tar -a -c -f ..\..\personify-chatgpt-plugin.zip .codex-plugin skills

   The zip is written two levels up, outside the plugin folder, so it is not included in itself. Replace `personify` and the zip name to package a different plugin.

4. Check the contents before uploading. The first entries should start with `.codex-plugin/` and `skills/`, with no `personify/` prefix:

       unzip -l ../../personify-chatgpt-plugin.zip

   On Windows, `tar -tf ..\..\personify-chatgpt-plugin.zip` lists the same thing.

5. Upload `personify-chatgpt-plugin.zip` in ChatGPT's plugin upload.

Tips:
- `.codex-plugin` starts with a dot, so Finder and File Explorer may hide it. Right-clicking the folder and choosing Compress often leaves it out or adds a wrapping folder, so use the command line.
- Upload the plugin zip, not a zip that only contains a `SKILL.md`. ChatGPT's plugin upload also requires `.codex-plugin/plugin.json` to include `author` and an `interface` object (`displayName`, `shortDescription`, `longDescription`, `developerName`, `category`, `capabilities`). The `personify` manifest already has these, and a new plugin needs them too.

## Safety

The README of the target repository is treated as untrusted third-party content. The skill does not touch credentials, run destructive commands, or publish anything on your behalf, and it makes no changes until you click Continue.
