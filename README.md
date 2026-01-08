# Multilingual Paraphrase Evaluation: A Cross-Lingual Reasoning Study

## Motivation
Multilingual language models are often evaluated using aggregate accuracy scores, but such metrics hide important questions:
Do these models actually reason about meaning across languages, or do they rely on surface-level patterns learned during pretraining?

This project investigates how a strong multilingual transformer behaves when performing paraphrase reasoning across languages with varying linguistic distance from English. The goal is not to improve performance, but to understand *where and why* it fails.

---
## Problem Definition

We study the task of **paraphrase identification**, which aims to determine whether two sentences convey the same underlying meaning.

Formally, given a sentence pair $(s_1, s_2)$, the model predicts a binary label $y \in \{0, 1\}$, defined as:

$$
y =
\begin{cases}
1 & \text{if } s_1 \text{ and } s_2 \text{ are semantically equivalent} \\
0 & \text{otherwise}
\end{cases}
$$

Unlike surface-level text classification tasks, paraphrase identification requires capturing **semantic equivalence** while remaining sensitive to **word order**, **negation**, and **syntactic structure**. This makes it a suitable probe for evaluating cross-lingual reasoning, where meaning must be preserved across linguistically diverse inputs.

---

## Why Paraphrase Detection?
Paraphrase detection is a deceptively simple task that exposes several weaknesses of multilingual models:
- Lexical overlap does not guarantee semantic equivalence
- Word order changes can alter meaning
- Translation artifacts affect sentence structure across languages

Because PAWS-style datasets contain adversarial examples, high performance requires more than keyword matching.

---

## Dataset Choice: PAWS-X
We use **PAWS-X**, a multilingual extension of the PAWS benchmark.

Key reasons for choosing this dataset:
- Designed to break shallow heuristics
- Contains parallel paraphrase structures across languages
- Widely used in multilingual NLP research

Languages evaluated:
- English (reference language)
- French (closely related to English)
- Hindi (linguistically distant)

This selection allows us to observe cross-lingual degradation patterns.

---

## Model Selection Rationale
We use **XLM-RoBERTa (XLM-R)**, a transformer pretrained on large-scale multilingual corpora.

Why XLM-R?
- Shared subword vocabulary enables cross-lingual token alignment
- Strong baseline for multilingual understanding
- Commonly used in academic and industrial research (including MSR)

Rather than fine-tuning from scratch, we use a PAWS-X–fine-tuned checkpoint to focus on evaluation and analysis rather than optimization.

---

## Experimental Setup
- No language-specific fine-tuning is performed
- The same model is evaluated independently on each language
- Accuracy is used as a coarse metric
- Qualitative error analysis is used to uncover failure modes

This setup isolates **cross-lingual generalization** from training effects.

---

## Results and Observations
Performance follows a clear trend:
- Highest accuracy on English
- Moderate drop on French
- Larger degradation on Hindi

This suggests that shared multilingual representations are imperfect and influenced by linguistic similarity and pretraining data imbalance.

---

## Error Analysis
To move beyond metrics, incorrect predictions were manually analyzed.

Common failure patterns include:
- Misinterpretation of negation
- Over-reliance on lexical overlap
- Sensitivity to word order changes
- Inconsistent handling of implicit meaning

These errors indicate that semantic equivalence is not consistently preserved across languages.

---

## Robustness Evaluation
We perform small perturbations such as:
- Removing punctuation
- Reordering non-critical words

In multiple cases, predictions changed despite meaning being preserved, highlighting limited robustness.

This suggests that the model’s decision boundary is sensitive to surface-level variations.

---

## Key Insights
- Multilingual models exhibit partial semantic alignment across languages
- Cross-lingual performance degrades with linguistic distance
- Qualitative analysis reveals limitations not visible through accuracy alone

Understanding these behaviors is critical before deploying multilingual systems in real-world settings.

---

## Limitations
- Limited number of languages
- No controlled linguistic feature analysis
- Small-scale manual evaluation

These choices were made to prioritize interpretability over scale.

---

## Future Directions
- Extend to low-resource languages
- Compare multiple multilingual architectures
- Analyze attention patterns for error cases
- Study code-mixed inputs

---

## Tools and Libraries
- Python
- Hugging Face Transformers
- Hugging Face Datasets
- PyTorch
