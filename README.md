# The Unofficial Guide
Adaya Head
Tool can review the city_guides corpus.

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

This project indexes the city guides corpus and answers practical travel questions about places, transport, food, and local conditions. It reads guide files, chunks them into short, self-contained units, retrieves the most relevant chunks for a question, and then uses the model to answer from those results. The goal is to help travelers find useful local information quickly without searching through the full guide manually.

## Chunking Strategy

**Chunk size:** 2 sentences  
**Overlap:** 1 sentence

Chunks are usually two sentences as long and stay on one practical idea, so at least four or five sample chunks contain a complete thought and do not break across unrelated topics. 

This works for the city guides corpus because most sections describe one practical fact or recommendation in a short paragraph, such as a place to eat. A two sentence trunk keeps the idea complete without mixing multiple topics together.

## Sample Chunks
======================================================================
Chunk 1  |  source: guide_accessibility.md#0  |  produced by: chunker.py::split_documents
======================================================================
An honest assessment rather than a promotional one. Some of these places are
difficult and it is better to know in advance.

======================================================================
Chunk 2  |  source: guide_corry_vale.md#7  |  produced by: chunker.py::split_documents
======================================================================
There is a farm shop at the valley mouth that sells bread, cheese and little else, and it closes at 4pm. Bring supplies; this is not a place with options.

======================================================================
Chunk 3  |  source: guide_givens_mill.md#3  |  produced by: chunker.py::split_documents
======================================================================
Everything is on one street along the river. The mill is at one end and the church at the other, eight minutes apart.

======================================================================
Chunk 4  |  source: guide_kestrelford.md#15  |  produced by: chunker.py::split_documents
======================================================================
The nearest full hospital is in Brightwater; there is
a minor injuries unit locally with limited hours.

======================================================================
Chunk 5  |  source: guide_regional_transport.md#8  |  produced by: chunker.py::split_documents
======================================================================
Parking is the constraint rather than driving. Both Halden Bay lots fill by
10am on summer weekends.


## Sample Answer

**Question:**
"Is Kestrelford good to walk in snow"
**Answer:**
(best distance 0.426, cutoff 0.5)

According to `guide_walking.md`, the Kestrelford approach road is impassable in snow because it is not gritted above the second village, which can cut the town off for a day or two most winters.

Sources retrieved: guide_kestrelford.md, guide_regional_transport.md, guide_thornby_wells.md, guide_walking.md

#   distance   source                           preview
----------------------------------------------------------------------------------------------------
1   0.4289     guide_walking.md                 harder going than the distance suggests. The single ...
2   0.5413     guide_kestrelford.md             # Kestrelford  Kestrelford is a hill town of 12,000,...
3   0.5668     guide_regional_transport.md      car park is free and involves a steep walk up.  ## W...
4   0.5976     guide_walking.md                 # Walking in the region  ## Easy, on good surfaces  ...
5   0.6269     guide_regional_transport.md      oncentrate on weekday daytimes. Sunday service is mi...

Gate: best distance 0.429 is under the 0.6 cutoff

```
```

**My relevance cutoff:**

     I have decided to use 0.5 as my relevance cutoff. I'm using this number because most distances for each response to my questions had better results at the 0.4 or 0.5 mark and I want to keep that number as low as possible. For my questions that I came up with, these answers were easily found in the documentation, so their numbers ran low like 0.3 or 0.5 distance wise. For the questions that were out of scope, their numbers were very high like 0.7 or 0.9. The gap came when questions were out of scope so the distances were all higher than an in scope question.

| Question | In corpus? | Best distance |
|---|---|---|
| What do students say about good places to eat Outside Marchwood? | Yes | 0.456 |
| When does Brightwater's Tuesday market close? | Yes | 0.394 |
| Is Halden Bay's seafood fresh? | Yes | 0.396 |
| What does Corry Vale sell | Yes | 0.476 |
| Is Kestrelford good to walk in snow | Yes | 0.429 |
| What is the capital of Mongolia? | No | 0.887 |
| How do I change the oil in a diesel engine? | No | 0.897 |
| Who won the 1994 World Cup? | No | 0.903 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.829 |
| How do I write a for loop in Rust? | No | 0.853 |

## How I Used AI

**1.**
I used AI to help me understand how to break down city_guides into proper chunks. This was a crucial part of the project as I didn't understand "chunks" as a key word for this project. Copilot was used to break down my thought process and allowed me to understand the proper next steps.

For example, I wanted to simply break down chunks as two sentences per chunk, but Copilot explained that the project needs deeper thinking. Chunks need to consider a whole lot of context before being separated and this idea was explained multiple times through this project as I set up my questions to ask and understood the types of responses I got back from the tool.

**2.**
I used AI to help me complete the chunker function. I did not understand how to parse words myself, so I talked it through with Copilot. Copilot suggested one fix and I went through the solution line by line, especially because I saw it add and delete imports, which I didn't know was an appropriate response yet. It's solution helped me complete this project!

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

Results file: `results/run_2026-09-23_1831.md`, produced by `run_eval.py::main` (3 runs per question, cache off). `run_eval.py` doesn't measure criteria 1, 4 and 5 directly, so I measured those separately with no changes to the system since that run.

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 4/5 | 4/5 | 5/5 | MISSED |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Sampled chunks read as a complete thought | at least 3 of 5 | 0/5 | 0/5 | 0/5 | MISSED |
| 5. Answers in under 30 seconds | 5 of 5 under 30 s | 5/5 | 5/5 | 5/5 | MET |

How each row was counted:
- **1** — `python app.py retrieve` / `store.py::search`, top 5 per question; checked whether any chunk contains the `expects` answer from `questions.py`. Retrieval is deterministic, so the same count goes in all three columns.
- **2** — counted answers in the results file that name a source file.
- **3** — `run_eval.py::check_out_of_scope`, one deterministic pass, so the same number goes in all three columns.
- **4** — the chunks the system actually searches, not a fresh re-chunk: run 1 = the top 5 retrieved for the Marchwood question, run 2 = Halden Bay, run 3 = Kestrelford. A chunk counts only if it starts and ends on a full sentence.
- **5** — timed `python app.py ask` on all five questions, cache off (`AI201_CACHE=0`), three rounds.

### Real output

**Criterion 1** — `store.py::search`, "When does Brightwater's Tuesday market close?", chunk #1 (guide_eating.md, distance 0.394):
```
Kestrelford's Saturday market has run since the 1400s and is the region's best,
though much reduced from November to February. Brightwater's Tuesday market
sets up at 7am in the square and is finished by 1pm.
```

**Criterion 2** — `run_eval.py::main`, "What do students say about good places to eat Outside Marchwood?", run 1 (passed the gate at 0.4654, no source named):
```
I do not have enough information to answer what students say about good places to eat outside Marchwood, as the documents do not mention students.
```
Compare a passing answer, "When does Brightwater's Tuesday market close?", run 1:
```
Brightwater's Tuesday market is finished by 1pm. 

Source: guide_eating.md
```

**Criterion 3** — `run_eval.py::check_out_of_scope`, cutoff 0.5, refused 5 of 5:
```
| What is the capital of Mongolia? | 0.887 | refused |
| How do I change the oil in a diesel engine? | 0.897 | refused |
| Who won the 1994 World Cup? | 0.903 | refused |
| What is the recommended dosage of ibuprofen for a headache? | 0.829 | refused |
| How do I write a for loop in Rust? | 0.853 | refused |
```

**Criterion 4** — `store.py::search`, "Is Halden Bay's seafood fresh?", chunk #2 (guide_eating.md). Every chunk in the index is labelled `produced_by=chunker.py::fallback_split`:
```
its best on a weekday
morning.

## Local specifics
```
Chunk #4 of the same query starts mid-word:
```
oncentrate on weekday daytimes. Sunday service is minimal to non-existent
outside the Brightwater town routes.
```

**Criterion 5** — `app.py ask`, seconds per question (Marchwood, Brightwater, Halden Bay, Corry Vale, Kestrelford):
```
run 1: 4.2 4.2 4.1 4.1 4.7
run 2: 4.2 4.2 4.2 4.1 4.1
run 3: 4.1 4.1 4.2 4.2 4.2
```

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

All five verdicts use the targets in `criteria.md` exactly as I wrote them in unit 1. A target counts as MET only if it held in all three runs.

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunks contain the answer (target 4 of 5) | MET | For all 5 questions, at least one of the top 5 chunks contained the `expects` answer, and retrieval gave the same chunks every run, so 5/5 three times clears 4 of 5. |
| 2 | Every answer names a source (target 5 of 5) | MISSED | The Marchwood answer named no source in runs 1 and 2, so the runs came out 4, 4, 5, and a 5-of-5 target has to hold every run, not just once. |
| 3 | Gate stops out-of-corpus questions (target 4 of 5) | MET | All 5 `OUT_OF_SCOPE` questions had a best distance of 0.829 or higher against my 0.5 cutoff and were refused, so 5/5 clears 4 of 5. |
| 4 | Sampled chunks read as a complete thought (target at least 3) | MISSED | I checked the top 5 chunks the system actually retrieved for three questions, and all 15 started or ended mid-sentence, so 0 of 5 each time is well short of 3. |
| 5 | Answers in under 30 seconds (target under 30 s) | MET | I timed all five questions three times with the cache off; every answer took between 4.1 and 4.7 seconds, so all 15 were under 30 seconds. |

**The close calls, and the case for the opposite verdict:**

- **Criterion 1 is the closest.** The Marchwood question asks what *students* say, and no document mentions students. I counted it as a pass because chunk #3 contains the answer I wrote down in advance ("Kestrelford's pubs serve 12 to 2 and 6 to 8:30…"). The case for MISSED: the chunk answers "where can I eat outside Marchwood", not the question I actually asked. Even if I count Marchwood as a fail, the result is 4/5 in every run, which still meets the 4-of-5 target, so the verdict stays MET either way.
- **Criterion 2:** run 3 did reach 5/5, and it's tempting to call the criterion met on that run. I didn't, because the target has to hold across all three runs, and a source that shows up one time in three is not "every answer".
- **Criterion 4:** the criterion doesn't say where to sample from. `python app.py chunks` re-runs my chunker and gives clean chunks (the Unit 1 samples above would pass), but those aren't the chunks the system searches. I judged the chunks in the index because that's what answers questions, and it's the reading that shows the real problem.
- **Criterion 5 is not close**, but the target was set low. The slowest answer took 4.7 seconds against a 30-second target, and I'd tighten it next time.

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

I have two misses in the system. These are criteria 2 and 4. The following are the diagnoses.

### Criterion 2 — Every answer names a source (MISSED: 4/5, 4/5, 5/5)

**Stage: generation.**

**What happened:** The Marchwood question ("What do students say about good places to eat Outside Marchwood?") passed the gate at 0.4654, but in runs 1 and 2 the model answered "I do not have enough information…" and named no source.

**How I found the stage:** I worked backwards from the chunks. Retrieval for that question returned chunk #3 from `guide_eating.md`, which contains "Outside Marchwood, kitchens across the region stop serving at 9pm… Kestrelford's pubs serve 12 to 2 and 6 to 8:30". The material was there, so loading, chunking and retrieval did their job and the problem is after them.

**Mechanism:** The prompt in `generate.py` (`GROUNDING_INSTRUCTION`) gives the model two rules that pull against each other: "If the documents don't cover the question, say you don't have enough information" and "Name the document your answer came from." My question asks what *students* say, and no guide mentions students, so the model takes the first rule and refuses. A refusal has no "document your answer came from", so nothing tells it to cite anything, and it drops the source. In run 3 it happened to list the three files anyway, which is why the result moves between runs. The citation rule only covers real answers, not refusals.

### Criterion 4 — Sampled chunks read as a complete thought (MISSED: 0/5, 0/5, 0/5)

**Stage: chunking.** More exactly, the index was built with the wrong chunker.

**What happened:** All 15 retrieved chunks I checked start or end mid-sentence, some mid-word — e.g. `oncentrate on weekday daytimes…` and `…kitchens across the region stop serving at 9p`.

**How I found the stage:** Every chunk that comes back from `store.py::search` is labelled `produced_by=chunker.py::fallback_split`, not `chunker.py::split_documents`.

**Mechanism:** `fallback_split` is the starter's original chunker. It cuts every 800 characters with 120 overlap (`CHUNK_SIZE` / `CHUNK_OVERLAP` in `config.py`) wherever that lands, so chunks begin and end in the middle of sentences and words, and they include `#` headings. In unit 1 I rewrote `split_documents` to split on paragraphs and sentences, but I never re-ran `python app.py index` afterwards, so the Chroma index still holds the old chunks. I didn't notice because `python app.py chunks` runs the chunker fresh instead of reading the index. That's why my Unit 1 sample chunks look clean and are labelled `split_documents`, even though the system has never searched them.

### Pattern across the misses

The two misses don't share a cause: one is in generation (the prompt has no rule for citing on a refusal), the other is in chunking (a stale index built by the old chunker). They are two separate problems, not one.

There is one link. The Marchwood question is behind the criterion 2 miss and the only close call in criterion 1, because it asks about "students" in a corpus of city guides that never mentions them. Part of the problem is my question wording, not just the system.

One more observation: the old chunks didn't stop criterion 1 from passing. The 800-character windows are big enough that the answer usually sits somewhere inside one, so retrieval still found it. Bad chunk boundaries hurt readability and precision here more than they hurt whether the answer is found.

## The Improvement

**What I changed:**
Criterion 4 diagnosis is having an issue with being successful. Rebuilt the index with python app.py index, so the system searches chunks made by chunker.py::split_documents instead of the stale 800-character fallback_split chunks

**Why I picked it:** 
My criterion 4 diagnosis found every indexed chunk came from fallback_split; rebuilding the index is the direct fix for that miss.

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

Results file: `results/run_2026-09-30_0047_after.md`, produced by `run_eval.py::main` (3 runs per question, cache off) after rebuilding the index with `python app.py index`. The new index holds 197 chunks, 136 characters on average (shortest 22, longest 277), all produced by `chunker.py::split_documents`. Nothing else changed: same corpus, same model, top-k 5, cutoff 0.5, same prompt. Criteria 1, 4 and 5 were measured the same way as in the Before log.

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 4/5 | 4/5 | 4/5 | MET |
| 2. Every answer names a source | 5 of 5 | 4/5 | 4/5 | 4/5 | MISSED |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Sampled chunks read as a complete thought | at least 3 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Answers in under 30 seconds | 5 of 5 under 30 s | 5/5 | 5/5 | 5/5 | MET |

**Before vs after, side by side:**

| Criterion | Before (runs 1/2/3) | After (runs 1/2/3) | Change |
|---|---|---|---|
| 1. Retrieved chunk contains the answer | 5/5, 5/5, 5/5 — MET | 4/5, 4/5, 4/5 — MET | worse by one question |
| 2. Every answer names a source | 4/5, 4/5, 5/5 — MISSED | 4/5, 4/5, 4/5 — MISSED | no better (lost run 3's lucky 5/5) |
| 3. Gate stops out-of-corpus questions | 5/5 — MET | 5/5 — MET | same |
| 4. Sampled chunks read as a complete thought | 0/5, 0/5, 0/5 — MISSED | 5/5, 5/5, 5/5 — MET | fixed |
| 5. Answers in under 30 seconds | 5/5 (max 4.7 s) — MET | 5/5 (max 17.9 s) — MET | same verdict |

### Real output (after)

**Criterion 1** — `store.py::search`, "What do students say about good places to eat Outside Marchwood?". This is the question that now fails. Chunk #1 (guide_eating.md, distance 0.4680):
```
This catches visitors out more than anything else. Outside Marchwood, kitchens
across the region stop serving at 9pm and often earlier.
```
The next sentence in `guide_eating.md`, "Kestrelford's pubs serve 12 to 2 and 6 to 8:30…", is now in a different chunk that isn't in the top 5. None of the 5 retrieved chunks contains "Kestrelford's".

A passing one, "When does Brightwater's Tuesday market close?", chunk #1 (guide_eating.md, distance 0.2657, was 0.3940 before):
```
Kestrelford's Saturday market has run since the 1400s and is the region's best,
though much reduced from November to February. Brightwater's Tuesday market
sets up at 7am in the square and is finished by 1pm.
```

**Criterion 2** — `run_eval.py::main`, Marchwood question, run 3 (named a source before, doesn't now):
```
I do not have enough information to answer what students say about good places to eat outside Marchwood.
```

**Criterion 3** — `run_eval.py::check_out_of_scope`, cutoff 0.5, refused 5 of 5:
```
| What is the capital of Mongolia? | 0.778 | refused |
| How do I change the oil in a diesel engine? | 0.880 | refused |
| Who won the 1994 World Cup? | 0.714 | refused |
| What is the recommended dosage of ibuprofen for a headache? | 0.849 | refused |
| How do I write a for loop in Rust? | 0.870 | refused |
```

**Criterion 4** — `store.py::search`, "Is Halden Bay's seafood fresh?", chunks #1 and #3. Every chunk is now labelled `produced_by=chunker.py::split_documents`:
```
Halden Bay's seafood is genuinely fresh � the two harbour restaurants buy
directly from boats that land in the early morning. Givens Mill's tearoom sells
bread made from flour ground twenty metres away.
```
```
Seafood, unsurprisingly, and it is genuinely fresh � the boats land in the early morning and the two harbour restaurants buy directly. Prices on the harbour front are roughly double those on Fell Street, one level up, for comparable food.
```

**Criterion 5** — `app.py ask`, seconds per question (Marchwood, Brightwater, Halden Bay, Corry Vale, Kestrelford), cache off:
```
run 1: 4.1 4.2 4.1 4.1 4.1
run 2: 4.1 4.1 4.1 4.1 4.6
run 3: 8.7 17.9 4.2 4.6 4.1
```

**Did it help?**

Yes for the miss it was aimed at, but it wasn't free. Criterion 4 went from 0/5 to 5/5 in every run: every retrieved chunk now starts and ends on a full sentence. Distances also got much tighter. Halden Bay's best match went from 0.396 to 0.148 and Brightwater's from 0.394 to 0.266, so the right chunk now stands out more clearly.

It made criterion 1 slightly worse. The Marchwood question went from pass to fail because two-sentence chunks split "Outside Marchwood, kitchens… stop serving at 9pm" apart from "Kestrelford's pubs serve…". The old 800-character windows held both. Criterion 1 still meets its 4-of-5 target, but only just.

It didn't fix criterion 2, which I expected: that miss is in the prompt, not the chunks. The Marchwood question now refuses without a source in all three runs instead of two. The one run that happened to list sources before was luck, not a fix.

How I know: same five questions, same cutoff and prompt, three runs each, before and after files both in `results/`.

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
