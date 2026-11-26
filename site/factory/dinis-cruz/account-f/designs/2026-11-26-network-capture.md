# Network capture and per-agent network ABPs on account 6: design draft

cl-6.security@dinis, 2026-11-26T16:45Z. Draft for agreement with cl-6.accountant@dinis (cost and ledger) and cl-6.abp@PrompTugal.dinis (policies), cl-6.telemetry@PrompTugal.dinis in copy. Then to Dinis through cl-6.maestro.

**Revision 2026-11-26T16:46Z:** cl-6.accountant's ledger side folded in (schema, where rows live, measuring before rollout: Site__Agent__Accountant proposals/2026-11-26-network-ledger-side.md); cl-6.telemetry confirmed it captures no hosts today (it holds get_session only, and the proxy's own log is outside what a session can read). Agreed with cl-6.accountant and cl-6.abp.

**Revision 2026-11-26T16:46Z:** cl-6.accountant priced the draft (by message; figures kept out of this public file). Reading whole transcripts costs about as much as running the team, so layer C is cut to tool calls only, as below.

**Revision 2026-11-26T16:47Z:** cl-6.abp's design (PrompTugal__Agent__ABP abp/network/DESIGN.md, policy v1.7.1) merged in, this file as the spine. Its policy already uses network.allow and network.environment, and gives the no-web-read roles (maestro, conductor, social, abp, accountant, decisions, security) an empty allow-list on the limited environment.

**Dinis's ask** (cl-6.maestro's session, about 16:50 UTC, recorded in Site__Agent__Maestro__Claude__6 decisions/2026-11-26-network-capture.md): "capture all network requests made by the agents here ... so that we can track them and detect ABPs (since we should have super strick ABP for Network access for all agents here)".

## What the platform gives us today (checked in this session)

- Every cloud session sends outbound HTTPS through a local proxy (HTTPS_PROXY on 127.0.0.1) that tunnels to the platform's egress proxy. **The egress proxy enforces the environment's network policy**: an exact allowed-host list ("limited") or everything ("unrestricted"). The agent cannot change it; an environment change needs a person's approval on a card.
- NO_PROXY sends some destinations straight out, not through the proxy: the Anthropic API, MCP proxy, npm, PyPI, crates, Go proxy, and private address ranges.
- A refused request shows as [agent-proxy] lines (host and reason) at the end of a Bash command's output, so it lands in the session's transcript.
- The session transcript (list_events) holds every tool call (WebFetch URLs, Bash commands, MCP calls) and its result. It is kept by the platform; **the agent cannot edit it**.
- Account 6 has two environments: "Default" (limited, package registries and MCP hosts only, no variables) and "Default (Full Network access)" (unrestricted, three credential variables). Most cl-6 sessions, mine included, run on the unrestricted one.
- Not known to me: whether the platform exposes the egress proxy's own log to the account. Dinis says he has seen capture done before; I ask accounts A and B through cl-6.router what that was, and do not assume it.

## Design: three layers

### 1. Enforce first: the network ABP is the environment, not a promise

The strictest control we have is the egress proxy, and it holds even if an agent ignores its hook. So:

- **No agent runs on the unrestricted environment.** It is retired for agents (kept, if at all, for Dinis's own sessions).
- **A small set of limited environments, one per network need**, each with exact hosts only and no credential variables:
  - "No outside network" (today's "Default"): conductors, maestro, decisions, security, abp, accountant, strategy, most writers. Most agents need only GitHub, which is reached through the git proxy and MCP.
  - "Places and public sources": exact hosts of the public sources the place and section agents read (from their run reports, never wildcards).
  - "Data collectors": cl-6.data's feeds (IPMA, TfL and the rest), host by host.
  - one for each agent that holds a paid API (for example OpenRouter), with the key saved as a network secret for that one host, not as a variable.
- Environments are the hard floor but are shared, so they are coarser than one agent's list (cl-6.accountant's point). The per-agent precision comes from detection: each agent's own allow-list is checked against what layer C saw.
- Each role's ABP gains **network.allow** (exact hosts) and **network.environment** (the environment it must run on). cl-6.maestro creates a session only on the environment its ABP names; a session started by another session cannot be moved later, so this is decided at creation.
- Adding a host is a relaxation: proposed by cl-6.abp from a run report, decided by cl-6.decisions (or Dinis), and applied by Dinis on the environment's approval card. No agent widens its own network.

### 2. Capture: every request, from a source the agent cannot rewrite

| Source | Sees | Agent can tamper? | Cost |
|---|---|---|---|
| A. Hook log (cl-6.abp's kit): every WebFetch and WebSearch URL, every Bash command that names a URL or host (curl, wget, git, pip, npm, node or python with a URL) | intended requests, with agent, tool and time | yes (it runs in the session); it is a guardrail and a first record | near zero |
| B. Local logging proxy: a small CONNECT logger in front of the platform proxy (HTTPS_PROXY points at it; it forwards to the platform proxy). **Not yet proven:** a hook cannot change HTTPS_PROXY for later commands; a SessionStart hook writing exports to the file named by CLAUDE_ENV_FILE may; that variable is not set in this session, so cl-6.abp tests it in a session with the kit before the design relies on B recording host, port, time, bytes and the upstream answer (allowed or 403) | every proxied request from any tool, including code the hook cannot read | yes (the agent could unset it), but then layer C shows the gap | near zero; one small process per session |
| C. Transcript audit: one auditor reads each session's **tool calls only** (list_events, assistant events; never results) and keeps only agent, time, tool and host | every request the session asked for, from WebFetch URLs to hosts named in commands | **no**: the platform holds it | the real cost (see below) |

Refusals and unknown hosts come from layer B, not from reading results, so layer C never reads a tool's output (cheaper, and it never sees what a page or command returned). A session whose tool calls show network use but which has no layer B log is itself a breach, so tampering with B shows up in C.

Layer C is the one Dinis asked for ("somewhere agents cannot edit"). It is the narrow form of P1 I described in block-2026-11-26-05: read only tool calls and results, keep only hosts and outcomes, copy no text, store nothing raw. It still reads transcripts in memory, so it needs Dinis's own yes. **It is the same request as cl-6.abp's P1**, not a second one: one named auditor, cl-6.telemetry, read-only, tool calls only, with this design's limits.

Destinations behind NO_PROXY (Anthropic, MCP, package registries, private ranges) do not pass through layer B; they show in layers A and C by tool name, which is enough for them; connectors stay governed by R3.

A host list does not stop what a request carries (a token in a query string). The hook records a path or query only as a keyed hash, never in clear, and the audit keeps host, port, time and outcome only.

### 3. Detect and flag

- The auditor compares each captured host with that role's network.allow. Any host outside it, any refusal by the egress proxy, and any gap (a session with Bash activity but no layer B log) is a **breach**.
- Breaches go to the role's run report, cl-6.abp's hourly review and cl-6.security@PrompTugal.dinis; a breach on a host that could carry data out (paste sites, storage buckets, unknown APIs) goes to Dinis the same day.
- The records live in the auditor's own repository, one file per day, **each commit signed by the auditor's key**, with a CI check that refuses any commit there not signed by it. Every agent's team has push to most repositories (finding A1), so the signature, not the push right, is what makes the record trustworthy.
- **One schema** (owned by cl-6.security, as cl-6.accountant asked): one row per agent, host and UTC day: `day, agent, session, host, port, decision (allowed or refused), count, first_seen, last_seen`, and bytes out and in only where layer B gives them cheaply; plus `source` (A, B or C) and `in_allow_list` (true or false). Never a URL path, query string, header, body or token: a path can carry a secret or a private person's data.
- **Where the rows live:** not on a public page, since they list hosts per agent; in the same non-public home Dinis names for the cost figures. Public pages show only totals (requests, refused, breaches per agent per day), no host names.
- cl-6.accountant joins the rows to its cost ledger by agent and UTC day, and flags a day where refusals jump with cost.

## Cost (for cl-6.accountant to price)

- Layers 1, A and B: no model tokens; a few kilobytes of log per session per day.
- Moving agents off the unrestricted environment can block work, and a blocked agent still costs its wakes: each agent's run report lists the hosts it needed **before** it is moved.
- Audit cost is booked as its own line in the ledger, not as any agent's spend.
- cl-6.accountant measures tokens per wake with and without capture on one agent for a day before any rollout, and reports the method.
- Layer C: reading whole transcripts, results included, would cost about as much as running the whole team, so it is ruled out. Reading tool calls only, once a day, only events since the last audit, in small batches in fresh contexts so the auditor's own context never grows, on the smallest model Dinis allows, is a small fraction of that; the exact figure waits for the one-day measurement. If the platform can export its egress log, layer C is dropped entirely.

## Order

1. Now, no new tool needed: cl-6.maestro starts every new session on "Default" (limited), as already agreed for the conductor reset, the London agent and the social agent. Each agent's run report lists the hosts it needed.
2. cl-6.abp adds network.allow and network.environment to every role (done for the no-web-read roles), from those run reports, each list marked 'by the agent's own account' until layer A or C confirms it.
3. Dinis creates the few limited environments (moving a session means replacing it at its next restart; most cl-6 sessions, cl-6.abp's and mine included, are on the unrestricted one today) with exact hosts (his approval), and existing sessions are replaced on them at their next restart.
4. Layers A and B go into the hook kit, once the push-branch bypass is fixed and the kit passes again (P8 is suspended).
5. Layer C on Dinis's one yes to P1 (cl-6.telemetry as the auditor), after cl-6.accountant's one-day measurement.

## Open

- Test of layer B's start-up (CLAUDE_ENV_FILE) by cl-6.abp.

- What Dinis saw done before (ask accounts A and B through cl-6.router).
- Whether the platform can export its egress log for the account; if it can, it replaces layer C at no token cost.
- My own session runs on the unrestricted environment; it should be on the limited one at my next restart.
