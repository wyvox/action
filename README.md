# `wyvox/action`

The GitHub action that does all setup for pnpm projects.
- checkout
- node
- volta support (without the volta action)
- pnpm + cache
- local turborepo server



## Usage:

```yaml
steps:
  - uses: wyvox/action@v1
```

See action.yml for options

# Pnpm and Cache

This is managed by [wyvox/action-setup-pnpm](https://github.com/wyvox/action-setup-pnpm/)

This respects volta entries in package.json.

## Turborepo

Turborepo is not required, and not used for anything by default.

This is managed by [felixmosh/turborepo-gh-artifacts](https://github.com/felixmosh/turborepo-gh-artifacts)

To enable the turborepo server, add these 3 environment variables to the workflow:
```yaml
env:
  TURBO_API: http://127.0.0.1:9080
  TURBO_TOKEN: this-is-not-a-secret
  TURBO_TEAM: myself
```

Since the turbo server only runs locally, the token and team don't matter, but need to be specified as the turbo server does not expect to be ran in a github action context.

This saves from working with an external turbo cache provider. By using turbo in GH actions, we save on network and download time.


## Ejecting

`wyvox/action` doesn't do a whole lot, but when managing many repos with the same configuration, the number of lines adds up.
This below ejected config does not convey all behavior, but it is the base behavior.

```yaml 
env:
  TURBO_API: http://127.0.0.1:9080
  TURBO_TOKEN: this-is-not-a-secret
  TURBO_TEAM: myself

# ...

  steps:
    - name: 'Checkout repository'
      uses: actions/checkout@v3

    - name: 'Setup local TurboRepo server'
      uses: felixmosh/turborepo-gh-artifacts@v2
      with:
        repo-token: ${{ secrets.GITHUB_TOKEN }}

    - name: 'Setup dependencies and cache'
      uses: wyvox/action-setup-pnpm@v2
```
