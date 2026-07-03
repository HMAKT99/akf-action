# AKF Certify — GitHub Action

Trust certification for AI-generated code in your CI pipeline.

Your agents stamp what they verify (`akf stamp file --evidence "42/42 tests passed"`). This action checks those stamps on every PR and posts a report reviewers can act on — who made each file, what was verified, and whether it clears your trust bar.

## Usage

```yaml
name: AKF Trust Certification
on: [pull_request]

permissions:
  contents: read
  pull-requests: write

jobs:
  certify:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: HMAKT99/akf-action@v1
        with:
          paths: '.'
          min-trust: '0.7'
          fail-on-untrusted: 'true'
          post-comment: 'true'
```

## The PR comment

```
✅ 12/14 files certified · average trust 0.78 · 2 failed

|   | File          | Trust | Evidence               | Issues            |
|---|---------------|-------|------------------------|-------------------|
| ❌ | `auth.py`     | 0.26  | —                      | provenance_gap    |
| ❌ | `utils.py`    | 0.31  | —                      | ai_content_without_review |
| ✅ | `api.py`      | 0.84  | `test_pass`, `human_review` | —            |
| ✅ | `models.py`   | 0.79  | `test_pass`            | —                 |
```

Failures sort first. The evidence column shows what was actually verified — a stamp claiming tests passed scores higher than a bare AI stamp, and a human-reviewed file higher still.

## What it does

1. Scans changed files for AKF trust metadata
2. Computes effective trust (evidence-weighted, decay-aware)
3. Posts the certification report as a PR comment (updates in place on new pushes)
4. Fails the workflow if files don't meet your trust threshold

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `paths` | `.` | File or directory to certify |
| `min-trust` | `0.7` | Minimum trust score (0.0–1.0) |
| `fail-on-untrusted` | `true` | Fail if any file isn't certified |
| `evidence-file` | — | Path to JUnit XML or JSON evidence to attach |
| `format` | `markdown` | Output: summary, json, markdown |
| `post-comment` | `true` | Post results as PR comment |
| `python-version` | `3.11` | Python version to use |
| `akf-version` | `1.5.0` | AKF version to install (pinned for reproducible runs) |

## Works with

Any agent that stamps its work — Claude Code, Cursor, Copilot, OpenClaw, or any MCP client using [`mcp-server-akf`](https://pypi.org/project/mcp-server-akf/). Run `akf init` in your repo to wire stamping hooks automatically.

## Links

- [AKF — The AI Native File Format](https://akf.dev)
- [GitHub](https://github.com/HMAKT99/AKF)
- [PyPI `akf`](https://pypi.org/project/akf/) · [npm `akf-format`](https://www.npmjs.com/package/akf-format)

MIT licensed.
