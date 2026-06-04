# grill-with-facts

`grill-with-facts` is an agent skill for stress-testing a plan one question at a time and saving the resolved decisions in `.facts` instead of chat history.

it is built around [facts](https://github.com/av/facts), a small CLI for keeping project specs as atomic, verifiable claims.

use it when a proposal is still loose, risky, or full of hidden assumptions, and you want the outcome to become durable facts that future agents can refine, implement, or verify.

## when to use it

- a plan, feature idea, architecture proposal, or migration strategy needs pressure-testing.
- decisions, risks, and domain vocabulary should be captured in `.facts`.
- the session should produce `@spec` facts for implementation and `@draft` facts for unresolved risks.

do not use it as a replacement for the facts lifecycle skills. it feeds that workflow:

- `facts-refine` sharpens existing `@draft` facts into precise `@spec` facts.
- `facts-implement` builds ready `@spec` facts and tags verified work as `@implemented`.
- `facts-discover` audits or syncs a fact sheet against existing code when explicitly requested.

## install

copy `SKILL.md` into your agent's skills directory as `grill-with-facts/SKILL.md`.

for Codex, the installed location is:

```sh
~/.codex/skills/grill-with-facts/SKILL.md
```

for projects using the `facts` CLI, run this first inside the project:

```sh
facts init
```

`facts init` installs the core facts skills and adds the facts workflow to agent instructions.

## how it works

the skill starts by reading the current facts workflow and fact sheet:

```sh
facts skills show facts
facts ll
facts check
facts ll --tags "draft or spec"
```

then it asks one high-leverage question at a time. each settled answer is immediately captured as a fact:

```sh
facts add "webhook retries stop after 3 failed attempts" --section payments/webhooks --tags "spec"
facts add "decision needed: webhook dead-letter retention period is not defined" --section payments/webhooks --tags "draft"
```

the session closes only when the major branches are resolved or intentionally deferred, and every durable outcome has been written to `.facts`.

## repo contents

- `SKILL.md` - the skill file agents load.
- `examples/grilling-session.md` - a compact example of the intended interaction.
- `.facts` - the repository's own fact sheet.

## license

MIT
