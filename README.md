# codeql-nix-action

GitHub Action wrapping [codeql-nix](https://github.com/JamieMagee/codeql-nix)
— scan `.nix` source files with CodeQL and upload SARIF to GitHub code
scanning.

## Usage

```yaml
name: codeql-nix
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: "0 6 * * 1"

permissions:
  contents: read
  security-events: write

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: JamieMagee/codeql-nix-action@v0
        with:
          source-root: .         # default
          version: latest        # or a specific `v0.1.0`
          category: codeql-nix   # SARIF category in the Security tab
```

Findings appear in the **Security → Code scanning** tab of the scanned
repository.

> [!IMPORTANT]
> This action downloads the CodeQL CLI bundle from `github/codeql-action`
> at runtime. The download is governed by GitHub's standard
> [CodeQL Terms & Conditions](https://github.com/github/codeql-cli-binaries/blob/main/LICENSE.md),
> which permit use in GitHub Actions workflows.

## Inputs

| Input | Default | Description |
|---|---|---|
| `source-root` | `.` | Directory containing the `.nix` files to scan. |
| `query-suite` | `code-scanning` | Either `code-scanning` for the curated suite, or a path to a `.qls` / `.ql` file inside the workflow checkout. |
| `category` | `codeql-nix` | SARIF category — distinguishes this analysis in the Security tab if you upload multiple. |
| `version` | `latest` | `vX.Y.Z` tag of [codeql-nix](https://github.com/JamieMagee/codeql-nix/releases) to use, or `latest`. |
| `upload` | `true` | If `false`, skips the SARIF upload step. The SARIF is still produced at the `output` path. |
| `output` | `codeql-nix-results.sarif` | Path to write the produced SARIF file to. |

## Outputs

| Output | Description |
|---|---|
| `sarif-file` | Path of the produced SARIF file. |

## Runner support

Linux x86-64 only for v0. macOS and Windows runners are not yet
supported — the pre-built extractor pack is currently linux64-only.

## License

MIT — see [LICENSE](./LICENSE).
