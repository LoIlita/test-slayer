# 50 distinct benchmarks for small base models — method (v0.3.1)

This document describes how the 50-benchmark panel is selected, sampled and scored. It contains no questions, answers or model outputs. Machine-readable definitions:

- `tasks_50.json` — the 50 benchmarks, one primary protocol each (language, metric, shots, split).
- `benchmark_sources.json` — exact task definitions, group members, datasets, pinned revisions and observed licence tags.
- `results.schema.json` — the results format.
- `data_access.json` — parquet access for script-only datasets (section 8).

## 1. What counts as one benchmark

One benchmark is one named, source-identified collection of tasks, not one YAML file. Splits, rounds, prompt formats, number of shots and translations do not add benchmarks: ARC, ANLI and PoLEMo2 count once; MMLU (57 subjects), BLiMP (67 paradigms), MATH (7) and BBH (27) count once each. GLUE/SuperGLUE are not counted on top of their component datasets. Datasets that reuse another selected benchmark's question pool (QNLI vs SQuAD, SWAG vs HellaSwag, MMLU-Pro vs MMLU, MATH-500 vs MATH) were left out.

Result: 50 benchmarks, 38 English and 12 Polish, from 44 dataset repositories; 204 source tasks underneath. Fifty distinct collections are not fifty independent pieces of evidence.

Not included: code execution benchmarks (need a separate sandbox) and agent/tool-use benchmarks.

## 2. Harness and runtime

- Evaluation harness: EleutherAI lm-evaluation-harness, English tasks at commit `6d2abda4fd171e68a8789330c4149e37c1ca0bda`; Polish tasks from the OpenPL fork at `21d0ea9cf4fd6153dfff4d84d6ad0aab5488f302`. Separate environments, hash-locked dependencies.
- Settings: float32, batch size 1, maximum context 4096 tokens (longer inputs are rejected, never truncated), no chat template, greedy decoding. BOS follows each model's own tokenizer convention (for Hugging Face models `add_bos_token` from the pinned tokenizer files; recorded per run), with exactly one leading BOS per request; for empty contexts and rolling perplexity the BOS is an unscored prefix. This avoids the duplicate scored BOS that the pinned harness produces in those two paths when `add_bos_token` is true. Harness seeds (python, numpy, torch, fewshot) = (0, 1234, 1234, 1234).
- Offline: no network during evaluation; datasets, metric modules and NLTK data are pinned and staged before the run. Dataset loading scripts are never executed (see section 8).
- Before any benchmark is scored on a model, the runtime is calibrated on that model against the original harness forward: summed log-likelihood difference per target token ≤ 2e-5, full-vocabulary logits bitwise equal, rolling windows count each token once, greedy generation identical.

## 3. Unit and sampling (panel200)

- Unit = one source document after the task's own `process_docs`. Identity = benchmark, source task, and the document's native id or, if none, the SHA-256 of the canonical document; byte-identical documents without an id get an ordinal in pinned file order (section 8).
- Per benchmark, `min(200, N_eligible)` documents are selected before any model is run, by SHA-256 of `20261028|benchmark_id|member_task|source_id`. All models are scored on the same documents.
- Groups: the 200 slots are per benchmark, not per member. Every member gets `min(2, N_i)` documents first; remaining slots go to the member with the largest deficit against `budget × source_weight`. BLiMP members are weighted equally; MMLU, MATH and BBH proportionally to member population. Member scores are re-aggregated with these fixed weights.
- WikiText-2 is scored on the full test stream with byte perplexity.
- `fullB` is a separate full-split run of the same primary protocols, without the 200 cap.

## 4. Demonstrations and generation

- Number of shots as listed per benchmark (37 zero-shot, 12 with 5 shots, BBH with its source 3-shot prompts). Demonstrations come from a disjoint bank selected by hash; a demonstration never appears among scored documents. Fixed source prompts (BBH) are kept as published.
- Generation budget from the task definition (default 256 tokens; IFEval 1280). If prompt + budget exceed 4096, optional demonstrations are removed from the end; questions, options and references are never cut.
- Source task filters are applied unchanged (e.g. `remove_whitespace` for NQ-Open and TriviaQA).
- BBH: the generated answer is stripped of leading and trailing whitespace before exact match. Without this, byte-level BPE tokenizers keep the leading space of the first token and every answer fails, while SentencePiece tokenizers drop it on decoding; the source-literal value is kept as a diagnostic. This correction was introduced after the first run, once the zero score was traced to whitespace; it applies identically to all models.
- MATH: the 0-shot source extractor takes the text between the first and last `$` of the answer and does not read `\boxed{}`. Scores are reported exactly as the source gives them, and MATH is not used to rank models. A re-scoring by the last `\boxed{}`, made after the first run when both models' outputs were seen, is shown only as a labelled diagnostic and never as the MATH score.
- IFEval: `langdetect` is seeded (`DetectorFactory.seed = 0`) for reproducibility. It affects only IFEval's language instructions.

## 5. Metrics and reporting

- One pre-declared primary metric per benchmark, in its source unit and scale (accuracy, F1, exact match, Levenshtein similarity, MCC, byte perplexity, IFEval prompt-level strict accuracy, …). MCC may be negative. There is **no average over the 50 benchmarks**.
- Uncertainty: 95% bootstrap confidence interval, 10 000 replicates, seed 20261028, resampling within group members with fixed weights. Byte-identical documents are resampled together as one cluster. For model-vs-model contrasts: paired bootstrap and a two-sided paired permutation test (10 000 swaps, +1 correction) with Holm correction over all pre-declared contrasts. When a contrast has at most 20 clusters with a non-zero difference, the permutation p-value is computed exactly; otherwise rows whose Holm-adjusted p falls between 0.02 and 0.10 are recomputed with 200 000 swaps. This precision rule was added after the first computation, when Monte Carlo error alone decided one verdict; it applies to every contrast and can add or remove significance. Each contrast states the items it was computed on (`subset`: the full frozen sample, or a hit-free subset defined by our model's training corpus and identical for both models) and its n; hit-free variants carry their own 95% CI.

## 6. Contamination

- For our own models the full training corpus is scanned against every scored split before results are published: each scored text field is checked separately: fields of 13+ words by word 13-grams after normalisation (as in GPT-3); otherwise whole fields of 3–12 words (at least 60% of the item's longest field) by exact normalised match; categorical fields (labels, ids) are excluded; fields stored as serialized lists or dicts are parsed and their parts checked as separate fields; bare numbers are ignored; items with no field of 3+ words are unscannable. Positive and negative controls are mandatory.
- **Reference values per result:** expected value of uniform random choice on the frozen sample, value of always predicting the majority gold class (fixed-class tasks only), share of the model's most frequent prediction, and a constant-prediction flag when the model gives one class for every item. A constant-prediction result is a real measurement but carries no information about ability and is not used for comparisons.
- **Contamination by field role:** for flagged benchmarks, hit-free values are reported twice: without any hit, and without hits in answer-bearing fields (question, options, target); context-only hits (e.g. a Wikipedia passage) indicate exposure to the source text, not to the question and label.
- States per (model, benchmark): `clean`; `flagged` (reported on the full sample and on the hit-free subset, with hit rate; when no item of the sample is hit-free, the hit-free value is empty with n = 0 — this can happen for passage-based sets (e.g. built on Wikipedia); then only the answer-field variant can carry a number); `partially_scanned` (more than 5% unscannable); `unscanned` (not published); `unknown` (third-party model whose training data cannot be scanned — shown with the label "training data unknown").

## 7. Data use

Evaluation only. Public outputs are limited to benchmark id, revision, licence, aggregate result, n and CI95. Questions, answers, few-shot examples and per-item outputs are not redistributed, and nothing from these datasets is used for training. Users fetch the data themselves from the pinned revisions under each dataset's own licence.

## 8. Clarifications in v0.3.1

- **Script-only datasets** (PIQA, WSC273, LogiQA, MathQA, PubMedQA, Social IQa, PoQuAD, PolQA): benchmark identity stays at the pinned revision; data are read from the Hugging Face auto-converted parquet at the listed `refs/convert/parquet` commit. Columns must match what the task reads, otherwise the benchmark is blocked. PubMedQA additionally requires the parquet test split to equal the labeled test set of the pinned revision by id and label.
- **Duplicate documents** without a native id: identity = document hash + ordinal in pinned file order; bootstrap resamples identical documents as one cluster; counts are reported.
- **Likelihood vs generation:** 39 benchmarks are scored by likelihood or perplexity, 11 by generation (PolQA, PoQuAD, GSM8K, MATH, IFEval, SQuAD 2.0, DROP, CoQA, NQ-Open, TriviaQA, BBH). They may be run in separate windows; a benchmark not yet run is `not_run`.

- **Public results file:** exported from qualifying runs only (runner and runtime implement every rule above). Values, CIs and digests are copied unchanged, and the sources are listed by sha256 in `provenance`. Our own models' results without a completed contamination scan are published as `skipped` without a value.
