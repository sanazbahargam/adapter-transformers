# Examples

Version 2.9 of `transformers` introduces a new [`Trainer`](https://github.com/adapter-hub/adapter-transformers/blob/master/src/transformers/trainer.py) class for PyTorch, and its equivalent [`TFTrainer`](https://github.com/adapter-hub/adapter-transformers/blob/master/src/transformers/trainer_tf.py) for TF 2.
Running the examples requires PyTorch 1.3.1+ or TensorFlow 2.1+.

Here is the list of all our examples:
- **grouped by task** (all official examples work for multiple models)
- with information on whether support for training **Adapters** has been added to one of the example scripts (currently PyTorch only)
- with information on whether they are **built on top of `Trainer`/`TFTrainer`** (if not, they still work, they might just lack some features),
- whether they also include examples for **`pytorch-lightning`**, which is a great fully-featured, general-purpose training library for PyTorch,

This is still a work-in-progress – in particular documentation is still sparse – so please **contribute improvements/pull requests.**

**Note**: For more information on training Adapters, please refer to the [Training section in the Adapter-Hub documentation](https://docs.adapterhub.ml/training).


## The Big Table of Tasks

| Task | Example datasets | Adapter support (pytorch) | Trainer support | TFTrainer support | pytorch-lightning
|---|---|:---:|:---:|:---:|:---:|
| [**`language-modeling`**](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/language-modeling)       | Raw text        | ✅ | ✅ | -  | -  
| [**`text-classification`**](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/text-classification)   | GLUE, XNLI      | ✅ | ✅ | ✅ | ✅ 
| [**`token-classification`**](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/token-classification) | CoNLL NER       | ✅ | ✅ | ✅ | ✅ 
| [**`multiple-choice`**](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/multiple-choice)           | SWAG, RACE, ARC | ✅ | ✅ | ✅ | - 
| [**`question-answering`**](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/question-answering)     | SQuAD           | ✅ | -  | ✅ | - 
| [**`text-generation`**](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/text-generation)     | -           | n/a | n/a  | n/a | n/a 
| [**`distillation`**](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/distillation)       | All               | - | -  | -  | - 
| [**`summarization`**](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/summarization)     | CNN/Daily Mail    | - | -  | -  | ✅ 
| [**`translation`**](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/translation)         | WMT               | - | -  | -  | ✅ 
| [**`bertology`**](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/bertology)             | -                 | - | -  | -  | - 
| [**`adversarial`**](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/adversarial)         | HANS              | - | ✅ | -  | - 


<br>

## Important note

**Important**
To make sure you can successfully run the latest versions of the example scripts, you have to install the library from source and install some example-specific requirements.
Execute the following steps in a new virtual environment:

```bash
git clone https://github.com/adapter-hub/adapter-transformers
cd transformers
pip install .
pip install -r ./examples/requirements.txt
```

## One-click Deploy to Cloud (wip)

#### Azure

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json)

## Running on TPUs

When using Tensorflow, TPUs are supported out of the box as a `tf.distribute.Strategy`.

When using PyTorch, we support TPUs thanks to `pytorch/xla`. For more context and information on how to setup your TPU environment refer to Google's documentation and to the
very detailed [pytorch/xla README](https://github.com/pytorch/xla/blob/master/README.md).

In this repo, we provide a very simple launcher script named [xla_spawn.py](https://github.com/adapter-hub/adapter-transformers/tree/master/examples/xla_spawn.py) that lets you run our example scripts on multiple TPU cores without any boilerplate.
Just pass a `--num_cores` flag to this script, then your regular training script with its arguments (this is similar to the `torch.distributed.launch` helper for torch.distributed).

For example for `run_glue`:

```bash
python examples/xla_spawn.py --num_cores 8 \
	examples/text-classification/run_glue.py
	--model_name_or_path bert-base-cased \
	--task_name mnli \
	--data_dir ./data/glue_data/MNLI \
	--output_dir ./models/tpu \
	--overwrite_output_dir \
	--do_train \
	--do_eval \
	--num_train_epochs 1 \
	--save_steps 20000
```

Feedback and more use cases and benchmarks involving TPUs are welcome, please share with the community.
