Here’s a **clean, professional GitHub README**, concise and technical, suitable for recruiters and engineers. You can paste this directly into `README.md`.

---

# Custom Byte-Level Tokenizer: Data vs Algorithm Study

## Overview

This project implements a **byte-level BPE tokenizer from scratch** and benchmarks it against a GPT-style tokenizer to study what actually drives tokenizer efficiency.

The key goal is **not** to outperform production tokenizers, but to experimentally evaluate the impact of **training data scale** while keeping the **algorithm fixed**.

---

## Motivation

Tokenizers strongly influence:

* context length
* inference cost
* memory efficiency

Yet they are often treated as solved infrastructure. This project explores tokenizer behavior from first principles to answer a simple question:

> **Does tokenizer efficiency depend more on algorithm design or on training data scale?**

---

## Tokenizer Design

The tokenizer follows a standard, production-aligned pipeline:

1. **UTF-8 byte encoding**

   * Eliminates out-of-vocabulary issues
   * Guarantees full reversibility

2. **Byte-to-symbol mapping**

   * Provides a mergeable representation

3. **Byte Pair Encoding (BPE)**

   * Greedy frequency-based merges
   * No heuristics or language-specific rules

4. **Frozen vocabulary**

   * Clear separation between training and inference

5. **Full round-trip validation**

   ```
   decode(encode(text)) == text
   ```

Correctness is verified before any benchmarking.

---

## Experiments

### Initial Training (Small Data)

* Trained on a small code snippet
* Resulted in poor token efficiency
* Token ratios vs GPT tokenizer ≈ **0.2**

This establishes a realistic baseline.

---

### Scaled Training (Real Code)

* Trained on **millions of symbols of Python code** from the CPython repository
* Same algorithm, same implementation
* Only the **training data size** changed

**Result:**
Token efficiency improved to **0.45–0.58** relative to GPT, reducing the gap by more than **2×**.

---

## Key Result

> **Tokenizer performance improved significantly without changing the algorithm — only by increasing training data.**

This demonstrates that once a tokenizer design is reasonable, **data scale dominates efficiency**, mirroring broader machine learning scaling laws.

---

## Visualization

The core result is summarized by a single plot showing token efficiency versus training data size (log scale), revealing:

* rapid early gains
* diminishing returns at scale
* convergence toward production tokenizers

---

## Limitations

* Naïve Python BPE implementation (not optimized for speed)
* Training data far smaller than production tokenizers
* No identifier-aware or code-specific heuristics
* Vocabulary size not constrained

These limitations are intentional to keep the experiment interpretable.

---

## Takeaway

Tokenizers are **data-driven compression systems**, not linguistic models.
Their effectiveness depends far more on **training data scale and relevance** than on algorithmic tricks.

This project provides a concrete, reproducible demonstration of that principle.

---

## Repository Contents

* Jupyter notebook with full implementation and experiments
