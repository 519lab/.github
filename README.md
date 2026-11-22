# 519lab/.github

Organization-wide defaults for 519lab. Each file applies to every 519lab repository, private ones included, that has no file of that type of its own. The repository has to stay public for that to work.

A repository's own file replaces the default. For issue templates it's all or nothing: any valid form or `config.yml` in a repository's own `.github/ISSUE_TEMPLATE` turns the whole default set off there. Keep your own file only for project-specific content, such as setup steps in `CONTRIBUTING.md`, and link to the default for the shared rules.

| File | Applies to |
| --- | --- |
| `SECURITY.md` | Security policy: private vulnerability reporting, response timeline and disclosure |
| `CODE_OF_CONDUCT.md` | Contributor Covenant 3.0; reports go to conduct@519lab.com |
| `CONTRIBUTING.md` | Branch model, commits, changelog and pull request conventions |
| `SUPPORT.md` | Where to ask questions and report bugs, security issues and conduct concerns |
| `.github/PULL_REQUEST_TEMPLATE.md` | Pull request description: summary, changes, verification, post-merge |
| `.github/ISSUE_TEMPLATE/` | Bug report, feature request and question forms; blank issues are off and point to the security policy |

This repository holds no workflows. The shared reusable workflows (`promote`, `version`, `release`) live in [`ggrace519/.github`](https://github.com/ggrace519/.github). Pin them to a release commit SHA there, never to a copy here.
