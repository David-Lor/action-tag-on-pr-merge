# "Tag on Pull Request" GitHub Action

A GitHub Action that creates a new Tag on the repository, when a Pull Request (with the tag specified in the PR body) is merged.

## Usage

Your repo must contain a workflow that is triggered when a pull request is closed against your main branch. This workflow must do a repository checkout, then run the "Tag on Pull Request" action (you can find an example workflow [here](.github/workflows/tag_release.yaml)).

Your Pull Request body must contain a line equal to: `Tags {NewTag}` (example: `Tags 1.25.3`).
The tag format can be validated using a regex (see Action input below).

The merge commit on the base branch will be tagged with the tag specified on the body.
The `Tags` word can be singular (`Tag`), and also be lowercase.
This action supports defining just a single tag per PR (the first detected tag on the PR body will be used).

The tag supports the three existing merge strategies:

| Merge strategy | Where tag is created |
|----------------|----------------------|
| Merge          | Merge commit         |
| Squash         | Squashed commit      |
| Rebase         | Last commit rebased  |

## Action inputs

| Name           | Description                           | Default                                                                                                                               |
|----------------|---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| `push-tag`     | If `true`, push the detected tag      | `false`                                                                                                                               |
| `tag-regex`    | Regex for validating tags             | None (no tag validation)                                                                                                              |
| `github-token` | Github Token or Personal Access Token | `${{ github.token }}` (automatically available on all repositories, you can usually avoid passing a PAT if your repository is public) |

### tag-regex examples:

- `^(\d+\.)?(\d+\.)?(\*|\d+)$`: numeric tags in format `x.y.z`, like `1.25.3`

## Action outputs

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
