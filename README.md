Generally the LLM will generate the desired output when you use the same format for the fine-tuning dataset, that the base model was formatted in. This formatting helps "steer" or "contextualize" the generation text.

So always first determine what is the acceptable prompt format for that specific model.

Then format your new (custom) data in that same format.

=======================================================
## Alpaca datasets


```
### Instruction:
(Instruction Text)

### Input:
(Auxiliary Input Text)

### Response:
(Desired Response Text)

```
=======================================================

## Vicuna datasets


A chat between a human and an assistant.

```
### Human:
(Question Text)
### Assistant:
(Response Text)

```

### So a Vicuna inference prompt will look like this

### Human:
What shape is the Earth?
### Assistant:
If you are using transformers directly in python to perform inference you will have to add the "### Assistant:\n" line to the end of you prompt minding the how line breaks '\n' are handled in the dataset. LLM's are glorious auto-completes if not quite a stochastic parrots.

=======================================================

## Microsoft Orca paper:

<System>: (You are a helpful <role> ai assistant)
<Instruction>: (Instruction Text Goes Here)
<Input>: (Other input goes here)
<Response>: (The desired response goes here)


Be mindful of line breaks ( also end of text symbols if the LLM model has any in pretraining ) in your dataset and prompt.


=======================================================

# Prompt format of mistralai/Mixtral-8x7B-v0.1 model

https://huggingface.co/mistralai/Mixtral-8x7B-v0.1/discussions/22


That model is a base model, therefore it doesn't need to be prompted in a specific way in order to get started with the model. If you want to use the instruct version of the model, you need to follow the template that is on the model card: https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1#instruction-format

The template used to build a prompt for the Instruct model is defined as follows:

```
<s> [INST] Instruction [/INST] Model answer</s> [INST] Follow-up instruction [/INST]
```

=======================================================

## llama-2

The prompt template for the first turn looks like this:

```

<s>[INST] <<SYS>>
{{ system_prompt }}
<</SYS>>

{{ user_message }} [/INST]

```

To spell it out in full clarity, this is what is actually sent to the language model when the user enters some text (There's a llama in my garden 😱 What should I do?) in our 13B chat demo to initiate a chat:



```
<s>[INST] <<SYS>>

You are a helpful, respectful and honest assistant. Always answer as helpfully as possible, while being safe.  Your answers should not include any harmful, unethical, racist, sexist, toxic, dangerous, or illegal content. Please ensure that your responses are socially unbiased and positive in nature.

If a question does not make any sense, or is not factually coherent, explain why instead of answering something not correct. If you don't know the answer to a question, please don't share false information.
<</SYS>>

There's a llama in my garden 😱 What should I do? [/INST]

```


As you can see, the instructions between the special <<SYS>> tokens provide context for the model so it knows how we expect it to respond. This works because exactly the same format was used during training with a wide variety of system prompts intended for different tasks.

As the conversation progresses, all the interactions between the human and the "bot" are appended to the previous prompt, enclosed between [INST] delimiters. The template used during multi-turn conversations follows this structure.

```
<s>[INST] <<SYS>>
{{ system_prompt }}
<</SYS>>

{{ user_msg_1 }} [/INST] {{ model_answer_1 }} </s><s>[INST] {{ user_msg_2 }} [/INST]

```