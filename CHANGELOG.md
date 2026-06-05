# Changelog

All notable changes to this project are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Lint and validate CI workflow (`.github/workflows/ci.yml`). It runs `uv sync`, validates the notebook JSON, and compile-checks the notebook without executing it.
- Issue forms (bug report, question) and a pull request template under `.github/`.
- `CODE_OF_CONDUCT.md`, `SECURITY.md`, and this `CHANGELOG.md`.

## [1.0.0] - 2026-05-28

### Added
- Initial public release of the LangGraph cache-aware RAG agent lesson.
- `src/cache_aware_rag.ipynb`: a tool-using ReAct agent over a local FAISS index of the Apollo corpus plus live Wikipedia search, with `CachePolicy` attached to the `tools_fetch` node.
- Committed FAISS store (`src/faiss_store/`) so re-runs skip the Wikipedia embed step.
- Uncached vs cached benchmark and a verification section that asserts the cached path is no slower than uncached and that the answers stay similar.
- README, CONTRIBUTING, and MIT license.

[Unreleased]: https://github.com/sheldonlsides/langgraph-cachepolicy-rag-agent/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/sheldonlsides/langgraph-cachepolicy-rag-agent/releases/tag/v1.0.0
