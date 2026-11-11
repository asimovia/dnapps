# Governance

DNApps is developed in the open. Anyone can comment, propose changes and
build implementations.

## Stages

| Stage | What it means | Changes allowed |
|---|---|---|
| **Draft for comment** (v0.x) | The idea and the format are open to question. | Anything, including breaking changes. |
| **Candidate** | The format is believed complete. At least two independent agents build the reference capsules and pass their tests. | Fixes and clarifications. Breaking changes need a strong reason. |
| **Stable** (1.0) | Safe to depend on. | Only backward-compatible additions. Breaking changes go to 2.0. |

v0.1 is a draft for comment until **January 10, 2027**. After that date we
publish a summary of every comment and what we did with it, and release v0.2.

## How decisions are made

- Every proposal is an issue or a pull request, and every one gets an answer.
- The editors look for rough consensus in the thread. When there isn't any,
  the editors decide and explain why in writing.
- Accepted changes are listed in `CHANGELOG.md`, crediting the person who
  proposed them.
- Editors today: the MeshKore team (Asimovia). We want editors from outside
  Asimovia and will invite regular contributors.

## Toward a standards body: the W3C Community Group

We have proposed a W3C Community Group for DNApps (shortname `dnapps`). Once
it is formed:

- **This draft is input, not a constraint.** The group may use it, change it
  or set it aside. The group's participants decide what it publishes.
- **If the group decides to adopt the draft**, Asimovia contributes the text
  it owns under the
  [W3C Community Contributor License Agreement](https://www.w3.org/community/about/process/cla/)
  (CLA). From then on the specification is developed in the group, under the
  W3C Community Group process, and this repository points to it.
- **The licenses here don't change.** The CLA grants a non-exclusive license,
  so what is already published stays available under CC BY 4.0 (text) and
  Apache-2.0 (schema). The [patent pledge](PATENTS.md) also stays. It goes
  further than the CLA's royalty-free commitment, which covers only claims
  essential to implementing the specification.
- **Contributions from other people.** Asimovia can only contribute what it
  owns. If you have contributed text to this repository and the group adopts
  the draft, we will ask you to join the group and contribute that text
  yourself under the CLA.

If the community prefers another venue, we're open to it.
