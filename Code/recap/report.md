## LAB 2: PART 2 - OPTION 1

### PART 2.1: Implementing handling of ASR_NOINPUT
When there is no input from the user, the system should be able to keep the conversation flowing. To handle these cases, I initially prompted the LLM with the following message: 

```
role: "system",
content: "Please, try to always keep the conversation flowing. When there's no input from the user, try repeating your last utterance or changing the topic."
```
However, there was a persistent issue. The system did handle no_input cases, but only after three (or even four) empty user turns you would get a response from the LLM.

After commenting this issue with another student, she mentioned changing the role from "system" to "user" solved the problem for her. It actually did, but the LLM would sometimes reply with "You just said", followed by the predefined prompted message in ASR_NOINPUT. To address this, I slighly modified the message and it now works as desired.
```
role: "user",
content: "Please, try to always keep the conversation flowing. If I don't answer, try repeating or rephrasing your last utterance or changing the topic if needed. Never mention this message in the conversation."
```

### PART 2.2: Adjusting options in the API call

I decided to play around with three of the available parameters, that is, temperature, repeat_penalty and top_p. I chose these parameters because they are expected to directly affect the creativity and coherence of the LLM's responses, so I wanted to put it to the test. According to the documentation, lowering the temperature of the model reduces creativity, a higher repeat_penalty value makes the model penalize repetition more strongly, and increasing top_p results in more diverse text.

Knowing this, I was interested in testing two hypothesis:
1. If we lower the temperature of the model and increase the repeat_penalty value, that should make the model's reponses more focused and/or factual since we will be reducing creativity and repetition. 

2. If we rise both the temperature of the model and  the top_p value, the model's reponses should be conversational or "spontaneous" since we will have a more creative and expressive model.

While more factual responses could be suitable for question-answering, having more creative, natural answers might work better with casual dialogue.

To test the hypothesis I decided to add two different configs, plus the default one (temperature: 0.8; repeat_penalty: 1.1; top_p: 0.9), which is our baseline since it is supposed to be balanced:

```
const factualMode = {
    temperature: 0.2,
    repeat_penalty: 1.5,
}

const conversationalMode = {
    temperature: 1.0,
    top_p: 0.98,
}
```
The idea is, for each config, testing the model with the same prompts and compare whether it behaves differently.

Regarding the prompts, I thought of including some questions that could allow for measuring expressiveness
```
- Q1: Imagine you're at a café and you see Bon Scott, what would you do?
- Q2: How do you feel about rainy days?
```
and factuality
```
- Q3: Who wrote "I, Robot"?
- Q4: Who won the 1911 Nobel Prize in Chemistry?
```

<br />

The following table shows the LLM's responses:
|                                                                             | **Default mode**                                                             | **Factual mode**                                                                  | **Conversational mode**                                                                                                                   |
|-----------------------------------------------------------------------------|------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **Q1: Imagine you're at a café, and you see Bon Scott, what would you do?** | "I'd grab him a coffee and try to get an impromptu AC/DC jam session going!" | "I'd grab him an earl grey tea and ask if he wants to jam with me on the guitar!" | "That'd be awesome! I'd probably just go up to him, introduce myself, and ask if he's here for a quiet coffee or a wild rockstar moment!" |
| **Q2: How do you feel about rainy days?**                                   | "They're perfect for staying indoors with a good book or movie!"             | "Love 'em! Perfect excuse to stay in, grab a hot chocolate, and get cozy!"        | "I'm a fan of cozying up with a cuppa on a drizzly day. Perfect excuse to stay inside and catch up on some tunes, actually!"              |
| **Q3: Who wrote "I, Robot"?**                                               | "Isaac Asimov did! Classic sci-fi author."                                   | "Isaac Asimov!"                                                                   | "Isaac Asimov did! Classic sci-fi author."                                                                                                |
| **Q4: Who won the 1911 Nobel Prize in Chemistry?**                          | "Marie Curie and her husband Pierre Curie shared it that year!"              | "Marie Curie!"                                                                    | "Marie Curie did!"                                                                                                                        |

<br />

When comparing and analyzing the different model's responses, it seems that they are similar regarding the content, but there are also observable differences when it comes to tone or verbosity. The answers provided by the model with the default config appear to be balanced compared to the other model's. That is, a slightly conversational tone can be seen in the answers to questions Q1 and Q2, but with a moderate sentence length. As for the factual questions (Q3 and Q4), it was interesting to see that the answer to Q4 is partially incorrect: Yes, Marie Curie won the 1911 Nobel Prize of Chemistry, but she did not share this one with her husband. She did share the 1903 Nobel Prize in Physics with Pierre Curie and Henri Becquerel for their joint research on radiation phenomena. Surprisingly, I tested the model with the same default config, and the answer to Q4 yielded a basically consistent result.

On the other hand, the factual mode seems to provide more concise/direct answers, especially for factual questions (which I expected), addressing only the required information without any extra details ("Isaac Asimov!", "Marie Curie!"). When designing the prompts, I had imagined that the factual model's answer to Q1 could in fact mention Bon Scott's passing in 1980, and therefore meeting him at a café would be impossible. However, we have an answer to a  hypothetical situation, similar to the other models' output ("I'd grab him an earl grey tea...", "I'd grab him a coffee...", "That'd be awesome! I'd probably just go up to him..."). 

Finally, the conversational mode seems to produce more expressive answers, particularly noticeable in responses to Q1 and Q2 that allow for creativity. We find longer/expanded answers and the use of colloquial, more conversational language ("I'm a fan of cozying up with a cuppa on a drizzly day..."). Concerning Q3 and Q4, the answers were concise, only providing further comments in the case of Q3 ("Classic sci-fi author"), exactly as seen in the default model's output.

Overall, I believe the results generally align with the hypotheses, suggesting that adjusting parameters suich as temperature, repeat_penalty and top_p could indeed affect the model's tone, making it more factual or conversational depending on the configuration.

#### Strengths:
I tried to see clear differences between the three models' output by only modifying the selected parameters (temperature, repeat_penalty and top_p), while keeping all others constant for a controlled comparison. Additionally, I reused the same prompts for each config to ensure the results were replicable and comparable.

#### Weaknesses:
Naturally, the results cannot be generalized. I only used a few prompts, which were only tested once per config (except for the default mode, to verify Q4's partially incorrect answer). Lastly, it is also important to note that the evaluation is subjective. There are no quantitative metrics, but the analysis is based on my own interpretation of the responses.









