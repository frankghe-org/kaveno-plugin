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

## Cowork and the Claude desktop app

Two things are needed, and either one alone leaves the tools missing.

**1. Allow the server's host on the network allowlist.** Cowork runs the session in an
isolated environment whose network access follows the egress settings configured for it.
A host that is not on that allowlist is unreachable no matter how correctly everything
else is set up. Add the host from the plugin's `.mcp.json` URL — for the shared
deployment that is `kaveno.aigent.biz`.

**2. Add the server as a connector, and switch it on for the chat.** Take the full URL
from `.mcp.json` — `https://kaveno.aigent.biz/mcp` for the shared deployment — and add
it under **Customize → Connectors**. Connectors are then chosen **per chat**, from the
`+` menu in the chat box, so one that is installed but not switched on for the
conversation you are typing in looks exactly like one that was never added.

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
