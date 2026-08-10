# NQNC Drug Repositioning MCP

A **free** Model Context Protocol (MCP) server that ranks drug-repurposing
candidates by three independent evidence pillars — **mechanism, clinical trials,
and post-market safety** — so researchers can tell a decision-grade candidate
from a mere research lead.

> ⚠️ This tool generates **ranked hypotheses for research prioritization**, not
> medical advice. Every candidate must be validated experimentally and clinically.

## Why

Developing a new drug from scratch averages 10–15 years and $1–2B with a high
failure rate. Repurposing an already-approved, safety-characterized drug for a
new indication is faster and cheaper — but the bottleneck is **ranking** the
hypotheses, not generating them. NQNC returns three *separate* evidence chains
instead of one opaque score.

## Evidence triangle

| Pillar | Tool | What it returns |
|---|---|---|
| Mechanism | `get_mechanism_path` | Biological path drug → target → gene → disease (`gene_shared` or `pathway_mediated`) |
| Efficacy | `get_clinical_trials` | Real co-occurrence of the drug–disease pair in ClinicalTrials.gov (count + phase) |
| Safety | `get_faers_signals` | Post-market adverse-event signal from the FDA FAERS database |

A candidate with a clear mechanism **and** Phase III trial co-occurrence **and** a
manageable safety profile is *decision-grade*; one with only a mechanism path is a
*research lead*. NQNC keeps the three pillars separate so you can tell them apart.

## Quick start

1. Get a free API key at **https://nqnc.com/** (enter your email, key is emailed instantly).
2. Connect any MCP client (Claude Desktop, Cursor, Zed, etc.) with your key:

```json
{
  "mcpServers": {
    "nqnc": {
      "url": "https://nqnc.com/mcp",
      "headers": { "X-API-Key": "YOUR_KEY" }
    }
  }
}
```

3. Ask naturally, e.g. *"Rank repurposable oncology drugs for multiple myeloma by
   mechanism and trial evidence."*

## Example calls

```text
get_mechanism_path(drug="thalidomide", disease="multiple myeloma")
get_clinical_trials(drug="metformin", condition="type 2 diabetes")
get_faers_signals(drug="hydroxychloroquine")
```

## Coverage & honesty

- Direct gene-bridge coverage is ~70% of candidate pairs, ~85% including pathway
  mediation (Reactome + KEGG).
- Disease graph covers 6,039 disease keys. A drug without a mapped gene target, or
  a rare disease absent from the graph, returns **"no path found"** — reported
  plainly, never invented.
- Real ClinicalTrials.gov co-occurrence covers ~11% of candidate pairs; the rest are
  genuine research gaps worth investigating, not negative results.

## Data sources

- Knowledge graph: compound–target, target–gene, disease–gene, gene–pathway edges
- ClinicalTrials.gov API v2 (derived MeSH sections)
- FDA FAERS adverse-event database

## Links

- Platform & free key: https://nqnc.com/
- Tools reference: https://nqnc.com/api.html
- Use cases: https://nqnc.com/use-cases.html
- FAQ: https://nqnc.com/faq.html
- MCP listing: https://smithery.ai/servers/5928301/drug-repositioning

## License

MIT — see [LICENSE](LICENSE).
