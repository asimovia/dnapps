# Why DNApps

## Ship the DNA, not the organism

You don't ship a person, you ship their DNA, and a person grows from it. The
same genome grows differently depending on climate, food and surroundings. The
genetics are respected, but no two individuals come out the same.

DNApps applies that to software:

| Biology | DNApps |
|---|---|
| DNA, the genome | The capsule: what the app does, what can never change, the acceptance criteria |
| RNA, which expresses the genes | The `rna` section: the ordered build plan, with a "done when" for every task |
| The phenotype, the individual that grows | The app a coding agent builds from the capsule |
| The environment: climate, food, surroundings | The person: language, devices, operating system, abilities, preferences, the hosting and AI keys they already have |
| Genes that can't change and genes the environment shapes | `fixed`, `preferred` and `adapt` genes |
| Lineage | Forks of a capsule, and the lock file that records each build's choices |

The analogy has one limit. A capsule is read by an AI agent, not by chemistry,
so the same file can grow slightly differently each time. That is why each
capsule carries acceptance criteria and a test harness: they decide whether a
build is healthy.

## Purpose 1: the app is built for the person from the start

Today an app is built once for an average user. Then each person translates
it, configures it and works around what doesn't fit them. With a capsule, the
person's context goes in before any code exists:

- their language from the first line, not a translation added later;
- their devices and operating system: web, iOS, Android or desktop;
- their abilities: a blind person gets an app designed to be used by voice,
  and a deaf person gets one where nothing depends on sound;
- the infrastructure they already pay for: their own cloud account, their own
  AI key, their design system.

The agent writes every choice into a lock file. When the capsule gets a new
version, the agent grows it again with the same choices for that person.

## Purpose 2: applications compress

Source code is text, and a zip of a repository isn't big. A capsule is a
different kind of compression: it keeps what the app must be and drops how
one copy happens to be written. The agent rebuilds the rest.

What we measured on the example capsule, Plate, in November 2026:

| | Size | gzip |
|---|---|---|
| The capsule | 15 to 17 KB | 6 to 7 KB |
| The app an agent built from it (code only, without tests) | 75 to 95 KB | 21 to 24 KB |
| Ratio | about 5× | about 3.5× |

Plate is small, so the ratio is modest. We think it grows with bigger apps,
because the code that implements a behavior grows faster than the description
of that behavior. We haven't measured that yet, and we'd like help doing it.

We expect the larger saving to come from keeping one capsule instead of a
separate codebase for each platform and language: the same file grown into a
web app, an iOS app and an Android app, all following the capsule when it
changes. We haven't shown that with Plate yet; so far it has only been built
as a web app. And because you keep the capsule rather than the code, you can
rebuild the app with a better agent on a newer platform years from now.

## What we have tested

We gave the Plate capsule to Claude Code in an empty folder, three times,
without answering any of its questions. Every build showed us a gap in the
capsule, and we fixed the capsule each time. The third build passed all 39 of
its tests and our manual checks. The details and the lessons are in
[guide.md](guide.md).

## What we don't know yet

- Has someone already built this? We found tools that cover parts of it, and
  [spec.md §10](spec.md) lists them. If you know one that does all of it,
  please [tell us](https://github.com/asimovia/dnapps/issues/new?template=prior-art.md).
- Will the same capsule pass the same tests with Claude Code, Codex, Cursor and
  others?
- How far does compression go on a large app?
- Is YAML the right carrier?
- How should capsules be signed and trusted?
