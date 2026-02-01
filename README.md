# Agent Skills

Agent Skills are folders of instructions, scripts, and resources that AI agents can discover and use to perform specific tasks. Write once, use everywhere.

This repository contains skills that our team uses internally, now open-sourced for the community.

## About Agent Skills

Skills teach AI agents how to complete specific tasks in a repeatable way. Each skill is self-contained in its own folder with a `SKILL.md` file containing instructions and metadata that agents use.

Learn more about the Agent Skills standard at [agentskills.io](https://agentskills.io).

## Available Skills

| Skill | Description |
|-------|-------------|
| [git-commit-message](skills/git-commit-message) | Analyze uncommitted changes and suggest English commit messages in Conventional Commits format |

## Installation

### Codex

Skills can be installed in Codex using the `$skill-installer` with the GitHub URL:

```
$skill-installer install https://github.com/fingersnap-dev/skills/tree/main/skills/git-commit-message
```

> After installing a skill, restart Codex to pick up new skills.

### Claude Code

Add the marketplace and install the plugin:

```bash
/plugin marketplace add fingersnap-dev/skills
/plugin install git-commit-message@fingersnap-skills
```

After installing, you can use the skill by mentioning it in your prompt:

> "Use the git-commit-message skill to suggest commit messages for my current changes"

## License

Each skill's license can be found in the `LICENSE.txt` file within its directory. Most skills in this repository are released under the MIT License.

## Contributing

Contributions are welcome! Feel free to submit pull requests with new skills or improvements to existing ones.