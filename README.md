# grill-with-facts

`grill-with-facts` is an agent skill for stress-testing a plan one question at a time and saving the resolved decisions in `.facts` instead of chat history.

Use it when a proposal is still loose, risky, or full of hidden assumptions, and you want the outcome to become durable facts that future agents can refine, implement, or verify.

## when to use it

- You have a plan, feature idea, architecture proposal, or migration strategy that needs pressure-testing.
- You want decisions, risks, and domain vocabulary captured in `.facts`.
- You want the session to produce `@spec` facts for implementation and `@draft` facts for unresolved risks.

Do not use it as a replacement for the facts lifecycle skills. It feeds that workflow:

- `facts-refine` sharpens existing `@draft` facts into precise `@spec` facts.
- `facts-implement` builds ready `@spec` facts and tags verified work as `@implemented`.
- `facts-discover` audits or syncs a fact sheet against existing code when explicitly requested.

## install

Copy `SKILL.md` into your agent's skills directory as `grill-with-facts/SKILL.md`.

For Codex, the installed location is:

```sh
~/.codex/skills/grill-with-facts/SKILL.md
```

For projects using the `facts` CLI, run this first inside the project:

```sh
facts init
```

`facts init` installs the core facts skills and adds the facts workflow to agent instructions.

## how it works

The skill starts by reading the current facts workflow and fact sheet:

```sh
facts skills show facts
facts ll
facts check
facts ll --tags "draft or spec"
```

Then it asks one high-leverage question at a time. Each settled answer is immediately captured as a fact:

```sh
facts add "webhook retries stop after 3 failed attempts" --section payments/webhooks --tags "spec"
facts add "decision needed: webhook dead-letter retention period is not defined" --section payments/webhooks --tags "draft"
```

The session closes only when the major branches are resolved or intentionally deferred, and every durable outcome has been written to `.facts`.

## repo contents

- `SKILL.md` - the skill file agents load.
- `examples/grilling-session.md` - a compact example of the intended interaction.
- `.facts` - the repository's own fact sheet.

## license

MIT
