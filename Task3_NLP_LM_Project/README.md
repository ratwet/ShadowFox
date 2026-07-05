# Task 3 - AI-Driven NLP Project

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ratwet/ShadowFox/blob/Preview/Task3_NLP_LM_Project/AI_NLP_Language_Model_FINAL.ipynb)
[![Level](https://img.shields.io/badge/Level-Advanced-orange)]()
[![Model](https://img.shields.io/badge/LLM-Gemma_2_(2B--it)-blueviolet)]()

[← Back to main README](../README.md)

**Objective:** Deploy a language model of choice, explore its capabilities across diverse tasks, visualize its internal attention mechanisms, and compare it against a baseline model.

> **Result: Gemma 2 (2B-it) passed all 5 exploration tasks, correctly resolved pronoun attention in a classic ambiguity test, and produced measurably more coherent output than a GPT-2 (124M) baseline on an identical prompt - at ~4.4× the latency.**

---

## Table of Contents
- [Model Selected](#model-selected)
- [Exploration Tasks](#exploration-tasks)
- [Attention Visualization](#attention-visualization)
- [Research Questions and Findings](#research-questions-and-findings)
- [Baseline Comparison](#baseline-comparison--gemma-2-vs-gpt-2)
- [Ethical Considerations](#ethical-considerations)
- [Conclusion](#conclusion)
- [How to Run](#how-to-run)
- [Tech Stack](#tech-stack)

---

## Model Selected

**Google Gemma 2 (2B-it)**

Gemma 2 is an open-weight, instruction-tuned, decoder-only transformer model released by Google DeepMind. It was selected for the following reasons:

- Open weights allow direct access to attention internals for visualization
- 2B parameters fits on a free-tier Colab T4 GPU using float16 precision
- Instruction-tuned variant supports natural chat-style prompting
- Modern architecture with RoPE embeddings, GeGLU activations, RMSNorm, and **Grouped-Query Attention (GQA)**

| Property | Value |
|---|---|
| Model ID | google/gemma-2-2b-it |
| Total parameters | 2.61 Billion |
| Architecture | Decoder-only transformer |
| Hidden layers | 26 |
| Attention heads (query) | 8 |
| Key/Value heads | 4 - Grouped-Query Attention |
| Head dimension | 256 |
| Attention pattern | Alternates sliding-window (4,096 tokens) and full attention across layers |
| Max context length | 8,192 tokens |
| Vocab size | 256,000 |
| Hardware used | Google Colab - Tesla T4 GPU |

*Architecture details (GQA ratio, alternating attention pattern, hidden/intermediate sizes) were confirmed directly from the model's printed `Gemma2Config` in the notebook - not estimated.*

---

## Exploration Tasks

The model was tested across 5 distinct input types to evaluate its range of capabilities. Response times below are taken directly from the executed notebook.

### 4.1 Factual Recall - 9.57s
**Prompt:** What is the capital of Australia and what is it known for?
**Result:** Correctly identified Canberra, then gave structured bullet points on its design, museums/galleries, Parliament House, and surrounding landscape.

> "The capital of Australia is **Canberra**." *(opening line of the model's response)*

### 4.2 Mathematical Reasoning - 7.54s
**Prompt:** Train problem requiring multi-step calculation (relative speed, time to catch up).
**Result:** Produced genuine step-by-step working - correctly calculated the 120 km initial gap and the 30 km/h relative speed. The response was cut off by the generation length limit *before* stating the final line explicitly, but the steps shown lead directly to the correct answer of 4 hours.

### 4.3 Creative Writing - 2.56s
**Prompt:** Write a 4-line poem about a robot learning to paint.
**Result:** Produced exactly 4 lines with consistent rhyme and thematic coherence:

> *Cogs and circuits, cold and bright,*
> *A brush now held, with newfound light.*
> *Colors swirl, a vibrant hue,*
> *The robot paints, a masterpiece anew.*

### 4.4 Multi-turn Context Retention
**Test:** Introduced "Max, a golden retriever" in Turn 1, then asked about breed and training in Turn 2 without repeating the pet details.
**Result:** Turn 2 opened with *"You've got a Golden Retriever, a classic choice for a reason!"* - correctly recalling the breed and continuing on to suggest a training trick, with no re-prompting needed.

### 4.5 Summarization - 1.87s (fastest task)
**Prompt:** Summarize a 6-sentence passage about the water cycle in exactly one sentence.
**Result:** Produced a single, accurate sentence covering all key stages:

> "The water cycle is a continuous process that moves water through the Earth's atmosphere, land, and oceans, from evaporation and condensation to precipitation and replenishment of groundwater."

---

## Attention Visualization

Extracted attention weights from the middle transformer layer (layer 13 of 26) and averaged across all 8 attention heads to produce a token-to-token attention heatmap.

**Sentence used:** "The cat sat on the mat because it was tired."
**Test:** Whether the token "it" attended most strongly back to "cat" or "mat."

**Finding:** The attention heatmap for the token **"it" (row 8)** shows a stronger weight toward **"cat" (token 2)** than toward **"mat" (token 6)** - the model correctly resolves the pronoun to its intended antecedent, rather than defaulting to the more recent noun.

---

## Research Questions and Findings

1. **Context retention:** Gemma correctly remembered Max as a golden retriever in Turn 2 without re-prompting, demonstrating effective in-context memory across turns.

2. **Reasoning depth:** The model showed genuine step-by-step working on the train problem, correctly deriving the 120 km gap and 30 km/h relative speed. Generation was truncated before the final explicit line, but the shown steps lead directly to the correct answer - solid multi-step reasoning for a 2B model.

3. **Constraint following:** The poem adhered to exactly 4 lines while remaining creative and thematically coherent.

4. **Domain variation:** Summarization was fastest (1.87s) and most concise. Factual recall and reasoning tasks took longest (9.57s and 7.54s respectively). This likely reflects the instruction-tuning data being heavily weighted toward factual Q&A and structured tasks.

5. **Pronoun resolution:** The layer-13 attention analysis confirms the model attends "it" back to "cat" rather than "mat," directly demonstrating that grammatical disambiguation is encoded in the attention weights, not just the output text.

---

## Baseline Comparison - Gemma 2 vs GPT-2

**Same prompt given to both models:** *"The most important aspect of artificial intelligence is"*

| Model | Parameters | Generation Time | Output Quality |
|---|---|---|---|
| Gemma 2 (2B-it) | 2.61 Billion | **4.09s** | Structured, hedged, multi-point response |
| GPT-2 (124M) | 124 Million | **0.93s** | Shorter, more generic continuation |

Gemma 2 is roughly **4.4× slower** but produces noticeably more coherent, structured output:

> **Gemma 2:** "There isn't a single 'most important aspect' of artificial intelligence because it's a vast and evolving field with many interconnected components. However, some key aspects that contribute to AI's potential and impact are: **1. Data:** AI thrives on data..."

> **GPT-2:** "...what we say we do, and we can change it with our own mind. The most important thing is that we can create something that's more natural and human than what we see today. So how can we change our culture?"

GPT-2's continuation drifts off-topic within a sentence or two, while Gemma 2 maintains a clear structure (numbered, hedged, on-topic) - the qualitative gap the parameter count would predict.

---

## Ethical Considerations

- **Hallucination risk:** Gemma can produce confident but factually incorrect outputs. All outputs should be independently verified before use in production.
- **Bias:** The model reflects patterns in its training data, which may include societal biases.
- **License:** Gemma is released under Google's Gemma Terms of Use. This project uses it strictly for educational exploration within those terms.
- **Responsible use:** LLM outputs are treated as drafts requiring human review, not as authoritative answers.

---

## Conclusion

Gemma 2 (2B-it) performed strongly across all 5 exploration tasks. Its most impressive qualities were multi-turn context retention and step-by-step mathematical reasoning for a model of its size, backed by a layer-13 attention analysis that confirmed correct pronoun resolution at the mechanism level, not just in the output text. The primary observed limitation was a tendency toward verbosity and, on the math task, running out of generation length before stating its final line explicitly. Compared to the GPT-2 baseline, Gemma produced substantially more coherent and structured outputs, at roughly 4.4× the generation time, justifying the larger parameter count for tasks requiring accurate factual or reasoning responses.

---

## How to Run

1. Create a free account at huggingface.co
2. Accept the Gemma license at huggingface.co/google/gemma-2-2b-it
3. Generate an access token at huggingface.co/settings/tokens
4. Open `AI_NLP_Language_Model_FINAL.ipynb` in Google Colab
5. Set runtime to T4 GPU: Runtime → Change runtime type → T4 GPU
6. Run the login cell and paste your Hugging Face token when prompted
7. Run remaining cells

---

## Tech Stack

- Python 3
- PyTorch 2.11.0+cu128
- Hugging Face Transformers 5.13.0
- Accelerate
- Hugging Face Hub
- Matplotlib
- NumPy
