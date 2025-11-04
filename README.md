[![add-on registry](https://img.shields.io/badge/DDEV-Add--on_Registry-blue)](https://addons.ddev.com)
[![tests](https://github.com/jcandan/ddev-ai-agent/actions/workflows/tests.yml/badge.svg?branch=main)](https://github.com/jcandan/ddev-ai-agent/actions/workflows/tests.yml?query=branch%3Amain)
[![last commit](https://img.shields.io/github/last-commit/jcandan/ddev-ai-agent)](https://github.com/jcandan/ddev-ai-agent/commits)
[![release](https://img.shields.io/github/v/release/jcandan/ddev-ai-agent)](https://github.com/jcandan/ddev-ai-agent/releases/latest)

# DDEV Ai Agent

## Overview

This add-on integrates Ai Agent into your [DDEV](https://ddev.com/) project.

## Installation

```bash
ddev add-on get jcandan/ddev-ai-agent
ddev restart
```

After installation, make sure to commit the `.ddev` directory to version control.

## Usage

| Command | Description |
| ------- | ----------- |
| `ddev describe` | View service status and used ports for Ai Agent |
| `ddev logs -s ai-agent` | Check Ai Agent logs |

## Advanced Customization

To change the Docker image:

```bash
ddev dotenv set .ddev/.env.ai-agent --ai-agent-docker-image="ddev/ddev-utilities:latest"
ddev add-on get jcandan/ddev-ai-agent
ddev restart
```

Make sure to commit the `.ddev/.env.ai-agent` file to version control.

All customization options (use with caution):

| Variable | Flag | Default |
| -------- | ---- | ------- |
| `AI_AGENT_DOCKER_IMAGE` | `--ai-agent-docker-image` | `ddev/ddev-utilities:latest` |

## Credits

**Contributed and maintained by [@jcandan](https://github.com/jcandan)**
