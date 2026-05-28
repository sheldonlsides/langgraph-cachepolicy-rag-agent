# Contributing

Thanks for your interest in this tutorial repo. It's a single-lesson project; changes should sharpen the lesson, not generalize it into a framework.

## Ground rules

- All tutorial logic lives in one file, the notebook (`src/cache_aware_rag.ipynb`): tool functions, agent node, graph topology, and the verification cell at the end. Keep the verification cell in step with anything you change upstream.
- The `agent` node (the LLM) is intentionally **not** cached. Don't "fix" that.
- The cached graph splits the tools step into `tools_fetch` (cached) and `tools_wrap` (uncached). Don't merge them or attach `CachePolicy` to `tools_wrap`; the wrap step depends on the live `tool_call_id`s and replaying a cached version produces OpenAI 400s on the next agent step.
- The `tools_fetch` node uses a custom `key_func` that hashes on `(tool_name, normalized_args)` only. Don't replace it with the default key (the full state) or you'll silently destroy the cache hit rate.
- `parallel_tool_calls=False` on `bind_tools` is a cache-stability requirement. Don't flip it on.
- Library pins are post-1.0 (`langgraph >= 1.2.0`, `langchain >= 1.3.1`). Don't downgrade imports.

## Dev setup

```bash
uv sync
cp .env.example .env   # add your OPENAI_API_KEY
```

## Verifying changes

The notebook's final cell (`## 11. Verify the cache claim`) is the verification pass. It hits the OpenAI API and Wikipedia (cost is small but non-zero) and asserts two things:

- **The cached path is not materially slower than uncached.** The cached-agent median latency across three warm runs must be within `NOISE_MARGIN_S = 1.0` second of the uncached run. This catches the cache-killer regressions (flipping `parallel_tool_calls=True`, dropping the custom `key_func`, accidentally caching the LLM) without failing on normal LLM-call variance.
- **Answer similarity stays at or above `0.6`** by `SequenceMatcher` ratio between the cached and uncached final answers.

The cell prints the actual speedup ratio for reference but does not assert on it; the magnitude is dominated by `SIMULATED_TOOL_LATENCY_S` and, in production, by your real tool latency.

Run it headless the same way CI does:

```bash
uv run jupyter nbconvert --to notebook --execute --inplace src/cache_aware_rag.ipynb
```

`nbconvert` exits non-zero if any cell raises, so the verification cell's `assert` statements are the pass/fail gate.

## Pull requests

- Keep PRs focused. A notebook tweak, a doc fix, and a dependency bump are three separate PRs.
- If you change a tool, the agent node, or the graph topology, update the notebook's verification cell in the same PR so the assertion still reflects reality.
- If you change cache semantics (TTL, key_func, what node the policy is attached to), explain why in the PR description and update the corresponding notebook narrative.

## Reporting issues

Please include:
- Python version (`python --version`)
- `uv pip list` output (or the diff vs. `uv.lock`)
- Whether you're on local or Colab
- The full traceback, not just the last line

## License

By contributing you agree that your contributions will be licensed under the [MIT License](LICENSE).
