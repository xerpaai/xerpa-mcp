---
name: xerpa
description: "Use Xerpa, the real-time sales co-pilot, from Claude. Use whenever the user mentions Xerpa, wants to set up their sales knowledge base, Talk Track, objections or battle cards, prep for a sales call, review a past call, or pull sales team numbers. If the Xerpa connector is not attached, this skill says how to add it."
---

# Xerpa

Xerpa is a real-time sales co-pilot for SDRs and AEs. During a live call the Xerpa desktop app puts guidance on the rep's screen: rebuttals to objections, pain points to dig into, answers from the company's own knowledge base, and whispers that suggest what to say next. Everything on a call is on screen. Xerpa never plays audio and never speaks to the prospect.

The Xerpa connector (a remote MCP server) lets this assistant set Xerpa up, prep calls, look back at past calls and read team reporting. This skill tells you how to use it well.

## Vocabulary

The "Talk Track" is the stage-by-stage guide reps follow on calls (the tools call it the sales guide, e.g. `get_sales_guide`); always call it the Talk Track to the user. Whispers are on-screen prompts. "Xerpa Conversations" is where a rep asks Xerpa questions grounded in their calls and CRM.

## Before anything else: is the connector attached?

Look for the Xerpa tools, such as `get_started`, `get_setup_progress` and `get_desktop_status`. Some clients show them with a prefix, for example `mcp__xerpa__get_setup_progress`.

If they are missing, the user has not connected Xerpa yet. Tell them how, then stop:

- Claude (web, desktop, Cowork): Customize, then Connectors, then Add custom connector, with the URL `https://api.xerpa.ai/mcp`.
- Claude Code: `claude mcp add --transport http xerpa https://api.xerpa.ai/mcp`, then run `/mcp` and sign in. Or install the `xerpa` plugin, which adds the connector and this skill together.
- Any other MCP client that supports remote servers with OAuth: the same URL.

Sign-in is the user's own Xerpa login in the browser. There is no API key.

The connector also carries its own instructions and prompts. When they and this skill disagree, follow the connector: it knows the user's role.

## Who can do what

Tools the user's role cannot use are not listed, so work with what you see.

- No Xerpa account yet: call `get_started` for the signup links. SDR seat for people who prospect and book meetings, AE seat for discovery, demos and closing. If their company already uses Xerpa, they should ask their Xerpa org admin for an invite.
- Solo owner (a personal account): sets up their own Xerpa here.
- Org admin (owner or admin role on a team account): sets up the whole org.
- Reps, AEs and managers: connect with their own login and see their own calls, prep and numbers. Managers also see team reporting.

When a tool says the user lacks a permission, tell them which role can do it and to ask their Xerpa org admin. Do not retry.

## "Set up Xerpa"

Call `get_setup_progress` first and say in two sentences where things stand. Then go in this order, one step at a time, skipping what is done:

1. Company. On a personal account, scan the website with `scan_company_website` and correct it with `update_company_profile`. Team accounts do this in the Knowledge Center of the Xerpa web app.
2. Knowledge base. Ask for product docs, pricing, FAQs and case studies, and add each with `add_knowledge_document`.
3. Talk Track, the stage-by-stage guide reps follow on calls. Read it with `get_sales_guide`, propose stages and what to cover in each, save with `update_sales_guide`.
4. Objections. Read the library with `list_objections`, add or sharpen rebuttals with `add_objection` and `update_objection`.
5. Battle cards for the main competitors. On a personal account use `draft_battle_card`; team accounts use the web app.
6. Invite the reps from the Xerpa web app. Each rep then connects the connector with their own login.
7. Desktop app on every rep's computer. `get_desktop_status` gives the download link.

If the connector offers the `set_up_xerpa` prompt, it walks the same path tailored to the user's role.

## "Prep me for my call"

1. `get_upcoming_calls`, and find the meeting the user means (the next one if they did not say). Ask if more than one could match.
2. `get_call_prep` with that meeting's `eventId`.
3. A one-screen brief: who they are meeting and their role, the company and deal stage, earlier calls, goals and discovery gaps to cover, the matched case study if any, and the three objections most likely to come up with Xerpa's rebuttal for each (`list_objections`).
4. Remind them that live coaching during the call happens in the Xerpa desktop app. Call `get_desktop_status`, and if the app is installed offer the `open_in_desktop` link for the prep.

## Live calls go to the desktop app

This connector gives you everything around the call: setup, prep, review and reporting. It cannot join or coach a live call. Live coaching, call detection and call recording only happen in the Xerpa desktop app. When the user asks for anything that happens during a call, call `get_desktop_status` and hand off to the app.

## Confirm before you write

Confirm before you write. Setup edits change what reps see on live calls. Summarize what you are about to save, get a yes, then call the write tool. Delete and restore-defaults tools require `confirm: true`; ask the user first.

Change one thing at a time and read it back after.

## Writing for Xerpa

Rebuttals, Talk Track coaching and battle cards are read by a rep in the middle of a call. Keep them short and plain: two short lines a rep can say, and a follow-up question. Do not use em dashes. Whispers appear on screen; never describe them as being heard.

Quote numbers as the reporting tools return them. Do not compute new metrics, and say so when a tool has no data.
