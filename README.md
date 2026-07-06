# EvalShortcut
## Overview
This project focuses on reducing the computational cost of evaluating large language models (LLMs) during training — particularly for generative benchmarks such as reasoning and code generation. Iterative evaluation is key for tracking model capability development, but traditional evaluation pipelines relying on full-text generation can be prohibitively time- and compute-intensive.

To address this, we reformulate generative (NLG) tasks into computationally cheaper natural language understanding (NLU) alternatives  — Log Likelihood (LL) and Multiple Choice (MC)  —  enabling faster and more scalable capability monitoring without significant loss in diagnostic power.

## Repository Contents

📊 Updated Corpora: Reformulated datasets and benchmarks are available on Hugging Face [fraunhofer-iis/evalshortcut](https://huggingface.co/collections/fraunhofer-iis/evalshortcut)

| Capability | Original Dataset (linked) | Reformulated Versions |
|-------------|---------------------------|------------------------|
| 🧮 Mathematical Reasoning| [GSM8K](https://huggingface.co/datasets/openai/gsm8k) | • **LL**  <br> • **MC-Random**  <br> • **MC-Smart** |
| 💻 Code Generation| [HumanEval](https://huggingface.co/datasets/openai/human_eval) | • **LL**  <br> • **MC-Random**  <br> • **MC-Smart** |
| 🌍 Factual Knowledge| [TriviaQA](https://huggingface.co/datasets/mandarjoshi/trivia_qa) | • **LL**  <br> • **MC-Random**  <br> • **MC-Smart** |
| 📖 Reading Comprehension| [SQuAD](https://huggingface.co/datasets/rajpurkar/squad) | • **LL**  <br> • **MC-Random**  <br> |

⚙️ Extended Eval Harness: Code to run both original and reformulated evaluations efficiently.


## Further details
### 🔄 NLG vs NLU Evaluation Formats

We evaluate LLMs using **three different approaches**:  
- **LL (Log-Likelihood)**  as part of NLU (Natural Language Understanding)
- **MC (Multiple Choice)**  as part of NLU (Natural Language Understanding)
- **NLG (Natural Language Generation)**  

The table below illustrates how the same question is processed in each format:
| Method | Step | <Q> := What is gravity? | Step output | Final output |
|--------|------|------------------------|------------|-------------|
| **LL** | 1 | <Q> Downward force on objects. | log(0.9) | Log-likelihood of correct answer: log(0.9) |
| **MC** | 1 | <Q> Downward force on objects. | log(0.9) | Selected answer index: 1 |
|        | 2 | <Q> What makes the sun rise. | log(0.1) | |
| **NLG** | 1 | <Q> | Downward | Generated answer: Downward force on objects. |
|        | 2 | <Q> **Downward** | force | |
|        | 3 | <Q> Downward **force** | on | |
|        | 4 | <Q> Downward force **on** | objects. | |
|        | 5 | <Q> Downward force on **objects.** | [EOS] | |

**Explanation:**

- **LL (Log-Likelihood):** Scores the probability of the correct answer directly. Only one forward pass per candidate answer is needed.  
- **MC (Multiple Choice):** Evaluates multiple answer options and selects the most probable one. Each option requires a separate forward pass.  
- **NLG (Natural Language Generation):** Generates the answer token-by-token, requiring multiple forward passes for a single output.  

### 🎯 Example: Random vs Smart Distractors (TriviaQA)

| Question | Which was the first European country to abolish capital punishment? |
|----------|------------------------------------------------------------------------|
| ✅ Correct answer | Norway |
| 🎲 Random distractors | Chicago Bears, Ballet, 6 |
| 🧠 Smart distractors | Germany, Italy, Poland |

> This example illustrates the difference between **random** and **smart** distractors. Random distractors are unrelated or absurd options, while smart distractors are plausible alternatives that make the multiple-choice task more challenging.

---

## 🧾 Citation
For details, please see our [paper](https://arxiv.org/pdf/2506.03592)

If you use our datasets, reformulated benchmarks, or evaluation code in your research, please cite the following work:

Viktor Hangya, Fabian Küch, and Darina Gold. 2025. "From Understanding to Generation: An Efficient Shortcut for Evaluating Language Models." In Proceedings of EMNLP. Suzhou, China. 



