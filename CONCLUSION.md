# Lab 05 — Conclusion

<!--
Read this before you write.

This file is graded by your instructor, by hand, and it is worth 6 of the 15 points.

- Type it yourself. Do not use a chatbot, an AI assistant, an AI autocomplete or a "rewrite my
  text" tool for any part of it, not even for grammar. English that is plain and a little
  clumsy but yours is better than polished text that is not.
- Use your own numbers. Every number you quote must be one your own notebook printed. Copy
  the number exactly as it appeared. A number your notebook did not print is marked as wrong.
- Answer in your own words, about 80-200 words per question. A short answer that uses your
  numbers beats a long answer that could have been written by anyone.
- Delete these comment lines if you like. Keep the three headings and the last line.
-->

## 1. Which layer changed your results the most?

Give the numbers before -> after for that layer (correct / wrong / no answer, out of 20) and say
whether it changed what the agent *can do* or only what it *admits*. If no layer changed the
number of correct answers, say so and explain what the layers did change instead.

Level 1 error handling had the most significant impact on the results. Prior to its implementation, the agent provided 16 correct answers and 4 incorrect ones, while failing to provide any response in 4 instances - out of a total of 24 questions in our dataset. Following the addition of Level 1, the metrics shifted to 18 correct answers, 6 incorrect answers, and 0 instances of no response.

This level expanded the agent's functional capabilities. Previously, if an error occurred while using a tool—for example, if a user entered Nur-Sultan instead of Astana - the agent would simply abort the task without providing any response. With the introduction of Level 1,nodel gained the ability to recognize error messages, correct the city name, and retry the operation. As a result,the number of correct answers increased by 2, while the number of incorrect answers also rose by 2. For comparison, Level 3 only affected the instances in which the agent acknowledges its own uncertainty; this shifted the metrics from 18/6/0 to 18/4/2.

## 2. One failure the full harness did not fix

Pick one question that the full harness (layer 3) still gets wrong or refuses. Give its id
(q01 ... q20). Open its trace in the notebook: at which step did it go wrong, and what did
the model do instead? Name one thing you would add to the harness, and what it would cost
(extra model calls, tokens, time, or a new way to fail).

One of the issues that the Level 3 (grounding) system has yet to resolve is q08.In question q08, the user asked to identify the region with the lowest numerical value. The correct answer was Mangystau, but the model selected Zhambyl instead. During the task execution, the tool functioned correctly, providing a list of regions sorted from the highest value to the lowest. The error occurred at the final stage, when the model was formulating its ultimate response: instead of selecting the last item in the list, it chose Zhambyl, which was located somewhere in the middle. The Level 3 validity check failed to detect this error because Zhambyl was indeed present in the data returned by the tool—the reasoning system deemed the answer acceptable.

I believe this can be fixed by adding a simple Python check that automatically identifies the minimum or maximum value within the list provided by the tool; this would eliminate the need for the model to rely on guesswork.

## 3. Workflow or agent for the monthly ranking?

Dala Insight wants to publish, every month, "which region is highest and which is lowest on one
indicator in one year". Would you ship the fixed workflow or the agent? Use the accuracy, the
number of model calls, the tokens and the time you measured in Part 1. Then say in one sentence
what kind of question would make you choose the other one.

I would choose the fixed workflow approach. In the first part, using a set of 15 questions as an example, the fixed workflow performed significantly better than the agent. It provided 13 correct answers out of 15, whereas the agent managed only 9. Furthermore, the fixed workflow required 15 calls to the model and 3,852 tokens, taking 17–18 seconds to complete. In contrast, the agent required 26 calls to the model and 19,835 tokens—more than five times as many - and took 53 seconds, roughly three times as long. Since preparing a monthly report is a consistent, recurring task, a fixed workflow proves to be faster, more cost-effective, and more accurate. An agent would only be the better choice if users began asking new, unexpected questions that our fixed workflow was not designed to handle.

---

I wrote this file myself, without any AI tool.
