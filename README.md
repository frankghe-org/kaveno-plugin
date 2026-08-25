# Kaveno — the plugin

Kaveno is a marketing and communication assistant for a small B2B operator. It
finds what is worth saying, drafts it, and says who to talk to — and it never
contacts anyone itself.

This repository holds the **procedure layer**: the skills Claude follows on your
behalf, and the connection to the Kaveno server. It is generated from Kaveno's
source repository, which is private — so everything you need is here, and nothing
here should be edited, since the next publish overwrites it.

## Install

```bash
claude plugin marketplace add frankghe-org/kaveno-plugin
claude plugin install kaveno@kaveno
```

Then start a new Claude session. That is the whole install — no token to paste,
no environment variable, no checkout, no Docker, no database.

`claude mcp list` should show:

```
kaveno: https://kaveno.aigent.biz/mcp (HTTP) - ✔ Connected
```

There is nothing else to set up. The server holds your map and enforces the
rules; these skills are how Claude knows what to do with it.

## Who you are

Kaveno works out who you are from a credential rather than asking, and every tool
derives the company and the operator from it. That credential is held by the
server your address points at — not by you, and not by this repository — which is
why there is nothing to paste.

One consequence worth knowing: everyone reaching a given server is the same
operator to it, so the map you see is the map everyone on that address sees.

## If something is wrong

**`claude mcp list` says `✔ Connected`, so everything must be fine.** It is not a
check you can rely on. The connection is established before any credential is
looked at, so this line says the server is reachable and nothing about whether it
will answer you.

**`the bearer token does not resolve to an operator`.** The credential your server
injects has expired or been withdrawn. Nothing on your machine can fix this and
nothing on your machine caused it — tell whoever administers the server.

**No `mcp__kaveno__*` tools in the session.** The session predates the install.
Start a new one; `claude plugin details kaveno@kaveno` should list seven skills
and one MCP server.

**Claude answers, but with a bare error rather than a sentence.** The server is
connected and the skills are not. Same check as above — if it lists no skills,
install the plugin again.

**Claude has to be told how to research or judge a source.** Same cause: the
skills are missing. When they are loaded, none of that needs explaining.

**`Failed to connect`.** The server itself is unreachable. That is the
administrator's end, not yours; send them the address shown in `claude mcp list`.

## Using it

You do not invoke skills by name and you never type a field name. Describe what
you want:

> *"What sources do we have?"*
> *"Find me groups where renovation contractors discuss suppliers"*

Kaveno refuses a great deal on purpose — unjustified candidates, duplicate
addresses, groups whose rules nobody has read — and explains what to do about each
refusal. That is the product, not friction.
