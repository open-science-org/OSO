# Integrations, plugins and agent access

Part of the [OSO website and v1 UI design](../ui-v1.md).

Integrations sit on the public API and webhooks, so outside contributors can build them without touching the core. Each is optional and can be switched off per community.

| Integration | What it does | Priority |
| --- | --- | --- |
| **BibTeX import** | Upload a `.bib` file in Submit; the "builds on" step is pre-filled with the cited works found in the graph | Phase A/B, high |
| **Notifications** (email, in app, Slack, Discord) | Alerts for draws, invitations, deadlines and changes | Phase B, required |
| **Zotero** | Import a library to find one's works and citations; later, two-way sync | Later |
| **Browser extension** | On an arXiv or journal page, show an OSO card: in the graph or not, what built on it, claim it | Later |
| **GitHub, Zenodo, Hugging Face** | Register code releases, datasets and models as ideas at fixed versions | Later; Hugging Face fits the seed field |
| **Embeddable badge** | "Built on by N ideas · rating 7" for lab pages and READMEs | Later |
| **Cite this idea** | A BibTeX entry with the work ID | Phase A, small |
| **Webhooks** | Events such as idea admitted or review posted, for community bots | Phase B |
| **MCP server** | Lets AI assistants query the graph (lineage, related work, ratings), laid out as a read-only filesystem ([Agent access: the graph as a filesystem](#agent-access-the-graph-as-a-filesystem)), plus an endpoint for every idea ([An MCP endpoint for every idea](#an-mcp-endpoint-for-every-idea)) | Phase A (read-only); executable ideas later |
| **Python client and data dumps** | Periodic open snapshots of the graph and a client library for meta-research | Later |

Before relying on a third-party service, check its current terms and plugin or API support.

## Agent access: the graph as a filesystem

The MCP server and command-line client follow the design of [Paperclip](https://paperclip.gxl.ai/) (GXL), which presents literature to AI agents as a read-only filesystem that they explore with familiar commands (`ls`, `cat`, `grep`) plus search ([how it works](https://gxl.ai/blog/biomedical-literature-as-a-filesystem/)). We borrow the idea, not code. Paperclip's client is open source under Apache-2.0; its index is a hosted service. In OSO, the same layout covers the idea graph, not only documents:

```
/ideas/<work-id>/
    idea.json            current version (OIP-16)
    versions/<n>.json    every version, with its hash
    content/             sections, figures and tables of the main file ([Data storage](storage.md))
    parents.json         approved parents, weights and evidence (OIP-14)
    children.json        ideas that build on it
    reviews/             reviews and the current rating (OIP-17)
    history.json         its ledger transactions
/communities/<id>/setup.json
/ledger/blocks/<n>.json
```

Commands: `search` (hybrid, [AI services and providers](ai.md)), `grep` within an idea or across the graph, `lineage`, `path`, and `cite` (BibTeX). Everything is read-only; actions that change state stay signed transactions through the normal API (OIP-10, OIP-13).

**Long-term objective: an open literature module.** Paperclip's index is a hosted, closed service. OSO's long-term aim is an open equivalent, built as a replaceable module (a `literature` slot under OIP-12), so the full-text layer of science is not owned by one company. It would:

- index openly licensed full text (for example arXiv papers with open licenses, the PubMed Central open-access subset, and other open-access sources) into sections and passages with stable IDs, plus metadata for everything else from open sources such as OpenAlex;
- offer the same filesystem and MCP interface as [Agent access: the graph as a filesystem](#agent-access-the-graph-as-a-filesystem), with hybrid search;
- be open source and self-hostable, so communities, libraries and universities can run mirrors, and any copy can be checked against content hashes ([Data storage](storage.md));
- respect licenses: full text is stored and served only where the license allows, otherwise metadata and links only.

This is not needed for v1, which uses the seed field's metadata and open full text directly, and may call hosted tools such as Paperclip through the swappable AI adapter ([AI services and providers](ai.md)). It would be specified in a later OIP and could become a community project of its own.

## An MCP endpoint for every idea

Every idea gets its own MCP endpoint, so any AI agent can work with one idea directly: `https://<api host>/mcp/ideas/<work-id>`. It is one shared server that routes by work ID, generated from the graph; authors do nothing to get it.

| Layer | What the endpoint offers | When |
| --- | --- | --- |
| **1. Idea endpoint (every idea)** | *Resources:* the idea and its versions, content sections with passage IDs, parents and weights with evidence, children, reviews and rating, ledger history. *Tools:* ask about this idea (answers cite passages), lineage, related ideas, compare with another idea, cite. *Prompts:* for example "What does this build on?" | Phase A, read-only, with the main MCP server ([Agent access: the graph as a filesystem](#agent-access-the-graph-as-a-filesystem)) |
| **2. Executable idea (optional)** | For ideas with code and data, tools that run the method itself, generated from the codebase, tested, versioned, and registered as a linked `code` idea that reviewers can check. Follows [Paper2Agent](https://arxiv.org/abs/2509.06917) (Miao, Davis, Zhang, Pritchard and Zou), which turns a paper and its code into a tested MCP server | Later: needs sandboxed compute and a budget; not every paper converts |
| **3. Structured claims (optional)** | A short file listing the idea's claims, evidence and provenance, so agents, reviewers and the pre-screen can check claim by claim. Follows [Knows](https://arxiv.org/abs/2604.17309) (Yu and Wang), a structured sidecar for research papers | Later |

**Attribution for AI agents.** Agents connect with their own agent identity (OIP-10 section 1b), so their use of ideas is recorded under a labelled, accountable name. When an agent uses idea endpoints while producing new work, its call log records which ideas it used. If that work is submitted as an idea, the log is offered as evidence for its parents and weights (OIP-14), to be checked and approved like any other evidence. Credit, and later value, then flows back to the ideas an AI relied on. Call counts are never paid out directly, since usage is easy to fake.

**Risks to handle:** compute cost and sandboxing for executable tools; prompt injection from paper content, treated as data (OIP-15 section 7); the licenses of papers and code when generating tools; and keeping tools in step with new versions of an idea.
