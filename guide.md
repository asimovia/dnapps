# Writing a capsule that builds right

A guide for DNApps authors. Spec: <https://meshkore.com/dnapps/spec.md> ·
Worked example: <https://github.com/asimovia/plate-dna>

A capsule is good when a coding agent that has never heard of you reads it
and builds the right app on the first try. You don't talk to the agent and it
doesn't ask you anything. Everything in this guide comes from building our
example capsule, Plate, with Claude Code and no human help, checking the
result, and fixing the capsule each time it fell short.

## 1. The agent fills every gap with its memory

When a capsule says nothing about something, the agent decides, and it
decides with whatever it remembers from training. That is where most of the
bad builds come from.

In our first unattended build of Plate the agent set the default goal to
"lose weight". The capsule only listed `goal` as a field and said "maintain"
in a section the agent didn't connect to the profile. It also picked AI models
that were more than a year old, because the capsule said "any vision model"
and nothing about how to choose one.

What to do:

- **Write defaults next to the thing they apply to.** "Profile defaults on
  first run: goal maintain, protein 2.0 g/kg" goes in the data rules, not in a
  general notes section.
- **Never let the agent pick versions from memory.** If the app depends on
  model ids, library versions or API endpoints, say that they must be checked
  online at build time and written into the lock with the date and the source.
  Ask for the source explicitly. In our second build the agent marked an old
  model as "checked" with no source at all.
- **Give numbers.** "Fast" isn't something a test can settle. "The result shows within 20 seconds" is, even if the number is a guess you revise later.

## 2. Every acceptance line has to become a test

The agent only builds as well as you can check it. A feature with no way to
check it gets built from the agent's idea of "good enough".

Write acceptance lines as GIVEN / WHEN / THEN with something observable at
the end:

```yaml
- "GIVEN a photo with no food in it WHEN analyzed THEN no meal is invented: the person is told it is not food"
```

Avoid lines a test can't settle, such as "the UI is intuitive" or "the
summary is helpful". Turn them into something the agent can check: "every
screen starts with a visible heading", "every button has an accessible name".

## 3. Ask for the test harness as a deliverable

Saying `testing: { levels: [unit, acceptance] }` is not enough. In our first
build the agent said the code was "unit-tested" and left no test files at
all. Since 1.1.0 the Plate capsule asks for the harness by name:

- **Where the tests live and how they run:** `tests/index.html`, opened with
  a double-click, with no framework and no network.
- **How they are named:** one group per acceptance line (F1.1, F1.2…), so a
  reader can map every result back to the capsule.
- **What happens with paid or external services:** they are replaced by a
  fake that returns recorded responses. These include a good answer, a
  malformed one, an error and a rate limit. No test needs a real key. An
  optional "live check" button can run one real call when the person has a
  key.
- **What is a must:** the list of things that went wrong before. These are
  translations leaking between languages, unlabeled inputs and charts that
  break with one day of data.

### Write the check down too

The agent writes the app and the tests, so the tests share its blind spots.
In our second build every test passed, including one called "no text left in
the wrong language", while the navigation bar still said "Today" and
"Settings" in the Spanish version. The test only looked inside the screen
area, and the navigation bar lives outside it.

For anything that matters, write the check itself into the capsule:

```yaml
- "Every screen in both languages: ... How to check: switch the app to each
  language, open every screen, collect the visible text and every accessible
  name in the WHOLE document, navigation included, and fail if any string
  belongs to the other language's dictionary."
```

And state the principle once: tests look at the app the way a person sees
it, the rendered page, not at the app's internal data. A test that reads the
same dictionary the interface reads proves nothing about what is on screen.

Then make passing the tests the definition of done:

```yaml
builder:
  done_when: "Every test in tests/index.html passes. The builder runs it before
    declaring the build finished, and reports the counts."
```

## 4. Say what the agent must leave behind

List the files of the result, not "an app". For Plate:

- `index.html`, which opens the app;
- `tests/index.html`, which runs the tests;
- a README with how to run both;
- the lock file (`plate.dna.lock.yaml`), with the capsule's hash and the test
  summary.

Without the list, every build leaves something different and you can't
compare two builds.

## 5. Fixed things need a reason that says what breaks

`mode: fixed` is a hard rule, and agents respect it. They respect it better
when the `why` tells them what breaks. Plate's run rule says "double-click
`index.html`". Its `why` explains that ES-module imports and fetching local
files both fail on `file://`. Thanks to that, the agent used classic script
tags without being told to.

## 6. Unattended doesn't mean silent

A capsule should build with nobody around, but a person may be watching.
Say both things: the agent MAY ask about the adaptable choices (language,
design, provider) and MUST NOT wait for an answer. Give it the defaults to use
when nobody replies.

## 7. The loop: build, test, fix the capsule, version it

Treat the capsule like code. The loop we use:

1. Start a coding agent in an empty folder with only the one-line prompt from
   the README. Give it permission to write files, fetch pages and run node, and
   nothing else. Don't answer its questions.
2. Open the result and its `tests/index.html`. Then check what the tests
   can't: the screens in each language, the flows by hand, the lock file.
3. For every problem ask: was this the agent's fault, or did the capsule leave
   a gap? It is almost always a gap. Fix the capsule, not the app.
4. Bump the capsule version, and write in the changelog which build found the
   problem.
5. Build again from scratch. Stop when a fresh build passes everything you
   check by hand.

## 8. Checklist before you publish a capsule

- [ ] Every feature has at least one acceptance line with an observable THEN.
- [ ] Every default is written next to the field it applies to.
- [ ] Nothing depends on the agent remembering a version, model id or API.
- [ ] Every `fixed` gene has a `why` that says what breaks.
- [ ] The test harness is a named deliverable, with fakes for external services.
- [ ] `builder.done_when` is "the tests pass", and the agent must report the
      counts.
- [ ] The deliverable files are listed.
- [ ] No secrets, no personal data, nothing the agent should run outside
      building the app.
- [ ] A fresh unattended build has passed.

## Plate build log

| Capsule | Build | Result |
|---|---|---|
| 1.0.2 | Claude Code, unattended, 16 min | The app worked: profile, targets, manual meals, charts, export, delete, offline queue. It had no tests. The default goal was wrong, the model ids were outdated, translations leaked into accessible names, some inputs had no label and a chart axis broke with one day of data. |
| 1.1.0 | Claude Code, unattended, 20 min | 48 tests written and passing, one per acceptance line. It used recent Gemini and Claude model ids it found online and applied the profile defaults. Two problems got through: the navigation bar stayed in English in the Spanish version, though its own translation test passed, and an old OpenAI model was marked "checked" without a source. |
| 1.2.0 | Claude Code, unattended, 34 min, about $7 | 39 tests, all passing, and this time they checked the rendered page. The navigation bar was translated, every control had an accessible name and the model ids came with their sources. One problem was left: the service worker cached the app under a fixed name, so an update only showed on the second launch. We noticed because our own test browser kept serving the previous build. |
| 1.2.1 | Claude Code, unattended, 38 min, about $9 | 36 tests, all passing, and the Spanish version was translated everywhere, navigation bar included. Four problems got through. The service worker's cache name had the version but not the hash of the app files that F9.3 asks for, and the test for it only looked for the version. The sha256 in the lock didn't match the capsule. It never looked up the model ids: two were old and marked unchecked, and a third was marked checked with no source. Two hidden file inputs had no accessible name. |
| 1.2.2 | Claude Code, unattended, 23 min, about $6 | Run with an isolated config this time: the earlier builds also loaded the builder's personal instructions. 56 tests passed and 1 was skipped, and the new node check passed 5 of 5. Everything 1.2.1 missed was right. The cache name carried a hash of the cached files, and in our test browser the old build gave way to the new one on the second launch. The lock hash matched the capsule. All three model ids were looked up, with sources we could open. One problem got through: in Spanish, some accessible names and placeholders stayed in English (the camera button, the language menu, the navigation bar). The capsule asks for that check on the rendered document, but the test compared the two dictionaries instead. |
| 1.2.3 | Claude Code, unattended, 33 min, about $6 | Same isolated setup and model as the fifth build, and a worse result. It reported 66 tests passing and 5 skipped, and its node check passed 35 of 35, but neither checked what the capsule asks. The lock hash was of the app's own index.html, not the capsule. The models had no source pages. The browser tests checked that Spanish strings exist, not what the rendered app shows, so the rendered-app check that 1.2.3 asks for never ran. In the app, a few accessible names and placeholders still stayed in the first language after switching. The cache name was right. Two builds of the same capsule by the same agent varied this much, so we think a capsule can't rely on the builder to write its own conformance checks. |
