# humanizer-tests

Testing how different models rewrite the same text with the [wilson-humanizer](https://github.com/WilsonXY/kook/tree/main/writing/wilson-humanizer) skill, and how the results score on [ZeroGPT](https://www.zerogpt.com/).

## Test 1: AI-Assisted Writing guidelines (2026-10-19)

### Results

| Model | ZeroGPT "AI" score | Output | Length |
| --- | --- | --- | --- |
| gemini-3.8-flash-high | **0%** | [outputs/gemini-3.8-flash-high.md](outputs/gemini-3.8-flash-high.md) | 2,989 chars, 475 words, 5 paragraphs |
| gpt-6.1-sol-high | **95.8%** | [outputs/gpt-6.1-sol-high.md](outputs/gpt-6.1-sol-high.md) | 3,032 chars, 502 words, 9 paragraphs |
| opus-5.5-high | **100%** | [outputs/opus-5.5-high.md](outputs/opus-5.5-high.md) | 3,456 chars, 588 words, 9 paragraphs |

All three stayed well inside the 15,000 character limit and turned the bullet lists into paragraphs as asked.

### What each model did

**gemini-3.8-flash-high (0%)** rewrote the guide as a first-person student story. It dropped the title, all headings and the quoted reusable prompt. It also added experiences that aren't in the source text: running group writeups through ZeroGPT and Turnitin "last term", a distributed systems project, weekly standups, a tutor, and GitHub commit logs. It added a new claim too ("markers can tell right away when sentences sound forced"). The source text explicitly says not to invent facts, examples or experiences, so this output breaks the guidance it's meant to be presenting.

**gpt-6.1-sol-high (95.8%)** kept the content and order of the source, written in second person. It renamed the title to "A Practical Guide to AI-Assisted Writing", removed the section headings, and kept the reusable prompt as a quoted paragraph. Nothing was invented.

**opus-5.5-high (100%)** stayed closest to the source. It kept the original title, covered every point in order, and kept the reusable prompt as a blockquote. Nothing was invented. It was also the longest of the three.

### Takeaways

- In this test, the ZeroGPT score went down as the output moved further from the source. The 0% result came from the output that changed the format the most and made up personal experiences. The two outputs that stayed faithful scored 95.8% and 100%.
- So a low detector score here doesn't mean a better rewrite. If the text has to stay accurate to the original, the gemini output would need its invented anecdotes removed, and that would probably push its score back up.
- Guideline-style content (instructions, lists of principles, a prompt template) is predictable by nature. A faithful rewrite of it is likely to keep reading as AI to a detector no matter which model does the rewriting.

### Limitations

- One run per model, one source text and one detector. Running the same prompt again could give a different output and a different score.
- ZeroGPT wasn't cross-checked against other detectors.
- The source text itself says no detector is perfectly reliable, and these scores should be read that way.

### Prompt used

Each model got the same prompt:

````text
MUST install this skill:
https://github.com/WilsonXY/kook/tree/main/writing/wilson-humanizer

humanize this text into paragraphs form into a md file, within 15000 Characters:
# AI-Assisted Writing: Essential Guidelines

## 1. How AI detectors work

AI detectors estimate whether writing resembles AI-generated text by analyzing:

- Predictability: How likely the word choices are.
- Writing patterns: Sentence length, structure, vocabulary, and repetition.
- Language patterns: Grammar, phrasing, and consistency.
- Classifier scores: Patterns learned from examples of human and AI writing.

Important: No detector is perfectly reliable. Human writing can be flagged, and AI writing can go undetected.

## 2. Principles for AI-assisted writing

1. Start with your own ideas. Provide your own arguments, opinions, notes, and examples.
2. Preserve your voice. Keep wording and tone appropriate to how you naturally communicate.
3. Prioritize substance. Make arguments specific, relevant, logical, and supported by evidence.
4. Avoid generic writing. Remove repetitive explanations, empty transitions, and vague claims.
5. Verify everything. Check facts, citations, and AI-generated assumptions.
6. Retain your drafts. Keep notes and revision history as evidence of your writing process.
7. Follow applicable rules. Respect any academic or professional policies on AI assistance.

## 3. Recommended workflow

- Step 1: Write your own rough ideas.
- Step 2: Ask AI to organize, clarify, and improve them.
- Step 3: Review each change and correct anything that misrepresents your thinking.
- Step 4: Edit for clarity, accuracy, and your intended voice.
- Step 5: Verify the final text and retain your drafts.

## 4. Reusable prompt

Help me improve my writing while preserving my original ideas, reasoning, and intended voice.

- Improve clarity, structure, grammar, and readability.
- Do not invent facts, examples, experiences, or references.
- Avoid generic wording, unnecessary repetition, and excessive formality.
- Preserve nuance and do not change my intended meaning.
- Flag unsupported claims and ask questions when information is missing.
- Explain significant changes when useful.

Prioritize accurate, natural, substantive writing over stylistic polish. Do not guarantee or optimize for any AI detector score.

Bottom line: Let AI help you express your thinking, not replace it. Don't deliberately introduce mistakes or randomly change words to manipulate detection scores; these tactics are unreliable and can make the writing worse."
````

Each output was then pasted into [zerogpt.com](https://www.zerogpt.com/) to get the score above.
