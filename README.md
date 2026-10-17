# setup-syg

Installs [Syg](https://syg.vishisht.tech) in a GitHub Actions job and puts it
on `PATH`, on Linux, macOS and Windows runners.

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: vishisht16/setup-syg@v1
  - run: syg check --format github
```

`--format github` turns every finding into an annotation on the pull request's
diff, at the line it is about. To upload findings to code scanning instead:

```yaml
  - run: syg check --format sarif > syg.sarif
  - uses: github/codeql-action/upload-sarif@v3
    with:
      sarif_file: syg.sarif
```

## Inputs

| Input | Default | |
|---|---|---|
| `version` | the latest release | a version to pin, e.g. `1.0.0` |
| `telemetry` | `true` | `false` turns off Syg's anonymous usage statistics for the job ([what they are](https://syg.vishisht.tech/privacy)) |

The installer verifies every download against the release's published
checksums.
