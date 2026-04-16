# skills-monorepo-demo

This repository is a small demonstration of how to manage multiple Agent
Skills in a single Git repository without treating the whole repo as one
versioned artifact.

## Intent

The goal is to show a practical monorepo pattern for skills:

- each skill lives in its own directory under `skills/`
- each skill keeps its own version in `SKILL.md` front matter under
  `metadata.version`
- each skill can be released independently, even though all skills share one
  repository

This avoids the ambiguity of repo-wide tags like `v1.2.3`, which can imply
that every skill changed together.

## Release Convention

This demo follows artifact-scoped tags in the form:

`skills/<skill-name>/vX.Y.Z`

Examples:

- `skills/skill-hello/v1.1.0`
- `skills/skill-hola/v1.0.0`

That convention makes it clear which skill the tag belongs to and supports a
multi-skill monorepo model where releases are independent.

## Example Installs

Consumers can install a single skill from this repository with
[`skills.sh`](https://skills.sh) and pin it to an immutable tag.

Example:

```bash
npx skills add "https://github.com/frayer/skill-monorepo-demo#skills/skill-hello/v1.1.0" --skill skill-hello
```

That install pattern makes the release model explicit:

- the Git repository is the source
- `skills/skill-hello/v1.1.0` is the immutable ref being installed
- `--skill skill-hello` selects the specific skill from the monorepo

## What The Workflow Does

The GitHub Actions workflow in
[`./.github/workflows/create-skill-tags.yml`](.github/workflows/create-skill-tags.yml)
is intended to automate that release model:

- on pushes to `main`, inspect the full pushed range
- detect which skills changed
- compare each changed skill's previous and current `metadata.version`
- create a tag only when a skill's version was bumped
- fail if the target tag already exists

In other words, changing a skill does not automatically create a release.
Bumping that skill's version does.

## Why This Repo Exists

This repo is meant to support discussion and experimentation around scaling for
skills:

- keeping many skills in one repo
- versioning them independently
- preserving clear, consumer-friendly refs
- leaving room for future packaging or distribution layers without changing the
  source-of-truth model in Git

It is not a full product template. It is a focused example of the repository
structure and tagging approach.
