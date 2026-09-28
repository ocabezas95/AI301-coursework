# Voice guide: how I talk upstream

## Who I am in threads

I am new to open source and I say so plainly, without apologizing for
it. I am here to reproduce issues carefully and hand maintainers
something they can act on.

What readers can expect from me: I did the work before I commented, I
show what I ran, and I say what I don't know instead of covering for it.

## Rules I write by

### Rule: Hedge the cause, not the observation

What I ran and what I saw are facts — I state them flat. Uncertainty
belongs on the cause, the scope, and the fix, where it is real.

- Wrong: "Sorry if this is obvious or I'm doing something wrong, but I
  think maybe the parser might be failing? Not sure if this is even the
  same bug."
- Right: "On 3.12 the parser raises `TypeError: expected str` at step 3
  (output below). I don't know yet whether the cause is the parser or
  the caller."

### Rule: Show before you ask

Evidence comes first. If I want the issue, I earn the ask with a
reproduction — and often the evidence is the whole comment and no ask is
needed.

- Wrong: "Hi! I'm new to open source and would love to work on this.
  Could you please assign it to me?"
- Right: "Reproduced on macOS 15 / Node 22.3 — the build exits 1 with
  the error from the issue (log below). I'd like to take this one."

### Rule: No dates I don't control

I say what I'm doing next, never when it will land. A missed date I
announced is worse than no date at all.

- Wrong: "I'll have a PR up tonight, tomorrow morning at the latest!"
- Right: "Next I'm narrowing this to a minimal case. I'll post what I
  find; I'm not putting a date on it."

### Rule: Quote the excerpt, not the log

I paste the few lines that carry the signature, and say where they came
from. A maintainer should be able to read my artifact without scrolling
past it.

- Wrong: _200 lines of build scrollback pasted whole, error somewhere in
  the middle._
- Right: "Relevant lines from `npm run build` (full log available if
  useful):" followed by the 5 lines around the failure.

### Rule: Disclose AI use plainly, once

Where the repo asks for it, one matter-of-fact line. No performance, no
over-explaining, and no hiding it either. "AI-assisted" is the term —
it covers investigating, running, and writing without my having to
itemize which part was which. Whatever the first sentence says, the
second one names what I ran and verified myself.

- Wrong: "(I should mention I used AI a bit here, I hope that's okay —
  I still verified everything myself and really did understand it, just
  wanted to be fully transparent!)"
- Right: "AI-assisted (Claude): used while investigating and writing
  this up. I ran the reproduction myself and verified the output above."

## Things I never post

- **Promises I can't keep.** Delivery dates, "I'll have this done",
  committing to scope past what I've actually reproduced.
- **"Same as above, can confirm."** If I haven't run it myself and shown
  what I got, I have nothing to add to the thread.
- **"+1", "bump", "any update on this?"** Every one of those costs every
  subscriber a notification and carries no information.
- **A confident diagnosis I haven't tested.** Naming a cause or a fix as
  fact when I've only read the code. This is the one I reach for when
  I'm tired and want the comment to sound finished.
