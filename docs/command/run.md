# Run Experiments

Execute your training code using the `steev run` command. Steev automatically runs your experiment code and analyzes the results, eliminating the need to write additional scripts.

!!! warning
    The current version of Steev only supports the [HuggingFace TRL](https://github.com/huggingface/trl) and [Unsloth](https://github.com/unslothai/unsloth) libraries.

## Basic Usage

```bash
steev run TRAIN_FILE [OPTIONS]
```

### Required Arguments

- `TRAIN_FILE`: Path to your training script (e.g., `train.py`)

### Optional Arguments

- `--steev-cfg PATH`: Path to your Steev configuration file (default: `steev.cfg`)
- `--kwargs "KWARG1=VAL1,KWARG2=VAL2"`: Keyword arguments to pass to your training script

## Examples

Run a basic training script:
```bash
steev run train.py
```

Specify a custom config file:
```bash
steev run train.py --steev-cfg custom_steev.cfg
```

Pass training parameters:
```bash
steev run train.py --kwargs "epoch=10 lr=0.001 batch_size=32"
# Or using commas
steev run train.py --kwargs epoch=10,lr=0.001,batch_size=32
```

## Passing Keyword Arguments

You can pass any keyword arguments to your training script using the `--kwargs` option. These work just like regular Python keyword arguments - no modifications to your training code required.

!!! note "Formatting kwargs"
    There are two ways to format your kwargs:

    1. Using spaces (must be quoted):
       ```bash
       steev run train.py --kwargs "epoch=10 lr=0.001"
       ```

    2. Using commas (no quotes needed):
       ```bash
       steev run train.py --kwargs epoch=10,lr=0.001
       ```

## Experiment Monitoring

During training, Steev actively monitors your experiment logs for anomalies. If any issues are detected:

1. You'll receive an email notification
2. The email will include an "Abort Experiment" button
3. Clicking the button will safely stop your experiment

## Steev Config

Steev config is a file that contains the configuration for your experiment. 
It is used to specify the parameters for tracking during your experiment.

Below is an example of a Steev config file:

```cfg
alert_rules:
  metrics: # Add metrics name to monitor
    - name: loss
    - name: grad_norm
  actions:
    notify_user: true
```

If you want to track additional metrics, add the metric names to the `metrics` list to monitor.
(For example, 'val_loss', 'val_accuracy', 'loss_mAP', etc.)