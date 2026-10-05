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
loc_of=dict(zip(gauge_ids,gauge_locs))
```
**The gauge you looked up, and what Python gave back:**
```
loc_of['RG03']
(34.81, -86.59)
```
**That same gauge's coordinates, read out of `lab_gauges.py` with your eyes:**
```
gauge_ids is RG03 with the coordinates in latitude and longitude 34.81, -86.59, respectively
```
### (b) Check it before you trust it
#### i. Do I have the gauges I expect?
**What you expected:**
```
Looking between the roster and gauge dataset, before running anything I would expect RG06 and RG07 to not send anything as neither of them are listed for the gauge_ids
```
**What you ran, and what it gave back:**
```
input:
actual={r['gauge'] for r in readings}
output:
{'RG05', 'RG01', 'RG03', 'RG04', 'RG02'}
```
**Optional. What you tried first, if it did not work:**
```

```
**Which gauges are on the roster and never sent anything:**
```
RG06 & RG07
```
**Which of the two a `False` actually supports, and what you would need in order to
tell them apa/rt:**
```
False reports that an item that you are requesting does not exist and does not explain on whether a gauge has no rain as in False reports that _no reading arrived_. In order to distinguish the difference the dataset would need to include an explicit number for a no rain reading such as 0.0 and a number representing no readings such as -999.0.
```
#### ii. Do I have every hour from every gauge?
**What you expected:**
```
#RG03 & RG04
```
**What you ran, and what it gave back:**
```
input:
for gauge in roster:
    hours = {
        r["hour"]
        for r in readings
        if r["gauge"] == gauge
    }

    if 0 < len(hours) < 5:
        print(gauge, sorted(hours))

output:
RG03 [0, 1, 2, 3]
RG04 [0, 1, 2, 4]
```
**Optional. What you tried first, if it did not work:**
```

```
**Which two gauges are short — the ones with fewer than five records:**
```
RG03 & RG04
```
#### iii. Are the values physical?
**What you expected:**
```
RG05: 5 readings of -999.00
```
**What you ran, and what it gave back:**
```
input:
negative_gauges={r["gauge"] for r in readings if r["rain_mm"] <0}
output:
{'RG05'}

```
**Optional. What you tried first, if it did not work:**
```

```
**How many readings are not physical rainfall:**
```
There are 5 readings with -999.00
```
**Which gauge they are from:**
```
Gauge RG05
```
### (c) Drop the values that are not rainfall
**How long a cleaned list has to be, worked out before you ran anything:**
```
The clean list needs to include 18 values. I made sure to count each rain_mm in gauges 1 - 4 (RG01 - RG04). RG01 & RG02 each have 5 readings while RG03 & RG04 each have 4 readings. 5 + 5 + 4 + 4 = 18 readings total.
```
**`len(clean)` after the in-place loop:**
```
20
```
**Whether any fill values are still sitting in `clean`:**
```
There are values of -999.00 still sitting in RG05
```
**What the loop did instead, in your own words:**
```
The clean variable is looking through each of the listings from each gauge and is checking if the reading is below 0. The variable then is proceeded through a "for loop" as clean moves to the next position after it is done looking at the first one. After the for loop comes the remove function, which leads to len(clean) outputting 20. This is because after removing the first -999.00, as it is below 0, the for loop looks for the next value in the set. But now the list has shifted left due to the initially removed value while the for loop is now looking at the new second value. 
```
**With the `print` added: which RG05 records the loop never looked at, and where
they were sitting when the loop went past:**
```
hour 1
hour 3
```
**The one line that builds a new list rather than reaching into the old one:**
```
clean_Prob_1 = [rec for rec in readings if rec['rain_mm'] >=0]
```
**Its length, against the number you wrote down:**
```
len(clean_prob_1) outputs 18 which corresponds with the number I wrote down previously
```
**What you called this cleaned list:**
```
clean_Prob_1
```
**`len(readings)` when you are done, confirming it is still 23:**
```
len(readings) outputs 23 while len(clean_Prob_1) outputs 18. All is well! 
```
---
## Problem 2 — Which gauge caught the most?
### (a) Do one of them by hand first
**RG01's five hourly readings, and the total, added up on paper:**
```
0.3 + 5.1 + 12.7 + 6.4 + 0.8 = 25.3 mm
```
### (b) Build the totals
**The version that uses `in`:**
```
input:
total_by_gauge={}
for record in readings:
    gauge = record['gauge']
    rain = record['rain_mm']

    if rain < 0:
        continue

    if gauge in total_by_gauge:
        total_by_gauge[gauge] += rain
    else:
        total_by_gauge[gauge] = rain
output:
total_by_gauge
{'RG01': 25.3, 'RG02': 23.299999999999997, 'RG03': 28.099999999999998, 'RG04': 8.299999999999999}
```
**AES 509 only. The second version, using `total_by_gauge.get(gauge, 0.0)`:**
```
input:
total_by_gauge_get={}
for record in readings:
    gauge = record['gauge']
    rain = record['rain_mm']

    if rain < 0:
        continue

    total_by_gauge_get[gauge] = total_by_gauge_get.get(gauge, 0.0) + rain
output:
total_by_gauge_get
{'RG01': 25.3, 'RG02': 23.299999999999997, 'RG03': 28.099999999999998, 'RG04': 8.299999999999999}
```
**AES 509 only. Whether the two versions give you the same dictionary:**
```
The 2 versions are the same dictionary. I used print(total_by_gauge == total_by_gauge_get) which outputted True, meaning all values within the dictionary are the same as indicated by the == check.
```
**AES 509 only. Which of the two you would rather read six months from now, and
why:**
```
Personally I would rather read the total_by_gauge_get dictionary in 6 months as it starts from 0.0 mm then progressively adds the amount of rainfall (+rain) afterwards. The +rain is more applicable in this situation.
```
**Optional. What you tried first, if it did not work:**
```

```
### (c) Check it against (a)
**The whole dictionary, printed:**
```
input:
total_by_gauge
output:
{'RG01': 25.3, 'RG02': 23.299999999999997, 'RG03': 28.099999999999998, 'RG04': 8.299999999999999}
```
**`total_by_gauge['RG01']` against the total you added up on paper:**
```
total_by_gauge['RG01'] gives 25.3 which is the number I added up on paper. Everything agrees! 
```
**How many keys there should be, worked out before you counted:**
```
There should be 4 keys in the dictionary. Each gauge should have a corresponding total. 4 keys (gauges) 4 values (totals)
```
**How many keys there are:**
```
4 keys 
```
### (d) Which gauge caught the most?
**What `max(total_by_gauge)` gave back:**
```
RG04
```
**Whether that agrees with the totals sitting above it on your screen:**
```
RG04 does not agree with the highest total of rainfall from a specific gauge as that title should be given to RG01.
``` 
**What `max` was actually looking at:**
```
From page 99 of py509_notes.pdf : "max on a dictionary gives you back the largest key, not the
largest value". RG04 is the greatest because 4 is greater than the rest of the gauge's numbers. 
```
### (e) Get the real answer
**The code:**
```
max(total_by_gauge, key=total_by_gauge.get)
```
**The gauge that caught the most, and its storm total:**
```
RG03 & ~28.1 mm of rainfall
```

### (f) Where was it?
**The latitude and longitude of the gauge that caught the most:**
```
RG03 is located 34.81 degrees by -86.59 degrees 
```
---
## Problem 3 — The whole network in one container
### One line before you start — `rain_at`
**The one line that builds `rain_at` from the cleaned records:**
```
rain_at = {(record['gauge'], record['hour']): record['rain_mm'] for record in readings if record['rain_mm'] >= 0}
```
**Its length — it should be 18, one key per cleaned record:**
```
input:
len(rain_at)
output:
18
```
### (a) Build it
**The loop that builds `network`:**
```
network={}
                    
for record in clean_Prob_1:
    gauge = record['gauge']

    if gauge not in network:
        network[gauge] = {
            'loc': loc_of[gauge],
            'rain': {}
        }

    network[gauge]['rain'][record['hour']] = record['rain_mm']
```
**Optional. What you tried first, if it did not work:**
```

```
**How many hours RG03's inner rainfall dictionary should hold, written down before
you counted:**
```
There should be 4 hours that RG03’s inner rainfall dictionary should hold. 1 for 00Z then 2 at 01Z and so on and so forth. 
```
**How many hours it actually holds:**
```
input:
len(network['RG03']['rain'])
output:
4
```
**One entry, printed out:**
```
input:
network['RG03']
output:
{'loc': (34.81, -86.59), 'rain': {0: 1.0, 1: 7.6, 2: 15.2, 3: 4.3}}
```
**How many keys `network` should have, written down before you counted:**
```
Network should have 4 keys within its dictionary: gauge, loc, rain, hour
```
**How many it has:**
```
input:
len(network)
output:
4
```
### (b) Ask it something
#### i. How much fell at RG03 in hour 2?
**The two lines, next to each other:**
```
input:
rain_at[('RG03',2)]
output:
15.2

input:
network['RG03']['rain'][2]
output:
15.2
```
**Which you would rather have:**
```
I would much rather use the "rain_at" selection as it has less typing to find out how much rain occurred at which gauge
```
#### ii. What is RG03's storm total?
**The code, and the total rounded to one decimal place:**
```
input:
round(sum(network['RG03']['rain'].values()), 1)
output:
28.1
```
**Whether it matches what you got in 2(e):**
```
The answer I got in 2e does match the answer I have for this question
```
### (c) AES 509
**AES 509 only. What `snapshot['RG03'] is network['RG03']` says:**
```
input:
snapshot['RG03'] is network['RG03']
output:
True
```
**AES 509 only. What `dict(network)` copied, and what it did not:**
```
dict(network) copied the outer dictionary, the network itself, but not the nested dictionaries such as the values of each gauge. 
```
**AES 509 only. `network['RG03']['rain'][2]` after you put it back, confirming it
reads 15.2:**
```
input:
network['RG03']['rain'][2] = 15.2
network['RG03']['rain']
output:
15.2
```
**AES 509 only. What a copy you could really scribble on would have to copy:**
```
Copying the outer dictionary and all the nested dictionaries inside it
```
## Problem 4 — Five containers, one fact
### (a) Which one answers which?
#### i. Where is RG05?
**Which of the five you would reach for:**
```
loc_of
```
**Exactly what you would type, and what it gave back:**
```
input:
loc_of['RG05']
output:
(34.9, -86.52)
```
**One thing that container is bad at:**
```
loc_of is bad at telling the user whether a gauge exists or not within the network 
```
#### ii. Did RG07 report at 03Z?
**First route — what you typed, what it gave back, and what it actually checked:**
```
input:
'RG07' in gauge_ids
output:
False

Checks if RG07 is in the list of gauges with locations
```
**Second route — what you typed, what it gave back, and what it actually checked:**
```
input:
'RG07' in network
output:
False

Checks if RG07 has an entry in the network built from the records
```

**Third route — what you typed, what it gave back, and what it actually checked:**
```
input:
any(r['gauge'] == 'RG07' and r['hour'] == 3 for r in readings)
output:
False

Checks if there is an actual raw reading for RG07 at hour 3
```

**The object in your session — not one of the five — that is the only thing which
knows RG07 is supposed to exist:**
```
'RG07' in roster
Roster is the only place where RG07 is contained 
```
### (b) A question none of them is built for
**How much fell across the whole network in each hour:**
```
00Z : 1.3 mm
01Z: 18.5 mm
02Z: 43.9 mm
03Z: 18.8 mm
04Z: 2.5 mm
```
**Which hour was the heaviest:**
```
Hour 02Z was the heaviest with 43.9 mm of rain
```
**Which two of the five cannot answer this at all:**
```
loc_of and total_by_gauge cannot answer question 4(b)
```
**What got thrown away in each of those two:**
```
loc_of only stores each gauge's location and has no rainfall data while total_by_gauge contains totals in general and not each total for each hour
```
**The three routes that do work, and what each one gave back:**
```
input:
hourly_records = {}

for record in records:
    if record['rain_mm'] < 0:
        continue

    hour = record['hour']
    hourly_records[hour] = hourly_records.get(hour, 0) + record['rain_mm']
output:
{0: 1.3, 1: 18.5, 2: 43.9, 3: 18.8, 4: 2.5}

input:
hourly_rain_at = {}

for (gauge, hour), rain in rain_at.items():
    hourly_rain_at[hour] = hourly_rain_at.get(hour, 0) + rain
output:
{0: 1.3, 1: 18.5, 2: 43.9, 3: 18.8, 4: 2.5}

input:
hourly_network = {}

for gauge in network:
    for hour, rain in network[gauge]['rain'].items():
        hourly_network[hour] = hourly_network.get(hour, 0) + rain
output:
{0: 1.3, 1: 18.5, 2: 43.9, 3: 18.8, 4: 2.5}
```
**Whether all three agree:**
```
All 3 do agree with the same outcome
```
**The thing you had to do all three times that you did not have to do either time
in (a):**
```
Make an empty dictionary and then fill it 
```
### (c) A correction arrives
**The code that fixes the RG01 hour 2 record in `records`:**
```
for record in records:
    if record['gauge'] == 'RG01' and record['hour'] == 2:
        record['rain_mm'] = 2.7
```
**The check that it took:**
```
input:
for record in records:
    if record['gauge'] == 'RG01' and record['hour'] == 2:
        print(record)
output:
{'gauge': 'RG01', 'hour': 2, 'rain_mm': 2.7}
```
**What each of the other four says about RG01 now:**
```
rain_at[('RG01', 2)] -> 12.7 
network['RG01']['rain'][2] -> 12.7 
total_by_gauge['RG01'] -> 25.3
loc_of['RG01'] -> (34.72, -86.64)
```
**How many of your five containers now disagree about a single number:**
```
3 out of the 5 disagree 
```
**How many of them said so:**
```
2 said so
```
#### i. One of the four was never going to be affected
**Which:**
```
loc_of
```
**Why:**
```
loc_of only contained gauge locations, not rainfall
```
#### ii. `network`'s number for RG01 hour 2 did not follow the record you just
changed
**What is sitting there — the record itself, or a value copied out of it when your
loop ran in 3(a):**
```
Value copied out of the record when the loop in 3(a) ran
```
**Which of the two `rain_at` is holding:**
```
rain_at is holding a copied value
```
#### iii. The three answers from (b), recomputed
**What the three give back when you run them again:**
```
hourly_records: {0: 1.3, 1: 18.5, 2: 33.9, 3: 18.8, 4: 2.5}
hourly_rain_at: {0: 1.3, 1: 18.5, 2: 43.9, 3: 18.8, 4: 2.5}
hourly_network: {0: 1.3, 1: 18.5, 2: 43.9, 3: 18.8, 4: 2.5}
```
**Which one is still right:**
```
records is the only right one as the other 2 still contain the old 12.7 value
```
#### iv. Putting it straight
**What you would have to do to put all five straight:**
```
For loc_of you would have to rebuild from gauge_ids and gauge_locs
For total_by_gauge you would need to recompute the gauge totals from the corrected records
For rain_at you would need to rebuild the (gauge, hour) from the rainfall dictionary
For the network, rebuild it from the corrected records and loc_of
Records is already the corrected source data
```
**How you would find out if you had missed one:**
```
Compare the derived containers, such as loc_of, total_by_gauge, rain_at, and network, to the original readings 
```
### (d) AES 509
**AES 509 only. The check that nothing gave you:**
```
for record in records:
    gauge = record['gauge']
    hour = record['hour']
    rain = record['rain_mm']

    if rain < 0:
        continue

    if rain_at[(gauge, hour)] != rain:
        print('rain_at mismatch:', gauge, hour)

    if network[gauge]['rain'][hour] != rain:
        print('network mismatch:', gauge, hour)

```
**AES 509 only. What it printed, run while the containers were still out of step:**
```
rain_at mismatch: RG01 2
network mismatch: RG01 2
```
**AES 509 only. What it printed after you rebuilt the three containers:**
```

```
**AES 509 only. What it would take to check `total_by_gauge` the same way:**
```
To check total_by_gauge the same way you would have to recalculate each gauge’s total from records and compare that total with total_by_gauge[gauge]
```
**AES 509 only. Why that one is harder than the other two:**
```
It is harder because total_by_gauge has only one number per gauge while rain_at and network does not keep the individual hour values to compare record by record.
```
