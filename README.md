# Changelog

[Vale](https://vale.sh) styles for changelogs: the format of
[Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/) as rules, and
an opt-in style for the tone of release notes. Release headings, ISO dates,
the six change types, the `[YANKED]` marker, and entries a reader can't use,
with a fix attached wherever there's only one right answer.

Works on [Markdown](https://docs.vale.sh/formats/markdown). The heading
rules read the Markdown source, so they see the brackets around a linked
version; the rest read the rendered text.

## Install

Requires Vale 3.21.0 or later.

```ini
StylesPath = styles
Packages = https://github.com/jdkato/changelog/releases/latest/download/Changelog.zip

[**/CHANGELOG.md]
BasedOnStyles = Changelog, ReleaseNotes
```

```console
$ vale sync
```

`Changelog` is the format and is the one to turn on. `ReleaseNotes` is an
opinion about tone you can take or leave, and it works on its own for
release notes and announcement posts.

| Style | What it checks |
| ----- | -------------- |
| `Changelog` | Release headings, dates, version brackets, change-type headings, the yanked marker, and entries that are vague, only a reference, or repeat their heading. See below. |
| `ReleaseNotes` | Excitement, exclamation points, `finally`, words that call a step simple or easy, and wording that blames the reader. |

### Changelog

| Rule | Level | Examples flagged | Fix |
| ---- | ----- | ---------------- | --- |
| `ReleaseHeading` | error | `## Version 1.2.0`, `## [1.1.0]` with no date, `## [1.0.0] (2017-06-20)`, `## [1.0]`, `## [unreleased]` | |
| `DateFormat` | error | `2017/06/20`, `2017-6-2`, `06/20/2017`, `June 20, 2017`, `3rd May 2017` | |
| `Yanked` | error | `(yanked)`, `YANKED`, `[Yanked]` | `[YANKED]` |
| `ChangeType` | error | `### Improvements`, `### Docs`, `### Performance` | |
| `ChangeTypeAlias` | error | `### New Features`, `### Bug Fixes`, `### Breaking Changes`, `### fixed`, `### Added:` | the type it means |
| `VersionLink` | warning | `## 1.0.0 - 2017-06-20`, `## Unreleased` | `[1.0.0]` |
| `VersionPrefix` | warning | `## [v1.0.0] - 2017-06-20` | `1.0.0` |
| `BareReference` | warning | `- #412`, `- [#412](…)`, `- 9fceb02` | |
| `Vague` | warning | `Bug fixes.`, `Various improvements`, `Minor changes`, `Misc` | |
| `RepeatedVerb` | suggestion | `Added support for X` under Added | `Support for X` |

A release heading is a level-two heading: the version in brackets, a
hyphen, and the date, `## [1.2.3] - 2024-03-05`, with ` [YANKED]` after the
date for a release that was pulled. `## [Unreleased]` is the one heading
without a date. The version may be a link, inline or by reference, and may
carry a pre-release or build suffix, `[1.0.0-rc.1]`. `ReleaseHeading`
reports a heading that isn't this shape, and `DateFormat`, `Yanked`,
`VersionLink`, and `VersionPrefix` report the part that's wrong in one that
is, so a heading gets one note per problem.

Change types are level-three headings, and there are six: Added, Changed,
Deprecated, Removed, Fixed, and Security. A heading with one clear meaning,
such as `Bug Fixes` or `New Features`, gets `ChangeTypeAlias` and its
replacement. One that could mean two types, such as `Improvements`, gets
`ChangeType` and no fix.

**Repeated verbs.** Under Added, Changed, Deprecated, and Removed, the
heading is the verb, so an entry that starts with it again, `Added support
for X` or `Add support for X`, can drop it. Fixed is exempt: `Fixed a crash
on an empty config` says a crash is gone, and without the verb, `A crash on
an empty config` reads as the bug itself. Security is exempt for the same
reason. Write Fixed entries either way.

### ReleaseNotes

| Rule | Level | Examples flagged | Fix |
| ---- | ----- | ---------------- | --- |
| `Excitement` | warning | `We're thrilled to announce`, `I am so excited`, `proud to introduce` | |
| `Exclamation` | warning | `Dark mode is here!` | `.` |
| `Blame` | warning | `was user error`, `users were calling it wrong`, `as clearly documented`, `RTFM` | |
| `Finally` | suggestion | `you can finally export`, `at long last` | delete |
| `Simply` | suggestion | `simply run`, `just restart`, `obviously`, `of course` | delete |
| `Easy` | suggestion | `easy`, `easily`, `trivial`, `straightforward`, `painless` | |

`Just` counts only before an instruction, `just run` or `just add`, since
`just as fast` and `more than just X` are fine. `Finally,` opening a list of
steps, `try/finally`, and a `finally` block are left alone, and so is
`easier`, which compares.

Every rule file starts with a comment saying what it checks and why, and
links to its source where it has one. See [Sources](#sources).

## Configuration

```ini
[**/CHANGELOG.md]
BasedOnStyles = Changelog, ReleaseNotes
Changelog.VersionPrefix = NO
Changelog.RepeatedVerb = NO
ReleaseNotes.Easy = NO

# Release notes and announcements, tone only
[docs/releases/*.md]
BasedOnStyles = ReleaseNotes
```

A project that tags releases `v1.2.3` and puts the `v` in its headings turns
off `VersionPrefix`; one that writes `Add X` under Added on purpose turns
off `RepeatedVerb`.

## What it doesn't check

Releases in date order, `[Unreleased]` at the top, a reference definition
for every bracketed version, empty change-type sections, and one entry per
change. A heading in a fenced code block is read as a heading by the
heading rules, which read the source.

## Development

`./test.sh` runs each rule's own cases with `vale test --coverage`, then
checks `fixtures/CHANGELOG.md` against the golden file in `testdata/` and
requires its rewrite in `fixtures/clean/` to pass clean. `./test.sh -u`
rewrites the golden file. `vale test` needs Vale 3.24.0 or later.

## Sources

| Rule | Source |
| ---- | ------ |
| `ReleaseHeading`, `VersionLink`, `BareReference` | Keep a Changelog 1.1.0, [Guiding Principles](https://keepachangelog.com/en/1.1.0/#principles) |
| `DateFormat` | Keep a Changelog 1.1.0, [Confusing Dates](https://keepachangelog.com/en/1.1.0/#confusing-dates) |
| `Yanked` | Keep a Changelog 1.1.0, [What about yanked releases?](https://keepachangelog.com/en/1.1.0/#yanked) |
| `ChangeType`, `ChangeTypeAlias` | Keep a Changelog 1.1.0, [Types of changes](https://keepachangelog.com/en/1.1.0/#types) |
| `Vague` | Keep a Changelog 1.1.0, [Commit log diffs](https://keepachangelog.com/en/1.1.0/#log-diffs) |
| `VersionPrefix` | Semantic Versioning 2.0.0, [Is "v1.2.3" a semantic version?](https://semver.org/#is-v123-a-semantic-version) |
| `Exclamation` | Google developer documentation style guide, [Exclamation points](https://developers.google.com/style/periods#exclamation-points) |
| `Simply` | Google developer documentation style guide, word list: [simple, simply](https://developers.google.com/style/word-list#simple) and [just](https://developers.google.com/style/word-list#just) |
| `Easy` | Google developer documentation style guide, word list: [easy, easily](https://developers.google.com/style/word-list#easy) |
| `Blame` | Google Technical Writing, Writing helpful error messages: [Don't blame the user](https://developers.google.com/tech-writing/error-messages/set-tone#dont_blame_the_user) |

`RepeatedVerb`, `Excitement`, and `Finally` are the package's own and have
no link.

## License

MIT. The rules follow [Keep a Changelog](https://keepachangelog.com/),
which is MIT licensed, and restate its format in the package's own words;
this package is not affiliated with it.
