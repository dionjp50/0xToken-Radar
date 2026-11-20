# 0xToken-Radar

A token launch radar built for make.fun's token-radar-01 bounty. It reads the
sealed DefiLlama snapshot, finds every protocol that records no token yet, and
ranks them by an implied launch value derived only from the snapshot's own
figures — each row with a concrete farm plan.

## How it works

- `main.ts` (Node 22, standard library only) reads `/input/snapshot.json`,
  writes `/output/board.json`, writes the dashboard's `data.js`, copies
  `dashboard/` and `screenshots/` to `/output`, and writes `dossier.json`
  with the vendored `vendor/makefun-board.ts`.
- `lib/model.ts` is the model (radar-2). The base multiple is not a constant:
  for each category it is the median market-cap-to-TVL ratio of that
  category's live-token protocols in the same snapshot, so the market's own
  pricing calibrates what a pre-token launch would be worth. Momentum
  (7-day TVL change), maturity (how long the protocol has run with no token
  on record) and chain breadth adjust it. No protocol name is special-cased.
- `dashboard/` is a static page with no network access: the ranked board, a
  detail view per protocol with the full factor breakdown and farm plan,
  filters, and a model sandbox that re-ranks the board live.

Run it locally:

    mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>

Implied value is a model, not a promise. A protocol on the board records no
token in the snapshot; that is DefiLlama's record, not proof it has none.
