# DNApps

**Draft v0.1, open for public comment until January 10, 2027.**
[Read the spec](spec.md) · [Comment or propose a change](https://github.com/asimovia/dnapps/issues) · [Website](https://meshkore.com/dnapps)

DNApps is an open specification for sharing the DNA of an app instead of the
app itself. A capsule is one text file that says:
- what the app does, with acceptance criteria;
- which decisions are fixed and which adapt to the person who builds it;
- the order to build it in.

You give the file to a coding agent (Claude Code, Codex, Cursor or an AI
operating system) and it grows the app for you.

It has two purposes:

1. **The app is built for the person from the start.** Their language, devices,
   operating system, abilities and the infrastructure they already have go in
   before any code exists. A blind person gets an app built to be used by
   voice. A deaf person gets one where nothing depends on sound.
2. **The capsule is much smaller than the app.** A capsule keeps what the app must be and drops
   how one copy happens to be written. Our example capsule is 15 KB and the app
   grown from it is about 75 to 95 KB of code, roughly 5 times larger. We don't
   know yet how the ratio behaves on large apps, and we'd like help measuring
   it.

Think of it like a genome. You don't ship a person, you ship their DNA, and the
same genome grows differently depending on climate, food and surroundings. The agent keeps the
decisions marked as fixed and adapts the rest to the person building the app.

## What's here

| File | What it is |
|---|---|
| [`WHY.md`](WHY.md) | The idea behind it: the DNA analogy, the two purposes, what we measured |
| [`ROADMAP.md`](ROADMAP.md) | The plan from this draft to a stable standard |
| [`spec.md`](spec.md) | The specification, v0.1 draft |
| [`schema.json`](schema.json) | JSON Schema for capsules |
| [`guide.md`](guide.md) | How to write a capsule that builds right, learned from real unattended builds |
| [`examples/plate.dna.yaml`](examples/plate.dna.yaml) | Example capsule: Plate, a nutrition tracker that works from photos ([its own repo](https://github.com/asimovia/plate-dna)) |
| [`GOVERNANCE.md`](GOVERNANCE.md) | How the spec moves from draft to stable, and who decides |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | How to comment, propose changes and add examples |

## Try it

Open Claude Code or Codex in an empty folder and paste:

```
Build the app described in https://github.com/asimovia/plate-dna/blob/main/plate.dna.yaml following the DNApps standard at https://meshkore.com/dnapps/spec.md
```

## We want your criticism

This is a first draft. Please tell us:
- whether something like this already exists;
- where the format is wrong;
- what an agent needed that the capsule didn't say.

Open an issue, start a discussion or send a pull request.

## License

- The specification text and the guide are licensed under
  [CC BY 4.0](LICENSE-CC-BY-4.0.txt).
- The schema is licensed under [Apache-2.0](LICENSE-APACHE-2.0.txt).
- Example capsules keep their own license, which is written inside each one.
  Plate is MIT.
- You can implement the spec without paying anyone or asking permission. The
  [patent pledge](PATENTS.md) spells out what we commit to.

DNApps is published by [MeshKore](https://meshkore.com), a project of Asimovia.
