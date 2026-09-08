# Connecting Kaveno's server

Kaveno is two halves: the **skills** you already have, and a **server** that holds the
source map and enforces the rules. The skills cannot do anything on their own — every
one of them begins by reading the map — so this file exists to get the server connected
and to say plainly what it looks like when it is not.

**If `sources`, `sources_add` and `flag` are already among your tools, there is nothing
to do here.** Stop and use the plugin.

## What the failure looks like, so it is not mistaken for something else

The plugin installs cleanly. All seven skills load. `/sources` is there and answers.
And the three tools are simply absent, with nothing anywhere naming a cause.

That combination is not a broken plugin and not a broken install. **Skills arrive with
the package; the server has to be connected separately on some surfaces.** Reading it as
a packaging fault sends you looking in the wrong place — it did exactly that to the
people who built this.

## Before anything: Claude needs access to this repository

**Grant Claude access to your GitHub account, and to `frankghe-org/kaveno-plugin` in particular.** A
marketplace added from a GitHub repository is synced by Claude on your behalf, so without
that access it cannot read the repository at all.

This is the step that is easiest to miss, because **nothing about the failure names it**.
The marketplace appears in your list. Adding it again is refused as *"already added"*. The
plugin's Update button is simply greyed, for every release, indefinitely — and the version
you are on is whatever happened to be published when access last worked, which may be
months old. It reads as a stale cache or a broken publish, and it is neither. It cost the
people who built this the better part of a day.

If a marketplace has stopped picking up new versions, check this **first** and check it
before theorising about anything else.

## Cowork and the Claude desktop app

**Add the server as a connector, and switch it on for the chat.** Take the full URL from
`.mcp.json` — `https://kaveno.aigent.biz/mcp` for the shared deployment — and add it under **Customize →
Connectors**. Connectors are then chosen **per chat**, from the `+` menu in the chat box,
so one that is installed but not switched on for the conversation you are typing in looks
exactly like one that was never added.

**There is no network allowlist step, and adding the server's host to one will not help.**
Anthropic's own documentation is explicit: *"Network egress permissions don't apply to the
web fetch or web search tools or MCPs."* A connector is not reached from inside the
sandbox — *"connector authorization tokens never enter the sandbox; connector calls are
made on the server side"* — so the egress allowlist, which governs code execution, has
nothing to do with whether this server is reachable.

*This file told you to allowlist `kaveno.aigent.biz` first, until 8 September 2026. That was wrong,
and it was wrong in the expensive direction: it sent people to configure a setting that
does nothing and described the connector step as optional when it is the only one that
works on a cloud surface.*

**Why the plugin cannot carry the server for you here.** A plugin-bundled `.mcp.json`
registers a **local** MCP server, and *"local MCP servers don't run in sessions in the
cloud"*. So on a cloud surface the plugin gives you the skills and the connector gives you
the tools, and there is no packaging change that collapses the two.

No credential is asked for, and that is correct rather than a step you have missed. The
server authenticates the operator itself, from a token supplied by the deployment rather
than by you.

## Claude Code

Nothing to do. The bundled server connects on its own; start a new session and check
with `claude mcp list`.

Be aware that `✔ Connected` there is a weaker signal than it looks: the transport
connects before any credential is examined, so it says the server is reachable and
nothing about whether it will answer you. The real check is asking for your sources.

## Confirming it worked

Ask for your source map. An empty map on a new deployment is the correct answer and not
a fault. Then ask to add a source with no justification — it should refuse, and say what
for. A refusal is the best evidence the rules are live, because most of what this system
is worth sits in what it declines.

## When it still does not connect

- **`the bearer token does not resolve to an operator`** — the credential the deployment
  supplies has expired or been withdrawn. Nothing on this machine caused it or can fix
  it; tell whoever administers the server.
- **Tools still absent after both steps** — confirm the connector is switched on for
  *this* chat, not merely present in settings.
- **The address answers nothing at all** — the deployment is down, which is the
  administrator's end.
