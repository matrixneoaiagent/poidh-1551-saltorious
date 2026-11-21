# Recommended draft — exact publish payload

Status: **not published, not signed.** This file is the copy-paste source for `prepare_save_draft` → wallet `personal_sign` → `save_draft`.

> **Validation done (read-only):** the exact title/description/amount below were passed to `prepare_save_draft` on 2026-11-21 ~12:25 UTC. poidh accepted them and returned a `personal_sign / EIP-191` message with a 600 s expiry. **No signature was produced and nothing was published.** This confirms the payload passes poidh’s validation (title 64 chars, description 3,553 chars).

## Fields

| Field | Value |
|---|---|
| `creator` | `0xa15c58e7008Fc7A0331cFcc86A28Af99FD21b93c` (Base; poidh wallet in `knowledge/wallets.md`) |
| `title` | `The Personal Fine-Tune: beat the base model on your own workflow` |
| `amount` | `0.05` ETH (≈ $81 at the bounty’s implied $1,624/ETH; adjustable by the funder) |
| `chain` | `base` |
| `isOpenBounty` | `true` (others can add funds; set `false` for a solo bounty) |
| `album` | `""` |

Title length: 64 characters (limit 200). Description below: ~3.5k characters (limit 20,000).

## `description` (verbatim)

```text
THE PERSONAL FINE-TUNE: beat the base model on your own workflow

Why this exists. On 13 Nov 2026 @saltorious1 wrote: "I'm using and building @tryharness to be the rails for this, for me at least, and cutting fine tunes of local models I have on my data and my specific workflows." (https://x.com/saltorious1/status/2121252954887844185)

That is the step the local-model argument keeps skipping. Everyone compares models. The public, re-runnable version of that experiment — a model trained on your traces, measured against the base model on your work, on hardware you can afford — does not exist yet. This bounty buys that answer.

THE TASK
1. Pick one repetitive task from your own agent workflow with an objective pass/fail scorer: repo-specific edits that must pass tests, tool calls that must validate against a JSON schema and match expected arguments, or structured extraction with exact-match fields. No human "vibes" scoring. The task must be one you actually run at least weekly; state the weekly volume.
2. Collect at least 200 real examples from your own sessions. Freeze 50 held-out tasks BEFORE training (hash + timestamp). Use the rest for training. The held-out set is never used for prompt tuning, model selection, or cherry-picking.
3. Fine-tune one open-weight model up to 32B (QLoRA/LoRA is fine) on a single consumer GPU (24 GB VRAM or less) or 50 USD or less of rented GPU.
4. Score base vs tuned on the same 50 held-out tasks, same prompt template, temperature 0, same scorer. Report pass rate with 95% confidence intervals, cost per passing task, and p50 latency.
5. Publish everything: dataset card and PII scrub method, training config and logs, adapter weights with sha256, eval harness, every raw model output, and a 3-minute unedited video of the tuned model doing 3 held-out tasks in the real workflow.

WHAT COUNTS AS PROOF
- A stranger can run the eval from the repo and reproduce the published pass rates on the published adapter.
- The held-out set hash is committed before training begins.
- Base and tuned are scored by the identical script; all raw outputs are included, not just summaries.
- Every training example is traceable to a real session; no synthetic padding; no test leakage.
- The video is one unedited take showing the task, the command, and the result.

WINNER / ACCEPTANCE
- Positive: at least 10% relative improvement in held-out pass rate over base (or at least 5 points absolute) with a paired 95% CI on the difference that excludes zero (McNemar or bootstrap).
- Negative: a well-run attempt with the same artifacts and a failure analysis is eligible only if no positive entry verifies.
- One winner. Ties go to lower cost per passing task.
- Disqualified: test-set edits after freezing, synthetic data padding, unauditable private data, subjective scoring.

OUT OF SCOPE: frontier-model training, buying GPUs, and "it feels better" evaluations.

ELIGIBILITY: anyone, one entry per person. Code and adapter open-source (MIT or Apache-2.0). If the raw dataset cannot be redistributed, publish its schema, hashes, and a reproducible sampler instead.

TIMELINE: 21 days from funding; judging within 7 days of the deadline.

REWARD: 0.05 ETH on Base.

WHY SALTORIOUS WOULD FUND IT: it is the experiment he says he is already running -- fine-tunes of local models on his own data and workflows -- made public and reproducible. It gives Harness users a recipe, gives him a cost-per-passing-task number against frontier APIs, and replaces the local-vs-frontier food fight with a result anyone can re-run.
```

## MCP calls (sequence)

1. `prepare_save_draft` with the fields above (creator + title + description + amount + chain + isOpenBounty + album).
2. Sign the returned `message` exactly, EIP-191 `personal_sign`, with the creator wallet. No gas. Expires in 600 s.
3. `save_draft` with the same fields + `nonce` + `signature`.
4. Read back `https://poidh.xyz/d/<cid>` and `/data`; confirm the text matches this file before using the link in the claim.

## Notes

- Do **not** edit the message between prepare and sign; any change invalidates the signature.
- If the nonce expires, re-run step 1 and sign again (single-use).
- `amount` is the *proposed* reward; a draft does not escrow or create a bounty.
- If the funder prefers a solo bounty (no contributor voting), set `isOpenBounty: false`; the brief is unchanged.
