# AES 409/509 — Lab: Containers — Answers
- **Name:** Sam Kimball
- **Date:** 9/23/2026
Replace the placeholder text in each slot. Leave the headings alone.
Slots marked **AES 509 only** are extra work for 509 students. If you are in 409,
leave them blank rather than deleting them.
Save this file in your course repository as `lab1/lab1_containers.md`, on a branch
called `lab1`.
Problems 1 and 2 are the in-lab target. Fill those in, then commit, push, and open
the pull request before you leave the room. Finish problems 3 and 4 afterwards,
fill in the rest of this file, and push again — the pull request updates itself.
The pull request is the submission, and it wants the whole template, not the half
you had at the bell. The full instructions, with the commands, are at the front of
the handout under "How you turn this in."
---
## Problem 1 — Get it into shape, and check it
### (a) Shape
**The one line that builds `loc_of`:**
```python
#loc_of=dict(zip(gauge_ids,gauge_locs))
```
**The gauge you looked up, and what Python gave back:**
```
#loc_of['RG03'] & (34.81, -86.59)
```
**That same gauge's coordinates, read out of `lab_gauges.py` with your eyes:**
#gauge_ids is RG03 with the coordinates in latitude and longitude 34.81, -86.59, respectively
### (b) Check it before you trust it
#### i. Do I have the gauges I expect?
**What you expected:**
_what you thought the answer would be, before you ran anything_
**What you ran, and what it gave back:**
```
your code and its output
```
**Optional. What you tried first, if it did not work:**
```
leave this blank if your first try worked
```
**Which gauges are on the roster and never sent anything:**
_your answer_
**Which of the two a `False` actually supports, and what you would need in order to
tell them apa/rt:**
_a sentence or two_
#### ii. Do I have every hour from every gauge?
**What you expected:**
_what you thought the answer would be, before you ran anything_
**What you ran, and what it gave back:**
```
your code and its output
```
**Optional. What you tried first, if it did not work:**
```
leave this blank if your first try worked
```
**Which two gauges are short — the ones with fewer than five records:**
_your answer_
#### iii. Are the values physical?
**What you expected:**
_what you thought the answer would be, before you ran anything_
**What you ran, and what it gave back:**
```
your code and its output
```
**Optional. What you tried first, if it did not work:**
```
leave this blank if your first try worked
```
**How many readings are not physical rainfall:**
_the count — you need this again in (c)_
**Which gauge they are from:**
_your answer_
### (c) Drop the values that are not rainfall
**How long a cleaned list has to be, worked out before you ran anything:**
_your number, and how you got it_
**`len(clean)` after the in-place loop:**
```
the length the loop left you with
```
**Whether any fill values are still sitting in `clean`:**
```
what you found still in there
```
**What the loop did instead, in your own words:**
_your explanation_
**With the `print` added: which RG05 records the loop never looked at, and where
they were sitting when the loop went past:**
_your answer_
**The one line that builds a new list rather than reaching into the old one:**
```python
# your line here
```
**Its length, against the number you wrote down:**
```
the length, and whether it matches
```
**What you called this cleaned list:**
_the name you gave it — problem 4 calls it `records`_
**`len(readings)` when you are done, confirming it is still 23:**
```
the length
```
---
## Problem 2 — Which gauge caught the most?
### (a) Do one of them by hand first
**RG01's five hourly readings, and the total, added up on paper:**
_the five readings, and your total for RG01, from paper_
### (b) Build the totals
**The version that uses `in`:**
```python
# your code here
```
**AES 509 only. The second version, using `total_by_gauge.get(gauge, 0.0)`:**
```python
# your code here
```
**AES 509 only. Whether the two versions give you the same dictionary:**
_your answer, and how you checked_
**AES 509 only. Which of the two you would rather read six months from now, and
why:**
_a sentence_
**Optional. What you tried first, if it did not work:**
```
leave this blank if your first try worked
```
### (c) Check it against (a)
**The whole dictionary, printed:**
```
the output
```
**`total_by_gauge['RG01']` against the total you added up on paper:**
_the two numbers, and whether they agree_
**How many keys there should be, worked out before you counted:**
_your number, and how you got it_
**How many keys there are:**
```
the count
```
### (d) Which gauge caught the most?
**What `max(total_by_gauge)` gave back:**
```
the output
```
**Whether that agrees with the totals sitting above it on your screen:**
_your answer_
**What `max` was actually looking at:**
_a sentence_
### (e) Get the real answer
**The code:**
```python
# your code here
```
**The gauge that caught the most, and its storm total:**
_your answer_
### (f) Where was it?
**The latitude and longitude of the gauge that caught the most:**
```
your lookup and its output
```
---
## Problem 3 — The whole network in one container
### One line before you start — `rain_at`
**The one line that builds `rain_at` from the cleaned records:**
```python
# your line here
```
**Its length — it should be 18, one key per cleaned record:**
```
len(rain_at) and the count it gave back
```
### (a) Build it
**The loop that builds `network`:**
```python
# your code here
```
**Optional. What you tried first, if it did not work:**
```
leave this blank if your first try worked
```
**How many hours RG03's inner rainfall dictionary should hold, written down before
you counted:**
_your number, and how you got it — 1(b)(ii) counted RG03's records, and none of
them were fill_
**How many hours it actually holds:**
```
len(...) and the count it gave back
```
**One entry, printed out:**
```
the output
```
**How many keys `network` should have, written down before you counted:**
_your number, and how you got it_
**How many it has:**
```
the count
```
### (b) Ask it something
#### i. How much fell at RG03 in hour 2?
**The two lines, next to each other:**
```
the rain_at lookup and its output
the network lookup and its output
```
**Which you would rather have:**
_a sentence_
#### ii. What is RG03's storm total?
**The code, and the total rounded to one decimal place:**
```
your code and its output
```
**Whether it matches what you got in 2(e):**
_your answer_
### (c) AES 509
**AES 509 only. What `snapshot['RG03'] is network['RG03']` says:**
```
the output
```
**AES 509 only. What `dict(network)` copied, and what it did not:**
_a sentence or two_
**AES 509 only. `network['RG03']['rain'][2]` after you put it back, confirming it
reads 15.2:**
```
the output
```
**AES 509 only. What a copy you could really scribble on would have to copy:**
_a sentence_
---
## Problem 4 — Five containers, one fact
### (a) Which one answers which?
#### i. Where is RG05?
**Which of the five you would reach for:**
_container name_
**Exactly what you would type, and what it gave back:**
```
your code and its output
```
**One thing that container is bad at:**
_a sentence_
#### ii. Did RG07 report at 03Z?
**First route — what you typed, what it gave back, and what it actually checked:**
```
your code and its output
```
_what that one actually checked_
**Second route — what you typed, what it gave back, and what it actually checked:**
```
your code and its output
```
_what that one actually checked_
**Third route — what you typed, what it gave back, and what it actually checked:**
```
your code and its output
```
_what that one actually checked_
**The object in your session — not one of the five — that is the only thing which
knows RG07 is supposed to exist:**
_its name, and where it came from_
### (b) A question none of them is built for
**How much fell across the whole network in each hour:**
```
the per-hour totals
```
**Which hour was the heaviest:**
_your answer_
**Which two of the five cannot answer this at all:**
_the two container names_
**What got thrown away in each of those two:**
_a sentence_
**The three routes that do work, and what each one gave back:**
```
your code and its output
```
**Whether all three agree:**
_your answer_
**The thing you had to do all three times that you did not have to do either time
in (a):**
_a sentence_
### (c) A correction arrives
**The code that fixes the RG01 hour 2 record in `records`:**
```python
# your code here
```
**The check that it took:**
```
your code and its output
```
**What each of the other four says about RG01 now:**
```
rain_at[('RG01', 2)] ->
network['RG01']['rain'][2] ->
total_by_gauge['RG01'] ->
loc_of['RG01'] ->
```
**How many of your five containers now disagree about a single number:**
_a number_
**How many of them said so:**
_a number_
#### i. One of the four was never going to be affected
**Which:**
_container name_
**Why:**
_a sentence_
#### ii. `network`'s number for RG01 hour 2 did not follow the record you just
changed
**What is sitting there — the record itself, or a value copied out of it when your
loop ran in 3(a):**
_a sentence_
**Which of the two `rain_at` is holding:**
_a sentence_
#### iii. The three answers from (b), recomputed
**What the three give back when you run them again:**
```
the three answers
```
**Which one is still right:**
_your answer_
#### iv. Putting it straight
**What you would have to do to put all five straight:**
_a sentence_
**How you would find out if you had missed one:**
_a sentence_
### (d) AES 509
**AES 509 only. The check that nothing gave you:**
```python
# your code here
```
**AES 509 only. What it printed, run while the containers were still out of step:**
```
the output
```
**AES 509 only. What it printed after you rebuilt the three containers:**
```
the output
```
**AES 509 only. What it would take to check `total_by_gauge` the same way:**
_a sentence_
**AES 509 only. Why that one is harder than the other two:**
_a sentence_
