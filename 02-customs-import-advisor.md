# AI Customs Import Advisor — [customs.educore.software](https://customs.educore.software)

**Kind:** an assistant for importers in Egypt — "I want to import product X: am I allowed, and what do I need?"
**Role:** everything — data collection, knowledge base, agent loop, evaluation, deployment

## The idea
The importer describes the product in everyday Arabic (dialect included) and gets the 10-digit HS code,
the authorities that must approve, any ban or condition, duties and taxes, and the documents needed —
**from official sources only, with the source named**.

## Answers that are sourced, not guessed
The model does not answer from memory. Every code, approval, ban and rate in an answer must have come
back from a tool call in the same conversation, and the server checks that **before** saving the answer:

```
question ─► search_tariff ─► candidate codes (normalised Arabic + trigram + dialect synonyms)
            get_hs_details ─► the code, its parents, duties, official notes, importer conditions
            search_regulations ─► passages from laws, decrees and circulars
answer ─► citation check: a code no tool returned → one rewrite, otherwise "not determined"
       ─► verdict check: "allowed with conditions" must rest on real notes or rules
```

## Knowledge base (live numbers)
| Layer | Content | Count |
|---|---|---|
| Tariff | product → code | 15,767 lines |
| Official notes | bans, approvals, ports | 411 |
| Importer conditions | registry, ACI, food safety… | 1,092 rules |
| Regulations | laws and interpretations | 5,265 documents / 22,150 passages |

Polite crawlers (one request at a time per site, 2.5–3 s apart, back-off on 429/5xx, resumable),
and extracted rules start "unverified" until a specialist approves them in the admin.

## Evaluation, not impressions
A development set and a separate **holdout** set never used for tuning:

| Metric (holdout, 20 questions) | |
|---|---|
| Correct HS code | 94% |
| Correct verdict (allowed / conditional / banned) | 88% |
| Answers fully sourced from tools | **100%** |
| Same answer to rephrasings of the question | 100% |

Several models were compared on the same set before choosing the final setup.

`Python` `Django` `PostgreSQL full-text + pg_trgm` `Tool calling` `SSE` `Docker`
