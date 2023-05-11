# Overview

[Flan-T5 XL](https://huggingface.co/google/flan-t5-xl?text=Q%3A+%28+False+or+not+False+or+False+%29+is%3F+A%3A+Let%27s+think+step+by+step) is an open-source large language model developed by Google.

Flan-T5 XL has a number of use cases such as:

* Sentiment analysis
* Paraphrasing/sentence similarity
* Natural language inference
* Sentence completion
* Question answering

Flan-T5 XL is similar to T5 except it is "instruction tuned". In practice, this means that the model is comparable to GPT-3 in multitask benchmarks because it is fine-tuned to follow human inputs / instructions.


# Deploying to Baseten

To deploy this Truss on Baseten, first install the Baseten client:

```
$ pip install baseten
```

Then, in a Python shell, you can do the following to have an instance of CLIP deployed
on Baseten:

```python
import baseten
import truss

flan_t5_handle = truss.load(".")
baseten.deploy(flan_t5_handle, model_name="FLAN-T5")
```

# Usage

## Inputs
The input should be a list of dictionaries and must contain the following key:
* `prompt` - the prompt for text generation

Additionally; the following optional parameters are supported as pass thru to the `generate` method. For more details
 look towards the [official documentation](https://huggingface.co/docs/transformers/main/en/main_classes/
text_generation#transformers.generation_utils.GenerationMixin.generate)

* `max_length` - int - limited to  512
* `min_length` - int - limited to 64
* `do_sample` - bool
* `early_stopping` - bool
* `num_beams` - int
* `temperature`  - float
* `top_k` - int
* `top_p` - float
* `repetition_penalty` - float
* `length_penalty` - float
* `encoder_no_repeat_ngram_size` - int
* `num_return_sequences` - int
* `max_time` - float
* `num_beam_groups` - int
* `diversity_penalty` - float
* `remove_invalid_values` - bool

Here's an example input:

```json
[
    {
        "prompt": "If I was a billionaire, I would",
        "max_length": 50
    }
]
```

## Outputs
The result will be a dictionary containing:
* `status` - either `success` or `failed`
* `data` - the output text
* `message` - will contain details in the case of errors

```json
{"status": "success", "data": "If I was a billionaire, I would buy a plane.", "message": null}
```

## Example

You can invoke this model on Baseten with the following cURL command (just fill in the model version ID and API Key):

```bash
 curl -X POST https://app.staging.baseten.co/models/{MODEL_VERSION_ID}/predict \
  -H 'Authorization: Api-Key {YOUR_API_KEY}' \
  -d '{"prompt": "Answer the question: What is 1+1"}'
{"model_id": "v0VV40K", "model_version_id": "7qrmz03", "model_output": {"status": "success", "data": "Answer the question: What is 1+1?\n\nThe answer is 2.\n\nThe", "message": null}}
```