# Customer Simulation Skills

A Claude Code plugin marketplace that distributes skills for running customer simulations and acting on the results.

## Install

From inside Claude Code:

```
/plugin marketplace add <github-owner>/<repo-name>
/plugin install customer-simulation-skills@simulation-skills
/reload-plugins
```

Replace `<github-owner>/<repo-name>` with the GitHub path of this repo once published. After `/reload-plugins`, skills are available as `/customer-simulation-skills:simulation-testing` and `/customer-simulation-skills:using-simulations`.

## What's included

- **simulation-testing** — guidance for running simulations end-to-end.
- **using-simulations** — guidance for interpreting and acting on simulation results.

## Repo layout

```
.
├── .claude-plugin/
│   └── marketplace.json          # marketplace manifest
└── plugins/
    └── customer-simulation-skills/
        ├── .claude-plugin/
        │   └── plugin.json       # plugin manifest
        └── skills/
            ├── simulation-testing/SKILL.md
            └── using-simulations/SKILL.md
```

## Local development

Test the plugin without publishing:

```
claude --plugin-dir ./plugins/customer-simulation-skills
```
