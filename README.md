# Open Science Organization (OSO)

OSO is building an open, community-owned platform for the whole research cycle (fund, research, review, publish). Its core is a graph of ideas in which credit and value flow back to the work that new ideas build on.

## 2026 restart

OSO was active from 2017 to 2020 and then went dormant. In October 2026 the project restarted. Two things have changed since then: AI can now do much of the judgment work the original design left open, such as estimating how much one paper depends on another, screening for spam and plagiarism, and assisting review; and open citation data makes it possible to start from an existing graph of the literature instead of an empty one. The rapid growth of AI-generated research also makes open, accountable curation and attribution more urgent.

**Start here**

| Document | What it is |
| --- | --- |
| [OSO v1 design doc](docs/design-v1.md) | The overview: vision, north star and milestones, design principles, mechanisms, architecture, roadmap and open questions |
| [OSO Idea Proposals (OIPs)](https://github.com/open-science-org/OIPs) | The exact specifications, written and reviewed in the style of Ethereum's EIPs |

**v1 in one paragraph.** v1 is a working prototype of the core protocol: an idea graph with ownership, tokens and value flow-back, running on a simulated ledger whose log is public on GitHub. It is seeded from existing literature in one topic, includes a minimal interface for chatting about ideas, exploring related ideas and value flow, and submitting new ideas, and routes every submission through validation and then peer review. v1 tests accounting and incentive hypotheses; real money and a public blockchain come later, at milestone M3.

**Key specifications (drafts)**

| OIP | Title |
| --- | --- |
| [OIP-0](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-0.md) | OIP purpose and guidelines |
| [OIP-8](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-8.md) | Token structure |
| [OIP-9](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-9.md) | Reputation and expertise |
| [OIP-10](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-10.md) | Ownership and identity |
| [OIP-11](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-11.md) | Submission routing |
| [OIP-12](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-12.md) | Module interface and community setups |
| [OIP-13](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-13.md) | Public ledger and migration path |
| [OIP-14](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-14.md) | Idea attribution and value flow |
| [OIP-15](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-15.md) | AI services, models and costs |

**How to get involved.** Read the design doc and comment on the open questions at its end. To propose a change or a new idea, follow [OIP-0](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-0.md). You can also email contact@oso.network.

## Original vision (2017)

The text below is the project's original description, kept for history. Two points have since changed: voting weight now comes from earned reputation only, not from tokens staked ([OIP-9](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-9.md)); and a public blockchain is used only once real money and self-custody require it ([OIP-13](https://github.com/open-science-org/OIPs/blob/master/OIPS/oip-13.md)).

> Modern day science has been a collective effort of different researchers and research groups. The collective intellectual power gathered by the scientific ecosystem is akin to the group intelligence (the “wisdom of crowds”) where a wise crowd is generally more intelligent compared to individual members in the group. The additional gain in intelligence is primarily dependent on the efficient flow of value (capital, information, etc.) among the members. The scientific community is still dependent on the rudimentary system for value flow, which is highly centralized, opaque, and redundant. Thereby it suffers from several problems: funding agencies are controlled by a small number of people, majority of scientists’ time is consumed by grant application writing with small success rates, a slow publication process, institutional biases, underpaid researchers, irreproducible publications, high subscription fees for journals, and a general focus on quantity of scientific publications over quality. These problems can be solved and the overall efficiency of the scientific community can be improved by creating an open and decentralized scientific ecosystem based on blockchain technology (or its variants).
>
> Open Science Organization (OSO) is a non-profit Decentralized Autonomous Organization (DAO), which promises to create an open, decentralized, and efficient scientific ecosystem. The three key characteristics of OSO ecosystem are
>
> 1. an open democratic funding process
> 2. an open democratic perpetual review process
> 3. the flow of value (in OSO tokens) created by the scientific results (publications, products, patents, etc.) back to the funding, thus creating a perpetual system for scientific research
>
> In OSO ecosystem, all steps/procedures will be algorithmic or will be based on community voting. Even certain aspects of algorithmic steps (e.g. algorithms used for decision making) are subjected to change based upon voting. Any individual or organization who holds OSO tokens will be considered an entity (or a member) in the OSO ecosystem. Each entity will have a voting power proportional to their expertise (e-value) in the corresponding scientific domain and the amount of value (OSO tokens) they are willing to invest/stake.

## History

| Date | Document |
| --- | --- |
| Aug 2017 | [Original white paper](https://github.com/open-science-org/wiki/blob/master/OSO_white_paper.pdf) |
| May 2018 | [Technical design v0](OSO_design_v0.pdf) |
| Sep 2018 | [Proof of Idea v0.0](https://github.com/open-science-org/wiki/blob/master/Proof_of_Idea.pdf) |
| Nov 2018 | [OSO: An Idea Platform, v0.3](https://github.com/open-science-org/wiki/blob/master/OSO_Idea_Platform_whitepaper.pdf) |
| Oct 2026 | [OSO v1 design doc](docs/design-v1.md) and OIPs 0 and 8–15 |
