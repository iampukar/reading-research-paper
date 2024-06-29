# DeepSeek-Coder-V2 Summary

## Abstract

- DeepSeek-Coder-V2 is an open-source Mixture-of-Experts (MoE) code language model that achieves performance comparable to GPT4-Turbo in code-specific tasks.
- The model is pre-trained from an intermediate checkpoint of DeepSeek-V2 with an additional 6 trillion tokens.
- Substantially enhances coding and mathematical reasoning capabilities of DeepSeek-V2 while maintaining comparable performance in general language tasks.
- Demonstrates significant advancements in various aspects of code-related tasks, reasoning, and general capabilities.
- Expands support for programming languages from 86 to 338, while extending the context length from 16K to 128K tokens.
- Achieves superior performance compared to closed-source models such as GPT4-Turbo, Claude 3 Opus, and Gemini 1.5 Pro in coding and math benchmarks.

## Introduction 

- The development of models like StarCoder, CodeLlama, DeepSeek-Coder, and Codestral has significantly advanced open-source code intelligence.
- There is still a notable performance gap between open-source models and state-of-the-art closed-source models like GPT4-Turbo, Claude 3 Opus, and Gemini 1.5 Pro.
- DeepSeek-Coder-V2 aims to bridge the performance gap and advance the development of open-source code models.
- The model is introduced with 16B and 236B parameters, efficiently supporting diverse computational needs.
- This is the first attempt to develop an open-source, hundred-billion-parameter code model, advancing code intelligence. Released under a permissive license, allowing for both research and commercial use.
- DeepSeek-Coder-V2 outperforms all open-source models and matches leading closed-source models in code generation. Achieves a 90.2% score on HumanEval, 76.2% on MBPP, and 43.4% on LiveCodeBench.
- Exhibits strong mathematical reasoning, rivaling top closed-source models on both elementary and advanced benchmarks.
- Maintains strong general language performance, comparable to DeepSeek-V2.

## Data Collection 

- The pre-training data for DeepSeek-Coder-V2 primarily consists of 60% source code, 10% math corpus, and 30% natural language corpus.
- The source code includes 1,170 billion code-related tokens sourced from GitHub and CommonCrawl. The corpus supports 338 programming languages, a significant expansion from the 86 languages in previous models.
- 221 billion math-related tokens are collected from CommonCrawl, doubling the size of the previous DeepSeekMath corpus. Directly sampled from the training corpus in DeepSeek-V2, contributing to a total of 10.2 trillion training tokens.
- Files are filtered out if the average line length exceeds 100 characters or the maximum line length exceeds 1000 characters. Files with fewer than 25% alphabetic characters are removed. Specific rules are applied for different file types (e.g., XML, HTML, JSON, and YAML) to ensure quality and relevance.
- Ablation Studies Conducted to demonstrate the effectiveness of the new code corpus. The 1B parameter model showed improvements of 6.7% and 9.4% in accuracy on the HumanEval and MBPP benchmarks, respectively.
- Uses the Byte Pair Encoding (BPE) tokenizer from DeepSeek-V2 to improve recall accuracy for languages like Chinese.
- Multiple iterations of data collection and validation to ensure high-quality data. Comparative analysis experiments validate the quality and effectiveness of the collected data.

## Training Policy

### Training Strategy
- Uses Next-Token-Prediction and Fill-In-Middle (FIM) training objectives for the 16B model.
- The 236B model utilizes only the Next-Token-Prediction objective.
- Adopts the Prefix, Suffix, Middle (PSM) mode for FIM, applied at a rate of 0.5 to enhance training efficacy and model performance.

### Model Architecture
- Aligns with the DeepSeekV2 architecture.
- Hyperparameters settings for 16B and 236B correspond to those used in DeepSeek-V2-Lite and DeepSeek-V2, respectively.
- Addressed instability during training by reverting to conventional normalization methods.

### Training Hyper-Parameters
- Utilizes the AdamW optimizer with configurations: β1 = 0.9, β2 = 0.95, and a weight decay of 0.1.
- Batch sizes and learning rates are adjusted according to DeepSeek-V2 specifications.
- Learning rate scheduling employs a cosine decay strategy, starting with 2000 warm-up steps and reducing the learning rate to 10% of its initial value.

### Long Context Extension
- Extends context length to 128K using YARN (Yarn for Attention-Recurrent Networks).
- Training involves two stages: first with a sequence length of 32K and a batch size of 1152 for 1000 steps, and then with a sequence length of 128K and a batch size of 288 sequences for another 1000 steps.
- Evaluations on "Needle In A Haystack" (NIAH) tests indicate effective performance across all context window lengths up to 128K.

### Alignment
- Supervised Fine-Tuning includes Mixed instruction training dataset with code and math data.
- Collects 20K code-related instruction data and 30K math-related data from DeepSeek-Coder and DeepSeek-Math, sampling additional data from DeepSeek-V2.
- Uses a cosine schedule with 100 warm-up steps and an initial learning rate of 5e-6. Batch size of 1M tokens, with a total of 1B tokens for training.
- For Reinforcement Learning, Collected 40K data prompts related to code and math, each with corresponding test cases.
- Trains a reward model on compiler feedback for code and mathematical preference data to guide policy model training.
- GRPO (Group Relative Policy Optimization) algorithm used for alignment, proven effective and cost-efficient compared to PPO (Proximal Policy Optimization).

## Conclusion 

- DeepSeek-Coder-V2 improves coding and mathematical reasoning capabilities while maintaining general language performance.
- Supports 338 programming languages and extends context length to 128K tokens, a significant increase from previous versions.
- Achieves competitive performance in code and math-specific tasks, comparable to leading closed-source models like GPT-4 Turbo.
- Identifies a gap in instruction-following capabilities, highlighting an area for future enhancement.
- Future efforts will focus on improving instruction-following to better handle complex programming scenarios and enhance development productivity.