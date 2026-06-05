# Pull request

## What changed and why

<!-- Describe the change and the reason for it. Keep the scope tight: this is a single-lesson tutorial, not a framework. -->

## Checklist

- [ ] I ran the notebook end to end locally and the `## 10. Verify the cache claim` cell passes:
      `uv run jupyter nbconvert --to notebook --execute --inplace src/cache_aware_rag.ipynb`
- [ ] Committed notebook outputs are intentional, not stray debug prints.
- [ ] I did not cache the LLM (`agent`) node or `tools_wrap`, and did not change the `CachePolicy` topology unless that is the point of this PR.
- [ ] Imports stay on the post-1.0 LangChain / LangGraph API surface (no downgraded imports).
