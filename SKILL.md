---
name: eval-driven-iteration
description: "Evidence-first workflow for evaluating and iterating AI, Agent, RAG, Text2SQL, retrieval, and classification systems."
---

# Eval-Driven Iteration

Use this skill whenever an AI system's metrics change or someone proposes changing a prompt, model, retrieval strategy, threshold, or tool chain.

## Workflow

Baseline → capture per-case run records → classify failures by stage → state a falsifiable hypothesis → make one minimal general change → regress known failures → regress known successes → test independent samples → run the full benchmark.

## Rules

- Diagnose before tuning; a low score is not a root cause.
- Keep structured run records for inputs, stage outputs, timings, errors, and final labels.
- Separate retrieval, association, ranking, generation, validation/security, execution, presentation, and infrastructure failures.
- Exclude infrastructure failures from capability denominators and report them separately.
- Change one causal variable at a time; prefer general rules over case-specific patches.
- Compare identical cases, environments, configurations, and denominators. Show percentages with counts.
- Validate targeted failures and previously successful cases before claiming a fix.
- Treat small samples as directional evidence only.
- Use positive and negative controls when a failure pattern looks suspiciously uniform.

## Idempotency under randomness

Networks, service queues, and LLM sampling can change timing, token usage, and retry counts. Repeated runs must preserve the input/configuration snapshot, per-case final labels, successful standardized result sets, and metric definitions. Timing, P50/P95 latency, token usage, retries, backoff waits, and connection latency may vary and must be reported separately. Any changed final label or result set requires per-case diagnosis.

## Reporting and stop conditions

Record the phenomenon, evidence, failure class, falsifiable hypothesis, minimal fix, unaffected scope, failure/success regressions, independent-sample check, and readiness for full evaluation. Stop when evidence is insufficient, samples are not comparable, infrastructure failures exceed the agreed threshold, the budget is exceeded, or the proposed change cannot be isolated.
