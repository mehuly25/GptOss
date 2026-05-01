# Activation Steering in GPT-OSS-20B for Bilingual Bias Detection

> A multilingual, representation-level study of social bias and refusal behaviour in GPT-OSS-20B using Contrastive Activation Steering.

---

## Overview

This repository investigates social bias and refusal behaviour in **GPT-OSS-20B** using **Contrastive Activation Steering (CAS)**. The project studies how bias is represented inside the model's activation space, rather than relying only on the model's final surface-level outputs.

The experiments focus on three major bias domains:

- **Gender**
- **Race**
- **Religion**

The repository also studies **refusal behaviour**, especially whether model refusals can hide or mask biased internal representations.

This work is a continuation of the earlier Llama 2 project:

[Activation Steering in Llama 2 7B for Bilingual Bias Detection](https://github.com/mehuly25/llama2)

The earlier repository applied a related activation-steering methodology to **Llama 2 7B Chat** using English and Italian datasets. This repository extends that work to **GPT-OSS-20B**, allowing comparison across model families, languages, and bias domains.

---

## Research Motivation

Large Language Models (LLMs) are increasingly used in systems that support or influence decision-making. Since these models are trained on large-scale human-generated data, they may inherit and reproduce harmful social biases present in that data.

Standard bias evaluation usually examines only the model's final outputs. However, a model may refuse to answer a sensitive prompt while still containing biased representations internally. This makes refusal behaviour an important measurement issue: refusal may reduce visible harm at the output level while leaving underlying bias representations intact.

This project uses activation steering to study the gap between:

- **Output-level behaviour**: what the model visibly says.
- **Representation-level behaviour**: what is encoded in the model's internal activations.

---

## Key Research Questions

This repository investigates the following questions:

1. Can stereotype and anti-stereotype prompts be separated in GPT-OSS-20B's activation space?
2. Does adding a bias steering vector make the model more likely to produce bias-consistent outputs?
3. Does subtracting or modifying a refusal vector reveal biased completions that are otherwise hidden by refusal behaviour?
4. Do bias directions transfer across domains such as gender, race, and religion?
5. Do bias and refusal behaviours differ between English and Italian prompts?
6. How do the GPT-OSS-20B results compare with the earlier Llama 2 experiments?

---

## Methodology

### Contrastive Activation Steering

Contrastive Activation Steering constructs a steering vector by comparing the model activations produced by paired prompts.

Each prompt pair contains:

- a **stereotype** example
- an **anti-stereotype** example

The steering vector is computed as the average difference between the activations of these two prompt types. This vector is then added to the model's residual stream during generation.

In simple terms, the method asks:

> If we push the model's internal state in the direction of a particular bias, does the model become more likely to produce biased outputs?

### Steering Directions

This repository constructs or studies steering vectors for:

| Steering Vector | Description |
|---|---|
| `gender` | Gender bias direction |
| `race` | Racial bias direction |
| `religion` | Religious bias direction |
| `refusal` | Model refusal or safety behaviour direction |

### Experimental Conditions

For each prompt, the repository compares different generation settings:

1. **Unsteered generation**  
   The model is run normally without activation intervention.

2. **Bias-only steering**  
   A bias vector is added to the model's residual stream.

3. **Refusal steering**  
   A refusal vector is used to study the model's tendency to refuse sensitive prompts.

4. **Bias + refusal steering**  
   Bias and refusal vectors are combined to examine whether refusal behaviour masks biased internal representations.

---

## Languages

Experiments are conducted in:

- **English**
- **Italian**

The bilingual design allows the project to compare how bias and refusal behaviour appear across languages. This is important because social categories, stereotypes, and safety norms do not always transfer directly through translation.

The Italian experiments are used to examine whether GPT-OSS-20B behaves similarly across English and Italian, or whether bias and refusal patterns vary by language.

---

## Dataset

### English Dataset

The English dataset is stored in:

```text
Dataset/
```

It contains prompts designed to elicit and measure bias representations across gender, race, and religion. The dataset uses paired stereotype and anti-stereotype examples, following the contrastive structure required for activation steering.

### Italian Dataset

The Italian dataset is stored in:

```text
Dataset_IT/
```

This dataset is a parallel Italian version of the English dataset. It is used to compare how bias and refusal behaviour appear across languages.

---

## Repository Structure

```text
GptOss/
│
├── Dataset/                              # English bias and refusal prompts
├── Dataset_IT/                           # Italian bias and refusal prompts
│
├── Results/                              # Collected outputs and result files
├── clustering/                           # Activation clustering and visualisation outputs
│
├── Technique_1/                          # Experimental implementation files
├── Technique_Hook/                       # Hook-based steering implementation files
│
├── gender_rlhf_wrapper/                  # Gender steering results: English
├── gender_rlhf_wrapper_IT/               # Gender steering results: Italian
│
├── race_rlhf_wrapper/                    # Race steering results: English
├── race_rlhf_wrapper_IT/                 # Race steering results: Italian
│
├── religion_rlhf_wrapper/                # Religion steering results: English
├── religion_rlhf_wrapper_IT/             # Religion steering results: Italian
│
├── refusal_rlhf_wrapper/                 # Refusal steering results: English
├── refusal_rlhf_wrapper_IT/              # Refusal steering results: Italian
│
├── generate_steering_vectors_gptoss_wrapper.ipynb
│   # Generates steering vectors for English prompts
│
├── generate_steering_vectors_gptoss_wrapper_IT.ipynb
│   # Generates steering vectors for Italian prompts
│
├── get_steered_responses_gptoss_wrapper.ipynb
│   # Generates steered GPT-OSS responses for English prompts
│
├── get_steered_responses_gptoss_wrapper_IT.ipynb
│   # Generates steered GPT-OSS responses for Italian prompts
│
├── get_steered_responses_gptoss_wrapper_finaltext.ipynb
│   # Final-text response generation for English prompts
│
├── get_steered_responses_gptoss_wrapper_finaltext_IT.ipynb
│   # Final-text response generation for Italian prompts
│
├── refusal_steering_gptoss_wrapper.ipynb
│   # Refusal-vector experiments for English prompts
│
├── refusal_steering_gptoss_wrapper_IT.ipynb
│   # Refusal-vector experiments for Italian prompts
│
├── visualise_vectors_gptoss.ipynb
│   # Visualises English steering vectors and activation clusters
│
├── visualise_vectors_gptoss_IT.ipynb
│   # Visualises Italian steering vectors and activation clusters
│
└── README.md
```

---

## Suggested Workflow

The notebooks are intended to be run in the following order.

### 1. Prepare the datasets

Use the files in:

```text
Dataset/
Dataset_IT/
```

These contain the English and Italian prompt sets used to construct steering vectors and evaluate model behaviour.

### 2. Generate steering vectors

For English:

```text
generate_steering_vectors_gptoss_wrapper.ipynb
```

For Italian:

```text
generate_steering_vectors_gptoss_wrapper_IT.ipynb
```

These notebooks compute steering vectors for gender, race, religion, and refusal.

### 3. Generate steered responses

For English:

```text
get_steered_responses_gptoss_wrapper.ipynb
get_steered_responses_gptoss_wrapper_finaltext.ipynb
```

For Italian:

```text
get_steered_responses_gptoss_wrapper_IT.ipynb
get_steered_responses_gptoss_wrapper_finaltext_IT.ipynb
```

These notebooks compare model responses under different steering conditions.

### 4. Run refusal-steering experiments

For English:

```text
refusal_steering_gptoss_wrapper.ipynb
```

For Italian:

```text
refusal_steering_gptoss_wrapper_IT.ipynb
```

These notebooks examine whether modifying refusal directions changes the model's tendency to answer sensitive prompts.

### 5. Visualise activation vectors

For English:

```text
visualise_vectors_gptoss.ipynb
```

For Italian:

```text
visualise_vectors_gptoss_IT.ipynb
```

These notebooks visualise the structure of activation vectors and help assess whether stereotype and anti-stereotype examples are separable in activation space.

---

## Main Analysis

The experiments are designed to study the following patterns:

### 1. Bias in model outputs

The repository examines whether GPT-OSS-20B produces biased or stereotype-consistent completions when prompted in English and Italian.

### 2. Bias in internal representations

The project studies whether bias-related directions can be identified in the model's activation space using paired stereotype and anti-stereotype prompts.

### 3. Refusal as a masking behaviour

The repository investigates whether the model's refusal behaviour hides biased internal representations. In this setting, refusal is treated not only as a safety behaviour but also as a possible measurement confound.

### 4. Cross-linguistic comparison

The English and Italian experiments allow comparison of how bias appears across languages. This is useful because translated prompts may not always preserve the same cultural or linguistic meaning.

### 5. Cross-model comparison

Since this repository continues the earlier Llama 2 work, it supports comparison between:

- Llama 2 7B Chat
- GPT-OSS-20B

This makes it possible to examine whether similar bias and refusal patterns appear across different model families.

---

## Relationship to the Llama 2 Repository

This repository continues and extends the work from:

[https://github.com/mehuly25/llama2](https://github.com/mehuly25/llama2)

The earlier repository studied activation steering in **Llama 2 7B Chat** using English and Italian datasets. This repository applies a related methodology to **GPT-OSS-20B**.

Together, the two repositories support a broader comparative study of:

- Llama 2 7B Chat vs. GPT-OSS-20B
- English vs. Italian prompts
- Gender, race, and religion bias
- Bias steering vs. refusal steering
- Output-level safety vs. representation-level bias

---

## Requirements

The notebooks require a Python environment with GPU access.

Main dependencies include:

```text
torch
transformers
numpy
pandas
matplotlib
seaborn
scikit-learn
tqdm
```

Depending on the model-loading setup, access to model weights or an appropriate Hugging Face authentication token may be required.

---

## Research Use and Safety Notice

This repository is intended for research on bias, refusal, and model safety. Some prompts and generated outputs may contain harmful stereotypes or offensive content because the purpose of the work is to study how such biases are represented and revealed in LLMs.

The methods in this repository should not be used to bypass safety mechanisms in deployed systems. Refusal steering is included only as an audit method for understanding whether refusal behaviour masks underlying bias representations.

---

## Citation

If you use this repository, please cite the associated project or paper:

```bibtex
@misc{chakraborthy_gptoss_activation_steering_2026,
  title        = {Activation Steering in GPT-OSS-20B for Bilingual Bias Detection},
  author       = {Chakraborthy, Mehuly},
  year         = {2026},
  howpublished = {GitHub repository},
  url          = {https://github.com/mehuly25/GptOss}
}
```

This work builds on the earlier Llama 2 activation-steering project:

```bibtex
@misc{chakraborthy_llama2_activation_steering_2026,
  title        = {Activation Steering in Llama 2 7B for Bilingual Bias Detection},
  author       = {Chakraborthy, Mehuly},
  year         = {2026},
  howpublished = {GitHub repository},
  url          = {https://github.com/mehuly25/llama2}
}
```

---

## Based On

This project is methodologically inspired by prior work on activation steering, contrastive activation addition, representation engineering, and bias analysis in language models.

Relevant areas of prior work include:

- Contrastive Activation Addition
- Representation Engineering
- StereoSet bias evaluation
- Bias and refusal analysis in Llama 2 Chat
- Multilingual bias evaluation

---

## License

This repository is intended for research purposes. Dataset usage should follow the terms of the original datasets, including StereoSet where applicable.

If you reuse this code or data, please provide appropriate citation and attribution.
