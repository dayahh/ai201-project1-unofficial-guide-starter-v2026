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
