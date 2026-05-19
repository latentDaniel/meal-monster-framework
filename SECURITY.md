# Security Policy

## Project surface

Meal Monster is a collection of markdown prompts and templates. It executes no code, opens no network connections, ships no binaries, and collects no telemetry. The "runtime" is whatever AI you choose to pair it with.

What this means for security:

- There is no application server to harden, no installer to sign, no package to publish.
- The framework cannot exfiltrate or modify your data on its own — it can only describe instructions for an AI to follow.
- Any data risk comes from (a) the AI you choose to run it on and (b) the storage you choose for your reference documents.

## Reporting a concern

If you find something that worries you — a prompt that could mislead an AI into producing harmful output, ambiguous instructions that could be misinterpreted, a privacy implication we missed, or a typo in the disclaimer that materially changes its meaning — please open a GitHub issue describing the concern.

For anything you'd prefer not to discuss in public, use the repository's "Report a vulnerability" feature under the Security tab on GitHub.

## What to do if you accidentally publish your data

If you committed a populated `notebook-a-framework.md` or a weekly plan to a public repository:

1. Treat the data as already disclosed — Git history is durable and likely cached by indexers within minutes.
2. Rewrite history with `git filter-repo` or BFG and force-push, while understanding that this does not retroactively unpublish the data.
3. Consider whether you need to rotate any identifying information you included (e.g., specific provider names, scheduling details).

See the README's **Data & privacy** section for guidance on avoiding this in the first place.
