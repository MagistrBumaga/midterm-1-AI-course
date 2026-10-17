# Midterm 1: answers

GitHub username:magistrbumaga
Session code:kettle-62
Fingerprint:068e58b1

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration
I used ChatGPT to help me organize and understand data from three files: DATA.md, retrieval_ranks.csv, and doc_results.csv.

The main reason I used AI was to save time when working with files and finding the right formulas. Instead of opening the same files many times and searching for information again, I used ChatGPT to collect and organize the necessary data for each task.

I worked with the information step by step. ChatGPT helped me find the formulas, understand how they work, calculate and check the numbers, and gave suggestions for some written answers.

AI made the process faster and easier, especially when working with a lot of numbers and information. I used its explanations to understand the results and compare different options. I am responsible for checking my final answers and explaining my decisions.

(About half a page: which assistants or tools you used and for what. If you used none, say so.)

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.65 | 0.7167 | 0.733 | 0.69 |
| B | 0.4833 | 0.65 | 0.7 | 0.57 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3  |0.7167  |0.5923  |0.8149  |
| A, k=5  |0.7333  |0.6099  |0.8287  |
| B, k=3  |0.65  |0.5236  |0.7583  |
| B, k=5  |0.7  |0.5749  |0.801  |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 31814.21|
| A, k=5 | 42911.53|
| B, k=3 | 19194.09|
| B, k=5 | 21878.00|

### 1d. Decision (at most 150 words)

Chosen candidate:
I recommend B, k=5* because accuracy is more important for a bank. Both A options are over the budget of $26,800, so I would choose between B, k=3 and B, k=5.
B, k=3 is also a good option because it costs $19,194.09 per year and has 65% recall. B, k=5 costs $21,878 and has 70% recall. Their Wilson intervals overlap, so we cannot be sure that B, k=5 is actually better. However, B, k=5 found 3 more correct answers out of 60 questions. I think paying almost $2,700 more per year is acceptable for a bank if it helps reduce mistakes. Accuracy is worth paying for. For B, k=5, recall by language is EN 80%, RU 60%, and KK 70%. Russian users have the lowest recall.
(Decision, using your intervals and the budget. Give the recall of your choice for EN, RU and KK separately.)

### 1e. Next measurement (at most 60 words)
I would test B, k=3 and B, k=5 on 300 new questions, with 100 questions for each language . I would focus more on difficult Russian questions because they have the lowest recall. This would help us understand if B, k=5 is really better and if paying an extra $2,700 per year is worth it for the bank.
## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.8944|KK, clean |0.8333 |0.6644 |0.9266 |6274.35 |
| L | 0.8|KK, clean |0.6 |0.4232 |0.7541 |10800 |

Personal-data pages per year sent abroad under H:295650

### 2b. Memo (at most 300 words)
I recommend Pipeline L because I think keeping personal data inside the bank is very important. Pipeline H has better accuracy (89.44%) and costs only $6,274.35 per year. Pipeline L has 80% accuracy and costs $10,800 per year. Both systems have the lowest accuracy on clean Kazakh documents. H gets 83.33% (Wilson interval 66.44–92.66%), while L gets 60% (42.32–75.41%). The intervals overlap, so we cannot be completely sure about the difference. The overall accuracy also hides how poorly L works with Kazakh documents. With Pipeline H, all scans go to a foreign data centre, including 295,650 pages per year containing personal data. We need to check who stores these documents, where they are stored and how long they are kept. We also need to check the legal requirements because some documents contain an IIN. Pipeline L processes documents on the bank's server in Kazakhstan. The bank controls the storage, but we still need to define how long documents and backups are kept and who can access them.
For safety, I would use encryption, limited access, access logs and regular checks that old documents are deleted. I would also require manual checks for uncertain results, especially Kazakh documents. I choose L because I prefer keeping personal information under the bank's control. However, L still has a serious accuracy problem with Kazakh documents, so more testing and improvements are needed before full use.
## Part 3: Explain your own numbers

### Q1

Number:2.54
Why (at most 40 words): Input and output tokens have different prices. Output tokens cost more than input tokens. The Kazakh request uses more tokens, but the total price also depends on how many input and output tokens are used.

### Q2

Number:35.1%
Why (at most 40 words):Only the English chunk is too large. It has 789 tokens, but the model can read only 512. Russian and Kazakh chunks fit the limit. The extra English tokens are cut off and are not used to create the embedding.

### Q3

Number:93.7%
Why (at most 40 words):A low temperature makes the model more likely to choose the highest-scoring token, which is Monday. The order of tokens stays the same. All five tokens can still be selected, but some have very small probabilities.

### Q4

Number:18.3%
Why (at most 40 words):I left out multiplication and number sequences because the model can learn rules to solve them. Account numbers and birthdays are random facts, so rare facts are harder to predict. A system prompt cannot teach the model missing facts.

### Q5

Number:17.80
Why (at most 40 words): This model uses 356 tokens per parameter, compared with 20 for Chinchilla. It was trained on much more data for its size, so it is over-trained by this rule. Chinchilla showed that GPT-3-like models were under-trained.
