# What Are Foundation Models?

![Bridge metaphor: apps are the road; the foundation model holds them up](images/foundation-bridge-metaphor.jpg)

*The apps are the road. The foundation model is what holds them up.*

If you use ChatGPT, Claude, Gemini, a coding copilot, or an image generator, you are already sitting on foundation models. This note keeps the real definitions and the technical ideas — written so they land on the first read. Three questions: what is a foundation model, why do they still differ if they “train the same way,” and how do you tell which is which.

Here is the formal definition from Stanford’s Center for Research on Foundation Models (CRFM), 2021:

> A foundation model is any model that is trained on broad data (generally using self-supervision at scale) that can be adapted (e.g., fine-tuned) to a wide range of downstream tasks.[1](https://arxiv.org/abs/2108.07258)

In plain language: train one big model on a huge, mixed pile of data; then reuse that same model for many different jobs — chat, code, search, images, tools. NVIDIA’s explainer says the same thing: a neural network trained on mountains of raw data that can be steered to a wide range of tasks.[2](https://blogs.nvidia.com/blog/what-are-foundation-models/) Older machine learning was usually the opposite — one small model for one labeled job. Foundation models flip that: general base first, specialize later.[3](https://www.ibm.com/think/topics/foundation-models)

They called it “foundation” on purpose. It is **central** — many products stand on it — and **incomplete** — it is not the finished product until you adapt it.[1](https://arxiv.org/abs/2108.07258)

CRFM summarized the stakes in two words. **Emergence:** at scale, models show useful behaviors nobody programmed line by line (for example, doing a new task from a prompt). **Homogenization:** many teams converge on the same few base models and methods. That reuse amortizes effort. It also creates a shared crack: if the foundation is biased or brittle, every adapted system inherits the problem.[1](https://arxiv.org/abs/2108.07258)

The usual training method is **self-supervised learning** (CRFM often says self-supervision at scale). You do not hand-label every example. You make the model predict a missing piece of the raw data itself. For text, that is often next-word or masked-word prediction: “The cat sat on the ___.” Nobody labeled “mat.” Do this across a huge corpus and the model absorbs language structure and a lot of world-shaped patterns. Images and code use the same spirit — reconstruct a patch, predict the next tokens.

That huge practice run is **pretraining**: build the general base. Almost everyone else does **adaptation** — prompting, retrieval-augmented generation (RAG: fetch documents, then answer with them),[10](https://arxiv.org/abs/2005.11401) fine-tuning, parameter-efficient methods like LoRA (train a small add-on instead of every weight),[11](https://arxiv.org/abs/2106.09685) and alignment / preference training. Transfer learning at industrial scale. You usually do not rebuild the foundation. You stand on it.

![Pretrain once for generality. Adapt for a real job.](images/foundation-pretrain-finetune.svg)

*Pretrain once for generality. Adapt for a real job.*

A short technical spine: the **transformer** architecture (Vaswani et al., 2017 — *Attention Is All You Need*) made attention-based networks scale.[4](https://arxiv.org/abs/1706.03762) **BERT** (2018) made “one pretrained language model, many fine-tunes” normal.[5](https://arxiv.org/abs/1810.04805) **GPT-3** (2020) showed few-shot prompting on a model with about **175 billion parameters** — parameters are the learned knobs (weights) inside the network.[6](https://arxiv.org/abs/2005.14165) ChatGPT and diffusion image models made the shift visible outside the lab. The Stanford AI Index counted **149** foundation models published in 2023 alone.[7](https://aiindex.stanford.edu/)

An LLM (large language model) is one kind of foundation model — language. Foundations also cover speech (e.g. Whisper), images (e.g. diffusion / vision bases), and multimodal systems that take more than one input type. Same definition; different data and output form.

Why does everyone say training needs massive resources? Because **pretraining the base** burns three fuels at once: **data** (internet-scale text, code, images — filtered and tokenized), **compute** (thousands of accelerators for weeks or months; often reported as training FLOPs — floating-point operations, i.e. how much math the chips did), and **people + energy**. Using a chat app is not the same as building GPT.

Public dollar figures are estimates — labs rarely publish a full receipt. The AI Index, with Epoch AI, estimated *cloud-rental compute for the training run*: original Transformer ~**$900**; RoBERTa Large ~**$160,000**; GPT-4 ~**$78 million**; Gemini Ultra ~**$191 million**.[8](https://hai.stanford.edu/ai-index/2024-ai-index-report/research-and-development) Not total R&D. Still enough to see why few organizations pretrain and almost everyone else adapts.

![Training compute of notable models, 2012–23 (log scale)](images/foundation-ai-index-compute.jpeg)

*Training compute of notable models, 2012–23 (log scale). Epoch data via Stanford AI Index. Log scale means each step up is a large jump, not a small bump.*[7](https://aiindex.stanford.edu/)

So if the recipe sounds shared — broad data, self-supervision, then adapt — **why are foundation models different?**

Most language foundations still share the transformer family. Architecture matters most when the *job type* changes: text-only chat versus diffusion for images, or multimodal stacks.[9](https://www.ibm.com/think/topics/diffusion-models) Within chat models, the big splits are usually:

1. **Data mix** — how much code, math, dialogue, books, multilingual text, or images the model saw during pretraining (and continued training)  
2. **Post-training** — instruction tuning, preference / RLHF-style training, domain fine-tunes; this steers how it writes, codes, refuses, and reasons  
3. **Scale and training recipe** — parameter count, tokens seen, compute, designs such as mixture-of-experts  
4. **Evaluation** — what you measure; a model can lead on coding and lag on long-form writing  

That last point is how you read the market without drowning. A **benchmark** is a fixed test for a skill. A high score means “strong on *this* skill,” not “best foundation model overall.”

| If you care about… | Look at signals like… |
| --- | --- |
| Chat / writing preference | LMArena (human A/B votes)[14](https://lmarena.ai/) |
| Coding / agents | Coding arenas, SWE-bench-style suites |
| Hard science / math | GPQA, MATH, contest math (e.g. AIME) |
| Multimodal | MMMU and related vision–language suites |
| Quality vs price vs speed | Artificial Analysis[15](https://artificialanalysis.ai/leaderboards/models) |
| Open weights vs closed API | AI Index trends; Hugging Face hub[13](https://hai.stanford.edu/ai-index/2025-ai-index-report/technical-performance)[12](https://huggingface.co/models) |

Worked example — how to read a chart. The 2025 AI Index reprints OpenAI’s comparison of GPT-4o, o1-preview, and o1. Do not start with “who won.” Start with the **panel title**: that is the skill under test.

![GPT-4o vs o1-preview vs o1 on select benchmarks](images/foundation-compare-by-benchmark.png)

*GPT-4o vs o1-preview vs o1. Chart: 2025 AI Index (OpenAI, 2024).*[13](https://hai.stanford.edu/ai-index/2025-ai-index-report/technical-performance)

**MMLU** (broad general knowledge): bars sit close (~88–92%). Look only there and they seem almost the same. **MATH**, **GPQA Diamond** (hard science), and **AIME 2024** (contest math): the gap opens. On AIME, GPT-4o is about **9%**; o1 about **74%**. Same lab, same era; the benchmark shows *which capability* moved. Habit for any leaderboard: name the skill first, then trust the ranking for that skill only.

Families you will keep meeting (versions change; roles stay recognizable):[2](https://blogs.nvidia.com/blog/what-are-foundation-models/)

| When | Family | Role |
| --- | --- | --- |
| 2018 | **BERT** | Pretrain + fine-tune for understanding / search[5](https://arxiv.org/abs/1810.04805) |
| 2020 | **GPT-3** | Few-shot text; early API apps[6](https://arxiv.org/abs/2005.14165) |
| 2022 | **Stable Diffusion**, **Whisper** | Text-to-image; speech-to-text |
| 2022– | **GPT**, **Claude**, **Gemini** | Chat, coding help, multimodal assistants |
| 2023– | **Llama**, **Mistral**, **Qwen**, **DeepSeek**… | Open-weight bases to deploy or fine-tune[12](https://huggingface.co/models) |
| Also | Code models; **AlphaFold**, **SAM**, world models | Coding; science / vision / physical AI |

Major generations (GPT-3 → GPT-4 class) are usually new pretraining runs — new data, compute, and recipe — not “keep training the old checkpoint forever.” Inside a generation, a lot of product change is **post-training** on a related base.

Capability and failure arrive together. Models **hallucinate** (fluent but false), can amplify **bias** in the data, raise privacy and IP questions, and cost real energy. Homogenization means one cracked foundation can crack many products.[1](https://arxiv.org/abs/2108.07258) Grounding (e.g. RAG), filtering, red-teaming, and monitoring are part of using them — not optional polish.

A foundation model is not the product. It is the base layer.

**Formal idea, plain takeaway:** trained on broad data with self-supervision at scale, then adapted to many downstream tasks — same recipe, different data and post-training, different strengths. Pick by the job and the matching benchmark, then adapt.

---

## Notes

1. Bommasani et al., *On the Opportunities and Risks of Foundation Models* (Stanford CRFM / HAI, 2021). Formal definition + emergence / homogenization. [arXiv](https://arxiv.org/abs/2108.07258) · [CRFM](https://crfm.stanford.edu/report.html)
2. NVIDIA, “What Are Foundation Models?” [Link](https://blogs.nvidia.com/blog/what-are-foundation-models/)
3. IBM Think, “What are foundation models?” [Link](https://www.ibm.com/think/topics/foundation-models)
4. Vaswani et al., *Attention Is All You Need* (2017). [arXiv](https://arxiv.org/abs/1706.03762)
5. Devlin et al., *BERT* (2018). [arXiv](https://arxiv.org/abs/1810.04805)
6. Brown et al., GPT-3 (2020). [arXiv](https://arxiv.org/abs/2005.14165)
7. Stanford AI Index 2024. [Link](https://aiindex.stanford.edu/)
8. Stanford AI Index 2024 — training-cost estimates (Epoch AI). [Link](https://hai.stanford.edu/ai-index/2024-ai-index-report/research-and-development)
9. IBM Think, diffusion models. [Link](https://www.ibm.com/think/topics/diffusion-models)
10. Lewis et al., RAG (2020). [arXiv](https://arxiv.org/abs/2005.11401)
11. Hu et al., LoRA (2021). [arXiv](https://arxiv.org/abs/2106.09685)
12. Hugging Face model hub. [Link](https://huggingface.co/models)
13. Stanford AI Index 2025 — Technical performance. [Link](https://hai.stanford.edu/ai-index/2025-ai-index-report/technical-performance)
14. LMArena. [Link](https://lmarena.ai/)
15. Artificial Analysis. [Link](https://artificialanalysis.ai/leaderboards/models)
