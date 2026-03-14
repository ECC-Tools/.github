# ECC Tools

Public documentation and support hub for **ECC Tools**, the GitHub App for rolling ECC into real repositories.

ECC Tools analyzes repository history and agent-related configuration, then opens pull requests with project-specific skills, rules, hooks, commands, manifests, and safety checks for AI coding agents. It also runs hosted PR audits, security scans, and selective rollout planning so teams can adopt ECC repo by repo instead of installing everything at once.

## What ECC Is

ECC is a system for turning the way a repository actually works into reusable guidance for AI coding agents. It packages real project knowledge into skills, rules, hooks, commands, manifests, and safety checks so tools like Claude Code, Codex, Cursor, OpenCode, and similar agent harnesses can work with better context and safer defaults.

## What ECC Tools Adds

The open-source ECC repositories provide packs, local tooling, and templates. ECC Tools adds the hosted GitHub App layer:

- analyzes commit history, structure, and workflow patterns
- opens pull requests with repo-specific ECC outputs
- runs PR audits for risky changes to hooks, rules, agent configs, and MCP settings
- recommends selective rollout instead of dropping the full bundle everywhere
- provides lifecycle planning for setup, doctor, repair, and uninstall

## What It Can Generate

Depending on the repository and selected components, ECC Tools can generate:

- skills and workflow instructions
- rules, commands, and hook scaffolding
- install manifests and identity baselines
- Codex- and Claude-facing ECC bundle files
- security and harness audit results powered by AgentShield

## Core Commands

Use these in issues or pull requests after the app is installed:

```text
/ecc-tools analyze
/ecc-tools setup
/ecc-tools audit
/ecc-tools doctor
/ecc-tools repair
```

## Plans

### Open Source

- free
- public repositories and evaluation
- 10 analyses per month
- up to 200 commits per run

### Pro

**$19 per seat per month** on GitHub Marketplace for organization installs.

Includes:

- private repository analysis
- generated PRs for skills, rules, hooks, commands, and manifests
- PR-triggered audits and advanced AgentShield-backed scanning
- selective rollout planning for setup, repair, and uninstall
- priority support for rollout and billing issues

### Enterprise

Enterprise rollout, governance-heavy workflows, and higher-touch onboarding are available by inquiry at [ecc.tools](https://ecc.tools).

## Links

- Website: [ecc.tools](https://ecc.tools)
- Marketplace: [github.com/marketplace/ecc-tools](https://github.com/marketplace/ecc-tools)
- Status: [ecc.tools/status](https://ecc.tools/status)
- Privacy: [ecc.tools/privacy](https://ecc.tools/privacy)
- Terms: [ecc.tools/terms](https://ecc.tools/terms)
- Open-source ecosystem: [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code)
- Security scanner: [affaan-m/agentshield](https://github.com/affaan-m/agentshield)

## Support

Use this repository's Issues tab for:

- product questions
- documentation fixes
- marketplace and billing questions
- bug reports for the public GitHub App behavior

For security-sensitive disclosures, use the contact information at [ecc.tools](https://ecc.tools) instead of filing a public issue.
