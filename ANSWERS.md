# Midterm 1: answers

GitHub username:bauka123
Session code:kettle-62
Fingerprint:b24d2584

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration
I used ChatGPT to help understand the assignment, interpret the retrieval and document-processing results, check formulas, and draft the explanations in this file. I also used Python calculations to compute recall, MRR, Wilson confidence intervals, costs, and the numerical answers in Part 3. The individual exam data came from the course's own make_my_data.py generator, using my GitHub username and the session code. I did not use another student's answers. AI helped with calculations and language, but I am responsible for checking the final results and being able to explain my choices in the oral check. In particular, my recommendations consider the budget, uncertainty in a small test set, differences between languages, and the personal-data risks of sending scans abroad.

(About half a page: which assistants or tools you used and for what. If you used none, say so.)

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A	|0.6167|0.8000|	0.8833|0.7183|
| B	|0.3500|0.4667|	0.5500|0.4259|

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A |k=3|0.8000|0.6822|0.8817|
| A |k=5|0.8833|0.7782|0.9423|
| B |k=3|0.4667|0.3463|0.5911|
| B |k=5|0.5500|0.4249|0.6691|

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
|A|k=3|21013.00|
|A|k=5|28050.78|
|B|k=3|13470.22|
|B|k=5|15479.49|

### 1d. Decision (at most 150 words)

Chosen candidate: A, k=3
I recommend A,k=3.Its annual cost is $21,013,which is below the $24,500 budget, and its recall is 48\60=80%(95% Wilson interval : 68.22%-88,17%).The other A option k=5 has higher observed recall88.33%,but costs $28,050,78 and exceeds the budget; its interval overlaps A,k=3 so we cannot establish a difference from this test.Both B options are cheaper, bit their recall intervals do not overlap the interval for A,k=3(B,k=5 reaches only 66.91% and its upper bound).The B intervals overlap each other's their performance is not established.For A k=3 recall is EN 17\20=85%,RU 18\20=90% and KK 13\20=65%.Kazakh-language users are served worsts I would investigate Kazakh retrieval errors before deployment.


(Decision, using your intervals and the budget. Give the recall of your choice for EN, RU and KK separately.)

### 1e. Next measurement (at most 60 words)
Collect at least 200 new Kazakh-language questions from realistic support cases, then measure recall@3 with confidence intervals for A and B.Include questions whose correct answer appears near chunk boundaries.If Kazakh recall remains lowest language-specific chunking and re-ranking before launch.
## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H |0.9000|KK degraded|0.8000|0.6269|0.9050|9139.60|
| L |0.7944|KK degraded|0.5333|0.3614|0.6977|14400.00|
Personal-data pages per year sent abroad under H:

### 2b. Memo (at most 300 words)
To:Head of Operations 
I recommend pipeline l for now, with human verification of uncertain invoices.H is more accurate overall 162\180=90% and cheaper $9,139.60/year than L 142/180=79,44%;$14,400/year.However, these overall scores hide weaker service for degraded Kazakh scans.H gets 24/30 correct 8-%;95% Wilson interval 62,69%-90.50%,while L gets 16/30 53,33%;interval 36,14%-69.77%.The intervals overlaps this small sample does not prove a statistically clear difference in that cell.

For H, every image goes to an overseas data centre; 554,800 pages per year are expected to contain names and IINs. Kazakhstan's personal-data law No. 94-V covers IINs. Before using H, we must establish where scans are transferred, who can store or access the raw images and derived results (including subcontractors), and how long each copy and backup is retained. Vendor claims alone cannot establish compliance. For L, scans stay on the bank's server in Kazakhstan and the bank controls storage, but we must still confirm the exact storage location, authorized users, retention period, and backups.

Verifiable safeguards for L are encrypted storage and network connections, role-based access with reviewed logs, a documented deletion schedule tested against backups, and monthly accuracy audits by language and scan condition. Send low-confidence or degraded Kazakh scans to human review before payment processing. Test that the system sends no scans to external endpoints.

L costs more and misses more fields, especially on degraded Kazakh scans. That operational error risk remains and needs monitoring. Reconsider H only after a formal privacy/compliance review and a documented, tested data-handling design.
## Part 3: Explain your own numbers

### Q1

Number:2.29
Why (at most 40 words):
The total-token ratio is about 2.41,but output tokens cost six times more than input tokens.English and Kazakh have different input/output proportions, so their total-token ratio and cost ratio are different.

### Q2

Number:44,4%
Why (at most 40 words):
English has about 920.5 e5 tokens, so its part after token 512 is cut.Russian has 448 and Kazakh 266,so neither is cut.The missing English text cannot influence its embedding.
### Q3

Number:64.6
Why (at most 40 words):
Top-p keeps the smallest set whose cumulative probability reaches 0.8:Monday and Friday.It removes other tokens and renormalizes.Lower temperature instead makes probabilities sharper without directly removing tokens from the sample space.
### Q4

Number:+6
Why (at most 40 words):
The public leaderboard X 69 correct versus 66.This rewards guessing even when model is unsure.With penalties,y gets 44 points and X gets 38,so careful abstention is better.
### Q5
14,10
Number:
Why (at most 40 words):This model uses 282 tokens per parameter versus Chinchilla's 20,so its over-trained relative to that compute-optimal ratio.Chinchilla argued that large models such as GPT-3 had too few training tokens for their size.
