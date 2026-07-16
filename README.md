# runward-gate — Gemini CLI extension

The [runward](https://github.com/stranxik/runward) deterministic, zero-LLM conformance gate,
as a Gemini CLI extension. When the model finishes a turn, it runs `runward check --strict`
and surfaces the verdict in the loop.

This is a thin distribution repo: the manifest lives at its root so `gemini extensions install`
resolves directly (Gemini requires the extension manifest at the repository root). The gate,
the rules, the docs and every other harness packaging live in the main repo,
[stranxik/runward](https://github.com/stranxik/runward).

## Install

```sh
gemini extensions install https://github.com/stranxik/runward-gemini
```

Then, in a project you trust, scaffold a mission if you don't have one:

```sh
npx runward init          # or: npx runward init --example
```

## What it does

An `AfterAgent` hook runs `npx --yes runward check --strict` at the end of each turn and prints
the verdict. It is **advisory** (`|| true`): it surfaces the gate, it does not block the turn.

The **hard governance gate is CI** — the [runward gate GitHub Action](https://github.com/marketplace/actions/runward-gate)
as a required status check blocks the *merge* on a gap. See the full, honestly-tiered channel
map in the main repo: [docs/distribution.md](https://github.com/stranxik/runward/blob/main/docs/distribution.md).

> Known upstream caveat (`google-gemini/gemini-cli#15712`): `AfterAgent` may not fire on
> text-only turns (no tool call), so the advisory verdict can be silently skipped at turn end.
> The hard gate stays CI, which is unaffected.

## Not a runtime, not privileged

runward installs nothing on its own; you ran `gemini extensions install`. This is one packaging
among several (Claude Code, Codex, Copilot, Cursor, Kiro, the GitHub Action) — none privileged.
The canonical, vendor-neutral surface is `AGENTS.md` + `.agents/skills/`, which Gemini reads too.

MIT © Thibault Souris. Source of truth: [stranxik/runward](https://github.com/stranxik/runward).
