# Repository configuration

ECC Tools reads `.github/ecc-tools.yml` from the repository when deciding whether a **push** may start an automatic analysis. Issue and pull request comment commands are a separate switch. Turning off push-triggered analysis keeps `/ecc-tools` commands available.

The hosted app must implement this file. Until that ships, committing the file does not change app behavior. This document is the contract for that behavior, tracked in [issue #5](https://github.com/ECC-Tools/.github/issues/5).

## Opt out of automatic bundle pull requests

Put this on the default branch:

```yaml
# .github/ecc-tools.yml
push:
  autoAnalyze: false

commands:
  enabled: true
```

`push.autoAnalyze: false` means a push does not analyze the repository, does not open a `feat: add <repo> ECC bundle` pull request, and does not consume an analysis from the plan quota. Closing an older bundle pull request does not turn push analysis back on.

`commands.enabled: true` is the default. These comments still run:

```text
/ecc-tools analyze
/ecc-tools setup
/ecc-tools audit
/ecc-tools doctor
/ecc-tools repair
```

A command may still open a pull request, because a person asked for that run. That is not a push-triggered bundle pull request.

## Where the file is read

On a push that updates the default branch, read `.github/ecc-tools.yml` from the commit that the default branch moved to. If the app also auto-analyzes other branches, read the file from the commit that branch moved to.

Missing file: keep today's default. Push-triggered analysis stays allowed, and comment commands stay available.

## Keys

| Key | Values | When omitted |
| --- | --- | --- |
| `push.autoAnalyze` | boolean | Follow `autoPR.enabled` if that key is set. Otherwise allow push-triggered analysis. |
| `autoPR.enabled` | boolean | Legacy alias of `push.autoAnalyze`. Ignored when `push.autoAnalyze` is set. `false` does **not** disable comment commands. |
| `commands.enabled` | boolean | `true` |

`push.autoAnalyze` wins over `autoPR.enabled` when both are present.

| `push.autoAnalyze` | `autoPR.enabled` | `commands.enabled` | Push | Comment command |
| --- | --- | --- | --- | --- |
| omitted | omitted | omitted or `true` | analyze allowed | runs |
| `false` | any | omitted or `true` | skip | runs |
| omitted | `false` | omitted or `true` | skip | runs |
| `true` | `false` | omitted or `true` | analyze allowed | runs |
| any | any | `false` | follows the push columns | ignored |

## Invalid file

Apply these rules when the file exists:

- Empty file: same as a missing file.
- YAML that does not parse, or a non-boolean value for any key in the table: skip push-triggered analysis for that push, leave comment commands enabled, and record the parse error once per file blob. Do not post a new comment on every push.
- Unknown keys: ignore them. Do not treat unknown keys as a reason to reject the file.

Skipping the push protects quota for a repository that tried to opt out and shipped a typo.

## Dashboard

A per-repository dashboard control may set the same push switch for installations that cannot commit the file. When `.github/ecc-tools.yml` sets `push.autoAnalyze` or `autoPR.enabled`, the committed file wins. When the file sets neither key, the dashboard value applies. When neither the file nor the dashboard sets the switch, push-triggered analysis stays allowed.

## Example

See [docs/examples/ecc-tools.yml](examples/ecc-tools.yml).
