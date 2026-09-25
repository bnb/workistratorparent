# workistratorparent

This repository is the parent for [cutenode/workistrator](https://github.com/cutenode/workistrator). It holds workflow templates in `workflow-templates/`, and `.github/workflows/workistrator.yml` copies them byte-for-byte into `.github/workflows/` of the child repository [`bnb/workistratorchild`](https://github.com/bnb/workistratorchild).

The sync runs on every push to `main` that touches `workflow-templates/` or the sync workflow itself, and can also be run manually.

## Setup

The sync needs an Actions secret named `WORKISTRATOR_TOKEN` in this repository. It must be a fine-grained personal access token scoped to both `bnb/workistratorparent` and `bnb/workistratorchild` with:

- **Contents**: read and write
- **Workflows**: read and write

The default `GITHUB_TOKEN` cannot write to other repositories, so it won't work here.

## Adding a template

1. Add a workflow file to `workflow-templates/`. Only `*.yml` and `*.yaml` files directly inside that directory are synced; subdirectories are ignored.
2. Commit and push to `main`. The sync commits the file to `.github/workflows/` in the child repository under the same name.

Templates live only in `workflow-templates/`, so they don't run in this repository. Don't also copy them into this repository's `.github/workflows/`.

## Running the sync manually

```sh
gh workflow run workistrator.yml --repo bnb/workistratorparent --ref main
```

To preview what would change without committing to the child repository, add `-f dry-run=true`.

## Removing a template

Deleting a template from `workflow-templates/` does not delete the child's copy. workistrator only creates and updates files, so remove the workflow from `bnb/workistratorchild` by hand.
