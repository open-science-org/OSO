# Data storage

Part of the [OSO website and v1 UI design](../ui-v1.md).

**For now, store centrally; long term, decentralize.** Everything public is addressed by its SHA-256 hash and kept behind the storage module interface (OIP-12), so moving it to decentralized storage later changes no IDs and no code outside the module.

| Data | Where it lives in v1 | Public? | Rebuildable? |
| --- | --- | --- | --- |
| Ledger: ideas and versions, links and weights, votes, review records, balances, claims | Public GitHub repo, one file per block, signed commits, with regular state snapshots and the latest block hash copied to independent mirrors (OIP-13) | Yes | It is the source of truth |
| Current state: balances, graph, scores | Database on the OSO node, rebuilt by replaying the log | Yes, through the API | Yes |
| Uploaded files (papers, data, code archives) and text written in OSO (reviews, replies, authors' notes) | **Central content store**: an S3-compatible object-storage bucket run by the operator, files named by hash, with a size limit and a backup in a separate location (OIP-16 section 5a) | Yes once admitted; before admission only owners and drawn validators | From backups; verifiable by hash |
| Structured text of each paper: sections, figures, tables, with an ID for every passage | Derived from the content store (or from open full text for imports) and kept beside it; the ID lets AI assessments, chat answers and reviews cite exact passages. Adapted from [Paperclip](https://paperclip.gxl.ai/) (GXL), which gives every line an identifier traceable to its source ([description](https://gxl.ai/blog/biomedical-literature-as-a-filesystem/)) | Same as the source content | Yes, from the content |
| Imported metadata and AI outputs | On the ledger, as recorded inputs | Yes | Yes |
| Search and similarity index | Alongside the database on the node | No | Yes |
| Accounts, emails, linked sign-ins, notification settings | Operator's private database, encrypted backups; never on the ledger | No | No |
| Signing keys held for users in v1 | Encrypted, ideally in a cloud key-management service, under the published custody policy (OIP-10) | No | No |
| Chat conversations | Not recorded; kept briefly or not at all | No | Not needed |
| Website and sign-up list | GitHub Pages; the outside form or mailing-list service | Site yes, list no | — |

**Path to decentralization**

| Stage | Content | Ledger |
| --- | --- | --- |
| Now (v1) | Central content store, backups, everything verifiable by hash | Public GitHub log with independent mirrors |
| Next | Copies of public content on decentralized storage (for example IPFS with pinning by OSO and volunteers); any copy is valid if it matches the hash | More independent mirrors and replays |
| M3 | Decentralized copies become primary; a permanent-storage network for key works; the central store becomes one copy among many | Smart contracts on a layer-2 chain (OIP-13 section 8) |

A later OIP will specify replication, pinning incentives and who pays. Private data (accounts, keys) is never put on decentralized storage.

**Before Phase B:** choose where the node, database and content store are hosted; set the file size limit; set up encrypted backups; and publish a privacy policy, since storing emails brings data-protection duties (for example for users in the EU).
