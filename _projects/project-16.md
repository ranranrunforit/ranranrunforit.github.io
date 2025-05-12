---
title: "Interview Preparations - Prompt Engineering"
excerpt: "Quick Study Notes about Prompts. <br/><img src='/images/Prompt.png'>"
collection: projects
date: 2025-05-12
category: pythons
tags:
  - Zero prompting
  - Few shot prompting
  - System prompting
  - Role prompting
  - Contextual prompting
  - Step-back prompting
  - Chain of thought
  - Self consistency
  - Tree of thoughts
  - ReAct
---

# Prompt engineering
Remember how an LLM works; it’s a prediction engine. The model takes sequential text as
an input and then predicts what the following token should be, based on the data it was
trained on. The LLM is operationalized to do this over and over again, adding the previously
predicted token to the end of the sequential text for predicting the following token. The next
token prediction is based on the relationship between what’s in the previous tokens and what
the LLM has seen during its training.

When you write a prompt, you are attempting to set up the LLM to predict the right sequence
of tokens. <span style="background-color: #FFFF33;">Prompt engineering is the process of designing high-quality prompts that guide LLMs to produce accurate outputs. This process involves tinkering to find the best prompt,
optimizing prompt length, and evaluating a prompt’s writing style and structure in relation
to the task. In the context of natural language processing and LLMs, a prompt is an input
provided to the model to generate a response or prediction. </span>

These prompts can be used to achieve various kinds of understanding and generation
tasks such as text summarization, information extraction, question and answering, text
classification, language or code translation, code generation, and code documentation
or reasoning.

Besides the prompt, you will also need to tinker with the various configurations of a LLM.

# LLM output configuration
Once you choose your model you will need to figure out the model configuration. Most LLMs
come with various configuration options that control the LLM’s output. Effective prompt
engineering requires setting these configurations optimally for your task.

## Output length
An important configuration setting is the number of tokens to generate in a response.
<span style="background-color: #FFFF33;">Generating more tokens requires more computation from the LLM, leading to higher energy consumption, potentially slower response times, and higher costs.</span>

Reducing the output length of the LLM doesn’t cause the LLM to become more stylistically
or textually succinct in the output it creates, it just causes the LLM to stop predicting more
tokens once the limit is reached. If your needs require a short output length, you’ll also
possibly need to engineer your prompt to accommodate.

<span style="background-color: #FFFF33;">Output length restriction is especially important for some LLM prompting techniques, like ReAct, where the LLM will keep emitting useless tokens after the response you want.</span>

## Sampling controls
LLMs do not formally predict a single token. <span style="background-color: #FFFF33;">Rather, LLMs predict probabilities for what the
next token could be, with each token in the LLM’s vocabulary getting a probability. Those
token probabilities are then sampled to determine what the next produced token will be.
Temperature, top-K, and top-P are the most common configuration settings that determine
how predicted token probabilities are processed to choose a single output token.</span>

### Temperature
<span style="background-color: #FFFF33;">Temperature controls **the degree of randomness** in token selection. **Lower** temperatures
are good for prompts that expect a more **deterministic** response, while **higher** temperatures
can lead to more **diverse or unexpected** results. A temperature of 0 (*greedy decoding*) is
**deterministic**: the **highest probability** token is always selected (though note that if two tokens
have the same highest predicted probability, depending on how tiebreaking is implemented
you may not always get the same output with temperature 0).</span>

<span style="background-color: #FFFF33;">Temperatures **close to the max** tend to create **more random output**. And **as temperature gets
higher and higher, all tokens become equally likely to be the next predicted token**.</span>

The Gemini temperature control can be understood in a similar way to the softmax function
used in machine learning. <span style="background-color: #FFFF33;">A **low temperature** setting mirrors a **low softmax temperature** (T),
emphasizing **a single, preferred temperature with high certainty**. A **higher** Gemini temperature
setting is like a **high softmax temperature**, making **a wider range of temperatures around
the selected setting more acceptable**. This increased uncertainty accommodates scenarios
where a rigid, precise temperature may not be essential like for example when experimenting
with creative outputs.</span>

### Top-K and top-P
<span style="background-color: #FFFF33;">Top-K and top-P (also known as nucleus sampling) are two sampling settings used in LLMs
to restrict the predicted next token to come from tokens with the top predicted probabilities.
Like temperature, these sampling settings control the randomness and diversity of
generated text.</span>

- <span style="background-color: #FFFF33;">**Top-K** sampling selects the top K **most likely tokens from the model’s predicted
distribution**. The **higher** top-K, the more **creative and varied** the model’s output; the
**lower** top-K, the more **restive and factual** the model’s output. **A top-K of 1 is equivalent to
greedy decoding.**</span>
- <span style="background-color: #FFFF33;">**Top-P** sampling selects the top tokens **whose cumulative probability does not exceed
a certain value (P)**. **Values for P range from 0 (greedy decoding) to 1 (all tokens in the
LLM’s vocabulary)**.</span>

The best way to choose between top-K and top-P is to experiment with both methods (or
both together) and see which one produces the results you are looking for.

Another important configuration setting is the number of tokens to generate in a response.
Be aware, generating more tokens requires more computation from the LLM, leading
to higher energy consumption and potentially slower response times, which leads to
higher costs.

### Putting it all together

Choosing between top-K, top-P, temperature, and the number of tokens to generate,
depends on the specific application and desired outcome, and the settings all impact one
another. It’s also important to make sure you understand how your chosen model combines
the different sampling settings together.

<span style="background-color: #FFFF33;">**If temperature, top-K, and top-P are all available (as in Vertex Studio), tokens that meet
both the top-K and top-P criteria are candidates for the next predicted token, and then
temperature is applied to sample from the tokens that passed the top-K and top-P criteria.** If
only top-K or top-P is available, the behavior is the same but only the one top-K or P setting
is used.</span>

<span style="background-color: #FFFF33;">**If temperature is not available, whatever tokens meet the top-K and/or top-P criteria are then
randomly selected from to produce a single next predicted token.**</span>

<span style="background-color: #FFFF33;">**At extreme settings of one sampling configuration value, that one sampling setting either
cancels out other configuration settings or becomes irrelevant.**</span>

- <span style="background-color: #FFFF33;">If you **set temperature to 0, top-K and top-P become irrelevant–the most probable
token becomes the next token predicted**. If you **set temperature extremely high (above
1–generally into the 10s), temperature becomes irrelevant and whatever tokens make
it through the top-K and/or top-P criteria are then randomly sampled to choose a next
predicted token.**</span>

- <span style="background-color: #FFFF33;">If you **set top-K to 1, temperature and top-P become irrelevant. Only one token passes the
top-K criteria, and that token is the next predicted token.** If you **set top-K extremely high,
like to the size of the LLM’s vocabulary, any token with a nonzero probability of being the
next token will meet the top-K criteria and none are selected out**.</span>

- <span style="background-color: #FFFF33;">If you **set top-P to 0 (or a very small value), most LLM sampling implementations will then
only consider the most probable token to meet the top-P criteria, making temperature and
top-K irrelevant.** If you **set top-P to 1, any token with a nonzero probability of being the
next token will meet the top-P criteria, and none are selected out.**</span>

<span style="background-color: #FFFF33;">As a general starting point, a **temperature of .2, top-P of .95, and top-K of 30** will give you
relatively coherent results that **can be creative but not excessively so**. If you want **especially
creative results**, try starting with a **temperature of .9, top-P of .99, and top-K of 40**. And if you
want **less creative results**, try starting with a **temperature of .1, top-P of .9, and top-K of 20**.
Finally, if your task **always has a single correct answer (e.g., answering a math problem)**, start
with a **temperature of 0**.</span>

<span style="background-color: #FFFF33;">**NOTE:** With **more freedom (higher temperature, top-K, top-P, and output tokens)**, the LLM
might **generate text that is less relevant**.</span>

# Prompting techniques

LLMs are tuned to follow instructions and are trained on large amounts of data so they can
understand a prompt and generate an answer. But LLMs aren’t perfect; <span style="background-color: #FFFF33;">the clearer your
prompt text, the better it is for the LLM to predict the next likely text.</span> Additionally, specific
techniques that take advantage of how LLMs are trained and how LLMs work will help you get
the relevant results from LLMs

## General prompting / zero shot

 <span style="background-color: #FFFF33;">A *zero-shot* prompt is the simplest type of prompt. It only provides a description of a task
and some text for the LLM to get started with. This input could be anything: a question, a start of a story, or instructions. The name zero-shot stands for 'no examples'.</span>

The table format as used below is a great way of documenting prompts. Your prompts will likely go through many iterations before they end up in a codebase, so it’s important to keep track of your prompt engineering work in a disciplined, structured way. More on this table format, the importance of tracking prompt engineering work, and the prompt development
process is in the Best Practices section later in this chapter ("Document the various prompt attempts").

The model temperature should be set to a low number, since no creativity is needed, and we use the gemini-pro default top-K and top-P values, which effectively disable both settings (see 'LLM Output Configuration' above). Pay attention to the generated output. The words disturbing and masterpiece should make the prediction a little more complicated, as both
words are used in the same sentence.

When zero-shot doesn’t work, you can provide demonstrations or examples in the prompt, which leads to "one-shot" and "few-shot" prompting.


## One-shot & few-shot

When creating prompts for AI models, it is helpful to provide examples. These examples can help the model understand what you are asking for. Examples are especially useful when you want to steer the model to a cefiain output structure or pattern.

 <span style="background-color: #FFFF33;">A **one-shot** prompt, provides *a single* example, hence the name one-shot. The idea is the model has an example it can imitate to best complete the task.</span>

 <span style="background-color: #FFFF33;">A **few-shot** prompt provides *multiple* examples to the model. **This approach shows the model a pattern that it needs to follow. The idea is similar to one-shot, but multiple examples of the desired pattern increases the chance the model follows the pattern**.</span>
 
The number of examples you need for few-shot prompting depends on a few factors, including the complexity of the task, the quality of the examples, and the capabilities of the generative AI (gen AI) model you are using. <span style="background-color: #FFFF33;">As a general rule of thumb, you should use at least three to five examples for few-shot prompting. However, you may need to use more examples for more complex tasks, or you may need to use fewer due to the input length limitation of your model.</span>

When you choose examples for your prompt, use examples that are relevant to the task you want to perform. The examples should be diverse, of high quality, and well written. One small mistake can confuse the model and will result in undesired output.

If you are trying to generate output that is robust to a variety of inputs, then it is important to include edge cases in your examples. Edge cases are inputs that are unusual or unexpected, but that the model should still be able to handle.
 
## System, contextual and role prompting

System, contextual and role prompting are all techniques used to guide how LLMs generate text, but they focus on different aspects:

-	<span style="background-color: #FFFF33;">**System prompting** sets the **overall context and purpose** for the language model. It defines the ‘big picture’ of what the model should be doing, like translating a language, classifying a review etc.</span>
-	<span style="background-color: #FFFF33;">**Contextual prompting** provides **specific details or background information** relevant to the current conversation or task. It helps the model to understand the nuances of what’s being asked and tailor the response accordingly.</span>
-	<span style="background-color: #FFFF33;">**Role prompting** assigns **a specific character or identity** for the language model to adopt. This helps the model generate responses that are consistent with the assigned role and its associated knowledge and behavior.</span>

There can be considerable overlap between system, contextual, and role prompting. E.g. a prompt that assigns a role to the system, can also have a context.

However, <span style="background-color: #FFFF33;">each type of prompt serves a slightly different primary purpose</span>:

-	<span style="background-color: #FFFF33;">System prompt: **Defines the model’s fundamental capabilities and overarching purpose**.</span>

-	<span style="background-color: #FFFF33;">Contextual prompt: **Provides immediate, task-specific information to guide the response. It’s highly specific to the current task or input, which is dynamic**.</span>

-	<span style="background-color: #FFFF33;">Role prompt: **Frames the model’s output style and voice. It adds a layer of specificity and personality**.</span>
 
Distinguishing between system, contextual, and role prompts provides a framework for designing prompts with clear intent, allowing for flexible combinations and making it easier to analyze how each prompt type influences the language model’s output.

Let’s dive into these three different kinds of prompts.

### System prompting

<span style="background-color: #FFFF33;">System prompts can be useful for generating output that meets specific requirements. The
name 'system prompt' actually stands for 'providing an additional task to the system'.</span> For
example, you could use a system prompt to generate a code snippet that is compatible with
a specific programming language, or you could use a system prompt to return a certain
structure. 
 
There are some benefits in returning JSON objects from a prompt that extracts data. In
a real-world application I don’t need to manually create this JSON format, I can already
return the data in a sorted order (very handy when working with datetime objects), <span style="background-color: #FFFF33;">but most
importantly, by prompting for a JSON format it forces the model to create a structure and
limit hallucinations.</span>

<span style="background-color: #FFFF33;">System prompts can also be really useful for safety and toxicity. To control the output, simply add an additional line to your prompt like: 'You should be respectful in your answer.'.</span>


### Role prompting

<span style="background-color: #FFFF33;">Role prompting is a technique in prompt engineering that involves assigning a specific role to the gen AI model. This can help the model to generate more relevant and informative output, as the model can craft its responses to the specific role that it has been assigned.</span>

For example, you could role prompt a gen AI model to be a book editor, a kindergafien teacher, or a motivational speaker. Once the model has been assigned a role, you can then give it prompts that are specific to that role. For example, you could prompt a teacher to create a lesson plan that they can then review. Have a look into Table 5, where the model takes the role of a travel guide.

<span style="background-color: #FFFF33;">Defining a role perspective for an AI model gives it a blueprint of the tone, style, and focused expefiise you’re looking for to improve the quality, relevance, and effectiveness of your output.</span>

Here are some styles you can choose from which I find effective:

<span style="background-color: #FFFF33;">**Confrontational, Descriptive, Direct, Formal, Humorous, Influential, Informal, Inspirational, Persuasive**</span>
 
### Contextual prompting

<span style="background-color: #FFFF33;">By providing contextual prompts, you can help ensure that your AI interactions are as seamless and efficient as possible. The model will be able to more quickly understand your request and be able to generate more accurate and relevant responses.</span>

## Step-back prompting

<span style="background-color: #FFFF33;">Step-back prompting is a technique for improving the performance by prompting the LLM to first consider a general question related to the specific task at hand, and then feeding the answer to that general question into a subsequent prompt for the specific task. This 'step back' allows the LLM to activate relevant background knowledge and reasoning processes before attempting to solve the specific problem.</span>

By considering the broader and underlying principles, LLMs can generate more accurate and insightful responses. Step-back prompting encourages LLMs to think critically and apply their knowledge in new and creative ways. It changes the final prompt doing the task by utilizing more knowledge in the LLM’s parameters than would otherwise come into play when the LLM is prompted directly.

It can help to mitigate biases in LLM responses, by focusing on general principles instead of specific details, step-back prompting.

By using step back prompting techniques you can increase the accuracy of your prompts.
 
## Chain of Thought (CoT)

<span style="background-color: #FFFF33;">Chain of Thought (CoT) prompting is a technique for improving the reasoning capabilities of LLMs by generating **intermediate reasoning steps**. This helps the LLM generate more accurate answers. You can combine it with few-shot prompting to get better results on more complex tasks that require reasoning before responding as it’s a challenge with a zero-shot chain of thought.</span>

CoT has a lot of advantages. First of all, it’s low-effort while being very effective and works
well with off-the-shelf LLMs (so no need to finetune). You also get interpretability with CoT
prompting, as you can learn from the LLM’s responses and see the reasoning steps that were
followed. If there’s a malfunction, you will be able to identify it. Chain of thought appears
to improve robustness when moving between different LLM versions. Which means the
performance of your prompt should drift less between different LLMs than if your prompt
does not use reasoning chains. Of course there are also disadvantages, but they are
somewhat intuitive.

The LLM response includes the chain of thought reasoning, which means more output
tokens, which means predictions cost more money and take longer.

Chain of thought prompting can be very powerful when combined with a single-shot or few-shot.

Chain of thought can be useful for various use-cases. Think of code generation, for breaking down the request into a few steps, and mapping those to specific lines of code. Or for creating synthetic data when you have some kind of seed like "The product is called XYZ, write a description guiding the model through the assumptions you would make based on the product given title." span style="background-color: #FFFF33;">Generally, any task that can be solved by 'talking through' is a good candidate for a chain of thought. If you can explain the steps to solve the problem, try chain of thought.</span>

In the best practices section of this chapter, we will learn some best practices specific to Chain of thought prompting.

## Self-consistency

While large language models have shown impressive success in various NLP tasks, their ability to reason is often seen as a limitation that cannot be overcome solely by increasing model size. As we learned in the previous Chain of Thought prompting section, the model can be prompted to generate reasoning steps like a human solving a problem. However CoT uses a simple 'greedy decoding' strategy, limiting its effectiveness. <span style="background-color: #FFFF33;">Self-consistency combines sampling and majority voting to generate diverse reasoning paths and select the most consistent answer. It improves the accuracy and coherence of responses generated by LLMs.</span>

Self-consistency gives a pseudo-probability likelihood of an answer being correct, but obviously has high costs.

It follows the following steps:

1.	Generating diverse reasoning paths: The LLM is provided with the same prompt multiple times. A high temperature setting encourages the model to generate different reasoning paths and perspectives on the problem.
2.	Extract the answer from each generated response.
3.	Choose the most common answer.

Let's look into an example of an email classification system, which classifies an email as IMPORTANT or NOT IMPORTANT. A zero-shot chain of thought prompt will be sent to the LLM multiple times, to see if the responses differ after each submit. Notice the friendly tone, the word choice and the sarcasm that’s been used in the email. All this could trick the LLM.
 
You can use the above prompt and try to see if it returns a consistent classification. Depending on the model that you are using and the temperature configuration, it might return "IMPORTANT" or "NOT IMPORTANT".
 
By generating many Chains of Thoughts, and taking the most commonly occurring answer ("IMPORTANT"), we can get a more consistently correct answer from the LLM.

This example shows how self-consistency prompting can be used to improve the accuracy of an LLM's response by considering multiple perspectives and selecting the most consistent answer.

## Tree of Thoughts (ToT)

Now that we are familiar with chain of thought and self-consistency prompting, let’s review Tree of Thoughts (ToT). <span style="background-color: #FFFF33;">It generalizes the concept of CoT prompting because it allows LLMs to explore multiple different reasoning paths simultaneously, rather than just following a single linear chain of thought.</span>

![ToT](/images/ToT.png)

<span style="background-color: #FFFF33;">This approach makes ToT pafiicularly well-suited for complex tasks that require exploration. It works by maintaining a tree of thoughts, where each thought represents a coherent language sequence that serves as an intermediate step toward solving a problem. The model can then explore different reasoning paths by branching out from different nodes in the tree.</span>

## ReAct (reason & act)

<span style="background-color: #FFFF33;">Reason and act (ReAct) prompting is a paradigm for enabling LLMs to solve complex tasks using natural language reasoning combined with external tools (search, code interpreter etc.) allowing the LLM to perform cefiain actions, such as interacting with external APIs to retrieve information which is a first step towards agent modeling.</span>

ReAct mimics how humans operate in the real world, as we reason verbally and can take actions to gain information. ReAct performs well against other prompt engineering approaches in a variety of domains.

<span style="background-color: #FFFF33;">ReAct prompting works by combining reasoning and acting into a thought-action loop. The LLM first reasons about the problem and generates a plan of action. It then performs the actions in the plan and observes the results. The LLM then uses the observations to update its reasoning and generate a new plan of action. This process continues until the LLM reaches a solution to the problem.</span>
 
<span style="background-color: #FFFF33;">ReAct prompting in practice requires understanding that you continually have to resend the previous prompts/responses (and do trimming of the extra generated content) as well as set up the model with appropriate examples/instructions.</span> 

## Automatic Prompt Engineering

At this point you might realize that writing a prompt can be complex. Wouldn’t it be nice to automate this (write a prompt to write prompts)? Well, there’s a method: Automatic Prompt Engineering (APE). This method15 not only alleviates the need for human input but also enhances the model’s performance in various tasks.

You will prompt a model to generate more prompts. Evaluate them, possibly alter the good ones. And repeat.

For example, you can use automatic prompt engineering that will help with training a chatbot for a merchandise t-shifi webshop. We want to figure out all the various ways customers could phrase their order for buying a band merchandise t-shirt.

1.	Write the prompt which will generate the output variants. In this example, I am using gemini-pro to generate 10 instructions. 
2.	Evaluate all of the instruction candidates by scoring the candidates based on a chosen metric, For example, you can use. BLEU (Bilingual Evaluation Understudy) or ROUGE (Recall-Oriented Understudy for Gisting Evaluation).
3.	Select the instruction candidate with the highest evaluation score. This candidate will be the final prompt you can use in your software application or chatbot. You can also tweak the select prompt and evaluate again.
 
## Code prompting

Gemini focuses primarily on text-based prompts, which also includes writing prompts for returning code.

### Prompts for writing code

Gemini can also be a developer and help you with writing code in any programming language of choice. As a developer this can help you speed up the process of writing code.

Imagine a folder on your machine with hundreds of files that needs to be renamed. Renaming each file would take you a lot of time. You know a little Bash, and could write a script to automate this, but that may also take awhile. So let’s write a prompt. You can write the prompt in the public consumer chatbot Gemini, or if you are more concerned
about confidentiality, you can write these prompts within your Google Cloud account and open Vefiex AI Studio. The advantage of Vefiex AI Studio is that you can configure the temperature etc.
 
That seems like good code to me - it’s even documented! However, since LLMs can’t reason, and repeat training data, it’s essential to read and test your code first.

```
-------
Prompt
-------
Write a code snippet in Bash, which asks for a folder name.
Then it takes the contents of the folder and renames all the
files inside by prepending the name draft to the file name.

```

The moment we are all waiting for, does it really work?

It worked!

### Prompts for explaining code

As a developer when you work in teams you have to read someone else’s code. Gemini can help you with this as well. Let’s take the code output from Table 16, remove the comments and ask the large language model to explain what’s happening

```
-------
Prompt
-------
Explain to me the below Bash code:

Bash code part

```

### Prompts for translating code

The bash code from Table 16 seems to work fine. However this script could really be reusable when it would prompt me for file names, ideally it should work as a separate application with an UI. As a stafiing point, Python would be a better language for a (web) application than Bash. LLMs can help with translating code from one language to another. 

```
-------
Prompt
-------
Translate the below Bash code to a Python snippet.

Bash code part

```

NOTE: When prompting for (Python) code in the Language Studio in Vertex AI, you will have
to click on the 'Markdown' button. Otherwise you will receive plain text which is missing the
proper indenting of lines, which is important for running Python code.

### Prompts for debugging and reviewing code

```
-------
Prompt
-------
The below Python code gives an error:

Traceback (most recent call last):
File "/
Users/leeboonstra/Documents/test_folder/rename_files.py", line
7, in <module>
text = toUpperCase(prefix)
NameError: name 'toUpperCase' is not defined

Debug what's wrong and explain how I can improve the code.

python code part

```

That’s great. Not only did it tell me how to solve the problem, it also figured out that my code had more bugs and how to solve them, too. The last part of the prompt gave suggestions to improve the code in general.
 
### What about multimodal prompting?

Prompting for code still uses the same regular large language model. Multimodal prompting is a separate concern, it refers to a technique where you use multiple input formats to guide a large language model, instead of just relying on text. This can include combinations of text, images, audio, code, or even other formats, depending on the model’s capabilities and the task at hand.

# Best Practices

Finding the right prompt requires tinkering. Language Studio in Vefiex AI is a perfect place to play around with your prompts, with the ability to test against the various models.

Use the following best practices to become a pro in prompt engineering.

## Provide examples

<span style="background-color: #FFFF33;">The most important best practice is to provide (one shot / few shot) examples within a prompt. This is highly effective because it acts as a powerful teaching tool.</span> These examples showcase desired outputs or similar responses, allowing the model to learn from them and tailor its own generation accordingly. It’s like giving the model a reference point or target to aim for, improving the accuracy, style, and tone of its response to better match your expectations.
 
## Design with simplicity

Prompts should be concise, clear, and easy to understand for both you and the model. As a rule of thumb, if it’s already confusing for you it will likely be also confusing for the model. Try not to use complex language and don’t provide unnecessary information.

Examples:

BEFORE:

`I am visiting New York right now, and I'd like to hear more about great locations. I am with two 3 year old kids. Where should we go during our vacation?`

AFTER REWRITE:

`Act as a travel guide for tourists. Describe great places to visit in New York Manhattan with a 3 year old.`

Try using verbs that describe the action. Here’s a set of examples:

<span style="background-color: #FFFF33;">**Act, Analyze, Categorize, Classify, Contrast, Compare, Create, Describe, Define, Evaluate, Extract, Find, Generate, Identify, List, Measure, Organize, Parse, Pick, Predict, Provide, Rank, Recommend, Return, Retrieve, Rewrite, Select, Show, Sofi, Summarize, Translate, Write.**</span>
 
## Be specific about the output

<span style="background-color: #FFFF33;">Be specific about the desired output. A concise instruction might not guide the LLM enough or could be too generic. Providing specific details in the prompt (through system or context prompting) can help the model to focus on what’s relevant, improving the overall accuracy.</span>

Examples:

DO:

`Generate a 3 paragraph blog post about the top 5 video game consoles. The blog post should be informative and engaging, and it should be written in a conversational style.`

DO NOT:

`Generate a blog post about video game consoles.`

## Use Instructions over Constraints

Instructions and constraints are used in prompting to guide the output of a LLM.

•	An **instruction** provides explicit instructions on the desired format, style, or content of the response. It guides the model on what the model should do or produce.
•	A **constraint** is a set of limitations or boundaries on the response. It limits what the model should not do or avoid.
 
Growing research suggests that focusing on positive instructions in prompting can be more effective than relying heavily on constraints. This approach aligns with how humans prefer positive instructions over lists of what not to do.

Instructions directly communicate the desired outcome, whereas constraints might leave the model guessing about what is allowed. It gives flexibility and encourages creativity within the defined boundaries, while constraints can limit the model’s potential. Also a list of constraints can clash with each other.

Constraints are still valuable but in cefiain situations. To prevent the model from generating harmful or biased content or when a strict output format or style is needed.

If possible, use positive instructions: instead of telling the model what not to do, tell it what to do instead. This can avoid confusion and improve the accuracy of the output.

DO:

`Generate a 1 paragraph blog post about the top 5 video game consoles. Only discuss the console, the company who made it, the year, and total sales.`

DO NOT:

`Generate a 1 paragraph blog post about the top 5 video game consoles. Do not list video game names.`

As a best practice, start by prioritizing instructions, clearly stating what you want the model
to do and only use constraints when necessary for safety, clarity or specific requirements.
Experiment and iterate to test different combinations of instructions and constraints to find
what works best for your specific tasks, and document these.

## Control the max token length

To control the length of a generated LLM response, you can either set a max token limit in the configuration or explicitly request a specific length in your prompt. For example:

"Explain quantum physics in a tweet length message."

## Use variables in prompts

To reuse prompts and make it more dynamic use variables in the prompt, which can be changed for different inputs. E.g. a prompt which gives facts about a city. Instead of hardcoding the city name in the prompt, use a variable. Variables can save you time and effort by allowing you to avoid repeating yourself. If you need to use the same piece of information in multiple prompts, you can store it in a variable and then reference that variable in each prompt. This makes a lot of sense when integrating prompts into your own applications.

```
-------
Prompt
-------
VARIABLES
{city} = "Amsterdam"
PROMPT
You are a travel guide. Tell me a fact about the city: {city}
-------
Output
-------
Amsterdam is a beautiful city full of canals, bridges, and narrow streets. It’s a great place to visit for its rich history, culture, and nightlife.
```
Using variables in prompt
 
## Experiment with input formats and writing styles

Different models, model configurations, prompt formats, word choices, and submits can yield different results. Therefore, it’s impofiant to experiment with prompt attributes like the style, the word choice, and the type prompt (zero shot, few shot, system prompt).

For example a prompt with the goal to generate text about the revolutionary video game console Sega Dreamcast, can be formulated as a **question**, a **statement** or an **instruction**, resulting in different outputs:
-	**Question**: What was the Sega Dreamcast and why was it such a revolutionary console?
-	**Statement**: The Sega Dreamcast was a sixth-generation video game console released by Sega in 1999. It...
-	**Instruction**: Write a single paragraph that describes the Sega Dreamcast console and explains why it was so revolutionary.

## For few-shot prompting with classification tasks, mix up the classes

Generally speaking, the order of your few-shots examples should not matter much. However, when doing classification tasks, make sure you mix up the possible response classes in the few shot examples. This is because you might otherwise be overfitting to the specific order of the examples. By mixing up the possible response classes, you can ensure that the model is learning to identify the key features of each class, rather than simply memorizing the order of the examples. This will lead to more robust and generalizable performance on unseen data.

A good rule of thumb is to start with 6 few shot examples and stafi testing the accuracy from there.

## Adapt to model updates

It’s important for you to stay on top of model architecture changes, added data, and capabilities. Try out newer model versions and adjust your prompts to better leverage new model features. Tools like Vefiex AI Studio are great to store, test, and document the various versions of your prompt.

## Experiment with output formats

Besides the prompt input format, consider experimenting with the output format. For non- creative tasks like extracting, selecting, parsing, ordering, ranking, or categorizing data try having your output returned in a structured format like JSON or XML.

There are some benefits in returning JSON objects from a prompt that extracts data. In a real-world application I don’t need to manually create this JSON format, I can already
return the data in a sofied order (very handy when working with datetime objects), but most impofiantly, by prompting for a JSON format it forces the model to create a structure and limit hallucinations.

## Experiment together with other prompt engineers

If you are in a situation where you have to try to come up with a good prompt, you might want to find multiple people to make an attempt. When everyone follows the best practices (as listed in this chapter) you are going to see a variance in performance between all the different prompt attempts.

## CoT Best practices

For CoT prompting, putting the answer after the reasoning is required because the generation of the reasoning changes the tokens that the model gets when it predicts the final answer.

With CoT and self-consistency you need to be able to extract the final answer from your prompt, separated from the reasoning.

For CoT prompting, set the temperature to 0.

Chain of thought prompting is based on greedy decoding, predicting the next word in a sequence based on the highest probability assigned by the language model. Generally speaking, when using reasoning, to come up with the final answer, there’s likely one single correct answer. Therefore the temperature should always set to 0.
 
## Document the various prompt attempts

The last tip was mentioned before in this chapter, but we can’t stress enough how important
it is: document your prompt attempts in full detail so you can learn over time what went well
and what did not.

Prompt outputs can differ across models, across sampling settings, and even across different
versions of the same model. Moreover, even across identical prompts to the same model,
small differences in output sentence formatting and word choice can occur. (For example, as
mentioned previously, if two tokens have the same predicted probability, ties may be broken
randomly. This can then impact subsequent predicted tokens.).

We recommend creating a Google Sheet with Table 21 as a template. The advantages of
this approach are that you have a complete record when you inevitably have to revisit your
prompting work–either to pick it up in the future (you’d be surprised how much you can
forget after just a short break), to test prompt performance on different versions of a model,
and to help debug future errors.

Beyond the fields in this table, it’s also helpful to track the version of the prompt (iteration),
a field to capture if the result was OK/NOT OK/SOMETIMES OK, and a field to capture
feedback. If you’re lucky enough to be using Vertex AI Studio, save your prompts (using the
same name and version as listed in your documentation) and track the hyperlink to the saved
prompt in the table. This way, you’re always one click away from re-running your prompts.

When working on a *retrieval augmented generation* system, you should also capture the
specific aspects of the RAG system that impact what content was inserted into the prompt,
including the query, chunk settings, chunk output, and other information.

Once you feel the prompt is close to perfect, take it to your project codebase. And in the
codebase, save prompts in a separate file from code, so it’s easier to maintain. Finally, ideally
your prompts are part of an operationalized system, and as a prompt engineer you should
rely on automated tests and evaluation procedures to understand how well your prompt
generalizes to a task.

Prompt engineering is an iterative process. Craft and test different prompts, analyze,
and document the results. Refine your prompt based on the model’s performance. Keep
experimenting until you achieve the desired output. When you change a model or model
configuration, go back and keep experimenting with the previously used prompts.

# Summary

This whitepaper discusses prompt engineering. We learned various prompting techniques, such as:

-	Zero prompting
-	Few shot prompting
-	System prompting
-	Role prompting
-	Contextual prompting
-	Step-back prompting
-	Chain of thought
-	Self consistency
-	Tree of thoughts
-	ReAct

We even looked into ways how you can automate your prompts.

The whitepaper then discusses the challenges of gen AI like the problems that can happen when your prompts are insufficient. We closed with best practices on how to become a better prompt engineer.
 
# Acknowledgements
This study note was compiled with the help of deep and manual research, drawing inspiration and information from the excellent resource:

The prompt engineering whitepaper (Google 2025) : [Prompt Engineering - Lee Boonstra](https://www.gptaiflow.tech/assets/files/2025-01-18-pdf-1-TechAI-Goolge-whitepaper_Prompt%20Engineering_v4-af36dcc7a49bb7269a58b1c9b89a8ae1.pdf)
