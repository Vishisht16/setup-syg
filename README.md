# setup-syg

Installs [Syg](https://syg.vishisht.tech) in a GitHub Actions job and puts it
on `PATH`, on Linux, macOS and Windows runners.

```yaml
steps:
  - uses: actions/checkout@v7
  - uses: vishisht16/setup-syg@v1
  - run: syg check --format github
```

`--format github` turns every finding into an annotation on the pull request's
diff, at the line it is about. It works in any repository.

To upload findings to code scanning instead, the job needs permission to
write security events, and the upload runs even when the check fails:

```yaml
permissions:
  contents: read
  security-events: write
steps:
  - uses: actions/checkout@v7
  - uses: vishisht16/setup-syg@v1
  - run: syg check --format sarif > syg.sarif
  - uses: github/codeql-action/upload-sarif@v4
    if: always()
    with:
      sarif_file: syg.sarif
```

Code scanning is free for public repositories. A private repository needs
GitHub Code Security for it; without it, the upload is refused.

## Inputs

| Input | Default | |
|---|---|---|
| `version` | the latest release | a version to pin, e.g. `1.0.0` |
| `telemetry` | `true` | `false` turns off Syg's anonymous usage statistics for the job ([what they are](https://syg.vishisht.tech/privacy)) |

The installer verifies every download against the release's published
checksums.
