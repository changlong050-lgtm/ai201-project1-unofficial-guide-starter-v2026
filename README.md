# The Unofficial Guide

<!-- Replace this line with your name and which corpus you picked. -->

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->

## Chunking Strategy

**Chunk size:**
**Overlap:**

<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: `` — produced by: ``

```
```

**Chunk 2** — source: `` — produced by: ``

```
```

**Chunk 3** — source: `` — produced by: ``

```
```

**Chunk 4** — source: `` — produced by: ``

```
```

**Chunk 5** — source: `` — produced by: ``

```
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:**

**Answer:**

```
```

**My relevance cutoff:**

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

| Question | In corpus? | Best distance |
|---|---|---|
|  |  |  |

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1.**

**2.**

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. No chunk cuts a sentence in half | 4 of 5 | 3/5 | 3/5 | 3/5 | MISSED |
| 5. Correct source attribution | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |

### Criterion 1 — Retrieved chunk contains the answer

File: `results/run_2026-09-29_2204_before.md`, produced by `run_eval.py::main` and `store.py::search`.

All five questions retrieved chunks containing the answer in every run. For example, run 1 of "How many actions does a player take on their turn in Harbourmaster?" retrieved `board_game_turn.txt` (best distance 0.3305) and the answer was:

```
On a turn in Harbourmaster, a player takes exactly two actions. 

Source: board_game_turn.txt
```

### Criterion 2 — Every answer names a source

File: `results/run_2026-09-29_2204_before.md`, produced by `generate.py::answer_from_chunks`.

Every answer across all three runs names at least one source document. For example, run 1 of "How many coins do you earn for selling cargo to a port that accepts it?":

```
You earn two coins for selling cargo to a port that accepts it. 

Source: `board_game_ports.txt` (also mentioned in `board_game_rules_walkthrough.txt`)
```

### Criterion 3 — Gate stops out-of-corpus questions

File: `results/run_2026-09-29_2204_before.md`, produced by `run_eval.py::check_out_of_scope` and `gate.py::check`.

All five out-of-scope questions were refused. The gate is deterministic, so one pass is the whole measurement:

| Out-of-scope question | Best distance | Gate |
|---|---|---|
| What is the capital of Mongolia? | 0.961 | refused |
| How do I change the oil in a diesel engine? | 0.873 | refused |
| Who won the 1994 World Cup? | 0.860 | refused |
| What is the recommended dosage of ibuprofen for a headache? | 0.875 | refused |
| How do I write a for loop in Rust? | 0.838 | refused |

### Criterion 4 — No chunk cuts a sentence in half

Produced by `chunker.py::split_documents` (which currently calls `chunker.py::fallback_split`).

Sampled five chunks from the index. Three read as complete thoughts; two were cut mid-sentence by the fixed-size window. Example of a cut chunk (`board_game_house_rules.txt#3`, 181 chars):

```
sing the hold limit from three to four also does not work: cargo is
the constraint the whole game is built around, and removing it removes most of
the reason to plan a route at all.
```

This starts mid-word ("sing" is the tail of "Raising"). Score: 3/5 across all runs since chunking is deterministic.

### Criterion 5 — Correct source attribution

File: `results/run_2026-09-29_2204_before.md`, produced by `generate.py::answer_from_chunks`.

All five answers cite a source that actually contains the stated fact. For example, run 1 of "What is the maximum number of cargo cards a player's hold can contain?":

```
The maximum number of cargo cards a player's hold can contain is three, as the hold limit applies at all times. 

Source: board_game_faq.txt
```

The file `board_game_faq.txt` does contain the hold-limit rule. Score: 5/5 across all runs.

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 |  |  |  |
| 5 |  |  |  |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
