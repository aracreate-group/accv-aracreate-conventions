# Claude Code Conventions

How a Claude Code session, in the VS Code extension or the terminal, is run:
the phrases that end a session and what each does, how replies and questions
are shaped, and the habits every session keeps. It does not cover claude.ai
chats or scheduled cloud agents. It builds on
[repo §5](../repo/readme.md#5-logs) for logs and
[git §5](../git/git-conventions.md#5-publishing) for committing.

Every session starts from this document. Where a project's memory or files
disagree with it, this document wins (§6).

## Contents

| Part | Covers |
| --- | --- |
| [1 Ending a session](#1-ending-a-session) | "stop this session" and "close this session" |
| [2 The backlog](#2-the-backlog) | `.session/backlog.md`, its headings and items |
| [3 Replies](#3-replies) | Numbering, spacing, proposals, plans, reports |
| [4 Questions](#4-questions) | The AskUserQuestion dialog |
| [5 Links and files](#5-links-and-files) | URLs and paths printed to open |
| [6 Asking before acting](#6-asking-before-acting) | What is a go and what is not |
| [7 Changes to live system state](#7-changes-to-live-system-state) | Rehearsing in a throwaway HOME |
| [8 Working with files](#8-working-with-files) | Large files, `.session/`, `.tmp/`, operational docs |
| [9 Evidence](#9-evidence) | Run logs, chat conclusions |
| [10 Shell and harness](#10-shell-and-harness) | zsh `noclobber`, arrays, globbing, attribution |
| [11 Repeated actions](#11-repeated-actions) | Built into the project's tools, not run ad hoc |
| [12 Running a session](#12-running-a-session) | Naming, handoff upkeep, main thread, resume |
| [13 Terms](#13-terms) | Words that steer a session |

## 1 Ending a session

A session ends the day with "stop this session" and ends for good with
"close this session". Each is an instruction in its own right, for the
commits and the push it lists, and does what is listed here, nothing more.

| Phrase | When | Log and backlog | Session files | Commit and push | Afterwards |
|---|---|---|---|---|---|
| none | during the day | kept by §12 | stay in `.session/1-open/` | none | open |
| "stop this session" | end of the day | log updated | stay in `.session/1-open/` | its work and the logs | resumable |
| "close this session" | the effort is done | log updated, open work to the backlog | moved to `.session/3-closed/` | its work and the logs | finished |

A session left open during the day needs no step: its handoff, kept up to
date under §12, is what a return resumes from.

A session that was neither stopped nor closed on a day it worked is logged
by its next stop or close, each day under its own date, worked out from its
commits and the handoff's State. A resume after such a gap names the
unlogged days in its opening lines.

### 1.1 Stop this session

The work of the day is done; the effort is not.

1. Write the session's section in the day's `logs/log-YYYY-MM-DD.md` and
   its row in `logs/logs.md`, for each day it worked since its last log
   entry, in each repo the session changed (repo §5). The section names the
   ticked Todo items not yet in a log; they stay ticked in Todo (§12).
2. Bring the session's handoff up to date, if it has one, so a fresh session
   can resume from it. `.session/backlog.md` is left as it is at a stop; items
   taken up during the day have already moved (§2).
3. Commit the session's uncommitted work, one commit per concern (git §4).
   Work on a task still unfinished is named, with where it would be
   committed, and committed only on a yes; left, it stays uncommitted.
4. Commit the logs: the day's log and `logs/logs.md`, staged by file, as
   `chore(logs): record <session> stopped`. Where `logs/logs.md` carries
   other sessions' uncommitted rows, only this session's row is staged, as
   a patch.
5. Push each repo committed to.
6. Report the commits, the push and what stays uncommitted.

### 1.2 Close this session

The effort is finished, or handed on through the backlog.

1. Steps 1 to 3 of a stop.
2. Hand the open work to `.session/backlog.md` (§2): the open Todo items at
   the top of the part, the Parked items at its foot, each ending with its
   session and date.
3. Move the session's handoff, as it is, ticked items included, and its
   session files from `.session/1-open/` to `.session/3-closed/`, dated, with
   the links to them (§8).
4. Commit the logs as in a stop, as `chore(logs): record <session> closed`.
5. Push each repo committed to.
6. Report the commits and the push, name any other uncommitted work left in
   the repo, and say the session is closed. Work asked for after that is a
   new session.

Where a repo's logs are kept uncommitted, as beside a client's repos, stop
and close do every step but the log commit and say so.

### 1.3 Which repo

The log and backlog entries go in each repo the session changed. Work that
changed no repo, such as moves in a shared drive, is logged in the repo the
session ran from.

## 2 The backlog

`.session/backlog.md` is the local, short list of what is still to be done in a
repo and no open session owns. It is a working file, never committed: where a
repo is tracked on GitLab or GitHub, its work items are the record, and the
backlog is the quick local version.

It follows the [backlog template](templates/claude-template-backlog.md).

- One `#` heading per part of the project a session works on as a unit, such
  as an app, a command set or a service, named in param-case as the part is
  named in the repo. A session adds the heading only when there is none.
- Under each part, one flat list of tasks, the most important first. A task
  is work on that part.
- An item is one task-list line, `- [ ]`, verb first, about 100 characters at
  most, nested where it breaks down into steps. Context stays in the handoff
  or the log, and the item links to it.
- An item ends with its session and the date it was added, and a due date or
  what it waits for only where one exists:
  `(<session>, 2026-11-05, due 2026-11-20)`.
- A reminder is a check or a follow-up at a point in time, not work on a
  part: a token to rotate, a reply to chase. Reminders sit under
  `# reminders`, the first heading, one line each: `- [ ] <what>, on
  <date> (<session>, <date added>)`. A session starting in the repo names
  those due within seven days. One that falls due becomes a task, taken into
  a session's Todo. One with a hard consequence also gets a calendar entry,
  on the user's word.
- Work a session owns is in its handoff's Todo, never here. A session that
  takes up an item, or finishes one along the way, moves it from here into
  its Todo, ticked once done. At close, its open Todo and Parked items come
  here (§1.2).
- Nothing here is ticked: finished work is ticked in the handoff that did it
  and recorded in the log. An item done outside a session, or dropped, is
  deleted.

## 3 Replies

What Claude writes in the session, never in files, commits or documents.

- Every heading and point is numbered hierarchically, 1, 1.1, 1.1.1, so any
  matter can be referred to by number.
- One matter per point. A point with two matters is split into two. Sentences
  inside a point are not numbered.
- Numbering runs on through the session: a reply that ended at 6 is followed
  by one that opens at 7. Sub-numbers restart under each new parent. Code
  blocks and table rows stay unnumbered.
- A heading uses heading syntax with its number inside, `## 7. Scope`, never a
  bare numbered line, which renders as a list item, and never a bold lead-in
  paragraph, which reads as emphasis.
- A blank line separates points, not sentences.
- What does not change the next decision is cut.
- A reply gives the result and what the user has to decide, nothing more.
  Where a matter is critical enough to need explaining, the reply asks
  "explain 7.2?" rather than explaining unasked.
- A reply with open decisions ends on one line naming them:
  `Decisions: 7.2, 7.3.` The line names every decision still open, those
  carried from earlier replies included; one left off is no longer open.
- A list inside a point is a numbered list, 1, 2, 3, indented under it, never
  bare bullets. An item is referred to as the point's number and its own,
  27.3.2. A table only for short parallel points.
- A proposal numbers each decision. What was asked for is one number, with
  sub-steps if it needs them; anything added is the next number.
- A yes agrees with the direction. It is not approval to run the whole list:
  each number is done only when named, reported on, and the next one asked
  for.
- A plan lists numbered Steps first, end to end, then Context.
- A report covers the task in hand. It does not close with a roll-call of
  untouched files, other repos or items deferred earlier; an unrelated item is
  raised only when it blocks the work or would be swept into a commit.

## 4 Questions

- A reply that leaves something to the user ends on the AskUserQuestion
  dialog: its open decisions, or, with none open, the next step. The reply
  above stays whole, Decisions line included (§3); the dialog is its short
  version, shown below it without hiding it.
- One question per open decision, four to a dialog and the rest in the next.
  Each names its number and stands on its own in a line or two, offering the
  real alternatives, revising and stopping included. With no decision open,
  one question offers the next steps, such as `todo 7` or stopping the
  session.
- Each option's label is numbered, "1. Apply", "2. Skip", so an answer typed
  in Other can name it. Content never goes in the dialog's preview.
- No dialog follows "Noted." (§13 `input`) or a reply a command fixes, such
  as a number only. An answer by number in the message box, `7.2 go`, counts
  as much as one in the dialog.
- A result asked to be seen, such as a link, is given in the reply, not folded
  into the next question.

## 5 Links and files

- Anything created or posted outside the repo, a work item, a merge request, a
  comment, a document, is followed at once by its full URL in the reply, and
  the URL is repeated at the top of the next question.
- An external page referred to is given as its full URL, so it opens with a
  click.
- A local file is referred to as a link to its path. An image to be looked at
  is opened in the editor with `code -r <path>`.
- A file put up for review is given as that link in the reply itself, every
  time; saying it is in a file, or a path in backticks, is not a reference.

## 6 Asking before acting

- A question phrased as how, where or what is best asks for a recommendation,
  not for it to be carried out. Answer, lay out the approach, and wait.
- While the user gives inputs (§13 `input`), the reply is "Noted." and
  nothing else, a question among the inputs included. They are acted on only
  on `answer`, `revise`, `N go`, `todo N` or an answer in the dialog.
- "Look at X to start Y" asks for an inspection and a proposal, not for Y to
  run.
- Before a file is written, its content is shown as text, and the session
  waits.
- A long run, such as a backup, a sync, a bulk rename or a large scan, starts
  only on a go given right before it. Approving a plan that includes the run
  is not that go.
- A long run goes to a background subagent with a self-contained prompt, so
  the session stays free for discussion.
- Writing something for a tracker or another person is not sending it: draft
  it, show it, and post only on an explicit instruction to.
- A tool or MCP server to "look at" is researched, including who hosts it, and
  installed only on a go.
- "Read only, no writes" means no data is created, updated or deleted, not
  that every request is a GET. A POST carrying a filter and returning data is
  a read; confirm it has no side effects and say that it mutates nothing.
  PUT, PATCH, DELETE and any state-changing POST stay off limits.
- If a run stops because the working directory is the wrong repo, that covers
  the whole run, not only the steps that write. Nothing is read, reported or
  drafted from the wrong clone, and the right one is not reached with
  `git -C`. Name the directory to run in, the state the work is in and what
  has to be cleared first, and stop.
  A workspace root, a plain folder holding one clone per repo, is not a wrong
  repo: the clone is its subfolder whose `origin` matches, and every git step
  runs inside it.
- A state of the machine or a repo the user reports is fact; re-check it
  rather than trusting an earlier view, since things move between turns.
- A name given roughly is turned into the naming convention of wherever it
  lands, without asking; the reply states the final name.
- Where a project's memory or files disagree with this document, this document
  wins: say so, and bring the memory in line during that session.

## 7 Changes to live system state

Dotfiles, `$HOME` symlinks, shell startup, and anything a broken result locks
the user out of are rehearsed before they are applied.

- The change is written once as a script taking the target root as an
  argument, so the rehearsal and the real run are the same tested artefact.
- The repo and any supporting state are copied into a scratch folder standing
  in for `$HOME`, the script is run with `HOME` set there, and the outcome is
  asserted.
- Only then is it run against the real target, with symlinks re-pointed at
  once so nothing stays dangling.

## 8 Working with files

- A large file, such as an export, a dump or a log, is inspected by script
  from its path, printing counts and small samples. It is never read into
  context whole. One that has already landed in context is a reason to start
  a fresh session before compaction keeps it there.
- A session keeps its handoff in the repo's `.session/1-open/`, named session
  first: `<session>-handoff.md`. The handoff holds only what a compaction or a
  fresh session needs to carry on, and is an index to the rest. It follows the
  [handoff template](templates/claude-template-handoff.md), with Goal, State
  and Todo first. Parked, Decisions, Side, Read first and Files are added once
  they have content, any other section goes after them, and State's date
  moves with every update. It stays under 100 lines (§12).
- What the session needs only now and then, a plan, a draft, a review list,
  test results, timestamps, a subagent's report, goes in a session file
  beside the handoff, `<session>-<what>.md`, such as
  `fix-login-test-results.md`. The handoff's Files section lists each with a
  line on what it holds, and a session opens one only when a Todo item needs
  it. Session files are renamed with the handoff, and move with it to
  `.session/3-closed/` at close, or to `.session/2-later/` when the session is
  shelved without closing.
- A session keeps its plain name, `billing` rather than `billing-3`,
  so one name has at most one handoff in `1-open/` and `2-later/`. At close,
  its files take the closing date after the name,
  `<session>-<YYYY-MM-DD>-<what>.md`, with `-2` for a second close that day,
  so the name can be used again and a closed file is never overwritten.
- When a session's files move, the links to them in `backlog.md` and in the
  files of `1-open/` and `2-later/` follow to the new path. Closed files and
  logs stay as written.
- `.session/` at the repo root holds `backlog.md` (§2), the handoffs and their
  session files in `1-open/`, `2-later/` and `3-closed/`, and in `output/` the
  files a command writes outside a session. It is gitignored (repo §5), so
  nothing in it is ever committed.
- Code or output Claude writes for its own working, such as a trial script or
  an intermediate listing, goes in `.tmp/`, which is gitignored, and is
  deleted or left there once built into where it is useful.
- `logs/` holds only the dated logs and `logs/logs.md`, all committed.
- A routine, skill or command file states current facts only: what it does,
  when it runs, how to change it. After a fix it reads as if the problem never
  happened; the story goes in the commit message or the conversation. This
  matters most for a prompt pushed into a live trigger, which is read by an
  agent with no context.

## 9 Evidence

- For a fault in a scheduled or automated run, read the run's log before
  proposing a cause. State read now is not evidence of state at run time, and
  an unsupported diagnosis is said to be one.
- A conclusion reached in a chat outside the session is discussion. It is
  recorded as an option or an open question until it is confirmed in the
  session at hand.

## 10 Shell and harness

- Where the Bash tool's shell is zsh with `noclobber` set, `>` onto an
  existing file fails while a loop carries on. Overwrite with `>|`, `perl -i`
  or the Write tool, and read back what a loop created before reporting it.
- In zsh, arrays index from 1. For lists of IDs, use a here-doc or `jq`
  rather than a shell array.
- In zsh, quote `[]` in arguments, such as `-f "ids[]=1"`, or globbing fails.
- Nothing committed or posted carries an attribution trailer of any form,
  "Generated with Claude Code", "sent by Claude", Co-Authored-By or the
  like, whatever the harness suggests (git §3). A post is the approved draft,
  word for word.

## 11 Repeated actions

- An action that will be run more than once, or over many items, is built
  into the project's existing script or tool and run from there, not as a
  one-off command or a script in the scratchpad.
- Where no tool fits, a new one is proposed where the project keeps them,
  such as `apps/` or `scripts/`, and written only on a go.
- A one-off inspection or check can stay a command.

## 12 Running a session

- A session is named by the user, at start or later with a rename command,
  in param-case; Claude suggests names from the goal
  when asked. The handoff, the log section and the backlog items carry it.
- Claude creates the handoff (§8) from the template once the goal is agreed,
  and updates State and Todo after each finished number and before a
  compaction, unasked. It stays under 100 lines: older material moves to the
  day's log, material not needed to carry on to a session file (§8), and
  ticked Todo items are shortened rather than removed.
- Todo is a task list, one `- [ ]` line per item, counted from 1 for
  `todo N`. A finished item is ticked, `- [x]`, and stays in Todo until the
  session is closed, then goes with the handoff to `.session/3-closed/`. A
  stop or close names it in the day's log; no tidy removes it, so `todo N`
  keeps its number.
- The handoff's Goal and Todo are the main thread. A request outside the Goal
  is named as such in one line, with `park` or `fork` as the choice.
- A session resumed after a gap opens with three lines from the handoff: the
  goal, where it stands, the next step, and the days not yet logged.

## 13 Terms

Words the user types to steer a session. Each does what is listed, nothing
more. Actions with steps of their own are session commands of their own.

A number from the replies comes first, then the verb: `27.3 go`. An item of
the handoff's Todo is named the other way round, `todo 6`, so the two never
read alike.

| Term | Meaning |
|---|---|
| `input` | The user is giving inputs. The only reply is "Noted." |
| `answer` | Answer the questions asked, and run nothing. |
| `revise` | Answer, rewrite the proposal, and run nothing. |
| `ask` | Put the open decisions into the AskUserQuestion dialog again, one question each, and run nothing. |
| `N go` | Do reply point N only, report on it, and stop. |
| `N park` | Move reply point N to the handoff's Parked section, as `- [ ] <point> (from N)`. |
| `N fork` | Take reply point N to a separate Claude Code session that the user opens, seeded by a handoff written for it in `.session/1-open/` and shown before it is written; never a subagent in this session. Merge its conclusion back under `## Side` unless the user rules the side session independent. |
| `N explain` | Explain reply point N plainly. |
| `todo N` | Do item N of the handoff's Todo only, report on it, and stop. |
| `todo` | `todo N` for the first open item of the handoff's Todo. |
