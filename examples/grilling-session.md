# Example Grilling Session

This is a compact example of the shape `grill-with-facts` should produce. The real session should ask one question at a time and write facts as soon as an answer becomes durable.

## Starting point

User:

```text
I want webhook retries for payment events. Grill the plan and capture the decisions in facts.
```

Agent:

```text
The highest-dependency decision is retry scope. I recommend retrying only delivery failures, not application-level payment failures, because the payment state machine should stay separate from transport recovery.

Should webhook retries apply only to failed HTTP delivery attempts, or also to downstream payment processing failures?
```

User:

```text
Only failed HTTP delivery attempts.
```

Agent records:

```sh
facts add "webhook retries apply only to failed HTTP delivery attempts" --section payments/webhooks --tags "spec"
```

## Unresolved risk

Agent:

```text
The next blocking decision is the dead-letter policy. I recommend a bounded retry count plus a dead-letter state so failed deliveries are inspectable instead of looping forever.

What should the max retry count and dead-letter retention be?
```

User:

```text
Max 3 attempts. I have not decided retention yet.
```

Agent records:

```sh
facts add "webhook retries stop after 3 failed delivery attempts" --section payments/webhooks --tags "spec"
facts add "decision needed: webhook dead-letter retention period is not defined" --section payments/webhooks --tags "draft"
```

## Closeout

Agent:

```text
Captured:
- @spec: webhook retries apply only to failed HTTP delivery attempts
- @spec: webhook retries stop after 3 failed delivery attempts
- @draft: decision needed: webhook dead-letter retention period is not defined

Next lifecycle step: use facts-refine for the retention draft, then facts-implement when the webhook retry specs are ready to build.
```
