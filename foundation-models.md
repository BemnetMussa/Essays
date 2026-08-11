# What Are Foundation Models?

![Bridge metaphor: apps are the road; the foundation model holds them up](images/foundation-bridge-metaphor.jpg)

*The apps are the road. The foundation model is what holds them up.*

If you use ChatGPT, Claude, Gemini, a coding copilot, or an image generator, you are already living on foundation models. What I wanted was a clean map: what they are, why they still differ even when they “train the same way,” and how to tell which is which.

A foundation model is an AI neural network trained on a huge, broad dataset — usually with little hand-labeling — that can then be adapted to many different jobs: answering questions, writing code, analyzing images, talking to tools, and more. Stanford CRFM researchers coined the term in 2021.[1](https://arxiv.org/abs/2108.07258) They chose “foundation” on purpose: the model is central enough that many products sit on top of it, and incomplete enough that it is not the product by itself — you still have to adapt it. Industry explainers say the same thing in plainer language — for example the NVIDIA overview: a neural net trained on mountains of raw data that can be steered toward a wide range of tasks.[2](https://blogs.nvidia.com/blog/what-are-foundation-models/) The contrast with older machine learning is the same idea — small models trained narrowly for one job, versus large general models you specialize later (IBM Think).[3](https://www.ibm.com/think/topics/foundation-models)

Two ideas from that Stanford work sit under everything that follows. **Emergence** means that at scale, models show skills nobody explicitly programmed. **Homogenization** means many teams converge on similar methods and even the same base models.[1](https://arxiv.org/abs/2108.07258) That reuse is powerful. It is also a shared crack: if the foundation is biased or brittle, every adapted product inherits the problem.

The shared training idea is simple. Most foundations learn with **self-supervised learning** on raw data. For text, that often means guessing a missing or next word: “The cat sat on the ___.” Nobody labeled “cat” or “mat.” Predict the next piece often enough, and the model absorbs patterns and context. Images and code use the same spirit — reconstruct a patch, predict the next tokens. Then comes the expensive rare step: **pre-training** a general base. Almost everyone else does **adaptation** — prompting, RAG, fine-tuning, LoRA, alignment — which is transfer learning at industrial scale. You usually do not rebuild the foundation. You stand on it.

![Pre-train once for generality, then adapt for a real job](images/foundation-pretrain-finetune.svg)

*Pre-train once for generality, then adapt for a real job.*

A short spine of history places the idea. In 2017 the transformer paper (*Attention Is All You Need*) made attention-based networks scale.[4](https://arxiv.org/abs/1706.03762) In 2018 Google’s BERT paper made “one pretrained language model, many fine-tunes” normal.[5](https://arxiv.org/abs/1810.04805) In 2020 OpenAI’s GPT-3 paper showed that a huge model could do many tasks from a prompt alone — about **175 billion** parameters in that paper.[6](https://arxiv.org/abs/2005.14165) Then ChatGPT and diffusion-based image tools made the shift visible outside the lab. The Stanford AI Index (2024) counted **149 foundation models** published in 2023 alone.[7](https://aiindex.stanford.edu/)

Why does everyone say training a foundation model takes massive resources? Because the rare step — **pretraining the base** — burns three expensive fuels at once: **data** (internet-scale text, code, images, filtered and tokenized), **compute** (thousands of GPUs/accelerators running for weeks or months), and **people + energy** (teams, clusters, power). Frontier pretraining is closer to building a power plant than installing an app.

Public dollar figures are estimates — labs rarely publish a full receipt — but the AI Index (with Epoch AI) put numbers on *cloud-rental compute for the training run* alone: the original Transformer (2017) on the order of **~$900**; RoBERTa Large (2019) around **~$160,000**; GPT-4 around **~$78 million**; Gemini Ultra around **~$191 million**.[8](https://hai.stanford.edu/ai-index/2024-ai-index-report/research-and-development) Those are not total company R&D. They are still enough to show the jump. Training compute on a log chart looks like a rocket:

![Training compute of notable models, 2012–23 (log scale)](images/foundation-ai-index-compute.jpeg)

*Training compute of notable models, 2012–23 (log scale). Epoch data in the Stanford AI Index chart.*[7](https://aiindex.stanford.edu/)

So the industry pattern makes sense: a few organizations pretrain foundations; almost everyone else **adapts** — prompt, retrieve (RAG), fine-tune, LoRA — on top of a base that already exists. “Massive resources” is about building the foundation, not every use of one.

So if the recipe sounds the same — broad data, self-supervision, then adapt — **why are foundation models different?**

Most language foundations still share the same broad architecture family: **transformers**. Architecture matters most when the *job type* changes: text-only versus multimodal (vision/audio), or diffusion for images versus a language decoder for chat.[9](https://www.ibm.com/think/topics/diffusion-models) Within chat models, the big splits are usually not “secret different brains.” They are:

1. **Data mix** — how much code, math, dialogue, books, multilingual text, or images the model saw.
2. **Post-training** — instruction tuning, preference training / RLHF, domain fine-tunes. This steers how it writes, codes, refuses, and reasons.
3. **Scale and training recipe** — size, compute, length of training, designs like mixture-of-experts.
4. **What you measure** — a model can look strong on coding and weaker on long essays, or the reverse.

That last point is how you read the landscape without getting lost. Whenever you see a benchmark or leaderboard, ask: **what skill is this actually testing?** A high score means “strong on *this* part,” not “best foundation model overall.” Coding boards pick coding-strong models; chat arenas pick writing people prefer; science suites pick hard reasoning. Same idea for price, speed, or open weights — each board answers a different question.

So when you compare foundations, read the benchmark first, then the names:

| Skill / question | Benchmarks / boards that speak to it |
| --- | --- |
| Chat / writing preference | LMArena / LMSYS Chatbot Arena (human votes)[14](https://lmarena.ai/) |
| Coding / software agents | Coding arenas, SWE-bench-style suite |
| Hard science / math reasoning | GPQA, MATH, contest math |
| Multimodal (text + image, …) | MMMU and related vision–language suites |
| Quality vs price vs speed | Artificial Analysis leaderboard[15](https://artificialanalysis.ai/leaderboards/models) |
| Open weights vs closed API | AI Index open/closed Arena trends[13](https://hai.stanford.edu/ai-index/2025-ai-index-report/technical-performance); Hugging Face hub[12](https://huggingface.co/models) |

Here is a worked example of reading a benchmark chart — the habit you can reuse anywhere. The Stanford AI Index 2025 reprints OpenAI’s comparison of three models from the same lab (GPT-4o, o1-preview, o1). Do not start with “who won.” Start with the **panel title**: that is the skill under test.

![GPT-4o vs o1-preview vs o1 on select benchmarks](images/foundation-compare-by-benchmark.png)

*GPT-4o vs o1-preview vs o1. Chart: 2025 AI Index (OpenAI, 2024).*[13](https://hai.stanford.edu/ai-index/2025-ai-index-report/technical-performance)

Read left to right. **MMLU** is broad general knowledge — the three bars sit close (~88–92%). If that were the only chart, you would think they were almost the same model. **MATH**, **GPQA Diamond** (hard science), and **AIME 2024** (contest math) are different skills — and the gap opens wide. On AIME, GPT-4o is about 9% while o1 is about 74%. Same family, same era; the benchmark tells you *which part* improved. So when you meet any leaderboard later: name the skill in the title, then trust the ranking for that skill only — not as “best foundation model.”

For live boards that keep moving (chat preference, price/speed, open vs closed), use LMArena and Artificial Analysis the same way — read what they measure first.[14](https://lmarena.ai/)[15](https://artificialanalysis.ai/leaderboards/models)

With that in mind, here is a reading map of **which foundations have actually been used** — families you will keep meeting. Versions change; the job of each line stays recognizable.[2](https://blogs.nvidia.com/blog/what-are-foundation-models/)

| When (approx.) | Model / family | Main use |
| --- | --- | --- |
| 2018 | **BERT** (Google) | Search / NLP understanding; early fine-tune workhorse.[5](https://arxiv.org/abs/1810.04805) |
| 2020 | **GPT-3** (OpenAI) | Few-shot text; early API apps.[6](https://arxiv.org/abs/2005.14165) |
| 2022 | **Stable Diffusion** | Open text-to-image |
| 2022 | **Whisper** (OpenAI) | Speech-to-text |
| 2022– | **GPT-class** (ChatGPT, GPT-4, GPT-4o, later) | Chat, coding help, multimodal assistants |
| 2023– | **Claude** (Anthropic) | Chat, long writing, coding (API) |
| 2023– | **Llama** + peers (**Mistral**, **Qwen**, **DeepSeek**, …) | Open-weight deploy and fine-tunes[12](https://huggingface.co/models) |
| 2023– | **Gemini** (Google DeepMind) | Multimodal / long-context in Google’s stack |
| 2022– | **Code models** (Codex → Copilot; Code Llama; coding-strong chat) | IDE completion and agents |
| Science / other | **AlphaFold**, **Segment Anything**, world/physical models | Biology, vision, robotics/sim |

So the path is: **same foundation idea** → **differences from data, post-training, modality, and scale** → **tell them apart with the benchmark that matches your job** → **then choose access and cost**. Before building on a base: What modality? Must weights stay in-house? Coding, chat, retrieval, or images? Which eval would convince me on *my* data? What latency and token cost can I afford?

Capability and failure arrive together. Models **hallucinate**, can amplify **bias**, raise privacy and IP questions, and cost real compute and energy. Homogenization means one cracked foundation can crack many products. The 2021 Stanford CRFM report said early what is still true: we deploy these systems widely while still learning when they fail.[1](https://arxiv.org/abs/2108.07258) Grounding with retrieval, filtering, red-teaming, and monitoring are part of the work.

A foundation model is not the product. It is the base layer. What I am taking away: the definition; that “big” means parameters *and* data *and* compute; the adapt stack; **why they differ even when the training idea is shared**; **how to tell which is which by job and matching benchmarks**; and evaluation before trust.

One line: same foundation idea, different data and tuning, different strengths — pick by the job, then adapt.

---

## Notes

Click a number in the essay to open the source.

1. Bommasani et al., *On the Opportunities and Risks of Foundation Models* (Stanford CRFM / HAI, 2021). [arXiv](https://arxiv.org/abs/2108.07258) · [CRFM](https://crfm.stanford.edu/report.html)
2. NVIDIA, “What Are Foundation Models?” [Link](https://blogs.nvidia.com/blog/what-are-foundation-models/)
3. IBM Think, “What are foundation models?” [Link](https://www.ibm.com/think/topics/foundation-models)
4. Vaswani et al., *Attention Is All You Need* (2017). [arXiv](https://arxiv.org/abs/1706.03762)
5. Devlin et al., *BERT* (2018). [arXiv](https://arxiv.org/abs/1810.04805)
6. Brown et al., GPT-3 (2020). [arXiv](https://arxiv.org/abs/2005.14165)
7. Stanford AI Index 2024. [Link](https://aiindex.stanford.edu/)
8. Stanford AI Index 2024 — Research and Development (training-cost estimates with Epoch AI: GPT-4 ~$78M compute, Gemini Ultra ~$191M). [Link](https://hai.stanford.edu/ai-index/2024-ai-index-report/research-and-development) · [chapter PDF](https://hai.stanford.edu/assets/files/hai_ai-index-report-2024_chapter1.pdf)
9. IBM Think, diffusion models. [Link](https://www.ibm.com/think/topics/diffusion-models)
10. Lewis et al., RAG (2020). [arXiv](https://arxiv.org/abs/2005.11401)
11. Hu et al., LoRA (2021). [arXiv](https://arxiv.org/abs/2106.09685)
12. Hugging Face model hub. [Link](https://huggingface.co/models)
13. Stanford AI Index 2025 — Technical performance (latest charts used above). [Link](https://hai.stanford.edu/ai-index/2025-ai-index-report/technical-performance)
14. LMArena / LMSYS Chatbot Arena. [Link](https://lmarena.ai/)
15. Artificial Analysis LLM leaderboard. [Link](https://artificialanalysis.ai/leaderboards/models)
