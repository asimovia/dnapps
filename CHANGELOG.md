# Changelog

## Unreleased, proposed for v0.2
- Capsules may carry their own conformance kit: a contract that makes the app
  testable from outside, the checks as files the builder writes verbatim, and
  a build loop that iterates until they pass. Builders record disputes instead
  of editing the checks. Tried in Plate 1.3.0 and 1.3.1 (see the guide, section
  8, and the build log). Proposed by MeshKore.

## v0.1 (2026-11-11), draft for comment
- First public draft: the capsule format (DNA + RNA), fixed / preferred /
  adapt genes, the expression lock, the builder profile and conformance rules.
- JSON Schema.
- Authoring guide, based on three unattended builds of the Plate example
  capsule with Claude Code.
