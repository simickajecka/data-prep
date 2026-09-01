# data-prep

Data layer of the Serbian real-estate price-prediction project: collection,
the schema contract, and typed storage. Produces the deduplicated dataset that
`modeling` consumes.

| submodule | concern | artifact |
| --- | --- | --- |
| `llm-benchmark` | the schema contract (attribute ontology) | `rule_cards_tree.json` |
| `listings-harvest` | collection from halooglasi + nekretnine | upserts into the DB |
| `listings-data-layer` | typed storage, dedup, export | `listings.db`, `listings.parquet` |
| `prototypes` | superseded attempts, kept for reference | nothing — frozen |

```
rule_cards_tree.json ──build_schema──► schema/ ──┐
      (the contract)                             │  (typed DDL + dtypes)
                                                 ▼
   listings-harvest ─live upsert─► listings.db ──dedup──► listings.parquet
                                   (typed store)      (canonical rows → modeling)
```

`prototypes` is an archive: four earlier takes on attribute extraction that
preceded `llm-benchmark`. Nothing in it runs, and nothing depends on it.

`listings-harvest` imports the write path from `listings-data-layer`, so
coerce → upsert → lifecycle is defined once and stays identical whether rows
arrive live from the scraper or from a CSV.

## Clone

```bash
git clone --recurse-submodules https://github.com/simickajecka/data-prep.git
```

Already cloned:

```bash
git submodule update --init --recursive
```
