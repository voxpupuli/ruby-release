# Ruby gem release

Release gems with [Rubygem's trusted publishing](https://guides.rubygems.org/trusted-publishing/) using minimal tools.
Unlike [rubygems/release-gem](https://github.com/rubygems/release-gem) this uses plain tools which avoids the need to install a whole bundler environment.

It assumes you have a single gemspec file in your repository root.

## Composite action usage

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

## Reusable workflow usage

If you want a release pipeline with separated permissions, GitHub release creation,
GitHub Packages publishing, and RubyGems trusted publishing in dedicated jobs,
use the reusable workflow instead of the composite action:

```yaml
---
name: Gem Release

on:
  push:
    tags:
      - '*'

permissions: {}

jobs:
  release:
    uses: voxpupuli/ruby-release/.github/workflows/gem-release.yml@v0
    with:
      allowed_owner: 'MyOwner'
      environment: 'release'
```

### Inputs

- `allowed_owner`: repository owner allowed to publish. This blocks releases from forks.
- `environment`: GitHub environment used for RubyGems trusted publishing. Defaults to `release`.
- `ruby_version`: Ruby version used to build and verify the gem. Defaults to `ruby`.
- `publish_github_packages`: whether to also publish to GitHub Packages. Defaults to `true`.

### When to use what

- Use the composite action when you only need a small direct RubyGems release job.
- Use the reusable workflow when you want:
  - minimal permissions per job
  - GitHub release creation
  - optional GitHub Packages publishing
  - artifact handoff between build and publish steps
  - a structure that is easy to roll out through modulesync
