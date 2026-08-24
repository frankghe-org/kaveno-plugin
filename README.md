# Kaveno — the plugin

Kaveno is a marketing and communication assistant for a small B2B operator. It
finds what is worth saying, drafts it, and says who to talk to — and it never
contacts anyone itself.

This repository holds the **procedure layer**: the skills Claude follows on your
behalf, and the connection to the Kaveno server. It is generated from the
[Kaveno source repository](https://github.com/frankghe-org/kaveno) and should not be edited here.

## Install

```bash
claude plugin marketplace add frankghe-org/kaveno-plugin
claude plugin install kaveno@kaveno
```

Then put your credential in the environment that starts Claude — your shell
profile is the usual place:

```bash
export KAVENO_MCP_TOKEN=<your token>
```

Start a new Claude session. `claude mcp list` should show:

```
kaveno: https://kaveno.aigent.biz/mcp (HTTP) - ✔ Connected
```

There is nothing else to install: no checkout, no Docker, no database. The server
holds your map and enforces the rules; these skills are how Claude knows what to
do with it.

## Your credential

Kaveno works out who you are from a token rather than asking, and every tool
derives the company and the operator from it — there is deliberately no anonymous
path. The token is a row with a stated expiry that can be withdrawn on the next
call, so it is not a secret in a config file and it is not in this repository.
Whoever administers your Kaveno server issues it.

**`claude mcp list` says `✔ Connected` even with no token, and that is not a
check you can rely on.** The connection is established before any credential is
looked at — Kaveno checks it on each request instead. So a missing or expired
token shows up not there but on the first thing you ask, as:

```
the bearer token does not resolve to an operator
```

If you see that, the usual cause is that `KAVENO_MCP_TOKEN` never reached the
environment Claude was started from. Export it and start a new session — a session
reads it once, at launch. If it is definitely set, the credential itself has
expired or been withdrawn, and whoever administers your server issues a new one.

## Using it

You do not invoke skills by name and you never type a field name. Describe what
you want:

> *"What sources do we have?"*
> *"Find me groups where renovation contractors discuss suppliers"*

Kaveno refuses a great deal on purpose — unjustified candidates, duplicate
addresses, groups whose rules nobody has read — and explains what to do about each
refusal. That is the product, not friction.
