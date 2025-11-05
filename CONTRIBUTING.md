# Contributing to DDEV AI Agent

## Development workflow

### 1. Clone the repository

```bash
git clone git@github.com:jcandan/ddev-ai-agent.git
cd ddev-ai-agent
```

### 2. Fire it up in VSCode

```bash
code ddev-ai-agent
```

### 3. Create a QA DDEV project

Use a clean directory (outside this repo):

```bash
mkdir ~/projects/my-ai-agent
cd ~/projects/my-ai-agent
ddev config --project-name=my-ai-agent --project-type=generic --docroot=.
```

### 3. Install the add-on from your local path

```bash
ddev add-on get ./path/to/ddev-ai-agent
ddev start
```

You should be able to open:

https://my-ai-agent.ddev.site:5678/

---

## Iterating on changes

While editing the add-on repo, you can update your QA project with:

```bash
ddev add-on get ../ddev-ai-agent && ddev restart
```

This reapplies the current add-on definition and restarts the environment.

---

## Resources

* [DDEV Add-ons: Creating, maintaining, testing](https://www.youtube.com/watch?v=TmXqQe48iqE) (part of the [DDEV Contributor Live Training](https://ddev.com/blog/contributor-training))
* [Advanced Add-On Techniques](https://ddev.com/blog/advanced-add-on-contributor-training/)
* [DDEV Add-on Maintenance Guide](https://ddev.com/blog/ddev-add-on-maintenance-guide/)
* [DDEV Documentation for Add-ons](https://ddev.readthedocs.io/en/stable/users/extend/additional-services/)
* [DDEV Add-on Registry](https://addons.ddev.com/)

---

## Testing

See [tests/test.bats]

---

## Communication

* Use [GitHub Issues](https://github.com/jcandan/ddev-ai-agent/issues) for bug reports and enhancements.
* PRs are welcome; include reasoning for architectural choices.
* Keep commits atomic and descriptive; use conventional commits.

## Maintainers

DO NOT MERGE PRs VIA GUI!

Surprisingly, GitHub does not provide a fast-forward merge options for pull
requests. When a PR is ready, perform a fast-forward merge locally.

<!-- @todo Add GitHub action to simplify fast-foward merges from PRs -->
