# Run Experiments

Execute your training code using the `steev run` command. Steev will automatically run your experiment code and analyze the results.

## Usage

```bash
steev run TRAIN_FILE [OPTIONS]
```

`steev` analyzes your experiment code, generates an executable script, and executes it for you. **This eliminates the need to write additional scripts or code to run experiments.**

!!! warning
    The current version of `steev` only supports the [transformers](https://github.com/huggingface/transformers) and [unsloth](https://github.com/unslothai/unsloth) libraries.

### Arguments:

- `TRAIN_FILE`: Path to the training file.
- `--steev-cfg PATH`: Path to your Steev configuration file. Default is `steev.cfg`. For more details, see [Steev Config](#steev-config).
- `--kwargs "KWARG1=VAL1,KWARG2=VAL2"`: Keyword arguments for training. For more details, see [Keyword Arguments (kwargs)](#keyword-arguments-kwargs).

### Examples:

```bash
steev run train.py
steev run train.py --steev-cfg steev.cfg
steev run train.py --kwargs epoch=10,lr=0.001,batch_size=32
steev run train.py --kwargs "epoch=10 lr=0.001 batch_size=32"
```

!!! note
    When using spaces in kwargs, enclose them in quotes. Without quotes, use commas without spaces between each kwarg.
    Remember there is no space between the key and the value when you use commas to separate kwargs.
    ```bash
    steev run train.py --kwargs epoch=10,lr=0.001
    steev run train.py --kwargs "epoch=10 lr=0.001"
    ```

### Keyword Arguments (kwargs)

Pass keyword arguments to the execution. For example, to set the learning rate to 0.001 and epoch to 10, use:

```bash
steev run train.py --kwargs "epoch=10 lr=0.001"
```

Editing `train.py` to accept kwargs is not required. You can pass kwargs as you usually do.

During training, if Steev detects anomalies in experiment logs, it will send you an email notification. You can abort the experiment by pressing the `Abort Experiment` button in the email, and Steev will stop the experiment.

### Steev Config

Sungman, this section is for you to complete.