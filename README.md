# Ruby gem release

Release gems with [Rubygem's trusted publishing](https://guides.rubygems.org/trusted-publishing/) using minimal tools.
Unlike [rubygems/release-gem](https://github.com/rubygems/release-gem) this uses plain tools which avoids the need to install a whole bundler environment.

It assumes you have a single gemspec file in your repository root.

## Example usage

This assumes you push a tag and it releases to Rubygems. It also uses the `release` environment, which is optional but recommended.

```yaml
---
name: Gem Release

on:
  push:
    tags:
      - '*'

jobs:
  release:
    name: Release gem
    runs-on: ubuntu-latest
    # Optional but recommended to use a specific environment
    environment: release
    # Prevent releases from forked repositories
    if: github.repository_owner == 'MyOwner'

    permissions:
      id-token: write

    steps:
      - uses: voxpupuli/ruby-release@v0
```
