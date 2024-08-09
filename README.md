## No Fuss Computing - GH Action / Workflow for bumping version


For the workflow to run the repo requires a [`.cz.yaml`](https://commitizen-tools.github.io/commitizen/config/#czyaml-or-czyaml) file at the root of the repo

``` yaml

---
commitizen:
  name: cz_conventional_commits
  prerelease_offset: 1
  tag_format: $version
  update_changelog_on_bump: false
  version: 0.0.1
  version_scheme: semver

```

To use this reusable workflow add the following file to path `.github/workflows/bump.yaml`

``` yaml

---

name: 'Bump'


on:
  workflow_dispatch:
    inputs:
      CZ_PRE_RELEASE:
        default: none
        required: false
        description: Create Pre-Release {alpha,beta,rc,none}
      CZ_INCREMENT:
        default: none
        required: false
        description: Type of bump to conduct {MAJOR,MINOR,PATCH,none}
  push:
    branches:
      - 'master'


jobs:

  bump:
    name: 'Bump'
    uses: nofusscomputing/action_bump/.github/workflows/bump.yaml@development
    with:
      CZ_PRE_RELEASE: ${{ inputs.CZ_PRE_RELEASE }}
      CZ_INCREMENT: ${{ inputs.CZ_INCREMENT }}
    secrets:
      WORKFLOW_TOKEN: ${{ secrets.WORKFLOW_TOKEN }}

```
