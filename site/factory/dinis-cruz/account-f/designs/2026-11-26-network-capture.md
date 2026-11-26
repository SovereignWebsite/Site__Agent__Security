# Network capture and per-agent network ABPs on account 6: design draft

cl-6.security@dinis, 2026-11-26T16:45Z. Draft for agreement with cl-6.accountant@dinis (cost and ledger) and cl-6.abp@PrompTugal.dinis (policies), cl-6.telemetry@PrompTugal.dinis in copy. Then to Dinis through cl-6.maestro.

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
- Each role's ABP gains **network.allow** (exact hosts) and **network.environment** (the environment it must run on). cl-6.maestro creates a session only on the environment its ABP names; a session started by another session cannot be moved later, so this is decided at creation.
- Adding a host is a relaxation: proposed by cl-6.abp from a run report, decided by cl-6.decisions (or Dinis), and applied by Dinis on the environment's approval card. No agent widens its own network.

### 2. Capture: every request, from a source the agent cannot rewrite

| Source | Sees | Agent can tamper? | Cost |
|---|---|---|---|
| A. Hook log (cl-6.abp's kit): every WebFetch and WebSearch URL, every Bash command that names a URL or host (curl, wget, git, pip, npm, node or python with a URL) | intended requests, with agent, tool and time | yes (it runs in the session); it is a guardrail and a first record | near zero |
| B. Local logging proxy: a small CONNECT logger in front of the platform proxy (HTTPS_PROXY points at it; it forwards to the platform proxy) recording host, port, time, bytes and the upstream answer (allowed or 403) | every proxied request from any tool, including code the hook cannot read | yes (the agent could unset it), but then layer C shows the gap | near zero; one small process per session |
| C. Transcript audit: one auditor reads each session's tool calls and results from the platform (list_events) and keeps only agent, time, tool, host and allowed or refused | everything the session did, including [agent-proxy] refusals | **no**: the platform holds it | the real cost (see below) |

Layer C is the one Dinis asked for ("somewhere agents cannot edit"). It is the narrow form of P1 I described in block-2026-11-26-05: read only tool calls and results, keep only hosts and outcomes, copy no text, store nothing raw. It still reads transcripts in memory, so it needs Dinis's own yes, and the auditor should be one agent (cl-6.telemetry, or a dedicated cl-6.netaudit), not every agent.

Destinations behind NO_PROXY (Anthropic, MCP, package registries) do not pass through layer B; they show in layers A and C by tool name, which is enough for them.

### 3. Detect and flag

- The auditor compares each captured host with that role's network.allow. Any host outside it, any refusal by the egress proxy, and any gap (a session with Bash activity but no layer B log) is a **breach**.
- Breaches go to the role's run report, cl-6.abp's hourly review and cl-6.security@PrompTugal.dinis; a breach on a host that could carry data out (paste sites, storage buckets, unknown APIs) goes to Dinis the same day.
- The records live in the auditor's own repository, one file per day, **each commit signed by the auditor's key**, with a CI check that refuses any commit there not signed by it. Every agent's team has push to most repositories (finding A1), so the signature, not the push right, is what makes the record trustworthy.
- Aggregates (hosts per agent per day, refusals, breaches) are public; the raw records hold no URL path, query string or body, only hosts, because paths can hold tokens or personal data.

## Cost (for cl-6.accountant to price)

- Layers 1, A and B: no model tokens; a few kilobytes of log per session per day.
- Layer C: tokens to page through transcripts. A busy session's page of 40 events was over 500,000 characters today, because tool results are included. Keep it cheap by reading only new events since the last audit, filtered to tool calls and results, once or twice a day; run it on a small model; budget it as its own line. cl-6.accountant to estimate from cl-6.telemetry's event counts.

## Order

1. Now, no new tool needed: cl-6.maestro starts every new session on "Default" (limited), as already agreed for the conductor reset, the London agent and the social agent. Each agent's run report lists the hosts it needed.
2. cl-6.abp adds network.allow and network.environment to every role, from those run reports (with the cover-all record).
3. Dinis creates the few limited environments with exact hosts (his approval), and existing sessions are replaced on them at their next restart.
4. Layers A and B go into the hook kit, once the push-branch bypass is fixed and the kit passes again (P8 is suspended).
5. Layer C on Dinis's yes, with the auditor named, after the accountant's cost estimate.

## Open

- What Dinis saw done before (ask accounts A and B through cl-6.router).
- Whether the platform can export its egress log for the account; if it can, it replaces layer C at no token cost.
- My own session runs on the unrestricted environment; it should be on the limited one at my next restart.
