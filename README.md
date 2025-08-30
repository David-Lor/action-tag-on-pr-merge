# "Tag on Pull Request" GitHub Action

A GitHub Action that creates a new Tag on the repository, when a Pull Request (with the tag specified in the PR body) is merged.

## Usage

This action is intended to be part of a workflow that is triggered when a Pull Request is closed, with your main branch as base ([**example workflow**](.github/workflows/tag_release.yaml)).

### Requirements, conditions and limitations

- The workflow's job must contain a condition `if: github.event.pull_request.merged == true`. This will avoid running the workflow and action when a PR is discarded (closed without merged). 
- Your Pull Request body must contain a line equal to: `Tags {NewTag}` (example: `Tags 1.25.3`). The tag format can be validated using a regex (see Action inputs below). The `Tags` word can be singular (`Tag`), and also be lowercase. The merge commit on the base branch will be tagged with the tag specified on the body.
- This action supports defining just a single tag per PR (the first detected tag on the PR body will be used).
- Tags cannot be repeated (if a tag already exists, the action will fail).

This action supports the three existing merge strategies:

| Merge strategy | Where tag is created |
|----------------|----------------------|
| Merge          | Merge commit         |
| Squash         | Squashed commit      |
| Rebase         | Last commit rebased  |

### Action inputs

| Name           | Description                           | Default                                                                                                                                                             |
|----------------|---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `push-tag`     | If `true`, push the detected tag      | `false`                                                                                                                                                             |
| `tag-regex`    | Regex for validating tags             | None (no tag validation)                                                                                                                                            |
| `github-token` | Github Token or Personal Access Token | GITHUB_TOKEN (automatically provided by GitHub). If your repository is public, you can skip this setting. If private, you must provide a PAT with full `repo` scope |

#### tag-regex examples:

- `^(\d+\.)?(\d+\.)?(\*|\d+)$`: numeric tags in format `x.y.z`, like `1.25.3`

### Action outputs

| Name  | Description                             |
|-------|-----------------------------------------|
| `tag` | Detected tag (empty if no tag detected) |

## Changelog

- 0.0.2
  - Replace deprecated set-output (#28)
- 0.0.1
  - Initial release, supporting:
    - Single tag detection on body of merged Pull Requests
    - Configurable push of detected tag
    - Configurable tag regex filter

## TODO

- Support detecting and pushing multiple tags
- Allow overriding tags (if tag already defined, delete and create a new tag)
- Add automated tests
