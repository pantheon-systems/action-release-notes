# Pantheon Documentation Release Notes Action

[![Unsupported](https://img.shields.io/badge/Pantheon-Unsupported-yello?logo=pantheon&color=ffdc28)](https://pr-9097-documentation.appa.pantheon.site/oss-support-levels#unsupported)
![GitHub License](https://img.shields.io/github/license/pantheon-systems/action-release-notes)
![GitHub Release](https://img.shields.io/github/v/release/pantheon-systems/action-release-notes)


**Automated release note generation**

_For internal use only._ This action will not work for and is not intended to be used on non-`pantheon-systems` repositories.

This Action is intended to be used on Pantheon repositories to automatically generate a Docs Release Note PR when a new release is created. By default, the action will pull the content of the release note from the GitHub release and add the appropriate release note syntax around it to standardize release notes for Pantheon OSS projects.

## Usage

```yaml
name: Create Release Notes PR
on:
  release:
    types: [published]

jobs:
  add-release-notes:
    runs-on: ubuntu-latest
    permissions:
      contents: read

    steps:
      - name: Generate release notes
        uses: pantheon-systems/action-release-notes@v1
        env:
          GH_TOKEN: ${{ secrets.TOKEN }}
        with:
          repo_name: Plugin Pipeline Example
          categories: wordpress,plugins
```

## Templating

A custom template file can be added to your `.github/workflows` directory to customize the release note format. The template file can be named anything, but must be specified in the passed inputs to the action. If used, it will automatically replace the following strings with the relevant valid equivalents:

* `%%REPO_NAME%%` - The name of the repository, inherited from `${{ inputs.repo_name }}`
* `%%RELEASE_TAG%%` - The tag or version of the release, pulled from the GitHub release
* `%%RELEASE_DATE%%` - The date the release was created, generated from the `date` command
* `%%RELEASE_LINK%%` - The URL to the GitHub release, parsed from the GitHub repository and release tag
* `%%RELEASE_BODY%%` - The body of the GitHub release, pulled from the GitHub release body

## Tokens
A valid Personal Access Token (PAT) with the `repo` and `workflow` scopes is required to run this action. Additionally, Pantheon Systems must be authorized on the PAT for the action to be able to create the PR in the documentation repository. Internally, the action uses the `${{ env.GH_TOKEN }}` set in the workflow step (see example above) but a different token can be set to push the branch to the Documentation repository.

## Inputs

### `github_token`
(Optional) A GitHub Personal Access Token with access to the Documentation repository.

default: `${{ env.GH_TOKEN }}`

### `categories`
(**Required**) Comma-separated list of categories for the release note. For a complete list of categories and descriptions, see https://github.com/pantheon-systems/documentation/blob/main/source/releasenotescategories/releaseNoteCategories.json

### `docs_repo`
(Optional) The documentation repository to create the pull request. (Included for testing against forked repositories.) Must be in `<vendor>/<repository>` format.

default: `pantheon-systems/documentation`

### `repo_name`
(**Recommended**) The repository or project name. Used to create the human-readable repository name (rather than the repo slug). Optional but recommended.

default: The repository slug (e.g. `$( basename ${{ github.repository }})`)

### `template_file`
(Optional) Custom release note template file. Should be stored in your `.github/workflows/` folder.

### `release_note_only_uses_template`
(Optional) Whether the release note should only use the template or to combine the release note template (via `template_file`) with the actual release notes from the release. Defaults to combine both.

default: `false`

### `release_body_override`
(Optional) Override the release body with a customized message. Can be used to generate release note copy dynamically in a previous step and saved to an environment variable (e.g. `release_body: ${{ env.RELEASE_BODY }}`). Can be used with `release_note_only_uses_template: true` to only use the template and/or the release body and drop the notes from the release. Can be used alongside a custom `template_file` if it exists.
