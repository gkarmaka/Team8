# Overnight review: Larkspur disruption-care agent

**To:** gkarmaka_Team8  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-09-22 17:22

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Overnight, Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py has no committed trace and no eval cases, so nothing about accuracy or cost is measured yet for this build.**

There is no readout-trace.json in the repository, so no run of run_agent survived the push. There is also no evals/cases.json, so no case has been scored against the tool schemas or the loop. Any claim about how well this agent handles a disruption is untested right now.

Run python3 run.py --all --trace and paste the totals footer.

**2. The diff fixes search_alternatives' description from the placeholder string "search" to a 355 character description, but the other eight tool descriptions are.**

The static scan shows search_alternatives at 355 characters after the team's edit, up from the shipped placeholder. hold_seat sits at 73 characters and send_confirmation at 71, both of which are the workshop's original text, not this team's. Whether those two short descriptions cause any mis-calls has not been checked against a case.

Run python3 verify.py 2.1 and paste the result for hold_seat and send_confirmation.

**3. EXTRA_TOOLS is empty and LOCAL_TOOLS has no executors, so this build runs on the nine shipped tools only.**

The static scan reports EXTRA_TOOLS declared: 0 and no LOCAL_TOOLS executors, and the diff does not touch either seam. Nothing in the repository adds a capability beyond lookup_booking, get_flight_status, search_alternatives, check_policy, hold_seat, confirm_rebooking, issue_voucher, escalate_to_human, and send_confirmation. A bigger model has nothing extra to call here; the ceiling on what this agent can do is the tool list, not the model behind it.

Run python3 run.py --show-tools and confirm the list matches these nine with no additions.

**4. TONE_ADDENDUM is still 0 characters, so the system prompt carries no team-authored tone or escalation guidance.**

The static scan confirms TONE_ADDENDUM: still empty (0) characters. run_agent concatenates runtime_preamble() plus SYSTEM_PROMPT plus TONE_ADDENDUM on every request, both at the first call and inside the while loop, so whatever tone or de-escalation instruction this team intends to add is not present in any call this file makes.

Paste the intended TONE_ADDENDUM string and run python3 verify.py 4.1 against it.

**5. The loop-fix diff moves from tracking a stale answer variable to returning text_of(response) after the loop exits, changing what final text the caller sees.**

The template previously set answer = text_of(response) inside each iteration and returned that stale answer after the loop, missing the model's last turn. The diff removes the answer variable entirely and returns text_of(response) once, after the while loop at MAX_TOOL_CALLS = 8 exits or stop_reason stops being tool_use. This is a functional change to what the customer-facing reply contains, but no trace file demonstrates it with a real multi-turn PNR case.

Run python3 run.py K7PQ2M --trace and confirm the final printed answer matches the model's last text block, not an earlier turn.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (230 lines)`
- `PITCH.md (unchanged template)`
- `TEAM.md (unchanged template)`

Reviewer: `claude-sonnet-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
