# setup-taplo

Setup [Taplo](https://taplo.tamasfe.dev/) in GitHub Actions.

## Usage

```yaml
- uses: uncenter/setup-taplo@v1.0.8
  with:
    version: "0.8.1"
```

## Examples

```yaml
name: Check formatting

on:
  push:

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: uncenter/setup-taplo@v1.0.8
        with:
          version: "0.8.1"

      - run: taplo fmt foo.toml --check
```

## License

[MIT](LICENSE)

## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
