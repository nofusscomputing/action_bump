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


## Additional Action Script

This Action/workflow will look for a shell script in location `.github/additional_actions_bump.sh` and execute it. This script if present will run before **any** git comiit occurs as part of the bump process. This is so that you can update version in additional files if required. Available environmental variables are as follows:

- `CURRENT_VERSION` _Set to the current version of the repo._
- `NEW_VERSION` _Set to the version the repo will be bumped to._
