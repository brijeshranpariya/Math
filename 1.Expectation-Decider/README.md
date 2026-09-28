# Expectation Decider 🎲

This practical is where I finally got probability to stop feeling abstract. I didn't want to just memorise formulas, so I took a dataset of 200 students and used Python to answer real questions about it. How likely is a student to pass? Does joining group discussions actually help? What happens when you pick three students at random?

Everything is in one Jupyter notebook: [Expectation_Decider_Probability_Analysis.ipynb](Expectation_Decider_Probability_Analysis.ipynb).

---

## The data

[student_exam_pass_dataset.csv](student_exam_pass_dataset.csv) has 200 rows, one per student, with these columns:

| Column                | What it means                                              |
| --------------------- | ---------------------------------------------------------- |
| `study_hours`         | Hours the student studied                                  |
| `attendance`          | Attendance percentage                                      |
| `group_discussion`    | Whether they took part in group discussions (`Yes` / `No`) |
| `previous_test_score` | Score on an earlier test                                   |
| `final_exam_pass`     | The outcome: `Pass` or `Fail`                              |

---

## What I did, step by step

### 1. Brushed up on the basics

Before writing any code I went back over the vocabulary: experiment, outcome, sample space, event, complement, intersection (AND), union (OR), conditional probability, independent events and mutually exclusive events. Once those were clear, the rest was mostly turning the definitions into pandas code.

### 2. Empirical vs. theoretical probability

- **Empirical:** I counted how many students actually passed. 69 out of 200 did, so **P(Pass) = 0.345**.
- **Theoretical:** if `Yes` and `No` for group discussion were equally likely, **P(Yes) = 0.5**.

Putting the two side by side showed me the difference between "what the data says" and "what we'd assume on paper".

### 3. Random variables and a probability distribution

I defined **X = the number of students who pass when 3 are picked at random**, and used the binomial formula with p = 0.345:

| X   | P(X)  |
| --- | ----- |
| 0   | 0.281 |
| 1   | 0.444 |
| 2   | 0.234 |
| 3   | 0.041 |

- **Mean (expected value)** = n·p = **1.035**
- **Variance** = n·p·q = **0.678**

So if you pick 3 students, you'd expect about one of them to pass. That's where the project name comes from: the expectation decides.

### 4. Venn diagram breakdown

I set up two events:

- **A** = studied more than 10 hours
- **B** = attendance above 80%

Then I counted each region:

| Region  | Students |
| ------- | -------- |
| A only  | 82       |
| A and B | 68       |
| B only  | 24       |
| Neither | 26       |

### 5. Contingency table

I cross-tabulated group discussion against the final result:

|                   | Fail | Pass | Total |
| ----------------- | ---- | ---- | ----- |
| **No discussion** | 70   | 19   | 89    |
| **Discussion**    | 61   | 50   | 111   |
| **Total**         | 131  | 69   | 200   |

From this table I worked out:

- **Joint** P(Discussion AND Pass) = **0.25**
- **Marginal** P(Pass) = **0.345**
- **Conditional** P(Pass | Discussion) = **0.450**

### 6. Are the events related?

To test independence I checked whether P(A ∩ B) = P(A) · P(B):

- P(Discussion) × P(Pass) = 0.555 × 0.345 = **0.191**
- Actual joint probability = **0.25**

The two numbers don't match, so **group discussion and passing are dependent**. Students who joined discussions passed more often (45% compared with 34.5% overall). They also aren't mutually exclusive, since plenty of students both discussed and passed.

### 7. Bayes' theorem

To finish, I worked a Bayes problem with these given values:

- P(High Attendance | Pass) = 0.70
- P(High Attendance | Fail) = 0.40
- P(High Attendance) = 0.60

I first used the law of total probability to get **P(Pass) ≈ 0.667**, then applied Bayes:

> **P(Pass | High Attendance) ≈ 0.778, or about 77.8%**

Note: these numbers come from the problem statement, not the dataset, so the P(Pass) here isn't the same as the 0.345 from earlier.

---

## What I took away from this

- Most probability ideas come down to counting carefully and then dividing by the right total.
- Conditional probability is where the useful insights are. "Does X help?" is really asking "does P(Pass | X) differ from P(Pass)?"
- Checking independence with actual numbers made the concept much clearer than the textbook definition did.
- Bayes' theorem looks scary, but it's just rearranging the relationship between P(A|B) and P(B|A).

---

## How to run it

1. Make sure you have Python with `pandas`, `numpy` and `matplotlib`:
   ```bash
   pip install pandas numpy matplotlib jupyter
   ```
2. Keep the CSV in the same folder as the notebook.
3. Open the notebook and run the cells from top to bottom:
   ```bash
   jupyter notebook Expectation_Decider_Probability_Analysis.ipynb
   ```

---

## Possible next steps

- Draw an actual Venn diagram (with `matplotlib-venn`) instead of just printing the counts.
- Look at how `previous_test_score` relates to passing, since I haven't used that column yet.
- Plot the binomial distribution as a bar chart.
