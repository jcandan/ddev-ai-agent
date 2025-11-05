[![add-on registry](https://img.shields.io/badge/DDEV-Add--on_Registry-blue)](https://addons.ddev.com)
[![tests](https://github.com/jcandan/ddev-ai-agent/actions/workflows/tests.yml/badge.svg?branch=main)](https://github.com/jcandan/ddev-ai-agent/actions/workflows/tests.yml?query=branch%3Amain)
[![last commit](https://img.shields.io/github/last-commit/jcandan/ddev-ai-agent)](https://github.com/jcandan/ddev-ai-agent/commits)
[![release](https://img.shields.io/github/v/release/jcandan/ddev-ai-agent)](https://github.com/jcandan/ddev-ai-agent/releases/latest)

# DDEV AI Agent

## Overview

A [DDEV](https://ddev.com) add-on to spin up a simple, opinionated, customizable
n8n/supabase AI Agent workflow stack with sensible defaults.

---

Makes it as simple as:

1. `ddev add-on get jcandan/ddev-ai-agent`
2. `ddev start`
3. Browse **n8n** at `https://<project>.ddev.site:5678`

## Requirements

* DDEV ≥ 1.24.3 (Uses Docker)

[Get Started](https://ddev.com/get-started/)

Install and start using DDEV.

> NOTE: DDEV touts itself as a Docker-based PHP development environment, but it
> really is a docker-compose abstraction that affords us the ability to simplify
> providing you with a customizable AI Agent development environment with
> sensible, opinionated defaults. If you're not interested in PHP, you can
> proceed to use this project undaunted.

## Getting Started

First, create a DDEV project.

```bash
mkdir ~/projects/my-ai-agent
cd ~/projects/my-ai-agent
ddev config --project-name=my-ai-agent --project-type=generic --docroot=.
```

Then, get this add-on and start the project.

```bash
ddev add-on get jcandan/ddev-ai-agent
ddev restart
```

After installation, make sure to commit the `.ddev` directory to version control.

## Usage

| Command                       | Description                 |
| ----------------------------- | --------------------------- |
| `ddev start` / `ddev restart` | Launch or rebuild the stack |
| `ddev describe`               | View services and URLs      |
| `ddev logs -s n8n`            | Tail n8n logs               |
| `ddev logs -s supabase-db`    | Tail Postgres logs          |

Access **n8n** at `https://<project>.ddev.site:5678`

## Roadmap

See [Roadmap](https://github.com/jcandan/ddev-ai-agent/wiki/Roadmap) for details
about our plans for the future.

## Credits

**Contributed and maintained by [@jcandan](https://github.com/jcandan)**

Inspired by [@coleam00](https://github.com/coleam00)'s [local-ai-packaged](https://github.com/coleam00/local-ai-packaged).
