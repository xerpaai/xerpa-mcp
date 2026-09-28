# Xerpa MCP

Use [Xerpa](https://xerpa.ai), the real-time sales co-pilot, from the AI assistant you already work in: Claude, Claude Code, Cowork, ChatGPT or any MCP client that supports remote servers.

Xerpa's desktop app coaches reps on live calls: rebuttals to objections, pain points to dig into, answers from your own knowledge base, and whispers that suggest what to say next, all on the rep's screen. Xerpa never plays audio and never speaks to the prospect.

The Xerpa connector brings everything around the call into your assistant:

- **Set up Xerpa**: scan your website, load knowledge base documents, build the Call Map (the sales guide reps follow on calls), write objections and rebuttals, draft battle cards.
- **Prep calls**: your upcoming meetings and the prep card for each, with who you are meeting, deal history, goals and the objections to expect.
- **Look back**: past calls, debriefs, transcript excerpts and Xerpa Conversations.
- **Read the numbers**: team dashboards, SDR and AE reporting, objection trends and the leaderboard.

Live coaching, whispers, call detection and recording stay in the desktop app. When you ask for them, the connector hands you off to it.

This repository holds the public pieces: a Claude Skill and a Claude Code plugin that bundle the connector. The server itself is hosted by Xerpa.

## Connect

**Server URL:** `https://api.xerpa.ai/mcp`

You sign in with your own Xerpa login through the browser (OAuth). There is no API key to create or paste.

### Claude (web, desktop and Cowork)

1. Open **Customize**, then **Connectors**.
2. Choose **Add custom connector** and enter `https://api.xerpa.ai/mcp`.
3. Sign in with your Xerpa account when asked.

Then ask Claude "what can Xerpa do for me?" or "help me set up Xerpa".

### Claude Code

Add the connector on its own:

```bash
claude mcp add --transport http xerpa https://api.xerpa.ai/mcp
```

Then run `/mcp` in a session and sign in.

Or install the plugin, which adds the connector and the Xerpa skill together:

```
/plugin marketplace add xerpaai/xerpa-mcp
/plugin install xerpa@xerpa
```

Cowork can install the same plugin: add a marketplace from a repository and paste `xerpaai/xerpa-mcp`.

### The Xerpa skill in Claude

The skill teaches Claude the setup order, who can do what, and when to hand off to the desktop app. It triggers on requests like "set up Xerpa" and "prep me for my call".

- Claude Code and Cowork: it comes with the plugin above.
- Claude (web and desktop): download `xerpa-skill.zip` from the [latest release](https://github.com/xerpaai/xerpa-mcp/releases/latest), then open **Customize**, then **Skills**, and upload it.

The skill needs the connector: add both.

### ChatGPT and other clients

Any MCP client that supports remote Streamable HTTP servers with OAuth can connect with the same URL. In ChatGPT, turn on developer mode in settings, create a connector with the URL above and choose OAuth for authentication.

## Prompts

The connector ships prompts your client can offer as shortcuts. You only see the ones your role can use.

| Prompt | What it does |
| --- | --- |
| `set_up_xerpa` | Walks through setup end to end, in order, one step at a time. |
| `prep_my_next_call` | A one-screen brief for your next meeting, or the one you name. |
| `weekly_team_readout` | Headline numbers, wins, top objections and who to coach this week. |
| `draft_objection_responses` | Drafts or sharpens rebuttals, grounded in your knowledge base. |

## Who sees what

The connector uses your Xerpa role. You see only the tools your role allows, and only your organization's data.

- **No Xerpa account yet**: the connector explains how to sign up, or to ask your company's Xerpa admin for an invite.
- **Solo owners** set up their own Xerpa through the connector.
- **Org admins** (owner or admin) set up the whole organization.
- **Reps and AEs** see their own calls, prep and numbers.
- **Managers** also see team reporting.

Anything that changes your Xerpa setup is confirmed with you before it runs, and deletes need an explicit confirmation.

## Privacy

The connector never returns call audio, recording links or payment details, and it never exposes another organization's data. See the [Xerpa privacy policy](https://xerpa.ai/privacy-policy/).

## Repository layout

```
.claude-plugin/marketplace.json        Claude Code marketplace listing this plugin
plugins/xerpa/.claude-plugin/plugin.json  plugin manifest
plugins/xerpa/.mcp.json                 the connector (remote HTTP server)
plugins/xerpa/skills/xerpa/SKILL.md     the Xerpa skill
```

## Support

Questions or problems: [info@xerpa.ai](mailto:info@xerpa.ai).

## License

[MIT](LICENSE)
