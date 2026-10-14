---
name: llm-app-testing
description: Test LLM/AI features and agents with deterministic mocks, eval datasets, rubric/assertion checks, streaming UI tests, and safety/prompt-injection probes.
---
# Testing LLM and AI apps
1. Split the layers:
   - Deterministic code (prompt building, parsing, tool routing): ordinary unit tests with the LLM mocked (e.g. pytest-mockllm or respx/VCR-style fixtures).
   - Model behavior: evals.
   - UI: streaming and latency.
2. Build an eval set of 20–200 real-ish inputs with expected properties (not exact strings). Version it in the repo.
3. Assertion types, from cheapest to most expensive:
   - schema/JSON validity
   - contains or does-not-contain
   - regex
   - tool-call correctness
   - embedding similarity
   - LLM-as-judge with a written rubric (spot-check it against human labels)
4. Tools: promptfoo, DeepEval, Ragas (for RAG: faithfulness, context precision), or plain pytest with parametrized cases.
5. Non-determinism: set temperature 0 where possible, run each case N times, report the pass rate, and gate on a threshold. Pin the model version in config.
6. RAG: test retrieval separately (is the right document in the top-k?) from generation (is the answer grounded?).
7. Safety: run prompt-injection probes in the inputs and in retrieved docs, test for PII leakage, check jailbreak refusals, and make sure tools cannot do unsafe actions without confirmation.
8. Streaming UI: wait for stream completion rather than fixed sleeps, and measure time-to-first-token and total time.
9. Agents: assert on the trace (tools called, order, final state), not just the final text. Add cost and step budgets.
10. Report: pass rate per category, regressions vs the last model or prompt, and examples of failures.
