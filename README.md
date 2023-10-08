# action-tag-and-releas

This action automates tag creation and moving single digit tag.
 
It also generate a github release with the changelog.

## Usage

### Pre-requisites

Create a workflow `.yml` file in your repository's `.github/workflows` directory. An [example workflow](#example-cache-workflow) is available below. For more information, see the GitHub Help Documentation for [Creating a workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file).

If you are using this inside a container, a POSIX-compliant `tar` needs to be included and accessible from the execution path.

If you are using a `self-hosted` Windows runner, `GNU tar` and `zstd` are required for [Cross-OS caching](https://github.com/actions/cache/blob/main/tips-and-workarounds.md#cross-os-cache) to work. They are also recommended to be installed in general so the performance is on par with `hosted` Windows runners.

### Example workflow

```yaml
name: tag-and-release

on:
  push:
    branches:
      - main

concurrency:
  group: ${{ github.workflow }}-${{ github.head_ref || github.ref_name }}
  cancel-in-progress: true

permissions:
  contents: write

jobs:
  tag-and-release:
    name: Tag and release
    runs-on: nodejs-16-small
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          token: ${{ secrets.CS_CI_GITHUB_TOKEN }}
          fetch-depth: 0
      - name: Tag and Release
        uses: action-tag-and-release@main
```
