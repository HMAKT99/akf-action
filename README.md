# AKF Certify — GitHub Action

The AI native file format. Trust certification for your CI pipeline.

Every PR gets a trust report — trust scores, provenance, compliance status.

## Usage

```yaml
name: AKF Trust Certification
on: [pull_request]

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

## What It Does

1. Scans files for AKF trust metadata
2. Validates trust scores against your threshold
3. Posts a certification report as a PR comment
4. Fails the workflow if files don't meet trust standards

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `paths` | `.` | File or directory to certify |
| `min-trust` | `0.7` | Minimum trust score (0.0–1.0) |
| `fail-on-untrusted` | `true` | Fail if any file isn't certified |
| `evidence-file` | — | Path to JUnit XML or JSON evidence |
| `format` | `markdown` | Output: summary, json, markdown |
| `post-comment` | `true` | Post results as PR comment |

## Links

- [AKF — The AI Native File Format](https://akf.dev)
- [GitHub](https://github.com/HMAKT99/AKF)
- [npm](https://www.npmjs.com/package/akf-format)
