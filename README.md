# Fine-Tuning SmolLM on Deep Reasoning Dataset

This project demonstrates how to fine-tune **SmolLM2-360M**, a small yet capable language model, on a custom deep reasoning dataset. The goal is to enhance the model's ability to respond to prompts with thoughtful, structured, and well-reasoned answers despite its relatively small parameter size.

---

## Motivation

Large models (like GPT or llama) are powerful but resource-heavy. This project explores how even small models like **SmolLM** can be aligned and fine-tuned to perform **deep reasoning tasks**, enabling deployment on low-resource devices while still producing high-quality answers.

---

## 🗂 Dataset

We use the `Deepthink-Reasoning` dataset, which contains:

- **Prompts**: Thought-provoking or open-ended questions.
- **Responses**: Ideal, well-reasoned, structured answers.

Dataset: [`prithivMLmods/Deepthink-Reasoning`](https://huggingface.co/datasets/prithivMLmods/Deepthink-Reasoning)

---

## ⚙️ Fine-Tuning Pipeline

1. **Model Loading**:
   - Model: `HuggingFaceTB/SmolLM2-360M`
   - Tokenizer: Loaded with chat format using `apply_chat_template`.

2. **Data Preprocessing**:
   - Pairs of (prompt, response) are tokenized using the tokenizer’s chat template.
   - Maximum length capped at 512 tokens.

3. **Training**:
   - Optimizer: `adamw_8bit`
   - Batch Size: 2
   - Accumulation Steps: 4
   - Learning Rate: `2e-4`
   - Mixed precision: `fp16` or `bf16` (based on GPU support)
   - Training Steps: 60 (demo-scale; increase for better performance)

4. **Trainer**:
   - `SFTTrainer` from `trl` used for supervised fine-tuning.

---

## Evaluation (Eye Test)

Post-fine-tuning, the model:

- Shows **substantial improvements** in logical coherence and depth of answers.
- Outperforms the default base model in reasoning tasks, even with small parameter count.

Sample Prompt:
> **"Explain AGI in simple terms?"**

Base Model Output:
> "Explain AGI ?\n\nA. AGI is a measure of the ability of a machine to perform a task.\n\nB. AGI is a measure of the ability of a machine to perform a task.\n\nC. AGI is a measure of the ability of a machine to perform a task.\n\nD. AGI is a measure of the ability of a machine to perform a task.\n\nAnswer: B\n\nQuestion 10. Which of the following is not a type of AGI?\n\nA. Machine learning\n\nB. Natural language processing\n\nC. Computer vision\n\nD. Speech recognition\n\nAnswer: B\n\nQuestion 11. Which of the following is not a type of AGI?\n\nA. Machine learning\n\nB. Natural language processing\n\nC. Computer vision\n\nD. Speech recognition\n\nAnswer: C\n\nQuestion 12. Which of the following is not a type of A'
" (repetitive, shallow)

Fine-Tuned Output:
> "Explain AGI ? Describe its importance and significance in modern business and technology. Explain how it is different from AI. Define AI? AI stands for Artificial Intelligence. It is the simulation of human intelligence processes by machines, especially computers. I.e., it imitates human-like reasoning, problem-solving, and decision-making. AGI is particularly sophisticated AI that can think creatively, learn from experience, and perform tasks requiring human intelligence. AI and AGI are interconnected, but AGI is far more sophisticated than AI. It can replicate human-level intelligence and surpass humans in performing tasks like speech recognition, translation, and creativity. Explain the significance of AGI? AGI has the potential to transform business and technology by: 1. Identifying new market opportunities. 2. Driving innovation and productivity. 3. Optimizing processes and operations. 4. Enhancing customer experiences. 5. Addressing ethical and employment challenges.
 " (includes summary, challenges, benefits, and impact)

---
