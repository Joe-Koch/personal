# Explaining P-Values to PM’s

When explaining technical concepts to non-technical people, **don’t answer what it is, answer what they need to know about it.**

Sometimes, you need to define technical concepts for non-technical people. Maybe you’re explaining the results of a hypothesis test, and the project manager asks:

> "So, what’s a p-value?"

When I was a new data scientist, I’d rack my brain for the most concise, precise answer, and try to give a definition like:

> "A p-value is the probability of obtaining the observed results, assuming that the null hypothesis is true."

Followed by a mini tutoring session explaining that low values mean more statistical significance, that .05 is the standard threshold, that it should not be confused with the probability of the null hypothesis being true or not, etc. etc.

By the next week, if I brought up p-values again, I’d usually get asked:

> "So, what’s a p-value, again?"

It’s an unrealistic expectation for most PM’s to understand the nuances of something that has a whole Wikipedia subsection on its misuse, and it’s an unrealistic expectation for me to teach it to them in under 5 minutes. But it’s important for them to understand our results, and their limitations. It’d be a mess if they misreported our findings, or overstated the certainty in the result and then it turned out to be wrong. There are things they need to know! So today, instead of answering “what’s a p-value”, I try to answer “what do you need to know about a p-value?”

**The p-value is used to gauge if the hypothesis was true. It's not a 100% guarantee, but our results met the industry standard to say the hypothesis was true.**

If asked,

> "What’s precision and recall?"

Don’t answer:
> "Precision is the ratio of true positive predictions to the total predicted positives, assessing the exactness of the model. Recall is the ratio of true positives to the total actual positives, assessing the model's completeness in identifying positive cases. You’d have perfect precision if there were no false positives, and perfect recall if there were no false negatives."

Do answer:
> "They’re two options for measuring if our model gave the right answer, and one might make more sense to use than the other. In this case, would it be worse if we approved someone for a loan and they defaulted, or if we didn’t approve the loan of someone who would have paid it off?"

I think of this like the basketball player Magic Johnson, who was known for not just throwing the ball to where his teammates were, but throwing them to where they needed to be. This has definitely been helpful for me and, hopefully, the non-technical people who work with me.
