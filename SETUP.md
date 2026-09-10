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

**Install the Claude GitHub App on the account that OWNS `frankghe-org/kaveno-plugin`** — not merely on
your own. Automatic sync will tell you so at the toggle: *"Auto-sync requires the Claude
GitHub App to have access to this repository."*

**This is required even though the repository is public, and that is not a
contradiction.** Public visibility governs who may READ the repository — anyone, with no
credential at all. Auto-sync is not a read; it is a NOTIFICATION that the repository
changed, and GitHub does not notify a third party about a repository unless an App is
installed on it. Reading needs nothing. Being told needs the App.

**App installations do not cross accounts.** A user account and an organisation are
separate installation targets with no inheritance between them, so an App installed on
your personal account grants exactly nothing for a repository owned by an organisation —
which `frankghe-org/kaveno-plugin` is. Install it on the organisation.

*This file told you to "grant Claude access to your GitHub account" until 10 September
2026, and said that without it Claude "cannot read the repository at all". Both halves
were wrong: the account is the wrong target when an organisation owns the repository, and
reading was never the thing that needed granting.*

Note what this does NOT fix: the **manual** Update button. That is a separate fault with a
separate cause, and having auto-sync working does not light it up.

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
