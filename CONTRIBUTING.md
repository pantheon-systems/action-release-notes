# Contributing

The default development branch is `develop`. [Autotag](https://github.com/pantheon-systems/autotag) automatically tags a release on merges to `main`.

This repository is mostly owned and maintained by [Chris Reynolds](https://github.com/jazzsequence) ([DevRel](https://github.com/orgs/pantheon-systems/teams/devrel)) and is intended _exclusively_ for internal Pantheon projects. (It is a public repository by necessity, because it interacts with other public repositories.)

Direct commits to `develop` are discouraged. Squash merges are generally preferred.

## Releases

When `develop` is ready for a release, merge `develop` into `main` and push to main.

```bash
git checkout develop && git pull
git checkout main && git pull
git merge develop --ff-only
git push origin main
```

## Todo

- [ ] Break out bash scripts into separate files rather than everything being inside the `action.yml`.
- [ ] Automate releases.