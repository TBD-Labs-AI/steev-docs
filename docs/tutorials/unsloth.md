# Finetune with Unsloth
In this tutorial, we will fine-tune the distilled <span style="color:violet;">**DeepSeek-R1**</span> model using <span style="color:violet;">**Unsloth**</span>.

DeepSeek-R1 is a state-of-the-art language model known for its efficiency in handling large datasets. It uses advanced techniques to minimize computational resources. Unsloth simplifies model training and fine-tuning, supporting optimizations like LoRA and 4-bit quantization. 
## Pre-requisites
!!! note "Pre-requisites"
    To follow this tutorial, you need to have <span style="color:red">**GPU and CUDA driver installed**</span>.  
    Additionally, <span style="color:violet">**Pytorch**</span> and <span style="color:violet">**Transformers**</span> are required.

First, install the necessary packages.
```bash
pip install unsloth steev-cli trl
```

To proceed, you need to authenticate with Steev.  
For more details on authentication, refer to [Authentication](../command/auth.md)
```bash
steev auth login
```
## Get the example code
We'll use an example from [Unsloth](https://github.com/unslothai/unsloth) repository.
Copy the following code and save it as `unsloth_train.py`

The code below is a simplified version of the example from [Unsloth Documentation](https://github.com/unslothai/unsloth?tab=readme-ov-file#-documentation).
```python
from unsloth import FastLanguageModel 
from unsloth import is_bfloat16_supported
from trl import SFTTrainer
from transformers import TrainingArguments
from datasets import load_dataset

# Dataset preparation
max_seq_length = 2048 
url = "https://huggingface.co/datasets/laion/OIG/resolve/main/unified_chip2.jsonl"
dataset = load_dataset("json", data_files = {"train" : url}, split = "train")

# Model preparation
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name = "deepseek-ai/DeepSeek-R1-Distill-Llama-8B",
    max_seq_length = max_seq_length,
    dtype = None,
    load_in_4bit = True,
)

# LoRA preparation
model = FastLanguageModel.get_peft_model(
    model,
    r = 16,
    target_modules = ["q_proj", "k_proj", "v_proj", "o_proj",
                      "gate_proj", "up_proj", "down_proj",],
    lora_alpha = 16,
    lora_dropout = 0,
    bias = "none",
    use_gradient_checkpointing = "unsloth",
    random_state = 3407,
    max_seq_length = max_seq_length,
    use_rslora = False,
    loftq_config = None,
)

# Training
trainer = SFTTrainer(
    model = model,
    train_dataset = dataset,
    dataset_text_field = "text",
    max_seq_length = max_seq_length,
    tokenizer = tokenizer,
    args = TrainingArguments(
        per_device_train_batch_size = 2,
        gradient_accumulation_steps = 4,
        warmup_steps = 10,
        max_steps = 60,
        fp16 = not is_bfloat16_supported(),
        bf16 = is_bfloat16_supported(),
        logging_steps = 1,
        output_dir = "outputs",
        optim = "adamw_8bit",
        seed = 3407,
    ),
)
trainer.train()
```
## Run the code with Steev with no configuration
To run the script with Steev, simply execute the following command.
```bash
steev run unsloth_train.py
```
  
If you want to modify the parameters, there is no need to change the code.
Just pass the parameters to the command. Steev handles the rest.
```bash
steev run unsloth_train.py --kwargs lr=0.1
```

