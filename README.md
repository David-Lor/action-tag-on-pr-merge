# "Tag on Pull Request" GitHub Action

A GitHub Action that creates a new Tag on the repository, when a Pull Request (with the tag specified in the PR body) is merged.

## Usage

Your repo must contain a workflow that is triggered when a pull request is closed against your main branch. This workflow must do a repository checkout, then run the "Tag on Pull Request" action (you can find an example workflow [here](.github/workflows/tag_release.yaml)).

Your Pull Request body must contain a line with the text: `Tags x.y.z`, being `x.y.z` your new version.
The merge commit on the base branch will be tagged with the tag specified on the body.
The `Tags` word can be singular (`Tag`), and also be lowercase.

## Action inputs

| Name           | Description                           | Default                                                                                                                               |
|----------------|---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| `push-tag`     | If `true`, push the detected tag      | `false`                                                                                                                               |
| `github-token` | Github Token or Personal Access Token | `${{ github.token }}` (automatically available on all repositories, you usually can avoid passing a PAT if your repository is public) |

## Action outputs

| Name  | Description                             |
|-------|-----------------------------------------|
| `tag` | Detected tag (empty if no tag detected) |

