# Prompt Engineering

## Task 2 – Prompt Engineering

**Internship:** Generative AI Internship
**Task:** Task 2 – Prompt Engineering
**Model:** Google Gemini 3.7
**Tool:** Google AI Studio

---

## 1. Objective

The objective of this task was to gain practical experience with prompt engineering techniques by testing different prompting approaches and comparing their outputs.

The task included three experiments:

1. Zero-shot vs Few-shot prompting
2. Direct prompting vs Step-by-step prompting
3. Structured JSON output

The experiments were performed using real prompts and actual model outputs.

---

# 2. Experiment 1 – Zero-Shot vs Few-Shot Prompting

## 2.1 Objective

The purpose of this experiment was to compare **zero-shot prompting** and **few-shot prompting** for sentiment classification.

The model was asked to classify customer reviews into:

* Positive
* Negative
* Neutral

Ten test reviews were used for both approaches.

---

## 2.2 Zero-Shot Prompt

```text
You are a sentiment classification system.
Classify the sentiment of the customer review as exactly one of:
- Positive
- Negative
- Neutral
Return only the classification label. Do not provide an explanation.
Customer review:
"<review>"
```

### Test Cases and Expected Labels

| Test | Customer Review                                                       | Expected |
| ---- | --------------------------------------------------------------------- | -------- |
| 1    | The product arrived earlier than expected and works perfectly.        | Positive |
| 2    | The headphones stopped working after only two days.                   | Negative |
| 3    | The package arrived yesterday.                                        | Neutral  |
| 4    | Customer support was extremely helpful and solved my problem quickly. | Positive |
| 5    | The quality is terrible and definitely not worth the price.           | Negative |
| 6    | The laptop has 8GB of RAM and a 512GB SSD.                            | Neutral  |
| 7    | I absolutely love this phone. The camera quality is amazing.          | Positive |
| 8    | The delivery was late and the box was damaged.                        | Negative |
| 9    | The product is okay, nothing particularly impressive.                 | Neutral  |
| 10   | Excellent build quality and very fast performance.                    | Positive |

### Raw Model Outputs

```text
1. Positive.
2. Negative.
3. Neutral.
4. Positive.
5. Negative.
6. Neutral.
7. Positive.
8. Negative.
9. Neutral.
10. Positive.
```

### Result

**Correct:** 10/10
**Accuracy:** 100%
**Output-format compliance:** 10/10

---

## 2.3 Few-Shot Prompt

```text
You are a sentiment classification system.
Classify the sentiment of the customer review as exactly one of:
- Positive
- Negative
- Neutral
Return only the classification label. Do not provide an explanation.

Here are examples:

Example 1:
Customer review: "I love this product. It works perfectly."
Classification: Positive

Example 2:
Customer review: "The product arrived broken and stopped working immediately."
Classification: Negative

Example 3:
Customer review: "The package was delivered yesterday."
Classification: Neutral

Example 4:
Customer review: "The customer service team was very helpful."
Classification: Positive

Example 5:
Customer review: "The product quality is terrible."
Classification: Negative

Now classify this review:

Customer review:
"<review>"
```

### Raw Model Outputs

```text
1. Positive.
2. Negative.
3. Neutral.
4. Positive.
5. Negative.
6. Neutral.
7. Positive.
8. Negative.
9. Neutral.
10. Positive.
```

### Result

**Correct:** 10/10
**Accuracy:** 100%
**Output-format compliance:** 10/10

---

## 2.4 Analysis

The zero-shot prompt achieved 10/10 because the task instructions were clear and the test reviews were relatively unambiguous.

The few-shot prompt also achieved 10/10. The examples demonstrated the expected classification labels and output format, but they did not improve accuracy on this particular dataset.

This shows that few-shot prompting does not necessarily improve results for every task. It can be more useful when the task is ambiguous, domain-specific, or when the desired output format is difficult to describe using instructions alone.

---

# 3. Experiment 2 – Direct vs Step-by-Step Prompting

## 3.1 Objective

The purpose of this experiment was to compare a direct prompt with a prompt that explicitly asked the model to solve a multi-step problem step by step.

### Problem

> A shop gives a 20% discount on a ₹2,500 product. After the discount, the customer pays an additional ₹100 delivery fee. How much does the customer pay in total?

**Correct answer: ₹2,100**

---

## 3.2 Direct Prompt

```text
Solve this problem and provide the final answer.

A shop gives a 20% discount on a ₹2,500 product. After the discount, the customer pays an additional ₹100 delivery fee. How much does the customer pay in total?
```

### Raw Model Output

```text
The customer pays a total of ₹2,100.

Original Price: ₹2,500
Discount (20%): ₹2,500 × 0.20 = -₹500
Discounted Price: ₹2,000
Delivery Fee: +₹100
Total Paid: ₹2,100
```

### Result

**Correct:** Yes
**Accuracy:** 100%

An interesting observation was that the model provided calculation steps even though the prompt only asked for the final answer.

---

## 3.3 Step-by-Step Prompt

```text
Solve this problem carefully.
Break the problem into clear steps, calculate each step, and then provide the final answer.

A shop gives a 20% discount on a ₹2,500 product. After the discount, the customer pays an additional ₹100 delivery fee. How much does the customer pay in total?
```

### Raw Model Output

```text
Step 1: Calculate the discount amount
20% of ₹2,500 = (20 / 100) × ₹2,500 = ₹500

Step 2: Calculate the discounted price
₹2,500 - ₹500 = ₹2,000

Step 3: Add the delivery fee
₹2,000 + ₹100 = ₹2,100

The customer pays ₹2,100 in total.
```

### Result

**Correct:** Yes
**Accuracy:** 100%

---

## 3.4 Analysis

Both prompting approaches produced the correct answer, so step-by-step prompting did not improve accuracy for this particular problem.

However, the step-by-step prompt produced a more explicitly organized calculation, making the solution easier to follow and verify.

The direct prompt also generated calculation steps even though they were not explicitly requested. Therefore, in this experiment, the main difference was the structure and clarity of the visible output rather than accuracy.

---

# 4. Experiment 3 – Structured JSON Output

## 4.1 Objective

The purpose of this experiment was to extract specific information from natural-language text and return it in a consistent JSON structure.

The required fields were:

* `name`
* `date`
* `amount`

### JSON Structure

```json
{
  "name": "string",
  "date": "YYYY-MM-DD",
  "amount": 0
}
```

---

## 4.2 Prompt

```text
You are an information extraction system.

Extract the following information from the given text:
- name
- date
- amount

Return the result as valid JSON using exactly this structure:

{
  "name": "string",
  "date": "YYYY-MM-DD",
  "amount": 0
}

Rules:
- Return only valid JSON.
- Do not include Markdown code fences.
- Do not add explanations.
- Convert the date to YYYY-MM-DD format.
- Return amount as a number without currency symbols.
- If a field is missing, use null.

Text:
[TEST INPUT]
```

---

## 4.3 Test 1

### Input

```text
Hi John, Thank you for purchasing our Python course. We received your payment of ₹2500 on September 3, 2026. Regards, ABC Academy.
```

### Raw Model Output

```json
{
"name": "John",
"date": "2026-09-03",
"amount": 2500
}
```

**Result:** Success

---

## 4.4 Test 2

### Input

```text
Customer Sarah ordered a Data Science course for $120. The payment was completed on 15 August 2026.
```

### Raw Model Output

```json
{
"name": "Sarah",
"date": "2026-08-15",
"amount": 120
}
```

**Result:** Success

---

## 4.5 Test 3

### Input

```text
Hello Alex! Your payment of ₹3,499 for the Machine Learning Bootcamp was received on September 7, 2026. Thank you for your purchase.
```

### Raw Model Output

```json
{
"name": "Alex",
"date": "2026-09-07",
"amount": 3499
}
```

**Result:** Success

---

## 4.6 Test 4

### Input

```text
Dear Priya, we have successfully received your payment of €75 for the Python Programming Workshop on 2 September 2026. Thank you for registering.
```

### Raw Model Output

```json
{
"name": "Priya",
"date": "2026-09-02",
"amount": 75
}
```

**Result:** Success

---

## 4.7 Test 5

### Input

```text
Hello! Your registration for the Generative AI course was confirmed on September 5, 2026. The payment amount was ₹1999. Thank you, Rahul.
```

### Raw Model Output

```json
{
"name": "Rahul",
"date": "2026-09-05",
"amount": 1999
}
```

**Result:** Success

---

## 4.8 Structured Output Results

| Test | Valid JSON | Correct Fields | Correct Date Format | Correct Amount | Result  |
| ---- | ---------- | -------------- | ------------------- | -------------- | ------- |
| 1    | Yes        | Yes            | Yes                 | Yes            | Success |
| 2    | Yes        | Yes            | Yes                 | Yes            | Success |
| 3    | Yes        | Yes            | Yes                 | Yes            | Success |
| 4    | Yes        | Yes            | Yes                 | Yes            | Success |
| 5    | Yes        | Yes            | Yes                 | Yes            | Success |

**Success rate: 5/5 = 100%**

---

## 4.9 Analysis

The structured output prompt successfully produced valid JSON for all five test inputs.

The main techniques that improved reliability were providing an explicit JSON structure, clearly defining each required field, specifying the date format, requiring numeric amounts without currency symbols, and instructing the model to return only JSON without explanations or Markdown code fences.

This experiment used prompt-based structured formatting. Gemini's separate structured-output/JSON-mode API feature was not used in this experiment.

---

# 5. Overall Results

| Experiment             | Method                       | Result         |
| ---------------------- | ---------------------------- | -------------- |
| Zero-Shot vs Few-Shot  | Zero-Shot                    | 10/10 – 100%   |
| Zero-Shot vs Few-Shot  | Few-Shot                     | 10/10 – 100%   |
| Direct vs Step-by-Step | Direct                       | Correct – 100% |
| Direct vs Step-by-Step | Step-by-Step                 | Correct – 100% |
| Structured JSON        | Prompt-based JSON extraction | 5/5 – 100%     |

---

# 6. Key Learnings

Through these experiments, I learned that prompt engineering is mainly about giving the model clear instructions, context, examples, constraints, and an expected output format.

The experiments showed that:

* Clear zero-shot instructions can be sufficient for simple and unambiguous tasks.
* Few-shot examples can demonstrate the expected behavior and output format, although they did not improve accuracy in this particular sentiment dataset.
* Step-by-step prompting can make multi-step solutions easier to follow and verify.
* Explicit schemas and formatting rules can improve the consistency of structured outputs.
* Prompt effectiveness depends on the task, input data, and how clearly the desired output is specified.

---

# 7. Conclusion

This task provided practical experience with important prompt engineering techniques including zero-shot prompting, few-shot prompting, step-by-step prompting, and structured output prompting.

The experiments demonstrated that different prompting techniques can influence the model's output format, clarity, and consistency. Testing prompts with real inputs and examining the actual model outputs helped me understand that effective prompt engineering requires experimentation and evaluation rather than relying only on theoretical instructions.

Overall, this task improved my practical understanding of how prompts can be designed and evaluated when building Generative AI applications.
