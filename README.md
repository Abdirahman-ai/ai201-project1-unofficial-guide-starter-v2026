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
The Unofficial Guide is a retrieval-based question answering system built using
the advice_threads corpus. It loads and chunks student advice posts, embeds the
chunks, and retrieves the most relevant information for a user's question.

The system answers questions using only the retrieved documents and includes the
source it used. It also uses a relevance cutoff so that questions outside the
corpus are rejected instead of producing unsupported answers.

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

**Chunk 1** — source: `thread_first_gen.txt#0` — produced by: `chunker.py::split_documents`

```
THREAD: Anything specific for first-generation students?

--- reply 1 (33 votes) ---
The advising office has a specific programme and it is genuinely good, but it is opt-in and badly publicised. Ask for it by name.

--- reply 2 (41 votes) ---
The thing I'd say: the unwritten rules are the hard part, not the coursework. Ask about the unwritten rules explicitly. People are happy to explain them and nobody volunteers them.

--- reply 3 (16 votes) ---
Emergency fund for textbooks and travel exists and is not means-tested beyond a short form.
```

**Chunk 2** — source: `thread_first_gen.txt#0` — produced by: `chunker.py::fallback_split`

```
THREAD: Anything specific for first-generation students?

--- reply 1 (33 votes) ---
The advising office has a specific programme and it is genuinely good, but it is opt-in and badly publicised. Ask for it by name.

--- reply 2 (41 votes) ---
The thing I'd say: the unwritten rules are the hard part, not the coursework. Ask about the unwritten rules explicitly. People are happy to explain them and nobody volunteers them.

--- reply 3 (16 votes) ---
Emergency fund for textbooks and travel exists and is not means-tested beyond a short form.
```

**Chunk 3** — source: `thread_laptop_specs.txt#0` — produced by: `chunker.py::split_documents`

```
THREAD: How much laptop do I actually need for CS courses?

--- reply 1 (31 votes) ---
Less than the recommended spec page says. 16GB of RAM is the one number worth paying for; everything else you'll never notice.

--- reply 2 (18 votes) ---
Adding: the lab machines exist and are better than anything you'll buy. For the heavy assignments people just use those.

--- reply 3 (12 votes) ---
I did two years on an 8GB machine and it was fine until the last project, at which point it very much wasn't. 16 is the answer.
```

**Chunk 4** — source: `thread_office_hours_etiquette.txt#0` — produced by: `chunker.py::split_documents`

```
THREAD: Is it weird to go to office hours with no specific question?

--- reply 1 (44 votes) ---
No, and this is the single most common thing first years get wrong. 'I'm following the lectures but I don't feel like I understand the shape of it' is a completely normal thing to say.

--- reply 2 (29 votes) ---
They're usually empty. You are doing the instructor a favour by turning up.

--- reply 3 (18 votes) ---
If it helps, treat it as a standing appointment. Go every week for a month and it stops feeling like a thing.
```

**Chunk 5** — source: `thread_professor_email.txt#0` — produced by: `chunker.py::split_documents`

```
THREAD: Do professors actually answer email?

--- reply 1 (21 votes) ---
Varies enormously. General rule I've found: if the syllabus states a response window, it's honoured. If it doesn't, assume 48 hours and don't panic before then.

--- reply 2 (33 votes) ---
Office hours are dramatically more effective than email for anything that takes more than two sentences to answer. They're also usually empty.

--- reply 3 (15 votes) ---
Empty office hours is the biggest unused resource here and I say that having wasted a year not going.

For each one, ask: could someone answer a question using only this,
without reading what came before or after?
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:** How much RAM do students recommend for a CS laptop?

**Answer:** Students recommend 16GB of RAM for a CS laptop, noting that 8GB can struggle by the final project and everything else is less noticeable.

**Source:** thread_laptop_specs.txt

```
```

**My relevance cutoff:** 0.65
My five in-scope questions had best distances from 0.200 to 0.484.
My five out-of-scope questions had best distances from 0.807 to 0.952.
I chose 0.65 because it falls clearly between the two groups.

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
For Unit 2, I used ChatGPT to help review my before-run results, compare them against the acceptance criteria I had already written, and identify that Criterion 5 was measuring exact wording rather than answer correctness.

I also used ChatGPT to help think through one retrieval improvement. I implemented hybrid semantic and BM25 retrieval, then compared the before and after results. The measured results showed that the change did not fix the specific ranking issue I was targeting, so I reported that result instead of continuing to tune it.he specific ranking issue I was targeting, so I reported that result instead of continuing to tune it.

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
| 1. Retrieved chunks contain the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Sampled chunks read as complete thoughts | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Final answer includes the expected word or phrase | 4 of 5 | 2/5 | 2/5 | 2/5 | MISSED |

**Criterion 5 revision:** The original exact-phrase measurement missed correct answers that expressed the expected facts using different wording. Using the revised Unit 2 criterion — checking for the expected facts rather than an exact full-string match — the result was 5/5 in all three runs.

### Real output used for the criteria

**Criterion 1 — Retrieved chunks contain the answer**

Produced by `store.py::search`.

For the question:

```text
When is laundry actually free in the dorms?
```

one of the retrieved chunks was:

```text
THREAD: When is laundry actually free in the dorms?

--- reply 1 (27 votes) ---
Tuesday and Wednesday mornings, every building. Sunday evening is the worst and it isn't close.
```

The same check was performed for all five test questions, and at least one retrieved chunk contained the answer for each one.

**Criterion 2 — Every answer names a source**

Produced by `run_eval.py::main`.

```text
Students recommend 16GB of RAM, noting that 8GB can struggle by the final project and that 16GB is the one number worth paying for (thread_laptop_specs.txt).
```

All five generated answers named at least one source in each of the three runs.

**Criterion 3 — Gate stops out-of-corpus questions**

Produced by `run_eval.py::check_out_of_scope`.

```text
refused  (best distance 0.918)  What is the capital of Mongolia?
refused  (best distance 0.930)  How do I change the oil in a diesel engine?
refused  (best distance 0.952)  Who won the 1994 World Cup?
refused  (best distance 0.807)  What is the recommended dosage of ibuprofen for a headache?
refused  (best distance 0.871)  How do I write a for loop in Rust?
-> gate refused 5 of 5
```

**Criterion 4 — Sampled chunks read as complete thoughts**

Produced by `chunker.py::split_documents` and displayed by `python app.py chunks`.

Example:

```text
THREAD: Do professors actually answer email?

--- reply 1 (21 votes) ---
Varies enormously. General rule I've found: if the syllabus states a response window, it's honoured. If it doesn't, assume 48 hours and don't panic before then.

--- reply 2 (33 votes) ---
Office hours are dramatically more effective than email for anything that takes more than two sentences to answer. They're also usually empty.

--- reply 3 (15 votes) ---
Empty office hours is the biggest unused resource here and I say that having wasted a year not going.
```

All five sampled chunks started and ended with complete thoughts. Re-running the chunk sample twice produced the same five chunks and the same 5/5 result.

**Criterion 5 — Final answer includes the expected word or phrase**

Produced by `run_eval.py::main`.

For example, `questions.py` expected:

```text
ask about the unwritten rules and use the advising office
```

but the generated answer was:

```text
First-generation students should ask for the advising office's specific programme by name, since it is opt-in and badly publicised (thread_first_gen.txt). Additionally, they should explicitly ask about the unwritten rules, because people are happy to explain them even though nobody volunteers them (thread_first_gen.txt).
```

The answer contains both expected facts, but not as one exact phrase. Under literal full-phrase matching, only 2 of 5 questions matched in each run. This exposed a problem with the original measurement, so Criterion 5 was revised in Unit 2 to measure the expected facts rather than exact wording.

## Verdicts

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunks contain the answer | MET | All 5 test questions retrieved at least one chunk containing the information needed for the answer, exceeding the target of 4 of 5. |
| 2 | Every answer names a source | MET | All 5 answers named at least one source document in each of the three runs. |
| 3 | Gate stops out-of-corpus questions | MET | The relevance gate refused all 5 out-of-corpus questions, exceeding the target of 4 of 5. |
| 4 | Sampled chunks read as complete thoughts | MET | All 5 sampled chunks started and ended cleanly and could be understood without text from another chunk. The same result appeared in all three checks. |
| 5 | Final answer includes the expected word or phrase | MISSED / REVISED | Exact full-phrase matching produced 2/5 in all three runs even though several answers contained the correct facts using different wording. I kept the original criterion and added a Unit 2 revision that measures whether the expected facts are present instead. Under the revised measurement, all 5 answers passed in each run. |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

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
## Diagnoses

After reviewing the before-run results, I did not find a true retrieval or generation failure after revising Criterion 5's measurement. Criteria 1 through 4 were met in all three checks, and the revised version of Criterion 5 was also met.

### Criterion 5 — original measurement miss

**Stage:** Generation / evaluation measurement

**Mechanism:** The generated answers contained the correct facts but often paraphrased them instead of repeating the full `expects` phrase from `questions.py` word-for-word. For example, the first-generation answer mentioned both the advising office and asking about the unwritten rules, but it did not reproduce the exact phrase `"ask about the unwritten rules and use the advising office"`.

Because of this, literal phrase matching scored correct answers as failures. The problem was with how the criterion measured correctness rather than with retrieval: the needed information was present in the retrieved chunks, and the generated answers used that information correctly.

### Pattern I noticed

The system consistently retrieved the information needed to answer all five test questions. However, some queries also returned unrelated chunks farther down the top-5 results. For the pass/fail question, for example, the first retrieved chunk only contained information about pass/fail limits, while the second retrieved chunk contained the information needed to answer the question.

This did not cause a failure in the current tests because the correct information was still available to the generator, but retrieval ranking could be made more precise.

### If I tightened a criterion

Since the system met the revised criteria, some of my original targets may have been fairly forgiving. I would tighten Criterion 1 from requiring the answer to appear anywhere in the retrieved results to requiring the answer to appear within the top 3 results, or possibly the top result.

That would test retrieval ranking quality rather than only checking whether the correct information appeared somewhere in the top 5.

## The Improvement

**What I changed:**  
I added hybrid retrieval that combines semantic similarity with BM25 keyword ranking using Reciprocal Rank Fusion. I kept the original cosine distance on each retrieved result so the existing relevance gate and its 0.65 cutoff continued to use the same measurement as before.

**Why I picked it:**  
My before-run results showed that the system consistently retrieved the correct information, but the most useful chunk was not always ranked first. For the pass/fail question, `thread_pass_fail.txt#1`, which mainly contained usage limits, ranked above `thread_pass_fail.txt#0`, which contained the information needed to answer the question. I chose hybrid search because keyword matching might improve the ranking of chunks containing the exact terms used in the question.

### Run Log — After

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunks contain the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Sampled chunks read as complete thoughts | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Final answer includes the expected word or phrase | 4 of 5 | 2/5 | 2/5 | 2/5 | MISSED |

**Criterion 5 revision:** Under the revised Unit 2 measurement that checks whether the expected facts are present rather than requiring an exact full-string match, the result remained 5/5 in all three runs.

### Real output — After

The after run was produced by `run_eval.py::main` using the updated `store.py::search`.

For the pass/fail question, the hybrid search still ranked the two chunks in this order:

```text
1 thread_pass_fail.txt#1 0.4224
2 thread_pass_fail.txt#0 0.4261
```

The second chunk contained the more directly useful answer about using pass/fail for a course outside the major and deciding after the midterm.

The generated answer still correctly used the available information:

```text
Based on the documents, you should use the pass/fail option for a course outside your major that you are taking out of curiosity. Additionally, you can take a course pass/fail and decide to declare it late—up to week eight—after taking the midterm first.

Source: thread_pass_fail.txt (and thread_first_year_regret.txt)
```

The relevance gate also continued to refuse all five out-of-corpus questions:

```text
What is the capital of Mongolia?                                  refused
How do I change the oil in a diesel engine?                      refused
Who won the 1994 World Cup?                                      refused
What is the recommended dosage of ibuprofen for a headache?      refused
How do I write a for loop in Rust?                               refused

Gate refused 5 of 5.
```

**Did it help?**

The hybrid search did not clearly improve the system on my five acceptance criteria. The before and after results were the same for every criterion, and the specific pass/fail ranking issue I was trying to improve remained: `thread_pass_fail.txt#1` stayed above the more useful `thread_pass_fail.txt#0`.

The change did alter some of the lower-ranked chunks returned for the test questions, but that did not affect whether the correct information was available or whether the generated answers were correct. The experiment therefore showed that adding BM25 to the current semantic retrieval was not enough to improve this particular ranking issue.

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

The main issue that is still unresolved is retrieval ranking for some questions. For the pass/fail question, the chunk containing usage-limit information still ranked above the chunk containing the more directly useful answer about when to use pass/fail.

If I continued working on this, I would test a different retrieval-ranking approach or adjust how exact terms from the question influence ranking. I stopped after the hybrid-search experiment because this unit requires one measured improvement, and I wanted to preserve a clear before-and-after comparison rather than keep tuning the system after seeing the results.

The original version of Criterion 5 also remains a poor way to measure answer correctness because it requires an exact expected phrase. I documented a Unit 2 revision that checks whether the expected facts are present instead of requiring identical wording.

## What I'd Do Differently

If I wrote the criteria again, I would make Criterion 1 stricter.

Instead of:

> For at least 4 of my 5 test questions, the retrieved chunks include one that contains the answer.

I would use something like:

> For at least 4 of my 5 test questions, one of the top 3 retrieved chunks contains the answer.

The original criterion only measured whether the correct information appeared somewhere in the top 5, so it did not capture ranking quality. My tests showed that the system could meet the criterion even when a less useful chunk ranked above the chunk containing the direct answer.

I would also avoid using one exact full phrase as the expected-answer measurement in Criterion 5. I would define the key facts that must appear so paraphrases can still count as correct.
