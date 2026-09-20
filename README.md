# Project Memory

[English](README.md) | [中文](README.zh-CN.md)

I have an idea that can save 90% of context.

The core idea is: add a local Project Memory between the cloud LLM and the project files (equivalent to giving the AI expert an assistant).

One sentence to explain the current context problem: the context is too long, so it is hard to find the key points. After context compression, key information disappears, and it has to be read and indexed again.

Solution:
       For example, in a chemistry laboratory, the AI large model is like an expert. Previously, every time the expert entered the laboratory, they had to read the chemical names, sources, manuals, and remaining grams. This is very inefficient, and the expert's memory is limited. Now give the expert an assistant who lives in the laboratory. It can tell the expert at any time the chemical names, sources, usage methods, remaining grams, and so on.

What it solves: ① greatly reduces context usage and improves efficiency. ② improves memory and accuracy. (For example, if an xxx was deleted before, after context compression the model has to recall again whether this file exists.) ③ reduces context storage through this weight activation.

The AI assistant is divided into two modules.
The first layer is Reasoning Memory, responsible for answering questions that need explanation.

For example:

"Where is this file?"
"What is this file used for?"
"What features is this module related to?"
"Why was this class deleted before?"

It can return short natural-language answers, allowing the large model to quickly understand the project without rereading dozens of files.

The second layer is Structured Memory, responsible for very fast judgment and retrieval.

For example:

"Which directory is this file most likely in?"

Return:

"src/payment: 0.82"
"src/services: 0.13"
"src/core: 0.05"

Or:

"Has this file already been deleted?"

Return:

"deleted: true"
"confidence: 0.99"

Or:

"Which files should be read first for the current task?"

Directly return file ID / path + probability.

So the whole flow becomes:

"User task"
→ "Cloud LLM"
→ "First ask the local Project Memory"
→ "Structured Memory quickly locates"
→ "Reasoning Memory adds explanation"
→ "Only read the files that are truly necessary"
→ "Cloud LLM completes the task"

The core value is:

In the past, AI first stuffed a large amount of project content into Context, and then searched for the answer inside it.

Now, it first decides locally what content is worth entering Context, and then only gives the necessary information to the large model.

Therefore, it is essentially a Context Filter / Context Router.

The final goal is not to make the model remember more content, but to let the model see less content while seeing more accurate content.

In one sentence:

"Use local structured prediction to decide what to find, use local reasoning to explain what, and ultimately greatly reduce the Context that the cloud large model actually needs to consume."