# aejn FiveM Dev

A skill that gives Claude practical lessons from building and fixing FiveM roleplay servers. It covers the asset and map side (MLOs, ymaps, collision, textures, clothing and vehicles) as well as QBCore scripts, NUI, packaging and crash debugging.

## Install

```
/plugin install aejn-fivem-dev --marketplace aejnstudio/aejn-plugins
```

## Example prompts

1. "My players crash when they walk up to my new MLO. Help me find the cause."
2. "My server lags with five players and I have 80 add-on cars. What should I check?"
3. "Review this QBCore shop script so players can't give themselves free items."

## What it runs, sends and fetches

The plugin is Markdown instructions only. It has no hooks, MCP servers, executables or network calls. Its guidance may lead Claude to suggest or run tools you already have (Python, Blender, CodeWalker, a browser), subject to your normal permission prompts.

## Limits and troubleshooting

- The advice is field experience and is not official documentation. Test on a test server and keep backups.
- The CodeWalker and Blender recipes assume Windows and those tools already installed.
- The Lua checker needs `pip install lupa` and does not understand every CfxLua extension.
- If the skill doesn't load, run `/plugin` to check it is enabled, then restart Claude Code.

This project is not affiliated with Cfx.re, Rockstar Games, Take-Two or Anthropic.

## Support and security

Open an issue at https://github.com/aejnstudio/aejn-plugins/issues. For security problems use the contact form at https://aejnstudio.com and do not post secrets in an issue.
