# Modular and customizable design

Part of the [OSO website and v1 UI design](../ui-v1.md).

OSO is a small fixed core with replaceable modules, and each community chooses its modules and parameters ([OIP-12](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-12.md)). The app mirrors this, so a community can change its rules, or a module can be replaced, without rebuilding the app.

1. **The setup drives the app.** On load, the app fetches the current community's setup (`GET /v1/communities/{id}/setup`): its module for each slot, the parameters, and its interface options. Screens read from it: the Submit side panel lists that community's steps; the validator queue shows its criteria and N; the review form shows its score scale; labels such as "5 validators" or "3 reviewers" are never typed into the UI.
2. **One UI module per protocol module.** Each protocol module (validation, review, edge weights, pre-screen, identity, AI services) has a matching front-end module that owns its screens and panels, for example `ui-validation-random-n` for the validator queue and the status timeline's validation step. A community that picks a different validation module (say, an invited panel) gets that module's UI in the same places. A small registry maps the setup's module IDs to UI modules, and an unknown module shows a plain fallback view instead of breaking.
3. **Shared building blocks.** Screens are built from shared components (idea card, graph view, timeline step, AI report card, score input, weight table) so new modules reuse them.
4. **Interface options are separate from rules.** A community may turn screens on or off (chat, fund simulator), choose which idea types it accepts, set its accent colour and edit labels and help text. These change presentation only; anything that changes who decides or what pays is a module or parameter, set through governance.
5. **Themes as design tokens.** Colours, type and spacing are tokens (CSS variables), with OSO's defaults; a community overrides a few tokens, never component code.
6. **Community switcher.** The header shows the current community and lets users switch; ideas stay global, and an idea's page shows the community it was submitted to.
7. **Forks.** The setup screen shows modules, parameters, interface options and version history, with "Propose a change" and "Fork this setup".

In Phase A there is one community and the setup comes from a fixture file, but the app already reads it, so Phase B adds communities without restructuring.

## Every choice is a default, not a dependency

Each concrete technology or rule in this document is the **v1 default** behind an interface. Replacing it is a configuration change or a new module, never a rewrite.

| Area | v1 default | How it is swapped | Defined in |
| --- | --- | --- | --- |
| Seed field | Parameter-efficient fine-tuning of LLMs | A new community with its own import sources and setup | Design doc; [Seed data](../ui-v1.md#5-seed-data) |
| Kinds of ideas | Paper, preprint, dataset, code, review, replication, hypothesis, other | Community subtypes (for example `other/legal-analysis`); new base types by OIP | OIP-16 section 2 |
| Import sources | OpenAlex, arXiv | Ingestion module; sources listed per community | OIP-12 section 6 |
| Sign-in | Google, email link, GitHub, ORCID | Identity module configuration (OpenID Connect) | OIP-10; [Sign-in and linked accounts](app.md#sign-in-and-linked-accounts) |
| Attestations that allow claims | ORCID for papers | Community setup (for example GitHub for code, Hugging Face for models) | OIP-10 section 6 |
| Admission criteria | The fixed list in OIP-11 | Communities add checkable criteria; never merit | OIP-11 section 4 |
| Validation | Random N = 5, majority | Validation module and parameters | OIP-11, OIP-12 |
| Review | Four layers: spam filter, 3 assigned expert reviewers (weighted draw), expert readers, community; score 0–10; expert and community ratings kept separate | Review module and parameters; extra questions per community | OIP-17 |
| Attribution rubric | Six categories | Communities add field-specific categories | OIP-14 section 4 |
| Reputation formula | Expertise × integrity | Reputation module | OIP-9 |
| AI models and providers | OpenRouter for chat and assessments; local models for embeddings | Configuration per task; per community in Phase B | OIP-15; [AI services and providers](ai.md) |
| Literature access | Direct import; hosted tools optional | Literature module (open index long term) | [Agent access: the graph as a filesystem](integrations.md#agent-access-the-graph-as-a-filesystem) |
| Content storage | Central S3-compatible store | Storage module; decentralized later | OIP-16 section 5a; [Data storage](storage.md) |
| Ledger publication | GitHub, with mirrors | Any public git host; smart contracts at M3 | OIP-13 |
| Database | SQLite (rebuildable from the log) | Any database behind the state interface | Design doc |
| Notifications | Email and in-app | Notification module (Slack, Discord and others) | OIP-12 |
| Hosting | To be decided | Any cloud or self-hosted server | [Data storage](storage.md) |
| Front-end stack | TypeScript and React; Cytoscape.js, Sigma.js, D3 | UI modules and shared components | [Modular and customizable design](modularity.md) and [Architecture](architecture.md) |
| Look and wording | OSO design tokens, English | Community tokens, labels and translations | [Modular and customizable design](modularity.md) |
| Governance | Founding team | Governance module, by milestone | OIP-0, OIP-12 |

**What stays fixed (the core, OIP-12 section 1):** the idea object, transactions and blocks, accounts, the mint and the value-flow waterfall, and the module interfaces. Fixing these is what lets different communities, modules and providers work together on one shared graph.

**General purpose.** Nothing in the core assumes a particular field, kind of contribution, language or vendor (OIP-12 section 7). The seed field is only where we start; the same app should serve a biology lab, a group of engineers, legal scholars or designers through their own community setup, subtypes, criteria and review questions. Every user-facing string is kept in translation files from the start, so communities can work in their own language.
