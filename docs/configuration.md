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

**Push.** When a push updates the default branch, read `.github/ecc-tools.yml` from the commit the default branch moved to. Use that commit for both `push.autoAnalyze` and `commands.enabled` on that push. If the app also auto-analyzes other branches, read the file from the commit that branch moved to.

**Comment.** For a `/ecc-tools` comment on an issue or pull request, read `.github/ecc-tools.yml` from the **default branch tip** at the time of the comment. `commands.enabled` comes only from that commit. Do not read it from the pull request head. A pull request that edits the file does not change command availability until it merges to the default branch.

The run a command starts may still inspect the pull request head, or another branch the comment names. That target does not change which commit supplies `commands.enabled`.

**Missing file.** On the commit selected above, a missing file keeps today's default. Push-triggered analysis stays allowed, and comment commands stay available.

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

- Empty file, or a file whose parsed value is null: same as a missing file.
- The root value must be a mapping. A list, a scalar, or any other non-mapping root is invalid. `[]` and `false` are invalid. `{}` is valid and means every key is omitted.
- When present, `push`, `commands`, and `autoPR` must each be a mapping. `push: false`, `commands: true`, and `autoPR: []` are invalid. `push: {}` is valid and means `push.autoAnalyze` is omitted.
- A recognized key whose value is not a boolean is invalid. An omitted key is not invalid.
- Unknown keys inside a valid mapping are ignored. Do not reject the file for an unknown key.
- Unparsable YAML and every invalid shape above: skip push-triggered analysis for that push, leave comment commands enabled, and record the error once per file blob. Do not post a new comment on every push.

Skipping the push protects quota for a repository that tried to opt out and shipped a typo. Leaving comment commands enabled on an invalid file avoids locking out `/ecc-tools` because of that typo.

## Dashboard

A per-repository dashboard control may set the same push switch for installations that cannot commit the file. When `.github/ecc-tools.yml` sets `push.autoAnalyze` or `autoPR.enabled`, the committed file wins. When the file sets neither key, the dashboard value applies. When neither the file nor the dashboard sets the switch, push-triggered analysis stays allowed.

## Example

See [docs/examples/ecc-tools.yml](examples/ecc-tools.yml).
