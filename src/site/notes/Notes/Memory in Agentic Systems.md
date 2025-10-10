---
{"dg-publish":true,"permalink":"/notes/memory-in-agentic-systems/"}
---

### The Introduction
Who are you? What are you? Your memory is arguably the biggest external thing (you can forget facts and stay yourself) that makes you you. 

Similarly, memory is arguably the most important concept in [[Notes/Agentic Systems\|Agentic Systems]], since in a very real sense, an agent's ability to remember is the biggest thing that makes it an "agent".

Without some way of maintaining state across session, or if not that throughout a session, LLMs would be nothing more than gimmicks. One-shot functions with some use, but not truly groundbreaking for the cost incurred.

Every LLM system comes integrated with a base form of memory in its context window, which would have as best human analog the working memory of a human being, or the RAM of a computer.

It is the information that is made readily available. But as can often be seen, even from flagship models who boast ridiculous context windows, this is not enough. When you start pushing that context window even slightly, you often start seeing the model forget key details, lose track of the current context and reply to things that are no longer relevant, or flat out spitting gibberish.

(This is from Gemini 2.5 Pro):
"
Wow Factor: Watching AI agents recurring themes and summarizes them as key insights. For example: "There is a strong negative sentiment around the app's battery construct arguments, rebut each other's points, and maintain a consistent persona is a powerful demonstration of advanced reasoning. It feels consumption. Multiple users have requested an integration with Calendar.
"

I didn't ask it to do that, and the initial conversation was about trying to brainstorm ideas for a workshop. So, if context window is not enough on its own, how can we get the agentic system to remember and use what it remembers effectively?

Let's go in this article over the types of memory, and how they can be replicated in agentic systems. The goal is to distill the knowledge available into an actionable--and hopefully narratively interesting--survey of (some of) the literature.

Sources will be sited at the bottom for future reference.

### Life imitates Art
