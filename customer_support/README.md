# PEFT Customer Support Chatbot Evaluation

## Summary of Results

- **Original Model Perplexity:** **1,943.24**
- **PEFT Fine-Tuned Model Perplexity:** **3.88**
- **Original Model SacreBLEU Score:** **0.0**
- **PEFT Fine-Tuned Model SacreBLEU Score:** **4.18**
- **PEFT Fine-Tuned Model ROUGE-1 F1 Score:** **0.3687**
- **PEFT Fine-Tuned Model ROUGE-L F1 Score:** **0.2451**
- **PEFT Fine-Tuned Model BERTScore F1:** **0.2725**

---

## Training Progress

The model showed consistent improvement during training.

| **Step** | **Training Loss** | **Validation Loss** |
|----------|-------------------|---------------------|
| 500      | 2.5969            | 2.2630              |
| 1000     | 2.0598            | 1.8346              |
| 1500     | 1.8482            | 1.6545              |
| 2000     | 1.7392            | 1.5457              |
| 2500     | 1.6512            | 1.4662              |
| 3000     | 1.5849            | 1.4066              |
| 3500     | 1.5970            | 1.3770              |
| 4000     | 1.5289            | 1.3602              |
| 4500     | 1.5377            | 1.3546              |

---

## Perplexity Evaluation

Perplexity measures how well a model predicts a sample and is defined as:

$$
\text{Perplexity} (\text{PPL}) = e^{\text{Loss}}
$$

Where:

- $\text{Loss}$ is typically the cross-entropy loss computed during evaluation.

> [!NOTE]
> **Lower Perplexity:** The model predicts the sample more accurately.
>
> **Higher Perplexity:** The model is less certain about its predictions.

### Results

- **Original Model Perplexity:**

  $$
  \text{Perplexity}_{\text{Original}} = e^{7.571} \approx 1,943.24
  $$

- **PEFT Model Perplexity:**

  $$
  \text{Perplexity}_{\text{PEFT}} = e^{1.3546} \approx 3.88
  $$


---

## BLEU Score Evaluation

> [!NOTE]
> **BLEU (Bilingual Evaluation Understudy)** is a precision-based metric that evaluates the quality of machine-generated text by comparing it to one or more reference texts.

The BLEU score is defined as:

$$
\text{BLEU} = BP \cdot \exp \left( \sum_{n=1}^{N} w_n \cdot \log p_n \right)
$$

Where:

- \( BP \) is the **brevity penalty**.
- \( p_n \) is the **modified precision** for n-grams.
- \( w_n \) is the **weight** assigned to each n-gram precision (usually equal weights).
- \( N \) is the maximum n-gram order (commonly 4).

#### Brevity Penalty (BP)

The brevity penalty penalizes generated texts that are shorter than the reference:

$$
BP =
\begin{cases} 
1 & \text{if } c > r \\
\exp\left(1 - \frac{r}{c}\right) & \text{if } c \leq r 
\end{cases}
$$

Where:

- \( c \) is the length of the candidate (generated) text.
- \( r \) is the effective reference corpus length.

#### Precision for n-grams \( p_n \)

$$
p_n = \frac{\text{Number of matching n-grams}}{\text{Total number of candidate n-grams}}
$$

> [!NOTE]
> **Higher BLEU Score:** Indicates closer similarity between the generated text and the reference text.
>
> **Lower BLEU Score:** Suggests greater divergence from the reference.

### BLEU Score Results

#### Original Model

- **SacreBLEU Score Calculation:**

  $$
  \text{SacreBLEU}_{\text{Original}} \approx 0.0
  $$

#### PEFT Fine-Tuned Model

- **SacreBLEU Score Calculation:**

  $$
  \text{SacreBLEU}_{\text{PEFT}} \approx 4.18
  $$

**Result:** A BLEU score of **4.18**, while low in absolute terms, shows a significant improvement over the original model's score of **0.0**. This indicates that the PEFT fine-tuned model generates responses that are more closely aligned with the reference responses.

---

## ROUGE Score Evaluation

**ROUGE (Recall-Oriented Understudy for Gisting Evaluation)** measures the overlap between the generated text and the reference text.

### PEFT Model ROUGE F1 Scores

- **ROUGE-1 F1 Score:** **0.3687**
- **ROUGE-L F1 Score:** **0.2451**

**Functionality of ROUGE F1 Scores:**

- **ROUGE-1:** Measures unigram overlap.
- **ROUGE-L:** Measures the longest common subsequence (LCS), considering the order of words.

**F1 Score Calculation:**

$$
\text{F1 Score} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}
$$

Where:

- **Precision:** Proportion of overlapping units in the generated text.
- **Recall:** Proportion of overlapping units in the reference text.

> [!TIP]
> The F1 Score balances both precision and recall to provide a single performance metric.

---

## BERTScore Evaluation

**BERTScore** uses contextual embeddings from BERT to evaluate the semantic similarity between the generated and reference texts.

### PEFT Model BERTScore F1

- **BERTScore F1:** **0.2725**

> [!TIP]
> A higher BERTScore indicates better semantic alignment with the reference responses.

---

## Comparison of Models

| **Metric**           | **Original Model** | **PEFT Model** |
|----------------------|--------------------|----------------|
| **Perplexity**       | 1,943.24           | 3.88           |
| **SacreBLEU Score**  | 0.0                | 4.18           |
| **ROUGE-1 F1 Score** | N/A                | 0.3687         |
| **ROUGE-L F1 Score** | N/A                | 0.2451         |
| **BERTScore F1**     | N/A                | 0.2725         |

---

## Conclusion

The PEFT fine-tuned model significantly outperforms the original model across all evaluated metrics, demonstrating an improved ability to generate relevant and coherent responses to customer queries.

> [!NOTE]
> - **Lower Perplexity:** Indicates increased confidence and accuracy in predictions.
> - **Higher BLEU and ROUGE Scores:** Reflect better overlap with reference responses.
> - **Higher BERTScore:** Suggests enhanced semantic similarity.

---

