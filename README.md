# Eval-Driven Iteration

> **When an AI metric drops, do not tune first. Diagnose first.**

An evidence-first workflow for teams building and evaluating AI, Agent, RAG, and data-query systems.

It helps you distinguish:

- model or prompt failures
- retrieval and ranking failures
- evaluation-logic mistakes
- infrastructure and availability failures

## Why this is worth using

This method grew out of repeated real-world AI evaluation cycles where a single score could not explain whether a change helped. It turns that experience into a portable discipline: establish a baseline, preserve per-request evidence, form falsifiable hypotheses, make one minimal change, and verify the result before scaling up.

## Three principles

1. **Evidence before intuition.** A low score is a symptom, not a root cause.
2. **One change, one hypothesis.** Keep improvements attributable and reversible.
3. **Protect the denominator.** Align cases and configurations; report infrastructure failures separately.

## A quick look

**Before:** A team sees a high failure percentage and immediately changes the model prompt.

**After:** The team checks the per-request record, finds that the model output was correct but the evaluator misclassified a downstream error, fixes the evaluator, and only then reassesses model quality.

Try the self-contained examples (no API key or database required):

```bash
git clone https://github.com/sheily899/eval-driven-iteration.git
cd eval-driven-iteration
open examples/
```

For Chinese documentation, see [`README.zh-CN.md`](README.zh-CN.md).

## How to use it

After installation, you usually do not need a special command. Mention the evaluation task and ask the agent to apply the skill explicitly:

```text
Use the eval-driven-iteration skill for this evaluation. First inspect the baseline and per-case evidence, classify failures by stage, and state a falsifiable hypothesis. Do not change code yet. Then propose the smallest isolated fix and a regression plan covering known failures, known successes, and independent samples.
```

Useful task templates:

```text
Use eval-driven-iteration to diagnose why metric X changed from A/B to C/D. Compare the same cases and denominator, separate infrastructure failures, and produce an evidence-based attribution report before suggesting a fix.
```

```text
Use eval-driven-iteration to compare version A and version B. Do not tune either version. Check configuration parity, per-case outcomes, failure categories, and whether the difference is statistically meaningful for this sample size.
```

```text
Use eval-driven-iteration before changing this prompt/retriever. Identify the failing stage, write one falsifiable hypothesis, specify what must not regress, and design the smallest known-failure/known-success validation.
```

For Claude.ai, upload or attach `SKILL.md` to the conversation if custom Skills are unavailable. For Claude Code and Codex, install the directory under the tool's skills path and start a new session; tasks matching the description will trigger it automatically. You can always force it with the phrase “use eval-driven-iteration”.

## Core workflow

Baseline → capture a structured record for each case → classify failures by stage → state a falsifiable hypothesis → make one minimal fix → regress known failures → regress known successes → test independent samples → run the full benchmark.

The agent-facing specification is [`SKILL.md`](SKILL.md). A Chinese reference is available in [`SKILL.zh-CN.md`](SKILL.zh-CN.md).

## How it differs from ad-hoc evaluation

| Ad-hoc evaluation | Eval-Driven Iteration |
|---|---|
| Tune the prompt when a metric drops | Locate the failing stage first |
| Re-run only failed cases | Regress known successes as well |
| Compare aggregate scores directly | Align cases, environments, and denominators |
| Count timeouts as capability failures | Report `infra_invalid` separately |
| Change several parameters at once | Test one falsifiable hypothesis at a time |
| Keep only aggregate percentages | Preserve per-case records and evidence |
| Optimize only for a higher score | Allow “stop tuning” or “insufficient evidence” |

## Installation

### Claude.ai / Claude Code

Copy the repository into Claude's skills directory so the path is:

```text
<skills-directory>/eval-driven-iteration/SKILL.md
```

Restart or refresh Claude Code after installation.

### Codex

Copy the repository into the Codex user skills directory:

```text
<CODEX_HOME>/skills/eval-driven-iteration/SKILL.md
```

On Windows this is commonly `%USERPROFILE%\\.codex\\skills\\eval-driven-iteration\\SKILL.md`. Reopen Codex after installation.

## Scope

This workflow is for evaluating and iterating AI/Agent/RAG/Text2SQL systems; it is not a requirement for ordinary unit tests. Under network and model randomness, final labels and standardized result sets are the idempotency core; latency, token usage, and retries should be reported separately and may vary.

## Examples

Sanitized examples are in [`examples/`](examples/):

- [`routing-attribution.md`](examples/routing-attribution.md)
- [`candidate-noise.md`](examples/candidate-noise.md)

## License

Released under the [MIT License](LICENSE).
