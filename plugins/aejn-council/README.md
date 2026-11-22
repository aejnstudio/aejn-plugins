# aejn Council

A skill for getting a straight second opinion. Four independent advisors (Believer, Skeptic, Investor and Contrarian) examine your decision without seeing each other's answers. A separate Judge then reads their reports and gives one verdict, the first steps to take and a signal for when to stop.

## Install

```
/plugin install aejn-council --marketplace aejnstudio/aejn-plugins
```

## Example prompts

1. "/aejn-council:council Should I charge monthly or a one-off price for my app?"
2. "Ask the council whether I should take this £500 client job with a two week deadline."
3. "Give me a brutal council verdict on opening a second online store."

## What it runs, sends and fetches

The plugin is Markdown instructions only. It has no hooks, MCP servers or executables. Each run starts five Claude subagents, which use more of your usage than a normal answer. The advisors receive what you said about the decision and may use Claude's web search to check public facts. The skill tells them to keep private details out of searches.

## Limits and troubleshooting

- The verdict is an AI opinion and is not legal, financial or medical advice.
- It needs a Claude surface that supports subagents.
- If the skill doesn't load, run `/plugin` to check it is enabled, then restart Claude Code.

## Support and security

Open an issue at https://github.com/aejnstudio/aejn-plugins/issues. For security problems use the contact form at https://aejnstudio.com and do not post secrets in an issue.
