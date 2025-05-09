---
title: "Interview Preparation - LLM Cheatsheet"
excerpt: "When I have only 1 hour right before the interview...😅 <br/><img src='/images/LLM.png'>"
collection: projects
date: 2025-05-08
category: pythons
description: "Notes from HuggingFace🤗 LLM course."
tags:
  - LLM
  - Transformers 
  - Attention
---


# Natural Language Processing and Large Language Models

## What’s the difference?

**NLP (Natural Language Processing)** is the broader field focused on enabling computers to understand, interpret, and generate human language. NLP encompasses many techniques and tasks such as sentiment analysis, named entity recognition, and machine translation.

**LLMs (Large Language Models)** are a powerful subset of NLP models characterized by their massive size, extensive training data, and ability to perform a wide range of language tasks with minimal task-specific training. Models like the Llama, GPT, or Claude series are examples of LLMs that have revolutionized what’s possible in NLP.


**NLP** is a field of linguistics and machine learning focused on understanding everything related to human language. The aim of NLP tasks is not only to understand single words individually, but to be able to understand the context of those words.

The following is a list of common NLP tasks, with some examples of each:

- **Classifying whole sentences**: Getting the sentiment of a review, detecting if an email is spam, determining if a sentence is grammatically correct or whether two sentences are logically related or not
- **Classifying each word in a sentence**: Identifying the grammatical components of a sentence (noun, verb, adjective), or the named entities (person, location, organization)
- **Generating text content**: Completing a prompt with auto-generated text, filling in the blanks in a text with masked words
- **Extracting an answer from a text**: Given a question and a context, extracting the answer to the question based on the information provided in the context
- **Generating a new sentence from an input text**: Translating a text into another language, summarizing a text

NLP isn’t limited to written text though. It also tackles complex challenges in speech recognition and computer vision, such as generating a transcript of an audio sample or a description of an image.

**A large language model (LLM)** is an AI model trained on massive amounts of text data that can understand and generate human-like text, recognize patterns in language, and perform a wide variety of language tasks without task-specific training. They represent a significant advancement in the field of natural language processing (NLP).

LLMs are characterized by:

- **Scale**: They contain millions, billions, or even hundreds of billions of parameters
- **General capabilities**: They can perform multiple tasks without task-specific training
- **In-context learning**: They can learn from examples provided in the prompt
- **Emergent abilities**: As these models grow in size, they demonstrate capabilities that weren’t explicitly programmed or anticipated

The advent of LLMs has shifted the paradigm from building specialized models for specific NLP tasks to using a single, large model that can be prompted or fine-tuned to address a wide range of language tasks. This has made sophisticated language processing more accessible while also introducing new challenges in areas like efficiency, ethics, and deployment.

However, LLMs also have important limitations:

- **Hallucinations**: They can generate incorrect information confidently
- **Lack of true understanding**: They lack true understanding of the world and operate purely on statistical patterns
- **Bias**: They may reproduce biases present in their training data or inputs.
- **Context windows**: They have limited context windows (though this is improving)
- **Computational resources**: They require significant computational resources

## Transformers, what can they do?

In this section, we will look at what Transformer models can do and use our first tool from the 🤗 Transformers library: the `pipeline()` function.

### Available pipelines for different modalities

The `pipeline()` function supports multiple modalities, allowing you to work with text, images, audio, and even multimodal tasks. In this course we’ll focus on text tasks, but it’s useful to understand the transformer architecture’s potential, so we’ll briefly outline it.

Here’s an overview of what’s available:

**Text pipelines**
- `text-generation`: Generate text from a prompt
- `text-classification`: Classify text into predefined categories
- `summarization`: Create a shorter version of a text while preserving key information
- `translation`: Translate text from one language to another
- `zero-shot-classification`: Classify text without prior training on specific labels
- `feature-extraction`: Extract vector representations of text

**Image pipelines**
- `image-to-text`: Generate text descriptions of images
- `image-classification`: Identify objects in an image
- `object-detection`: Locate and identify objects in images

**Audio pipelines**
- `automatic-speech-recognition`: Convert speech to text
- `audio-classification`: Classify audio into categories
- `text-to-speech`: Convert text to spoken audio

**Multimodal pipelines**
- `image-text-to-text`: Respond to an image based on a text prompt


Let’s explore some of these pipelines in more detail!

### Zero-shot classification

We’ll start by tackling a more challenging task where we need to classify texts that haven’t been labelled. This is a common scenario in real-world projects because annotating text is usually time-consuming and requires domain expertise. For this use case, the zero-shot-classification pipeline is very powerful: it allows you to specify which labels to use for the classification, so you don’t have to rely on the labels of the pretrained model. You’ve already seen how the model can classify a sentence as positive or negative using those two labels — but it can also classify the text using any other set of labels you like.

```python
from transformers import pipeline

classifier = pipeline("zero-shot-classification")
classifier(
    "This is a course about the Transformers library",
    candidate_labels=["education", "politics", "business"],
)
```

```
{'sequence': 'This is a course about the Transformers library',
 'labels': ['education', 'business', 'politics'],
 'scores': [0.8445963859558105, 0.111976258456707, 0.043427448719739914]}
```

This pipeline is called *zero-shot* because you don’t need to fine-tune the model on your data to use it. It can directly return probability scores for any list of labels you want!


### Text generation

Now let’s see how to use a pipeline to generate some text. The main idea here is that you provide a prompt and the model will auto-complete it by generating the remaining text. This is similar to the predictive text feature that is found on many phones. Text generation involves randomness, so it’s normal if you don’t get the same results as shown below.

```python
from transformers import pipeline

generator = pipeline("text-generation")
generator("In this course, we will teach you how to")
```

```
[{'generated_text': 'In this course, we will teach you how to understand and use '
                    'data flow and data interchange when handling user data. We '
                    'will be working with one or more of the most commonly used '
                    'data flows — data flows of various types, as seen by the '
                    'HTTP'}]
```

You can control how many different sequences are generated with the argument `num_return_sequences` and the total length of the output text with the argument `max_length`.


### Using any model from the Hub in a pipeline

The previous examples used the default model for the task at hand, but you can also choose a particular model from the Hub to use in a pipeline for a specific task — say, text generation. 

Let’s try the `HuggingFaceTB/SmolLM2-360M` model! Here’s how to load it in the same pipeline as before:

```python
from transformers import pipeline

generator = pipeline("text-generation", model="HuggingFaceTB/SmolLM2-360M")
generator(
    "In this course, we will teach you how to",
    max_length=30,
    num_return_sequences=2,
)
```

```
[{'generated_text': 'In this course, we will teach you how to manipulate the world and '
                    'move your mental and physical capabilities to your advantage.'},
 {'generated_text': 'In this course, we will teach you how to become an expert and '
                    'practice realtime, and with a hands on experience on both real '
                    'time and real'}]
```

You can refine your search for a model by clicking on the language tags, and pick a model that will generate text in another language. The Model Hub even contains checkpoints for multilingual models that support several languages.

Once you select a model by clicking on it, you’ll see that there is a widget enabling you to try it directly online. This way you can quickly test the model’s capabilities before downloading it.

### Inference Providers

All the models can be tested directly through your browser using the Inference Providers, which is available on the Hugging Face website. You can play with the model directly on this page by inputting custom text and watching the model process the input data.

Inference Providers that powers the widget is also available as a paid product, which comes in handy if you need it for your workflows. 

### Mask filling

The next pipeline you’ll try is fill-mask. The idea of this task is to fill in the blanks in a given text:

```python
from transformers import pipeline

unmasker = pipeline("fill-mask")
unmasker("This course will teach you all about <mask> models.", top_k=2)
```

```
[{'sequence': 'This course will teach you all about mathematical models.',
  'score': 0.19619831442832947,
  'token': 30412,
  'token_str': ' mathematical'},
 {'sequence': 'This course will teach you all about computational models.',
  'score': 0.04052725434303284,
  'token': 38163,
  'token_str': ' computational'}]
```

The `top_k` argument controls how many possibilities you want to be displayed. Note that here the model fills in the special `<mask>` word, which is often referred to as a *mask token*. Other mask-filling models might have different mask tokens, so it’s always good to verify the proper mask word when exploring other models. One way to check it is by looking at the mask word used in the widget.

### Named entity recognition

Named entity recognition (NER) is a task where the model has to find which parts of the input text correspond to entities such as persons, locations, or organizations. Let’s look at an example:

```python
from transformers import pipeline

ner = pipeline("ner", grouped_entities=True)
ner("My name is Sylvain and I work at Hugging Face in Brooklyn.")
```

```
[{'entity_group': 'PER', 'score': 0.99816, 'word': 'Sylvain', 'start': 11, 'end': 18}, 
 {'entity_group': 'ORG', 'score': 0.97960, 'word': 'Hugging Face', 'start': 33, 'end': 45}, 
 {'entity_group': 'LOC', 'score': 0.99321, 'word': 'Brooklyn', 'start': 49, 'end': 57}
]
```

Here the model correctly identified that Sylvain is a person (PER), Hugging Face an organization (ORG), and Brooklyn a location (LOC).

We pass the option `grouped_entities=True` in the pipeline creation function to tell the pipeline to regroup together the parts of the sentence that correspond to the same entity: here the model correctly grouped “Hugging” and “Face” as a single organization, even though the name consists of multiple words. In fact, as we will see in the next chapter, the preprocessing even splits some words into smaller parts. For instance, Sylvain is split into four pieces: `S`, `##yl`, `##va`, and `##in`. In the post-processing step, the pipeline successfully regrouped those pieces.

### Question answering

The `question-answering` pipeline answers questions using information from a given context:

```python
from transformers import pipeline

question_answerer = pipeline("question-answering")
question_answerer(
    question="Where do I work?",
    context="My name is Sylvain and I work at Hugging Face in Brooklyn",
)
```
```
{'score': 0.6385916471481323, 'start': 33, 'end': 45, 'answer': 'Hugging Face'}
```

Note that this pipeline works by extracting information from the provided context; it does not generate the answer.

### Summarization

Summarization is the task of reducing a text into a shorter text while keeping all (or most) of the important aspects referenced in the text. Here’s an example:

```python
from transformers import pipeline

summarizer = pipeline("summarization")
summarizer(
    """
    America has changed dramatically during recent years. Not only has the number of 
    graduates in traditional engineering disciplines such as mechanical, civil, 
    electrical, chemical, and aeronautical engineering declined, but in most of 
    the premier American universities engineering curricula now concentrate on 
    and encourage largely the study of engineering science. As a result, there 
    are declining offerings in engineering subjects dealing with infrastructure, 
    the environment, and related issues, and greater concentration on high 
    technology subjects, largely supporting increasingly complex scientific 
    developments. While the latter is important, it should not be at the expense 
    of more traditional engineering.

    Rapidly developing economies such as China and India, as well as other 
    industrial countries in Europe and Asia, continue to encourage and advance 
    the teaching of engineering. Both China and India, respectively, graduate 
    six and eight times as many traditional engineers as does the United States. 
    Other industrial countries at minimum maintain their output, while America 
    suffers an increasingly serious decline in the number of engineering graduates 
    and a lack of well-educated engineers.
"""
)
```

```
[{'summary_text': ' America has changed dramatically during recent years . The '
                  'number of engineering graduates in the U.S. has declined in '
                  'traditional engineering disciplines such as mechanical, civil '
                  ', electrical, chemical, and aeronautical engineering . Rapidly '
                  'developing economies such as China and India, as well as other '
                  'industrial countries in Europe and Asia, continue to encourage '
                  'and advance engineering .'}]
```

Like with text generation, you can specify a `max_length` or a `min_length`for the result.

### Translation

For translation, you can use a default model if you provide a language pair in the task name (such as "`translation_en_to_fr`"), but the easiest way is to pick the model you want to use on the Model Hub. Here we’ll try translating from French to English:
```python
from transformers import pipeline

translator = pipeline("translation", model="Helsinki-NLP/opus-mt-fr-en")
translator("Ce cours est produit par Hugging Face.")
```

```
[{'translation_text': 'This course is produced by Hugging Face.'}]
```
Like with text generation and summarization, you can specify a `max_length` or a `min_length` for the result.


### Image and audio pipelines

Beyond text, Transformer models can also work with images and audio. Here are a few examples:

### Image classification

```python
from transformers import pipeline

image_classifier = pipeline(
    task="image-classification", model="google/vit-base-patch16-224"
)
result = image_classifier(
    "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/pipeline-cat-chonk.jpeg"
)
print(result)
```

```
[{'label': 'lynx, catamount', 'score': 0.43350091576576233},
 {'label': 'cougar, puma, catamount, mountain lion, painter, panther, Felis concolor',
  'score': 0.034796204417943954},
 {'label': 'snow leopard, ounce, Panthera uncia',
  'score': 0.03240183740854263},
 {'label': 'Egyptian cat', 'score': 0.02394474856555462},
 {'label': 'tiger cat', 'score': 0.02288915030658245}]
```


### Automatic speech recognition

```python
from transformers import pipeline

transcriber = pipeline(
    task="automatic-speech-recognition", model="openai/whisper-large-v3"
)
result = transcriber(
    "https://huggingface.co/datasets/Narsil/asr_dummy/resolve/main/mlk.flac"
)
print(result)
```

```
{'text': ' I have a dream that one day this nation will rise up and live out the true meaning of its creed.'}
```

### Combining data from multiple sources

One powerful application of Transformer models is <span style="background-color: #FFFF33;">their ability to combine and process data from multiple sources. This is especially useful when you need to: </span>

1. <span style="background-color: #FFFF33;">Search across multiple databases or repositories </span>
2. <span style="background-color: #FFFF33;">Consolidate information from different formats (text, images, audio) </span>
3. <span style="background-color: #FFFF33;">Create a unified view of related information </span>

For example, you could build a system that:

- Searches for information across databases in multiple modalities like text and image.
- Combines results from different sources into a single coherent response. For example, from an audio file and text description.
- Presents the most relevant information from a database of documents and metadata.

## How do Transformers work?

- <span style="background-color: #FFFF33;">GPT-like (also called *auto-regressive* Transformer models)</span>

- <span style="background-color: #FFFF33;">BERT-like (also called *auto-encoding* Transformer models)</span>

- <span style="background-color: #FFFF33;">T5-like (also called *sequence-to-sequence* Transformer models)</span>

## Transformers are language models

All the Transformer models mentioned above (GPT, BERT, T5, etc.) have been trained as language models. This means they have been trained on large amounts of raw text in a self-supervised fashion.

*Self-supervised learning* is a type of training in which the objective is automatically computed from the inputs of the model. That means that humans are not needed to label the data!

This type of model develops a statistical understanding of the language it has been trained on, but it’s less useful for specific practical tasks. Because of this, the general pretrained model then goes through a process called *transfer learning* or *fine-tuning*. During this process, the model is fine-tuned in a supervised way — that is, using human-annotated labels — on a given task.

An example of a task is predicting the next word in a sentence having read the n previous words. This is called *causal language modeling* because the output depends on the past and present inputs, but not the future ones.

![casual](/images/casuallanguagemodeling.png)

Another example is *masked language modeling*, in which the model predicts a masked word in the sentence.

![masked](/images/maskedlanguagemodeling.png)


## Transfer Learning

*Pretraining* is the act of training a model from scratch: the weights are randomly initialized, and the training starts without any prior knowledge.

![Pretraining](/images/Pretraining.png)

The pretraining of a language model is costly in both time and money.
This pretraining is usually done on very large amounts of data. Therefore, it requires a very large corpus of data, and training can take up to several weeks.

*Fine-tuning*, on the other hand, is the training done **after** a model has been pretrained. To perform fine-tuning, you first acquire a pretrained language model, then perform additional training with a dataset specific to your task. Wait — why not simply train the model for your final use case from the start (**scratch**)? There are a couple of reasons:

- The pretrained model was already trained on a dataset that has some similarities with the fine-tuning dataset. The fine-tuning process is thus able to take advantage of knowledge acquired by the initial model during pretraining (for instance, with NLP problems, the pretrained model will have some kind of statistical understanding of the language you are using for your task).

- Since the pretrained model was already trained on lots of data, the fine-tuning requires way less data to get decent results.

- For the same reason, the amount of time and resources needed to get good results are much lower.

For example, one could leverage a pretrained model trained on the English language and then fine-tune it on an arXiv corpus, resulting in a science/research-based model. The fine-tuning will only require a limited amount of data: the knowledge the pretrained model has acquired is “transferred,” hence the term transfer learning.

![Fine-Tuning](/images/Fine-Tuning.png)

The fine-tuning of a language model is cheaper than pretraining in both time and money.
Fine-tuning a model therefore has lower time, data, financial, and environmental costs. It is also quicker and easier to iterate over different fine-tuning schemes, as the training is less constraining than a full pretraining.

This process will also achieve better results than training from scratch (unless you have lots of data), which is why you should always try to leverage a pretrained model — one as close as possible to the task you have at hand — and fine-tune it.


## General Transformer architecture

The model is primarily composed of two blocks:

<span style="background-color: #FFFF33;">**Encoder (left)**: The encoder receives an input and builds a representation of it (its features). This means that the model is optimized to acquire understanding from the input.</span>

<span style="background-color: #FFFF33;">**Decoder (right)**: The decoder uses the encoder’s representation (features) along with other inputs to generate a target sequence. This means that the model is optimized for generating outputs.</span>

 ![Encoder-Decoder](/images/Encoder-Decoder.png)

Each of these parts can be used independently, depending on the task:

- <span style="background-color: #FFFF33;">**Encoder-only models**: Good for tasks that require understanding of the input, such as sentence classification and named entity recognition.</span>
- <span style="background-color: #FFFF33;">**Decoder-only models**: Good for generative tasks such as text generation.</span>
- <span style="background-color: #FFFF33;">**Encoder-decoder models** or **sequence-to-sequence models**: Good for generative tasks that require an input, such as translation or summarization.</span>


### Attention layers

A key feature of Transformer models is that they are built with special layers called attention layers. This layer will tell the model to pay specific attention to certain words in the sentence you passed it (and more or less ignore the others) when dealing with the representation of each word.

To put this into context, consider the task of translating text from English to French. Given the input “You like this course”, a translation model will need to also attend to the adjacent word “You” to get the proper translation for the word “like”, because in French the verb “like” is conjugated differently depending on the subject. The rest of the sentence, however, is not useful for the translation of that word. In the same vein, when translating “this” the model will also need to pay attention to the word “course”, because “this” translates differently depending on whether the associated noun is masculine or feminine. Again, the other words in the sentence will not matter for the translation of “course”. With more complex sentences (and more complex grammar rules), the model would need to pay special attention to words that might appear farther away in the sentence to properly translate each word.

The same concept applies to any task associated with natural language: a word by itself has a meaning, but that meaning is deeply affected by the context, which can be any other word (or words) before or after the word being studied.

Now that you have an idea of what attention layers are all about, let’s take a closer look at the Transformer architecture.

### The original architecture
<span style="background-color: #FFFF33;">The Transformer architecture was originally designed for translation. During training, the encoder receives inputs (sentences) in a certain language, while the decoder receives the same sentences in the desired target language. In the encoder, the attention layers can use all the words in a sentence (since, as we just saw, the translation of a given word can be dependent on what is after as well as before it in the sentence). The decoder, however, works sequentially and can only pay attention to the words in the sentence that it has already translated (so, only the words before the word currently being generated). For example, when we have predicted the first three words of the translated target, we give them to the decoder which then uses all the inputs of the encoder to try to predict the fourth word.</span>

<span style="background-color: #FFFF33;">To speed things up during training (when the model has access to target sentences), the decoder is fed the whole target, but it is not allowed to use future words (if it had access to the word at position 2 when trying to predict the word at position 2, the problem would not be very hard!). For instance, when trying to predict the fourth word, the attention layer will only have access to the words in positions 1 to 3.</span>

The original Transformer architecture looked like this, with the encoder on the left and the decoder on the right:

 ![attention_layer](/images/attention_layer.png)

<span style="background-color: #FFFF33;">Note that the first attention layer in a decoder block pays attention to all (past) inputs to the decoder, but the second attention layer uses the output of the encoder. It can thus access the whole input sentence to best predict the current word. This is very useful as different languages can have grammatical rules that put the words in different orders, or some context provided later in the sentence may be helpful to determine the best translation of a given word.</span>

<span style="background-color: #FFFF33;">The *attention mask* can also be used in the encoder/decoder to prevent the model from paying attention to some special words — for instance, the special padding word used to make all the inputs the same length when batching together sentences.</span>

### Architectures vs. checkpoints

As we dive into Transformer models in this course, you’ll see mentions of *architectures* and *checkpoints* as well as *models*. These terms all have slightly different meanings:

- **Architecture**: This is the skeleton of the model — the definition of each layer and each operation that happens within the model.
- **Checkpoints**: These are the weights that will be loaded in a given architecture.
- **Model**: This is an umbrella term that isn’t as precise as “architecture” or “checkpoint”: it can mean both. This course will specify *architecture* or *checkpoint* when it matters to reduce ambiguity.

For example, BERT is an architecture while `bert-base-cased`, a set of weights trained by the Google team for the first release of BERT, is a checkpoint. However, one can say “the BERT model” and “the `bert-base-cased` model.”


## How 🤗 Transformers solve tasks

Before diving into specific architectural variants, it’s helpful to understand that most tasks follow a similar pattern: input data is processed through a model, and the output is interpreted for a specific task. The differences lie in how the data is prepared, what model architecture variant is used, and how the output is processed.

### How language models work

Language models work by being trained to predict the probability of a word given the context of surrounding words. This gives them a foundational understanding of language that can generalize to other tasks.

<span style="background-color: #FFFF33;">There are two main approaches for training a transformer model:</span>

- <span style="background-color: #FFFF33;">**Masked language modeling (MLM)**: Used by encoder models like BERT, this approach randomly masks some tokens in the input and trains the model to predict the original tokens based on the surrounding context. This allows the model to learn bidirectional context (looking at words both before and after the masked word).</span>

- <span style="background-color: #FFFF33;">**Causal language modeling (CLM)**: Used by decoder models like GPT, this approach predicts the next token based on all previous tokens in the sequence. The model can only use context from the left (previous tokens) to predict the next token.</span>

## Types of language models

<span style="background-color: #FFFF33;">In the Transformers library, language models generally fall into three architectural categories:</span>

- <span style="background-color: #FFFF33;">**Encoder-only models** (like BERT): These models use a bidirectional approach to understand context from both directions. They’re best suited for tasks that require deep understanding of text, such as classification, named entity recognition, and question answering.</span>

- <span style="background-color: #FFFF33;">**Decoder-only models** (like GPT, Llama): These models process text from left to right and are particularly good at text generation tasks. They can complete sentences, write essays, or even generate code based on a prompt.</span>

- <span style="background-color: #FFFF33;">**Encoder-decoder models** (like T5, BART): These models combine both approaches, using an encoder to understand the input and a decoder to generate output. They excel at sequence-to-sequence tasks like translation, summarization, and question answering.</span>

![transformers_architecture](/images/transformers_architecture.png)

As we covered in the previous section, language models are typically pretrained on large amounts of text data in a self-supervised manner (without human annotations), then fine-tuned on specific tasks. This approach, known as transfer learning, allows these models to adapt to many different NLP tasks with relatively small amounts of task-specific data.

In the following sections, we’ll explore specific model architectures and how they’re applied to various tasks across speech, vision, and text domains.

Understanding which part of the Transformer architecture (encoder, decoder, or both) is best suited for a particular NLP task is key to choosing the right model. Generally, tasks requiring bidirectional context use encoders, tasks generating text use decoders, and tasks converting one sequence to another use encoder-decoders.

### Text generation

Text generation involves creating coherent and contextually relevant text based on a prompt or input.

GPT-2 is a decoder-only model pretrained on a large amount of text. It can generate convincing (though not always true!) text given a prompt and complete other NLP tasks like question answering despite not being explicitly trained to.

![gpt2_architecture](/images/gpt2_architecture.png)

1. GPT-2 uses byte pair encoding (BPE) to tokenize words and generate a token embedding. Positional encodings are added to the token embeddings to indicate the position of each token in the sequence. The input embeddings are passed through multiple decoder blocks to output some final hidden state. Within each decoder block, GPT-2 uses a *masked self-attention* layer which means GPT-2 can’t attend to future tokens. It is only allowed to attend to tokens on the left. This is different from BERT’s `[mask]` token because, in masked self-attention, an attention mask is used to set the score to `0` for future tokens.

2. The output from the decoder is passed to a language modeling head, which performs a linear transformation to convert the hidden states into logits. The label is the next token in the sequence, which are created by shifting the logits to the right by one. The cross-entropy loss is calculated between the shifted logits and the labels to output the next most likely token.

GPT-2’s pretraining objective is based entirely on causal language modeling, predicting the next word in a sequence. This makes GPT-2 especially good at tasks that involve generating text.

### Text classification

Text classification involves assigning predefined categories to text documents, such as sentiment analysis, topic classification, or spam detection.

BERT is an encoder-only model and is the first model to effectively implement deep bidirectionality to learn richer representations of the text by attending to words on both sides.

1. BERT uses WordPiece tokenization to generate a token embedding of the text. To tell the difference between a single sentence and a pair of sentences, a special `[SEP]` token is added to differentiate them. A special `[CLS]` token is added to the beginning of every sequence of text. The final output with the `[CLS]` token is used as the input to the classification head for classification tasks. BERT also adds a segment embedding to denote whether a token belongs to the first or second sentence in a pair of sentences.

2. BERT is pretrained with two objectives: masked language modeling and next-sentence prediction. In masked language modeling, some percentage of the input tokens are randomly masked, and the model needs to predict these. This solves the issue of bidirectionality, where the model could cheat and see all the words and "predict" the next word. The final hidden states of the predicted mask tokens are passed to a feedforward network with a softmax over the vocabulary to predict the masked word.

    The second pretraining object is next-sentence prediction. The model must predict whether sentence B follows sentence A. Half of the time sentence B is the next sentence, and the other half of the time, sentence B is a random sentence. The prediction, whether it is the next sentence or not, is passed to a feedforward network with a softmax over the two classes (`IsNext` and `NotNext`).

3. The input embeddings are passed through multiple encoder layers to output some final hidden states.


To use the pretrained model for text classification, add a sequence classification head on top of the base BERT model. The sequence classification head is a linear layer that accepts the final hidden states and performs a linear transformation to convert them into logits. The cross-entropy loss is calculated between the logits and target to find the most likely label.


### Token classification

Token classification involves assigning a label to each token in a sequence, such as in named entity recognition or part-of-speech tagging.

To use BERT for token classification tasks like named entity recognition (NER), add a token classification head on top of the base BERT model. The token classification head is a linear layer that accepts the final hidden states and performs a linear transformation to convert them into logits. The cross-entropy loss is calculated between the logits and each token to find the most likely label.


### Question answering

Question answering involves finding the answer to a question within a given context or passage.

To use BERT for question answering, add a `span` classification head on top of the base BERT model. This linear layer accepts the final hidden states and performs a linear transformation to compute the `span` start and end logits corresponding to the answer. The cross-entropy loss is calculated between the logits and the label position to find the most likely span of text corresponding to the answer.

### Summarization

Summarization involves condensing a longer text into a shorter version while preserving its key information and meaning.

Encoder-decoder models like BART and T5 are designed for the sequence-to-sequence pattern of a summarization task. We’ll explain how BART works in this section, and then you can try finetuning T5 at the end.

![bart_architecture](/images/bart_architecture.png)

1. BART’s encoder architecture is very similar to BERT and accepts a token and positional embedding of the text. BART is pretrained by corrupting the input and then reconstructing it with the decoder. Unlike other encoders with specific corruption strategies, BART can apply any type of corruption. The text infilling corruption strategy works the best though. In text infilling, a number of text spans are replaced with a **single** `[mask]` token. This is important because the model has to predict the masked tokens, and it teaches the model to predict the number of missing tokens. The input embeddings and masked spans are passed through the encoder to output some final hidden states, but unlike BERT, BART doesn’t add a final feedforward network at the end to predict a word.

2. The encoder’s output is passed to the decoder, which must predict the masked tokens and any uncorrupted tokens from the encoder’s output. This gives additional context to help the decoder restore the original text. The output from the decoder is passed to a language modeling head, which performs a linear transformation to convert the hidden states into logits. The cross-entropy loss is calculated between the logits and the label, which is just the token shifted to the right.


### Translation

Translation involves converting text from one language to another while preserving its meaning. Translation is another example of a sequence-to-sequence task, which means you can use an encoder-decoder model like BART or T5 to do it. We’ll explain how BART works in this section, and then you can try finetuning T5 at the end.

BART adapts to translation by adding a separate randomly initialized encoder to map a source language to an input that can be decoded into the target language. This new encoder’s embeddings are passed to the pretrained encoder instead of the original word embeddings. The source encoder is trained by updating the source encoder, positional embeddings, and input embeddings with the cross-entropy loss from the model output. The model parameters are frozen in this first step, and all the model parameters are trained together in the second step. BART has since been followed up by a multilingual version, mBART, intended for translation and pretrained on many different languages.

## Modalities beyond text

Transformers are not limited to text. They can also be applied to other modalities like speech and audio, images, and video. Of course, on this course we will focus on text, but we can briefly introduce the other modalities.

### Speech and audio

Let’s start by exploring how Transformer models handle speech and audio data, which presents unique challenges compared to text or images.

Whisper is a encoder-decoder (sequence-to-sequence) transformer pretrained on 680,000 hours of labeled audio data. This amount of pretraining data enables zero-shot performance on audio tasks in English and many other languages. The decoder allows Whisper to map the encoders learned speech representations to useful outputs, such as text, without additional fine-tuning. Whisper just works out of the box.

![whisper_architecture](/images/whisper_architecture.png)

Diagram is from Whisper paper.

This model has two main components:

1. An **encoder** processes the input audio. The raw audio is first converted into a log-Mel spectrogram. This spectrogram is then passed through a Transformer encoder network.

2. A **decoder** takes the encoded audio representation and autoregressively predicts the corresponding text tokens. It’s a standard Transformer decoder trained to predict the next text token given the previous tokens and the encoder output. Special tokens are used at the beginning of the decoder input to steer the model towards specific tasks like transcription, translation, or language identification.

Whisper was pretrained on a massive and diverse dataset of 680,000 hours of labeled audio data collected from the web. This large-scale, weakly supervised pretraining is the key to its strong zero-shot performance across many languages and tasks.

Now that Whisper is pretrained, you can use it directly for zero-shot inference or finetune it on your data for improved performance on specific tasks like automatic speech recognition or speech translation!

The key innovation in Whisper is its training on an unprecedented scale of diverse, weakly supervised audio data from the internet. This allows it to generalize remarkably well to different languages, accents, and tasks without task-specific finetuning.


### Automatic speech recognition

To use the pretrained model for automatic speech recognition, you leverage its full encoder-decoder structure. The encoder processes the audio input, and the decoder autoregressively generates the transcript token by token. When fine-tuning, the model is typically trained using a standard sequence-to-sequence loss (like cross-entropy) to predict the correct text tokens based on the audio input.

The easiest way to use a fine-tuned model for inference is within a `pipeline`.

```python
from transformers import pipeline

transcriber = pipeline(
    task="automatic-speech-recognition", model="openai/whisper-base.en"
)
transcriber("https://huggingface.co/datasets/Narsil/asr_dummy/resolve/main/mlk.flac")
# Output: {'text': ' I have a dream that one day this nation will rise up and live out the true meaning of its creed.'}
```

### Computer vision

Now let’s move on to computer vision tasks, which deal with understanding and interpreting visual information from images or videos.

There are two ways to approach computer vision tasks:

1. Split an image into a sequence of patches and process them in parallel with a Transformer.
2. Use a modern CNN, like ConvNeXT, which relies on convolutional layers but adopts modern network designs.

A third approach mixes Transformers with convolutions (for example, Convolutional Vision Transformer or LeViT). We won't discuss those because they just combine the two approaches we examine here.  


ViT and ConvNeXT are commonly used for image classification, but for other vision tasks like object detection, segmentation, and depth estimation, we’ll look at DETR, Mask2Former and GLPN, respectively; these models are better suited for those tasks.

### Image classification

Image classification is one of the fundamental computer vision tasks. Let’s see how different model architectures approach this problem.

ViT and ConvNeXT can both be used for image classification; the main difference is that ViT uses an attention mechanism while ConvNeXT uses convolutions.

ViT replaces convolutions entirely with a pure Transformer architecture. If you’re familiar with the original Transformer, then you’re already most of the way toward understanding ViT.

![vit_architecture](/images/vit_architecture.jpg)


The main change ViT introduced was in how images are fed to a Transformer:

1. An image is split into square non-overlapping patches, each of which gets turned into a vector or *patch embedding*. The patch embeddings are generated from a convolutional 2D layer which creates the proper input dimensions (which for a base Transformer is 768 values for each patch embedding). If you had a 224x224 pixel image, you could split it into 196 16x16 image patches. Just like how text is tokenized into words, an image is “tokenized” into a sequence of patches.

2. A *learnable embedding* - a special `[CLS]` token - is added to the beginning of the patch embeddings just like BERT. The final hidden state of the `[CLS]` token is used as the input to the attached classification head; other outputs are ignored. This token helps the model learn how to encode a representation of the image.

3. The last thing to add to the patch and learnable embeddings are the *position embeddings* because the model doesn’t know how the image patches are ordered. The position embeddings are also learnable and have the same size as the patch embeddings. Finally, all of the embeddings are passed to the Transformer encoder.

4. The output, specifically only the output with the `[CLS]` token, is passed to a multilayer perceptron head (MLP). ViT’s pretraining objective is simply classification. Like other classification heads, the MLP head converts the output into logits over the class labels and calculates the cross-entropy loss to find the most likely class.

Notice the parallel between ViT and BERT: both use a special token (`[CLS]`) to capture the overall representation, both add position information to their embeddings, and both use a Transformer encoder to process the sequence of tokens/patches.

## Transformer Architectures

In the previous sections, we introduced the general Transformer architecture and explored how these models can solve various tasks. Now, let’s take a closer look at the three main architectural variants of Transformer models and understand when to use each one. Then, we looked at how those architectures are applied to different language tasks.

In this section, we’re going to dive deeper into the three main architectural variants of Transformer models and understand when to use each one.

Remember that most Transformer models use one of three architectures: encoder-only, decoder-only, or encoder-decoder (sequence-to-sequence). Understanding these differences will help you choose the right model for your specific task.

### Encoder models

Encoder models use only the encoder of a Transformer model. At each stage, the attention layers can access all the words in the initial sentence. These models are often characterized as having “bi-directional” attention, and are often called *auto-encoding* models.

The pretraining of these models usually revolves around somehow corrupting a given sentence (for instance, by masking random words in it) and tasking the model with finding or reconstructing the initial sentence.

<span style="background-color: #FFFF33;">Encoder models are best suited for tasks requiring an understanding of the full sentence, such as sentence classification, named entity recognition (and more generally word classification), and extractive question answering.</span>

As we saw in How 🤗 Transformers solve tasks, <span style="background-color: #FFFF33;">encoder models like BERT excel at understanding text because they can look at the entire context in both directions. This makes them perfect for tasks where comprehension of the whole input is important.</span>

Representatives of this family of models include:

- BERT
- DistilBERT
- ModernBERT

### Decoder models

<span style="background-color: #FFFF33;">Decoder models use only the decoder of a Transformer model. At each stage, for a given word the attention layers can only access the words positioned before it in the sentence. These models are often called auto-regressive models.</span>

<span style="background-color: #FFFF33;">The pretraining of decoder models usually revolves around predicting the next word in the sentence.</span>

<span style="background-color: #FFFF33;">These models are best suited for tasks involving text generation.</span>

<span style="background-color: #FFFF33;">Decoder models like GPT are designed to generate text by predicting one token at a time.</span> As we explored in How 🤗 Transformers solve tasks, <span style="background-color: #FFFF33;">they can only see previous tokens, which makes them excellent for creative text generation but less ideal for tasks requiring bidirectional understanding.</span>

Representatives of this family of models include:

- Hugging Face SmolLM Series
- Meta’s Llama Series
- Google’s Gemma Series
- DeepSeek’s V3

### Modern Large Language Models (LLMs)

Most modern Large Language Models (LLMs) use the decoder-only architecture. These models have grown dramatically in size and capabilities over the past few years, with some of the largest models containing hundreds of billions of parameters.

<span style="background-color: #FFFF33;">Modern LLMs are typically trained in two phases:</span>

1. <span style="background-color: #FFFF33;">**Pretraining**: The model learns to predict the next token on vast amounts of text data</span>
2. <span style="background-color: #FFFF33;">**Instruction tuning**: The model is fine-tuned to follow instructions and generate helpful responses</span>

This approach has led to models that can understand and generate human-like text across a wide range of topics and tasks.

### Key capabilities of modern LLMs

Modern decoder-based LLMs have demonstrated impressive capabilities:

| Capability         | Description                                 | Example                                   |
|--------------------|---------------------------------------------|-------------------------------------------|
| Text generation    | Creating coherent and contextually relevant text | Writing essays, stories, or emails        |
| Summarization      | Condensing long documents into shorter versions | Creating executive summaries of reports   |
| Translation        | Converting text between languages           | Translating English to Spanish            |
| Question answering | Providing answers to factual questions      | “What is the capital of France?”          |
| Code generation    | Writing or completing code snippets         | Creating a function based on a description|
| Reasoning          | Working through problems step by step       | Solving math problems or logical puzzles  |
| Few-shot learning  | Learning from a few examples in the prompt  | Classifying text after seeing just 2-3 examples |


### Sequence-to-sequence models

<span style="background-color: #FFFF33;">Encoder-decoder models (also called *sequence-to-sequence* models) use both parts of the Transformer architecture. At each stage, the attention layers of the encoder can access all the words in the initial sentence, whereas the attention layers of the decoder can only access the words positioned before a given word in the input.</span>

<span style="background-color: #FFFF33;">The pretraining of these models can take different forms, but it often involves reconstructing a sentence for which the input has been somehow corrupted (for instance by masking random words). The pretraining of the T5 model consists of replacing random spans of text (that can contain several words) with a single mask special token, and the task is then to predict the text that this mask token replaces.</span>

<span style="background-color: #FFFF33;">Sequence-to-sequence models are best suited for tasks revolving around generating new sentences depending on a given input, such as summarization, translation, or generative question answering.</span>

As we saw in How 🤗 Transformers solve tasks, <span style="background-color: #FFFF33;">encoder-decoder models like BART and T5 combine the strengths of both architectures. The encoder provides deep bidirectional understanding of the input, while the decoder generates appropriate output text. This makes them perfect for tasks that transform one sequence into another, like translation or summarization.</span>

### Practical applications

<span style="background-color: #FFFF33;">Sequence-to-sequence models excel at tasks that require transforming one form of text into another while preserving meaning.</span> Some practical applications include:

| Application               | Description                                 | Example Model      |
|---------------------------|---------------------------------------------|--------------------|
| Machine translation       | Converting text between languages           | Marian, T5         |
| Text summarization        | Creating concise summaries of longer texts  | BART, T5           |
| Data-to-text generation   | Converting structured data into natural language | T5             |
| Grammar correction        | Fixing grammatical errors in text           | T5                 |
| Question answering        | Generating answers based on context         | BART, T5           |


Representatives of this family of models include:

- BART
- mBART
- Marian
- T5


### Choosing the right architecture

When working on a specific NLP task, how do you decide which architecture to use? Here’s a quick guide:

| Task                              | Suggested Architecture      | Examples           |
|------------------------------------|----------------------------|--------------------|
| Text classification (sentiment, topic) | Encoder                | BERT, RoBERTa      |
| Text generation (creative writing) | Decoder                    | GPT, LLaMA         |
| Translation                       | Encoder-Decoder            | T5, BART           |
| Summarization                     | Encoder-Decoder            | BART, T5           |
| Named entity recognition          | Encoder                    | BERT, RoBERTa      |
| Question answering (extractive)   | Encoder                    | BERT, RoBERTa      |
| Question answering (generative)   | Encoder-Decoder or Decoder | T5, GPT            |
| Conversational AI                 | Decoder                    | GPT, LLaMA         |



<span style="background-color: #FFFF33;">When in doubt about which model to use, consider:</span>

1. <span style="background-color: #FFFF33;">What kind of understanding does your task need? (Bidirectional or unidirectional)</span>
2. <span style="background-color: #FFFF33;">Are you generating new text or analyzing existing text?</span>
3. <span style="background-color: #FFFF33;">Do you need to transform one sequence into another?</span>

<span style="background-color: #FFFF33;">The answers to these questions will guide you toward the right architecture.</span>


## Attention mechanisms

<span style="background-color: #FFFF33;">Most transformer models use full attention in the sense that the attention matrix is square. It can be a big computational bottleneck when you have long texts. Longformer and reformer are models that try to be more efficient and use a sparse version of the attention matrix to speed up training.</span>

<span style="background-color: #FFFF33;">Standard attention mechanisms have a computational complexity of O(n²), where n is the sequence length. This becomes problematic for very long sequences. The specialized attention mechanisms below help address this limitation.</span>

### LSH attention

Reformer uses LSH attention. In the softmax(QK^t), only the biggest elements (in the softmax dimension) of the matrix QK^t are going to give useful contributions. So for each query q in Q, we can consider only the keys k in K that are close to q. A hash function is used to determine if q and k are close. The attention mask is modified to mask the current token (except at the first position), because it will give a query and a key equal (so very similar to each other). Since the hash can be a bit random, several hash functions are used in practice (determined by a n_rounds parameter) and then are averaged together.

### Local attention

Longformer uses local attention: often, the local context (e.g., what are the two tokens to the left and right?) is enough to take action for a given token. Also, by stacking attention layers that have a small window, the last layer will have a receptive field of more than just the tokens in the window, allowing them to build a representation of the whole sentence.

Some preselected input tokens are also given global attention: for those few tokens, the attention matrix can access all tokens and this process is symmetric: all other tokens have access to those specific tokens (on top of the ones in their local window). This is shown in Figure 2d of the paper, see below for a sample attention mask:

![local_attention_mask](/images/local_attention_mask.png)

Using those attention matrices with less parameters then allows the model to have inputs having a bigger sequence length.

### Axial positional encodings

Reformer uses axial positional encodings: in traditional transformer models, the positional encoding E is a matrix of size l by d, l being the sequence length and d the dimension of the hidden state. If you have very long texts, this matrix can be huge and take way too much space on the GPU. To alleviate that, axial positional encodings consist of factorizing that big matrix E in two smaller matrices E1 and E2, with dimensions l1 × d1 and l2 × d2, such that
l1 × l2 = l and d1 + d2 = d (with the product for the lengths, this ends up being way smaller). The embedding for time step j in E is obtained by concatenating the embeddings for timestep j % l1 in E1 and j // l1 in E2.

## The Role of Attention

The attention mechanism is what gives LLMs their ability to understand context and generate coherent responses. When predicting the next word, not every word in a sentence carries equal weight - for example, in the sentence “The capital of France is …”, the words “France” and “capital” are crucial for determining that “Paris” should come next. This ability to focus on relevant information is what we call attention.

![AttentionSceneFinal](/images/AttentionSceneFinal.gif)

This process of identifying the most relevant words to predict the next token has proven to be incredibly effective. Although the basic principle of training LLMs—predicting the next token—has remained generally consistent since BERT and GPT-2, there have been significant advancements in scaling neural networks and making the attention mechanism work for longer and longer sequences, at lower and lower costs.

In short, the attention mechanism is the key to LLMs being able to generate text that is both coherent and context-aware. It sets modern LLMs apart from previous generations of language models.

### Context Length and Attention Span

Now that we understand attention, let’s explore how much context an LLM can actually handle. This brings us to context length, or the model’s ‘attention span’.

<span style="background-color: #FFFF33;">The context length refers to the maximum number of tokens (words or parts of words) that the LLM can process at once. Think of it as the size of the model’s working memory.</span>

<span style="background-color: #FFFF33;">These capabilities are limited by several practical factors:</span>

 - <span style="background-color: #FFFF33;">The model’s architecture and size</span>
 - <span style="background-color: #FFFF33;">Available computational resources</span>
 - <span style="background-color: #FFFF33;">The complexity of the input and desired output</span>

In an ideal world, we could feed unlimited context to the model, but hardware constraints and computational costs make this impractical. This is why different models are designed with different context lengths to balance capability with efficiency.

The context length is the maximum number of tokens the model can consider at once when generating a response.

### The Art of Prompting

When we pass information to LLMs, we structure our input in a way that guides the generation of the LLM toward the desired output. This is called prompting.

Understanding how LLMs process information helps us craft better prompts. <span style="background-color: #FFFF33;">Since the model’s primary task is to predict the next token by analyzing the importance of each input token, the wording of your input sequence becomes crucial.</span>

<span style="background-color: #FFFF33;">Careful design of the prompt makes it easier to guide the generation of the LLM toward the desired output.</span>

## The Two-Phase Inference Process

Now that we understand the basic components, let’s dive into <span style="background-color: #FFFF33;">how LLMs actually generate text. The process can be broken down into two main phases: prefill and decode. These phases work together like an assembly line, each playing a crucial role in producing coherent text.</span>

### The Prefill Phase

<span style="background-color: #FFFF33;">The prefill phase is like the preparation stage in cooking - it’s where all the initial ingredients are processed and made ready. This phase involves three key steps:</span>

- <span style="background-color: #FFFF33;">**Tokenization**: Converting the input text into tokens (think of these as the basic building blocks the model understands)</span>

- <span style="background-color: #FFFF33;">**Embedding Conversion**: Transforming these tokens into numerical representations that capture their meaning</span>

- <span style="background-color: #FFFF33;">**Initial Processing**: Running these embeddings through the model’s neural networks to create a rich understanding of the context</span>

<span style="background-color: #FFFF33;">This phase is computationally intensive because it needs to process all input tokens at once. Think of it as reading and understanding an entire paragraph before starting to write a response.</span>

### The Decode Phase
<span style="background-color: #FFFF33;">After the prefill phase has processed the input, we move to the decode phase - this is where the actual text generation happens. The model generates one token at a time in what we call an autoregressive process (where each new token depends on all previous tokens).</span>

<span style="background-color: #FFFF33;">The decode phase involves several key steps that happen for each new token:</span>

1. <span style="background-color: #FFFF33;">**Attention Computation**: Looking back at all previous tokens to understand context</span>
2. <span style="background-color: #FFFF33;">**Probability Calculation**: Determining the likelihood of each possible next token</span>
3. <span style="background-color: #FFFF33;">**Token Selection**: Choosing the next token based on these probabilities</span>
4. <span style="background-color: #FFFF33;">**Continuation Check**: Deciding whether to continue or stop generation</span>
<span style="background-color: #FFFF33;">This phase is memory-intensive because the model needs to keep track of all previously generated tokens and their relationships.</span>

## Sampling Strategies
Now that we understand how the model generates text, let’s explore the various ways we can control this generation process. Just like a writer might choose between being more creative or more precise, we can adjust how the model makes its token selections.

### Understanding Token Selection: From Probabilities to Token Choices

When the model needs to choose the next token, it starts with raw probabilities (called logits) for every word in its vocabulary. But how do we turn these probabilities into actual choices? Let’s break down the process:

![Token_Choices](/images/Token_Choices.png)

1. <span style="background-color: #FFFF33;">**Raw Logits**: Think of these as the model’s initial gut feelings about each possible next word</span>
2. <span style="background-color: #FFFF33;">**Temperature Scaling**: Like a creativity dial - higher settings (>1.0) make choices more random and creative, lower settings (<1.0) make them more focused and deterministic</span>
3. <span style="background-color: #FFFF33;">**Top-p (Nucleus) Sampling**: Instead of considering all possible words, we only look at the most likely ones that add up to our chosen probability threshold (e.g., top 90%)</span>
4. <span style="background-color: #FFFF33;">**Top-k Filtering**: An alternative approach where we only consider the k most likely next words</span>


### Managing Repetition: Keeping Output Fresh

<span style="background-color: #FFFF33;">One common challenge with LLMs is their tendency to repeat themselves - much like a speaker who keeps returning to the same points. To address this, we use two types of penalties:</span>

1. <span style="background-color: #FFFF33;">**Presence Penalty**: A fixed penalty applied to any token that has appeared before, regardless of how often. This helps prevent the model from reusing the same words.</span>
2. <span style="background-color: #FFFF33;">**Frequency Penalty**: A scaling penalty that increases based on how often a token has been used. The more a word appears, the less likely it is to be chosen again.</span>

![Managing_Repetition](/images/Managing_Repetition.png)

These penalties are applied early in the token selection process, adjusting the raw probabilities before other sampling strategies are applied. Think of them as gentle nudges encouraging the model to explore new vocabulary.

### Controlling Generation Length: Setting Boundaries

Just as a good story needs proper pacing and length, we need ways to control how much text our LLM generates. This is crucial for practical applications - whether we’re generating a tweet-length response or a full blog post.

<span style="background-color: #FFFF33;">We can control generation length in several ways:</span>

1. <span style="background-color: #FFFF33;">**Token Limits**: Setting minimum and maximum token counts</span>
2. <span style="background-color: #FFFF33;">**Stop Sequences**: Defining specific patterns that signal the end of generation</span>
3. <span style="background-color: #FFFF33;">**End-of-Sequence Detection**: Letting the model naturally conclude its response</span>

For example, if we want to generate a single paragraph, we might set a maximum of 100 tokens and use “\n\n” as a stop sequence. This ensures our output stays focused and appropriately sized for its purpose.

## Beam Search: Looking Ahead for Better Coherence

While the strategies we’ve discussed so far make decisions one token at a time, <span style="background-color: #FFFF33;">beam search takes a more holistic approach. Instead of committing to a single choice at each step, it explores multiple possible paths simultaneously - like a chess player thinking several moves ahead.</span>

![Beam_Search](/images/Beam_Search.png)

<span style="background-color: #FFFF33;">Here’s how it works:</span>

1. <span style="background-color: #FFFF33;">At each step, maintain multiple candidate sequences (typically 5-10)</span>
2. <span style="background-color: #FFFF33;">For each candidate, compute probabilities for the next token</span>
3. <span style="background-color: #FFFF33;">Keep only the most promising combinations of sequences and next tokens</span>
4. <span style="background-color: #FFFF33;">Continue this process until reaching the desired length or stop condition</span>
5. <span style="background-color: #FFFF33;">Select the sequence with the highest overall probability</span>

<span style="background-color: #FFFF33;">This approach often produces more coherent and grammatically correct text, though it requires more computational resources than simpler methods.</span>

## Practical Challenges and Optimization

As we wrap up our exploration of LLM inference, let’s look at the practical challenges you’ll face when deploying these models, and how to measure and optimize their performance.

### Key Performance Metrics

<span style="background-color: #FFFF33;">When working with LLMs, four critical metrics will shape your implementation decisions:</span>

1. <span style="background-color: #FFFF33;">**Time to First Token (TTFT)**: How quickly can you get the first response? This is crucial for user experience and is primarily affected by the prefill phase.</span>
2. <span style="background-color: #FFFF33;">**Time Per Output Token (TPOT)**: How fast can you generate subsequent tokens? This determines the overall generation speed.</span>
3. <span style="background-color: #FFFF33;">**Throughput**: How many requests can you handle simultaneously? This affects scaling and cost efficiency.</span>
4. <span style="background-color: #FFFF33;">**VRAM Usage**: How much GPU memory do you need? This often becomes the primary constraint in real-world applications.</span>

### The Context Length Challenge

One of the most significant challenges in LLM inference is managing context length effectively. Longer contexts provide more information but come with substantial costs:

- **Memory Usage**: Grows quadratically with context length
- **Processing Speed**: Decreases linearly with longer contexts
- **Resource Allocation**: Requires careful balancing of VRAM usage

![ContextWindow](/images/ContextWindow.png)

Recent models like Qwen2.5-1M offer impressive 1M token context windows, but this comes at the cost of significantly slower inference times. The key is finding the right balance for your specific use case.

### The KV Cache Optimization

<span style="background-color: #FFFF33;">To address these challenges, one of the most powerful optimizations is KV (Key-Value) caching. This technique significantly improves inference speed by storing and reusing intermediate calculations. This optimization:</span>

- <span style="background-color: #FFFF33;">**Reduces repeated calculations**</span>
- <span style="background-color: #FFFF33;">**Improves generation speed**</span>
- <span style="background-color: #FFFF33;">**Makes long-context generation practical**</span>

<span style="background-color: #FFFF33;">The trade-off is additional memory usage, but the performance benefits usually far outweigh this cost.


### Bias and limitations

If your intent is to use a pretrained model or a fine-tuned version in production, please be aware that, while these models are powerful tools, they come with limitations. The biggest of these is that, to enable pretraining on large amounts of data, researchers often scrape all the content they can find, taking the best as well as the worst of what is available on the internet.

To give a quick illustration, let’s go back the example of a fill-mask pipeline with the BERT model:

```python
from transformers import pipeline

unmasker = pipeline("fill-mask", model="bert-base-uncased")
result = unmasker("This man works as a [MASK].")
print([r["token_str"] for r in result])

result = unmasker("This woman works as a [MASK].")
print([r["token_str"] for r in result])
```

```
['lawyer', 'carpenter', 'doctor', 'waiter', 'mechanic']
['nurse', 'waitress', 'teacher', 'maid', 'prostitute']
```


When asked to fill in the missing word in these two sentences, the model gives only one gender-free answer (waiter/waitress). The others are work occupations usually associated with one specific gender — and yes, prostitute ended up in the top 5 possibilities the model associates with “woman” and “work.” This happens even though BERT is one of the rare Transformer models not built by scraping data from all over the internet, but rather using apparently neutral data (it’s trained on the English Wikipedia and BookCorpus datasets).

When you use these tools, you therefore need to keep in the back of your mind that the original model you are using could very easily generate sexist, racist, or homophobic content. Fine-tuning the model on your data won’t make this intrinsic bias disappear.

## Key concepts covered

### Natural Language Processing and LLMs

We explored what NLP is and how Large Language Models have transformed the field. You learned that:

- NLP encompasses a wide range of tasks from classification to generation
- LLMs are powerful models trained on massive amounts of text data
- These models can perform multiple tasks within a single architecture
- Despite their capabilities, LLMs have limitations including hallucinations and bias

### Transformer capabilities

You saw how the `pipeline()` function from 🤗 Transformers makes it easy to use pre-trained models for various tasks:

- Text classification, token classification, and question answering
- Text generation and summarization
- Translation and other sequence-to-sequence tasks
- Speech recognition and image classification

### Transformer architecture

We discussed how Transformer models work at a high level, including:

- The importance of the attention mechanism
- How transfer learning enables models to adapt to specific tasks
- The three main architectural variants: encoder-only, decoder-only, and encoder-decoder

### Model architectures and their applications

A key aspect of this chapter was understanding which architecture to use for different tasks:

| Model           | Examples                          | Tasks                                                      |
|-----------------|-----------------------------------|------------------------------------------------------------|
| Encoder-only    | BERT, DistilBERT, ModernBERT      | Sentence classification, named entity recognition, extractive question answering |
| Decoder-only    | GPT, LLaMA, Gemma, SmolLM         | Text generation, conversational AI, creative writing        |
| Encoder-decoder | BART, T5, Marian, mBART           | Summarization, translation, generative question answering   |


### Modern LLM developments
You also learned about recent developments in the field:

- How LLMs have grown in size and capability over time
- The concept of scaling laws and how they guide model development
- Specialized attention mechanisms that help models process longer sequences
- The two-phase training approach of pretraining and instruction tuning

### Practical applications
Throughout the chapter, you’ve seen how these models can be applied to real-world problems:

- Using the Hugging Face Hub to find and use pre-trained models
- Leveraging the Inference API to test models directly in your browser
- Understanding which models are best suited for specific tasks

# Acknowledgements

This overview was created with the help of deep and manual research, drawing inspiration and information from the excellent resource:

[HuggingFace🤗 LLM Course](https://huggingface.co/learn/llm-course/chapter1/3?fw=pt)
