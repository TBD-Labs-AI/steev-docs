# Finetune with HuggingFace TRL
In this tutorial, we will fine-tune the distilled <span style="color:violet;">**DeepSeek-R1**</span> model using <span style="color:violet;">**HuggingFace TRL**</span>.

DeepSeek-R1 is a state-of-the-art language model known for its efficiency in handling large datasets. It uses advanced techniques to minimize computational resources. HuggingFace TRL is a library that simplifies model training and fine-tuning.
## Pre-requisites
!!! note "Pre-requisites"
    To follow this tutorial, you need to have <span style="color:red">**GPU and CUDA driver installed**</span>.  
    Additionally, <span style="color:violet">**Pytorch**</span> and <span style="color:violet">**Transformers**</span> are required.

First, install the necessary packages.
```bash
pip install steev trl
```

To proceed, you need to authenticate with Steev.  
For more details on authentication, refer to [Authentication](../command/auth.md)
```bash
steev auth login
```
## Get the example code
We'll use an example from [HuggingFace TRL Documentation](https://github.com/huggingface/trl/blob/main/trl/scripts/sft.py).
Copy the following code and save it as `trl_train.py`

The code below is a simplified version of the example.
```python
import argparse
import multiprocessing
import os

import torch
from accelerate import PartialState
from datasets import load_dataset
from peft import LoraConfig
from transformers import (
    AutoTokenizer,
    BitsAndBytesConfig,
    logging,
    set_seed,
)
from trl import (
    SFTTrainer,
    SFTConfig,
    get_kbit_device_map,
)


def get_args():
    parser = argparse.ArgumentParser()
    parser.add_argument("--model_name_or_path", type=str, default="HuggingFaceTB/SmolLM2-1.7B")
    parser.add_argument("--dataset_name", type=str, default="bigcode/the-stack-smol")
    parser.add_argument("--dataset_config", type=str, default=None)
    parser.add_argument("--dataset_train_split", type=str, default="train")
    parser.add_argument("--dataset_test_split", type=str, default="test")
    parser.add_argument("--dataset_text_field", type=str, default="content")

    # Model arguments
    parser.add_argument("--model_revision", type=str, default=None)
    parser.add_argument("--trust_remote_code", type=bool, default=True)
    parser.add_argument("--attn_implementation", type=str, default=None)
    parser.add_argument("--torch_dtype", type=str, default="bfloat16")

    # Training arguments
    parser.add_argument("--max_seq_length", type=int, default=2048)
    parser.add_argument("--max_steps", type=int, default=1000)
    parser.add_argument("--per_device_train_batch_size", type=int, default=1)
    parser.add_argument("--gradient_accumulation_steps", type=int, default=4)
    parser.add_argument("--weight_decay", type=float, default=0.01)
    parser.add_argument("--learning_rate", type=float, default=2e-4)
    parser.add_argument("--lr_scheduler_type", type=str, default="cosine")
    parser.add_argument("--warmup_steps", type=int, default=100)
    parser.add_argument("--eval_strategy", type=str, default="no")
    parser.add_argument("--gradient_checkpointing", type=bool, default=False)
    parser.add_argument("--seed", type=int, default=0)
    parser.add_argument("--output_dir", type=str, default="finetune_smollm2_python")
    parser.add_argument("--num_proc", type=int, default=None)
    parser.add_argument("--push_to_hub", type=bool, default=True)
    parser.add_argument("--repo_id", type=str, default="SmolLM2-1.7B-finetune")

    # QLoRA arguments
    parser.add_argument("--use_qlora", type=bool, default=False)
    parser.add_argument("--lora_r", type=int, default=16)
    parser.add_argument("--lora_alpha", type=int, default=32)
    parser.add_argument("--lora_dropout", type=float, default=0.05)
    parser.add_argument("--lora_target_modules", type=str, nargs="+", default=["q_proj", "v_proj"])

    return parser.parse_args()


def main(args):
    # Convert torch_dtype from string to torch.dtype
    if args.torch_dtype != "auto":
        args.torch_dtype = getattr(torch, args.torch_dtype)

    # Model init kwargs
    model_kwargs = dict(
        revision=args.model_revision,
        trust_remote_code=args.trust_remote_code,
        attn_implementation=args.attn_implementation,
        torch_dtype=args.torch_dtype,
        use_cache=False if args.gradient_checkpointing else True,
    )

    # QLoRA config
    if args.use_qlora:
        quantization_config = BitsAndBytesConfig(
            load_in_4bit=True,
            bnb_4bit_quant_type="nf4",
            bnb_4bit_compute_dtype=args.torch_dtype if args.torch_dtype != "auto" else torch.bfloat16,
        )
        model_kwargs["device_map"] = get_kbit_device_map()
        model_kwargs["quantization_config"] = quantization_config

        peft_config = LoraConfig(
            r=args.lora_r,
            lora_alpha=args.lora_alpha,
            lora_dropout=args.lora_dropout,
            target_modules=args.lora_target_modules,
            bias="none",
            task_type="CAUSAL_LM",
        )
    else:
        quantization_config = None
        peft_config = None
        model_kwargs["device_map"] = {"": PartialState().process_index}

    # Load tokenizer
    tokenizer = AutoTokenizer.from_pretrained(
        args.model_name_or_path, trust_remote_code=args.trust_remote_code, use_fast=True
    )
    tokenizer.pad_token = tokenizer.eos_token

    # Load dataset
    token = os.environ.get("HF_TOKEN", None)
    dataset = load_dataset(
        args.dataset_name,
        name=args.dataset_config,
        token=token,
        num_proc=args.num_proc if args.num_proc else multiprocessing.cpu_count(),
    )

    # Format dataset to use the correct text field
    def formatting_func(example):
        return example[args.dataset_text_field]

    # Setup trainer
    trainer = SFTTrainer(
        model=args.model_name_or_path,
        train_dataset=dataset[args.dataset_train_split],
        eval_dataset=dataset[args.dataset_test_split] if args.eval_strategy != "no" else None,
        args=SFTConfig(
            per_device_train_batch_size=args.per_device_train_batch_size,
            gradient_accumulation_steps=args.gradient_accumulation_steps,
            warmup_steps=args.warmup_steps,
            max_steps=args.max_steps,
            learning_rate=args.learning_rate,
            lr_scheduler_type=args.lr_scheduler_type,
            weight_decay=args.weight_decay,
            max_seq_length=args.max_seq_length,
            gradient_checkpointing=args.gradient_checkpointing,
            logging_strategy="steps",
            logging_steps=10,
            output_dir=args.output_dir,
            optim="paged_adamw_8bit",
            seed=args.seed,
            run_name=None,
            report_to=[],
            push_to_hub=args.push_to_hub,
            model_init_kwargs=model_kwargs,
        ),
        processing_class=tokenizer,
        peft_config=peft_config,
        formatting_func=formatting_func,
    )

    # Train
    print("Training...")
    trainer.train()

    # Save and push to hub
    trainer.save_model(args.output_dir)
    if args.push_to_hub:
        trainer.push_to_hub(dataset_name=args.dataset_name)

    print("Training Done! 💥")


if __name__ == "__main__":
    args = get_args()
    set_seed(args.seed)
    os.makedirs(args.output_dir, exist_ok=True)
    logging.set_verbosity_error()
    main(args)
```
## Run the code with Steev with no configuration
To run the script with Steev, simply execute the following command.
```bash
steev run trl_train.py
```
  
If you want to modify the parameters, there is no need to change the code.
Just pass the parameters to the command. Steev handles the rest.
```bash
steev run trl_train.py --kwargs lr=0.1
```

