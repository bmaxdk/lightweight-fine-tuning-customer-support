# Applying Lightweight Fine-Tuning to a Foundation Model

## Introduction
Lightweight fine-tuning is a vital technique for customizing foundation models, enabling you to adapt them to your specific needs while minimizing computational demands.

> [!TIP]
> Implement **parameter-efficient fine-tuning** using the Hugging Face `PEFT` library.
> Lightweight fine-tuning is particularly useful when resources are limited but task customization is required.

## Overview
Process of using PyTorch + Hugging Face for training and inference:

1. Load a pre-trained model and evaluate its baseline performance.
2. Perform parameter-efficient fine-tuning using the pre-trained model.
3. Run inference on the fine-tuned model and compare its performance to the original model.

## Key Concepts
### Parameter-Efficient Fine-Tuning (PEFT)
Hugging Face PEFT allows you to fine-tune models by modifying only a fraction of the parameters. This approach is resource-efficient and highly effective.

**Require Steps for PEFT Training:**
1. Creating a **PEFT config**
2. **Convert the model to a PEFT model** using the config

> [!NOTE]
> Inference with a PEFT model works similarly to inference with non-PEFT models, but the model must be loaded as a PEFT model.

# Training with PEFT

## Creating a PEFT Config
The PEFT configuration sets up the adapter parameters for fine-tuning. The base class for this configuration is `PeftConfig`, but for low-rank adaptation, use `LoraConfig`:
```python
from peft import LoraConfig
config = LoraConfig()
```
> [!TIP]
> Review the [LoRA documentation](https://huggingface.co/docs/peft/package_reference/lora) for a complete list of adjustable hyperparameters.

## Converting a Transformers Model into a PEFT Model
First, load a pre-trained model, such as GPT-2:
```py
from transformers import AutoModelForCausalLM
model = AutoModelForCausalLM.from_pretrained("gpt2")
```
Then, convert it to a PEFT model:
```py
from peft import get_peft_model
lora_model = get_peft_model(model, config)
```
> [!CAUTION]
> Ensure that your model and PEFT configuration are compatible to avoid runtime errors.

## Training with a PEFT Model
Once `get_peft_model()` has been called, you can then use the resulting `lora_model` in a training process of your choice (PyTorch training loop or Hugging Face `Trainer`)

## Checking Trainable Parameters
To inspect the number of trainable parameters, use the `print_trainable_parameters()` method:
```py
lora_model.print_trainable_parameters()
```
Example output:
```bash
trainable params: 294,912 || all params: 124,734,720 || trainable%: 0.23643136409814364
```
> [!NOTE]
> This ensures you are only fine-tuning a minimal percentage of the model's parameters.

## Saving a Trained PEFT Model
After training, use `save_pretrained()` to save the model:
```py
lora_model.save_pretrained("gpt-lora")
```
> [!NOTE]
> This **saves only the adapter weights**, resulting in smaller file sizes than you might expected.

# Inference with a PEFT Model
## Loading a Saved PEFT Model
To use the trained adapter, load the model using the PEFT-specific class.
> [!CAUTION]
> Because you have only saved the adapter weights and not the full model weights, you can't use `from_pretrained()` with the regular Transformers class (e.g., `AutoModelForCausalLM`). Instead, you need to use the PEFT version (e.g., `AutoPeftModelForCausalLM`). For example:
```py
from peft import AutoPeftModelForCausalLM
lora_model = AutoPeftModelForCausalLM.from_pretrained("gpt-lora")
```
After completing this step, you can proceed to use the model for inference.

## Generating Text with a PEFT Model
Ensure inputs are passed as keyword arguments.
> [!TIP]
> You may see examples from regular Transformer models where the input IDs are passed in as a positional argument (e.g., `model.generate(input_ids)`). For a PEFT model, they must be passed in as a keyword argument (e.g., `model.generate(input_ids=input_ids)`). For example:
```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("gpt2")
inputs = tokenizer("Hello, my name is ", return_tensors="pt")
outputs = lora_model.generate(input_ids=inputs["input_ids"], max_new_tokens=10)
print(tokenizer.batch_decode(outputs))
```


# Resources
- [Hugging Face PEFT configuration](https://huggingface.co/docs/peft/package_reference/config)
- [Hugging Face LoRA adapter](https://huggingface.co/docs/peft/package_reference/lora)
- [Hugging Face Models save_pretrained](https://huggingface.co/docs/transformers/main/en/main_classes/model#transformers.PreTrainedModel.save_pretrained)
- [Hugging Face Text Generation](https://huggingface.co/docs/transformers/main_classes/text_generation)



