# What Are Foundation Models?

![Bridge metaphor: apps are the road; the foundation model holds them up](images/foundation-bridge-metaphor.jpg)

*The apps are the road. The foundation model is what holds them up.*

ChatGPT, Claude, Gemini, coding copilots, image tools — those all sit on **foundation models**. This note is a simple map: what that means, why two models can “train the same way” and still feel different, and how to tell which is which.

**A foundation model is a big AI model trained once on a huge, mixed pile of data, then reused for many jobs.** Write. Code. Answer. Look at images. Talk to tools. Same base, different uses later.[1](https://arxiv.org/abs/2108.07258)[2](https://blogs.nvidia.com/blog/what-are-foundation-models/)

Old machine learning was usually the opposite: one small model for one job. Foundation models flip that — train a general base, then specialize.[3](https://www.ibm.com/think/topics/foundation-models)

Stanford researchers coined “foundation” in 2021 on purpose. It is the floor many products stand on. It is not the finished product. You still have to adapt it.[1](https://arxiv.org/abs/2108.07258)

Two side effects matter. At scale, models pick up skills nobody listed by hand (**emergence**). And many companies end up using the same few bases (**homogenization**). Reuse is powerful. It is also risky: if the base is wrong, every app on top inherits the crack.[1](https://arxiv.org/abs/2108.07258)

How do they learn without a human labeling every example? Mostly **self-supervised learning**. For text, the model practices fill-in-the-blank: “The cat sat on the ___.” Do that enough times on enough text, and it absorbs language and patterns. Images and code use the same idea — predict a missing piece.

That practice at huge scale is **pretraining**. It is rare and expensive. Almost everyone else does the cheaper step: **adapt** the base with prompts, retrieval (RAG), fine-tuning, or small add-ons like LoRA. You usually do not rebuild the foundation. You stand on it.

![Pretrain once. Adapt for the real job.](images/foundation-pretrain-finetune.svg)

*Pretrain once. Adapt for the real job.*

A short history: transformers (2017) made this scale.[4](https://arxiv.org/abs/1706.03762) BERT (2018) made “pretrain, then fine-tune” normal.[5](https://arxiv.org/abs/1810.04805) GPT-3 (2020) showed a huge model could do many jobs from a prompt — about **175 billion** knobs (parameters) in that paper.[6](https://arxiv.org/abs/2005.14165) Then ChatGPT made it mainstream. In 2023 alone, the Stanford AI Index counted **149** new foundation models.[7](https://aiindex.stanford.edu/)

Why the “needs massive resources” talk? Because **building** the base burns three things at once: huge data, huge computer time (thousands of GPUs for weeks or months), and real teams plus electricity. Using ChatGPT is cheap by comparison. Pretraining is not.

Public dollar numbers are estimates (labs rarely publish a full bill). The AI Index, with Epoch AI, estimated cloud-rental **compute for the training run**: original Transformer ~**$900**; RoBERTa Large ~**$160,000**; GPT-4 ~**$78 million**; Gemini Ultra ~**$191 million**.[8](https://hai.stanford.edu/ai-index/2024-ai-index-report/research-and-development) That is not total company R&D. It is still enough to see the jump.

![Training compute of notable models, 2012–23 (log scale)](images/foundation-ai-index-compute.jpeg)

*Training compute of notable models, 2012–23 (log scale). Each step up is a big jump. Chart: Stanford AI Index / Epoch.*[7](https://aiindex.stanford.edu/)

So the industry split is simple: a few labs pretrain. Almost everyone else adapts.

Then the natural question: if the recipe is the same — big data, self-supervise, adapt — **why do models differ?**

For chat models, it is usually not a secret different brain. Most still use transformers. The real splits are:

1. **What they read** — more code, more math, more chat, more languages, more images  
2. **How they were tuned after** — instructions, human preference training, domain fine-tunes  
3. **How big the run was** — size, compute, training tricks  
4. **What test you look at** — strong at coding, weaker at long essays, or the reverse  

Architecture matters most when the *job type* changes — text chat versus image generation, for example.[9](https://www.ibm.com/think/topics/diffusion-models)

So when you see a leaderboard, do not ask “who is best overall?” Ask: **what skill is this test measuring?** A high score means strong on *that* skill. Nothing more.

| If you care about… | Look at… |
| --- | --- |
| Chat people prefer | LMArena[14](https://lmarena.ai/) |
| Coding | Coding arenas, SWE-bench-style tests |
| Hard math / science | GPQA, MATH, contest math |
| Text + images | MMMU and similar |
| Quality vs price vs speed | Artificial Analysis[15](https://artificialanalysis.ai/leaderboards/models) |
| Open weights vs API only | AI Index / Hugging Face[13](https://hai.stanford.edu/ai-index/2025-ai-index-report/technical-performance)[12](https://huggingface.co/models) |

Here is that habit on a real chart. Same lab. Same era. Three models: GPT-4o, o1-preview, o1. Start with each panel title — that is the skill — not with “who won.”

![GPT-4o vs o1-preview vs o1 on select benchmarks](images/foundation-compare-by-benchmark.png)

*Chart: 2025 AI Index (OpenAI, 2024).*[13](https://hai.stanford.edu/ai-index/2025-ai-index-report/technical-performance)

On general knowledge (**MMLU**), the bars are close (~88–92%). Look only there and they seem almost the same. On hard math and science (**MATH**, **GPQA**, **AIME**), the gap opens. On AIME, GPT-4o is about **9%**; o1 is about **74%**. Same family. Different strength. The benchmark told you *which part* got better.

Names you will keep meeting (versions change; the job of each line stays familiar):[2](https://blogs.nvidia.com/blog/what-are-foundation-models/)

| When | Family | Used for |
| --- | --- | --- |
| 2018 | **BERT** | Search / language understanding[5](https://arxiv.org/abs/1810.04805) |
| 2020 | **GPT-3** | Few-shot text APIs[6](https://arxiv.org/abs/2005.14165) |
| 2022 | **Stable Diffusion**, **Whisper** | Images; speech-to-text |
| 2022– | **GPT**, **Claude**, **Gemini** | Chat, coding, multimodal assistants |
| 2023– | **Llama**, **Mistral**, **Qwen**, **DeepSeek**… | Open weights you can run / fine-tune[12](https://huggingface.co/models) |
| Also | Code models; **AlphaFold**, **SAM**, … | Coding; science / vision |

None of this means “trust the base blindly.” Models invent facts (**hallucinate**), can carry bias, and cost real energy. If many products share one cracked foundation, they share the crack.[1](https://arxiv.org/abs/2108.07258)

A foundation model is the base layer — not the product.

**Same idea. Different data and tuning. Different strengths. Pick by the job, then adapt.**

---

## Notes

1. Bommasani et al., *On the Opportunities and Risks of Foundation Models* (Stanford CRFM / HAI, 2021). [arXiv](https://arxiv.org/abs/2108.07258) · [CRFM](https://crfm.stanford.edu/report.html)
2. NVIDIA, “What Are Foundation Models?” [Link](https://blogs.nvidia.com/blog/what-are-foundation-models/)
3. IBM Think, “What are foundation models?” [Link](https://www.ibm.com/think/topics/foundation-models)
4. Vaswani et al., *Attention Is All You Need* (2017). [arXiv](https://arxiv.org/abs/1706.03762)
5. Devlin et al., *BERT* (2018). [arXiv](https://arxiv.org/abs/1810.04805)
6. Brown et al., GPT-3 (2020). [arXiv](https://arxiv.org/abs/2005.14165)
7. Stanford AI Index 2024. [Link](https://aiindex.stanford.edu/)
8. Stanford AI Index 2024 — training-cost estimates (with Epoch AI). [Link](https://hai.stanford.edu/ai-index/2024-ai-index-report/research-and-development)
9. IBM Think, diffusion models. [Link](https://www.ibm.com/think/topics/diffusion-models)
10. Lewis et al., RAG (2020). [arXiv](https://arxiv.org/abs/2005.11401)
11. Hu et al., LoRA (2021). [arXiv](https://arxiv.org/abs/2106.09685)
12. Hugging Face model hub. [Link](https://huggingface.co/models)
13. Stanford AI Index 2025 — Technical performance. [Link](https://hai.stanford.edu/ai-index/2025-ai-index-report/technical-performance)
14. LMArena. [Link](https://lmarena.ai/)
15. Artificial Analysis. [Link](https://artificialanalysis.ai/leaderboards/models)
