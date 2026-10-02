# Marketing Ad Tester

Runs the three jobs at the end of a video ad pipeline: checking whether a produced clip matches what was specified, structuring the test so its result can actually be read, and reading the result honestly afterwards.

It never scores a dimension it has not measured. It writes the exact commands that produce the technical facts and the extracted frames, then judges from what comes back — and where something cannot be judged from frames at all, such as lip-sync or the audio mix, it returns not-reviewed with the reason rather than a plausible number. An invented score passes a clip nobody checked.

Its test setup moves ONE variable per cell, because a winner across two differences is a winner nobody can learn from, and it refuses a readout on a test that has not accumulated enough data to be read. A result below readability is not a weak signal; it is noise, and noise acted on is worse than no test.

It closes the loop. The readout produces the feedback that goes back to the angle, hook and script stages, so the next cycle starts from a result instead of from guesswork.

## What this is, precisely

A FindAgent **`mcp-tool`** agent. Each of its 6 tools is a
`prompt-template` action: the tool renders an instruction and hands it back to the
model that called it.

Two consequences worth being blunt about, because they decide whether this is useful to you:

- **It calls no model and reaches no network.** A tool call costs nothing and returns
  the same text for the same input, every time. There is no API key, no credential
  slot and no egress.
- **It observes nothing.** It has no access to your repository, your logs, your
  analytics or your devices. Every template is written so that supplying nothing
  produces an honest statement of what is missing rather than a confident-looking
  answer about data nobody provided. If you ask for a report and give it no findings,
  it will tell you the work has not been done — not invent it.

## Tools

| Tool | What it returns | Required input |
|---|---|---|
| `plan_qa_measurements` | Write the exact measurement commands QA needs for a set of clips, and say which dimensions those measurements can and cannot settle. | `clip_list` |
| `score_clip` | Apply the six-dimension rubric to measurements somebody else took, from supplied measurements and frames only, returning not-reviewed for anything the material cannot settle. | `measurements`, `spec` |
| `design_test_matrix` | Structure the test so exactly one variable moves per cell and each cell can reach a readable result inside the budget and window. | `variants_available` |
| `check_readout_readiness` | Decide whether a test has accumulated enough to be read at all, and refuse the readout when it has not, rather than producing a direction from noise. | `test_state` |
| `write_readout` | Read a completed test: what won, what died, what was attributable and what was not, separating a result from a difference that could be chance. | `results`, `test_setup` |
| `draft_feedback_documents` | Turn a readout into the feedback that goes back to the angle, hook and script stages, so the next cycle starts from a result rather than from guesswork. | `readout` |

Optional inputs render as empty when omitted. Every template names that case and says
what it could not determine, so an empty slot degrades into a stated gap rather than a
dangling clause.

## Part of a department

This agent is one member of the **marketing video ad** department, a
hub-orchestrator team of 8. The hub is `marketing-campaign-scoper`, which locks the brief every later
stage reads; the other members are
reached through it or called directly as `<alias>__<tool>`.

| Agent | Stage in the pipeline |
|---|---|
| `marketing-campaign-scoper` | 1 — interviews for the brief and freezes it (department hub) |
| `marketing-ad-researcher` | 2 — competitor harvest plan, longevity ranking, customer voice, coverage |
| `marketing-angle-strategist` | 3 — scored angle map with auditable arithmetic |
| `marketing-hook-writer` | 4 — the modular creative bank, built on verbatim customer language |
| `marketing-ad-scripter` | 5 — modules, continuity kits, prompts, assembly map, QA protocol |
| `marketing-production-planner` | 6 — blockers, tracks, cost estimate, shoot briefs, release gates |
| `marketing-introgen-briefer` | 7 — hands approved creative to IntroGen as a brief, an avoid list and a briefing record (runs only when IntroGen renders; no repo of its own) |
| `marketing-ad-tester` | 8 — clip QA, test design, readout, and the feedback loop back to 3, 4 and 5 |

Each member is published independently and works on its own.

## Provenance

`source/ad-tester.md` is the markdown skill this agent was converted from. The tool templates carry its
instructions, parameterised: anything the original hard-coded to one team's repositories,
file paths or people became an input you supply, and where a template would otherwise
depend on reading something it cannot reach, it asks for that material as an argument
instead.

## Licence and use

Published by Kata Team on FindAgent. Free to connect.
