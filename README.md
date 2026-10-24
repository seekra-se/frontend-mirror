# Frontend Mirror

This repository mirrors another Git repository to GitHub using GitHub Actions. The
mirror workflow runs every 30 minutes and can also be started manually.

## How it works

The workflow in [`.github/workflows/mirror.yml`](.github/workflows/mirror.yml):

1. Clones the configured source repository with `git clone --mirror`.
2. Generates a GitHub App installation token.
3. Pushes all source branches, tags, and notes to the configured destination
   repository.
4. Prunes destination branches that no longer exist in the source repository.

The workflow uses a concurrency group so that scheduled runs do not overlap.

## Configuration

Configure the following repository-level Actions variables and secrets:

| Name | Type | Description |
| --- | --- | --- |
| `SOURCE_URL` | Variable | URL of the repository to mirror |
| `APP_CLIENT_ID` | Variable | Client ID of the GitHub App used for authentication |
| `REPOSITORY_NAME` | Variable | Destination repository name |
| `APP_PRIVATE_KEY` | Secret | Private key for the GitHub App |

The GitHub App must be installed for the destination repository and have
permission to write repository contents. The workflow requests the minimum
workflow-level permission needed for pushing mirror data:
`contents: write`.

## Running the mirror

The workflow runs automatically on its schedule. To run it immediately:

1. Open the repository on GitHub.
2. Select **Actions**.
3. Select **Pull Mirror**.
4. Select **Run workflow**.

## Notes

- The destination repository is determined by the repository where this workflow
  runs, while `REPOSITORY_NAME` identifies the repository passed to the GitHub
  App token request and push URL.
- Because the workflow uses `git push --prune`, branches deleted from the source
  repository are also deleted from the destination.
- Tags and Git notes are mirrored in addition to branches.
