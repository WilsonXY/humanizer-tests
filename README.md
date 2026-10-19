# humanizer-tests

Testing how different models rewrite the same text with the [wilson-humanizer](https://github.com/WilsonXY/kook/tree/main/writing/wilson-humanizer) skill, and how the results score on AI detectors.

## Test 1: AI-Assisted Writing guidelines (2026-10-19)

### Results

AI score from each detector (higher means the detector thinks it's more likely AI-written):

| Model | [ZeroGPT](https://www.zerogpt.com/) | [Grammarly](https://www.grammarly.com/ai-detector) | [QuillBot](https://quillbot.com/ai-content-detector) | [GPTZero](https://gptzero.me/) | Average |
| --- | --- | --- | --- | --- | --- |
| gemini-3.8-flash-high | **0%** | 81% | **11%** | 100% | **48.0%** |
| gpt-6.1-sol-high | 95.8% | 89% | 58% | 100% | 85.7% |
| opus-5.5-high | 100% | **48%** | 23% | 100% | 67.8% |

Bold marks the lowest score in each column. GPTZero scored all three at 100%.

| Model | Output | Length |
| --- | --- | --- |
| gemini-3.8-flash-high | [outputs/gemini-3.8-flash-high.md](outputs/gemini-3.8-flash-high.md) | 2,989 chars, 475 words, 5 paragraphs |
| gpt-6.1-sol-high | [outputs/gpt-6.1-sol-high.md](outputs/gpt-6.1-sol-high.md) | 3,032 chars, 502 words, 9 paragraphs |
| opus-5.5-high | [outputs/opus-5.5-high.md](outputs/opus-5.5-high.md) | 3,456 chars, 588 words, 9 paragraphs |

All three stayed well inside the 15,000 character limit and turned the bullet lists into paragraphs as asked.

#### What each detector said

- **ZeroGPT:** Gemini was "Human written" at 0%. GPT (95.8%) and Opus (100%) were both "AI/GPT Generated".
- **Grammarly:** Gemini 81%, GPT 89%, Opus 48%. Opus was the only output Grammarly put under 50%.
- **QuillBot:** Gemini 11%, GPT 58%, Opus 23%. QuillBot rated "AI-refined" at 0% for all three, so the whole score is "AI-generated" and the rest is "human-written".
- **GPTZero:** all three were 100% AI, 0% mixed and 0% human, with "highly confident" verdicts. Gemini and GPT were checked with Basic Scan and Opus with Advanced Scan (see limitations).

### What each model did

**gemini-3.8-flash-high** rewrote the guide as a first-person student story. It dropped the title, all headings and the quoted reusable prompt. It also added experiences that aren't in the source text: running group writeups through ZeroGPT and Turnitin "last term", a distributed systems project, weekly standups, a tutor, and GitHub commit logs. It added a new claim too ("markers can tell right away when sentences sound forced"). The source text explicitly says not to invent facts, examples or experiences, so this output breaks the guidance it's meant to be presenting.

**gpt-6.1-sol-high** kept the content and order of the source, written in second person. It renamed the title to "A Practical Guide to AI-Assisted Writing", removed the section headings, and kept the reusable prompt as a quoted paragraph. Nothing was invented.

**opus-5.5-high** stayed closest to the source. It kept the original title, covered every point in order, and kept the reusable prompt as a blockquote. Nothing was invented. It was also the longest of the three.

### Takeaways

- **The detectors disagree a lot.** The same Gemini text scored anywhere from 0% to 100% depending on the detector. Opus ranged from 23% to 100%. A single detector score doesn't tell you much on its own.
- **No output passed everywhere.** GPTZero flagged all three at 100%. Every output was rated above 80% AI by at least two of the four detectors.
- **The detectors don't agree on which output is "most human".** Gemini scored lowest on ZeroGPT and QuillBot, Opus scored lowest on Grammarly, and GPTZero gave them all the same score.
- **GPT scored high on every detector.** At 58% to 100%, it had the highest average of the three.
- **A low score doesn't mean a better rewrite.** Gemini had the lowest average, but it got there by changing the format and making up personal experiences. If the text has to stay accurate to the original, those anecdotes would need to come out.
- Guideline-style content (instructions, lists of principles, a prompt template) is predictable by nature. A faithful rewrite of it is likely to keep reading as AI to most detectors.

### Limitations

- One run per model, one source text, and one scan per detector. Running the same prompt again could give a different output and different scores.
- The outputs were checked with their markdown syntax still in them. The GPT and Opus outputs have a `#` title (Opus also has a `>` blockquote), while the Gemini output is plain paragraphs. That difference might have affected the scores too.
- For ZeroGPT the outputs were pasted in. For Grammarly, QuillBot and GPTZero they were typed into each site's text box from the same files. The text was the same either way.
- QuillBot's word counts were 501 for GPT and 586 for Opus instead of 502 and 588, because QuillBot doesn't count the `#` and `>` markers as words. The text itself was complete.
- GPTZero scan types aren't matched. Gemini and GPT used the free Basic Scan. Opus used Advanced Scan, which was the default once signed in, and that used up the account's free Advanced Scans, so the other two couldn't be rerun on Advanced. All three still came out at 100%.
- Grammarly's Gemini result came from a signed-out scan. A signed-in rerun hit the daily free limit, but the 81% showing behind the limit message matched. An earlier signed-out GPT attempt showed a stale 81% and was thrown out. The 89% in the table is from a fresh signed-in scan.
- The source text itself says no detector is perfectly reliable, and these scores should be read that way.

### Screenshots

Each detector has its own folder under [screenshots/](screenshots/), with one file per model.

#### ZeroGPT

| gemini-3.8-flash-high: 0% | gpt-6.1-sol-high: 95.8% | opus-5.5-high: 100% |
| --- | --- | --- |
| ![ZeroGPT, Gemini: 0%](screenshots/zerogpt/gemini-3.8-flash-high.png) | ![ZeroGPT, GPT: 95.8%](screenshots/zerogpt/gpt-6.1-sol-high.png) | ![ZeroGPT, Opus: 100%](screenshots/zerogpt/opus-5.5-high.png) |

In the GPT and Opus screenshots, ZeroGPT highlights almost every sentence in yellow, starting with the `#` title line.

#### Grammarly

| gemini-3.8-flash-high: 81% | gpt-6.1-sol-high: 89% | opus-5.5-high: 48% |
| --- | --- | --- |
| ![Grammarly, Gemini: 81%](screenshots/grammarly/gemini-3.8-flash-high.jpg) | ![Grammarly, GPT: 89%](screenshots/grammarly/gpt-6.1-sol-high.jpg) | ![Grammarly, Opus: 48%](screenshots/grammarly/opus-5.5-high.jpg) |

#### QuillBot

| gemini-3.8-flash-high: 11% | gpt-6.1-sol-high: 58% | opus-5.5-high: 23% |
| --- | --- | --- |
| ![QuillBot, Gemini: 11%](screenshots/quillbot/gemini-3.8-flash-high.jpg) | ![QuillBot, GPT: 58%](screenshots/quillbot/gpt-6.1-sol-high.jpg) | ![QuillBot, Opus: 23%](screenshots/quillbot/opus-5.5-high.jpg) |

#### GPTZero

| gemini-3.8-flash-high: 100% | gpt-6.1-sol-high: 100% | opus-5.5-high: 100% |
| --- | --- | --- |
| ![GPTZero, Gemini: 100%](screenshots/gptzero/gemini-3.8-flash-high.jpg) | ![GPTZero, GPT: 100%](screenshots/gptzero/gpt-6.1-sol-high.jpg) | ![GPTZero, Opus: 100%](screenshots/gptzero/opus-5.5-high.jpg) |

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

Each output was then run through the four detectors above to get the scores.
