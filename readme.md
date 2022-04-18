# Architect Build Action

![Architect logo](https://github.com/architect/assets.arc.codes/raw/main/public/architect-logo-light-500b%402x.png#gh-dark-mode-only)
![Architect logo](https://github.com/architect/assets.arc.codes/raw/main/public/architect-logo-500b%402x.png#gh-light-mode-only)

This is a GitHub Action that builds an architect application and runs the automated tests.

## How does it work?

When called the action will:
- checkout the project
- set up node.js v14
- installs dependencies (works with npm, pnpm and yarn)
- runs `npm run vendor` if present
- runs `arc hydrate`
- runs `npm test`

## Usage

Typically, you will want to add this action as the first step in a workflow. Then if the tests pass you can send a message to Discord or Slack.

For example:

```yaml
jobs:
  # Test the build
  build:
    # Setup
    runs-on: ubuntu-latest

    # Go
    steps:
      - name: Build App
        uses: architect/action-build@v3

      - name: Notify
        uses: sarisia/actions-status-discord@v1
        # Only fire alert once
        if: github.ref == 'refs/heads/main' && failure() && matrix.node-version == '14.x' && matrix.os == 'ubuntu-latest'
        with:
          webhook: ${{ secrets.DISCORD_WEBHOOK }}
          title: "build and test"
          color: 0x222222
          username: GitHub Actions
```

## Contributing

[Find out more about contributing to Architect](https://arc.codes/docs/en/about/contribute)
