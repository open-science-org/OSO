# OSO website and v1 UI: high-level design

**Author:** Gajendra Jung Katuwal (@himalayajung) · **Builders:** Abinash, Bikrant · **Status:** Draft for discussion · **Last updated:** 2026-10-06

> **TL;DR.** Start building now, without waiting for the OIPs to settle. Work in two tracks. **Track 1** grows www.oso.network from one page into a small site that explains OSO and collects sign-ups. **Track 2** builds the v1 app against a **mock API** loaded with real papers from the seed sub-field: explore ideas, read an idea, see its graph and the whole field's lineage, simulate value flow, and chat about it. Everything that depends on rules still under discussion (tokens, reputation, validation) comes from the API, never from the UI, so rule changes do not touch the front end. Sign-in, submission and the validator and reviewer queues come in Phase B, once the ledger exists; their screens are designed now. AI calls go through one swappable adapter: OpenRouter or local open-weight models, chosen per task by configuration ([AI services and providers](ui/ai.md)). The app is **modular**: each community's setup decides which modules, steps, parameters and screens it uses, and the app reads that setup instead of hard-coding rules ([Modular and customizable design](ui/modularity.md)). Mockups of all fourteen screens, as images and HTML, are in [ui-mockups/](ui-mockups/README.md). The detail is split into the pages listed in [The design in pages](#4-the-design-in-pages). Parent docs: [design doc](design-v1.md), [OIP-16: Idea object](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-16.md).

## 1. Goals

1. Give newcomers a clear website that explains OSO, shows progress and collects interest.
2. Make the idea graph tangible before the ledger exists: anyone can browse real papers, see what each builds on, and watch value flow back.
3. Build the app shell that Phase B will plug the real ledger into, so nothing is thrown away.

**Not in this phase:** real accounts and sign-in, real submissions, validator and reviewer queues, wallets, anything on a blockchain.

**How the phases relate.** Phase A and Phase B are both inside **roadmap Phase 1 (v1)** in the design doc. Phase A is the part that can be built before the ledger exists (website, explorer, graph, simulator, chat); Phase B adds everything that writes to the ledger (sign-in, submission, validation, review, claims). Roadmap Phases 2 and 3 (pilot community, trustless launch) come after both.

## 2. What is stable enough to build on

| Area | State | How the UI treats it |
| --- | --- | --- |
| Idea object: work ID, versions, title, abstract, type, content links, attribution, license ([OIP-16](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-16.md)) | Draft, but unlikely to change much | Build directly on it |
| Graph: parents, children, citations, link types ([OIP-14](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-14.md) section 2) | Draft, stable shape | Build directly on it |
| Edge weights and the value-flow waterfall (OIP-14) | Rules may change | Show what the API returns; never compute in the UI |
| Tokens, mint split, α (OIP-8) | Numbers may change | Show API values; label them "simulated" |
| Reputation, validation, review (OIPs 9, 11, 17) | Under discussion | Phase B only; design screens, build later |
| Chat (OIP-15) | Stable enough | Build behind a feature flag, with a spending cap |

**Rule:** the UI never contains protocol logic. If a number depends on an OIP, the API computes it and the UI displays it. When an OIP changes, only the API (and later the ledger) changes.

## 3. Users and what they come for

| User | First question | Where they go |
| --- | --- | --- |
| Newcomer (researcher, engineer, funder) | What is OSO and should I care? | Website home, then the explorer |
| Researcher in the seed field | Is my work here, and what built on it? | Search, then their paper's idea page |
| Former team member or contributor | What is the plan and how can I help? | Website "Get involved", design doc, OIPs |
| Funder (later) | Where would my money go? | Value-flow simulator |

## 4. The design in pages

| Page | What it covers |
| --- | --- |
| [Website](ui/website.md) | Track 1: the pages of www.oso.network and sign-ups |
| [The v1 app](ui/app.md) | Track 2: screens, key interactions, workflow screens (submit, validate, review), graph explorer, claiming, profile and transparency, sign-in, design principles |
| [Modular and customizable design](ui/modularity.md) | How community setups drive the app, and the table of swappable defaults |
| [AI services and providers](ui/ai.md) | Where AI is used, the swappable adapter, provider options and costs |
| [Integrations, plugins and agent access](ui/integrations.md) | Plugins, the graph as a filesystem for AI agents, an MCP endpoint for every idea, the open literature module |
| [Data storage](ui/storage.md) | Where each kind of data lives, and the path to decentralized storage |
| [Architecture and API](ui/architecture.md) | Components and the API contract |
| [Mockups](ui-mockups/README.md) | All screens as images and HTML |

## 5. Seed data

- **Field:** parameter-efficient fine-tuning of large language models, as proposed in the design doc's open questions; confirm before importing.
- **Curated slice:** 50 to 100 key works, chosen with a domain expert; edges assessed by AI and reviewed by a person before they show weights.
- **Read-only import:** the wider LLM-adaptation literature from OpenAlex, searchable, with citation links but no weights.
- **Licensing:** store metadata and links only, not PDFs; respect OpenAlex and arXiv terms.

## 6. Work plan

Suggested split; Abinash and Bikrant should adjust it to their strengths.

| Week | Abinash (front end) | Bikrant (data and API) |
| --- | --- | --- |
| 1 | Website v2: pages, sign-up form, shared style and design tokens | Confirm stack; API skeleton with the contract above, fixture data and the community setup fixture |
| 2–3 | App shell (community switcher, setup loading, UI module registry), search, idea page | Import script for the seed field; search endpoint |
| 4–5 | Graph view, edge evidence panel, graph explorer (lineage view) | Graph endpoint; AI edge assessments for the curated slice (with review) |
| 6 | Fund simulator | Simulator endpoint implementing the OIP-14 waterfall, with tests from OIP-14 |
| 7 | Chat UI | AI adapter (OpenRouter and local providers), chat endpoint, spending cap |
| 8 | Field map, transparency page, polish, accessibility | Graph explorer and map endpoints; transparency endpoint; mock ledger; deploy |

**Priorities if time runs short:** search and idea page, then the lineage view, then the simulator, then chat; the field map and transparency page can slip. In Phase B, claim-your-work and notifications come first, because existing researchers and working queues depend on them.

Phase B work (submit, status, validator queue, review, claims, profile, notifications) starts once the ledger and OIPs 11 and 17 are accepted; the mockups and [Workflow screens (Phase B): submit, validate, review](ui/app.md#workflow-screens-phase-b-submit-validate-review) are its starting point.

**Definition of done for Phase A:** a newcomer can open the website, find a real paper in the seed field, see what it built on and what built on it, ask the chat about it, and run a funding simulation whose numbers match the OIP-14 test cases.

**Working rules:** short-lived branches with same-day pull requests; screenshots in pull requests for UI changes; no protocol logic in the front end; the API contract in this doc is updated in the same pull request as any change to it.

## 7. Working together

- **One board.** A GitHub Project with one issue per task in [Work plan](ui-v1.md#6-work-plan), each with an owner, a short acceptance check and a link to the doc section or mockup. Labels: `phase-a`, `phase-b`, `blocked-by-oip`, `decision-needed`.
- **Executable contract.** [Architecture](ui/architecture.md) as an OpenAPI file, with a mock server and fixtures of real seed-field papers, so front end and back end work in parallel from day one.
- **Kickoff, then a light rhythm.** A one-hour kickoff through this doc, the mockups and the week-one decisions. Then a weekly written update in GitHub Discussions (done, next, blocked) and a 30-minute call for what writing cannot settle.
- **Decision log.** One short entry per decision (what, why, who, date) in the repository, linked from the transparency page.
- **Pull requests.** Same-day review; a template asking for a summary, screenshots for UI changes, and the issue link; a preview deploy for every pull request.
- **Repository basics.** README and CONTRIBUTING; one-command local setup with all AI on a local model (free); CI with lint and tests; the OIP-14 test cases as automated tests for the simulator.
- **Component gallery.** One page showing the shared building blocks ([Modular and customizable design](ui/modularity.md), item 3), so new modules reuse them.
- **Early user feedback.** Around week 3, 20-minute sessions with three to five researchers in the seed field on the explorer and idea page.
- **Outward updates.** A dated monthly update on the website's Progress page and to the sign-up list, including which open questions we want outside input on.

## 8. Open decisions

1. Stack and repository name ([Architecture](ui/architecture.md)).
2. Domain for the app: a subdomain of oso.network or a path on the main site.
3. Sign-up service for the website, and who manages the list.
4. Which models to pin for each task, the monthly AI budget, and who pays ([AI services and providers](ui/ai.md)). Who holds the OpenRouter key.
5. Who chooses the curated slice and reviews its edges.
6. Whether Phase A shows the ledger explorer at all, or waits for the real ledger.
7. Privacy for Phase B: which identity details are public on the ledger (see the OIP-10 discussion).
8. Whether validators see the authors' names before voting. The mockup hides them until the vote closes; OIP-11 does not yet say.
9. Which interface options communities may change ([Modular and customizable design](ui/modularity.md), item 4), and who approves a change.
10. Which integrations to build first ([Integrations and plugins](ui/integrations.md)), and whether outside contributors may build them.
11. Which sign-in methods at launch, and a hosted sign-in service or our own ([Sign-in and linked accounts](ui/app.md#sign-in-and-linked-accounts)). When to add Sign in with Apple, who pays for the developer membership, and who holds the account.
12. Where to host the node, database and content store; file size limit; backups; privacy policy ([Data storage](ui/storage.md)).

## References

Ideas borrowed from other projects, with what we took:

- **[Paperclip](https://paperclip.gxl.ai/) (GXL)**, an agent-native filesystem over biomedical literature, with an open-source (Apache-2.0) client and a hosted index. GXL, "[Representing biomedical literature as a filesystem through agent-native indexing](https://gxl.ai/blog/biomedical-literature-as-a-filesystem/)" and the [Paperclip repository](https://github.com/GXL-ai/paperclip). Borrowed: the filesystem layout for agents ([Agent access: the graph as a filesystem](ui/integrations.md#agent-access-the-graph-as-a-filesystem)), passage-level identifiers and citations ([Key interactions](ui/app.md#key-interactions) and [Data storage](ui/storage.md)), and hybrid keyword-plus-embedding search ([AI services and providers](ui/ai.md)). No code is reused.
- **[Paper2Agent](https://arxiv.org/abs/2509.06917) (Miao, Davis, Zhang, Pritchard and Zou)**, "Paper2Agent: Reimagining Research Papers As Interactive and Reliable AI Agents", arXiv:2509.06917 (2025); reported as published in *Nature* in 2026. Borrowed: turning an idea's code into tested MCP tools ([An MCP endpoint for every idea](ui/integrations.md#an-mcp-endpoint-for-every-idea), layer 2).
- **[Knows](https://arxiv.org/abs/2604.17309) (Yu and Wang)**, "Knows: Agent-Native Structured Research Representations", arXiv:2604.17309 (2026). Borrowed: a structured claims and evidence file beside each paper ([An MCP endpoint for every idea](ui/integrations.md#an-mcp-endpoint-for-every-idea), layer 3).
- **Literature-search MCP servers**, such as [paper-mcp](https://github.com/MCPServings/paper-mcp) and [paper-search-mcp](https://mcpservers.org/servers/openags/paper-search-mcp), which search arXiv, Semantic Scholar and OpenAlex together. Reference for the whole-graph MCP server ([Agent access: the graph as a filesystem](ui/integrations.md#agent-access-the-graph-as-a-filesystem)).
- **Reciprocal rank fusion:** Cormack, Clarke and Büttcher, "Reciprocal Rank Fusion outperforms Condorcet and individual Rank Learning Methods", SIGIR 2009. Used to merge keyword and embedding search results ([AI services and providers](ui/ai.md)).
