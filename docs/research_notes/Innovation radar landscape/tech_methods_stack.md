# Technical methods and open-source stack for a Polish innovation radar (state on 2026-11-21)

Scope: semantic search, deduplication, classification, LLM extraction, matching and automatic detection for a Polish-language prototype. The prototype runs on CPU. The code gets the EUPL-1.2 license. All versions and dates below come from checks on 2026-11-21 (PyPI JSON API, GitHub API, Hugging Face API), unless a different date is given.

Method notes:
- The PyPI versions and licenses come from `https://pypi.org/pypi/<package>/json` (checked 2026-11-21).
- The Hugging Face licenses, gating flags and parameter counts come from `https://huggingface.co/api/models/<id>` (checked 2026-11-21).
- The GitHub licenses come from `gh api repos/<owner>/<repo>` (checked 2026-11-21).

## 1. Embedding models for Polish and for Polish-English cross-lingual search

### Takeaway
The best CPU-friendly choice with a clean license is `sdadas/mmlw-retrieval-roberta-large` (435M, Apache-2.0) or `sdadas/mmlw-roberta-base` (124M, Apache-2.0). Use `Qwen/Qwen3-Embedding-0.6B` (Apache-2.0) or `BAAI/bge-m3` (MIT) when long or English text is important. The newer sdadas models (v2, stella-pl-retrieval) have higher scores, but they have the Gemma license. Do not use jina-embeddings-v3 (CC BY-NC 4.0).

### Cited Findings
- PL-MTEB was revised for ACL 2026 Findings (arXiv v2, 24 Apr 2026). It has 30 tasks in 5 categories and evaluates 30 public models. — [PL-MTEB arXiv v2](https://arxiv.org/html/2405.10138); [ACL Anthology 2026.findings-acl.1773](https://aclanthology.org/2026.findings-acl.1773/)
- PL-MTEB v2, Table 2 (average / classification / clustering / pair classification / retrieval / STS): — [PL-MTEB arXiv v2](https://arxiv.org/html/2405.10138)
  - mmlw-roberta-base: 62.52 / 62.53 / 48.00 / 88.16 / 53.60 / 85.20
  - mmlw-roberta-large: 61.58 / 66.15 / 44.58 / 89.15 / 49.91 / 85.23
  - mmlw-retrieval-roberta-large: 63.69 / 63.90 / 45.18 / 88.48 / 57.23 / 84.71
  - mmlw-retrieval-roberta-large-v2: 63.09 / 64.62 / 39.08 / 86.53 / 58.35 / 85.64
  - stella-pl: 64.85 / 66.94 / 38.08 / 89.20 / 60.82 / 86.87
  - stella-pl-retrieval-8k: 64.98 / 68.14 / 35.42 / 89.56 / 61.59 / 86.56
  - multilingual-e5-small / base / large: 55.21 / 56.59 / 59.06 average; retrieval 46.00 / 47.63 / 52.43
  - Qwen3-Embedding-0.6B: 62.20 average; retrieval 48.59; clustering 56.65
  - Qwen3-Embedding-4B: 69.37 average; retrieval 56.65
  - Qwen3-Embedding-8B: 70.47 average (best average); retrieval 59.21
  - BGE-Multilingual-Gemma2: 69.81 average; retrieval 58.93
- The paper says that "there is no single universal model that performs best across all task types". It says that mmlw-roberta-base is strong among small models and mmlw-retrieval-roberta-large has the best average among large models. It also warns that some models had training data similar to the benchmark. — [PL-MTEB arXiv v2](https://arxiv.org/html/2405.10138)
- The PL-MTEB v2 table does not include bge-m3, multilingual-e5-large-instruct, EmbeddingGemma, jina or nomic. — [PL-MTEB arXiv v2](https://arxiv.org/html/2405.10138)
- mmlw-roberta-base: Apache-2.0, about 0.1B parameters, 768 dimensions. Queries need the prefix "zapytanie: ". The model was distilled on about 60 million Polish-English pairs with English BGE teachers. PIRB NDCG@10 is 53.60. — [HF sdadas/mmlw-roberta-base](https://huggingface.co/sdadas/mmlw-roberta-base)
- mmlw-retrieval-roberta-large: Apache-2.0, 0.4B parameters, 1024 dimensions, prefix "zapytanie: ", PIRB NDCG@10 58.46. — [HF sdadas/mmlw-retrieval-roberta-large](https://huggingface.co/sdadas/mmlw-retrieval-roberta-large)
- mmlw-retrieval-roberta-large-v2: license "gemma", 0.4B parameters, 1024 dimensions, prefix "[query]: ", PIRB NDCG@10 60.71. The training used BAAI/bge-reranker-v2.5-gemma2-lightweight to select passages. — [HF sdadas/mmlw-retrieval-roberta-large-v2](https://huggingface.co/sdadas/mmlw-retrieval-roberta-large-v2)
- stella-pl: MIT, about 1.5-2B parameters, 1024 dimensions, `trust_remote_code=True`. The card says that Polish and English embeddings are comparable. PIRB NDCG@10 is 60.52. — [HF sdadas/stella-pl](https://huggingface.co/sdadas/stella-pl)
- stella-pl-retrieval: license "gemma", PIRB NDCG@10 62.32. — [HF sdadas/stella-pl-retrieval](https://huggingface.co/sdadas/stella-pl-retrieval)
- On the Hugging Face API (2026-11-21), all sdadas models from September 2024 or later have the "gemma" license tag. The list is: — [HF API sdadas models](https://huggingface.co/api/models?author=sdadas)
  - stella-pl-retrieval, stella-pl-retrieval-8k, stella-pl-retrieval-mini-8k
  - mmlw-retrieval-roberta-large-v2
  - polish-reranker-roberta-v2, polish-reranker-roberta-v3, polish-reranker-bge-v2
  - The older mmlw-* models, mmlw-retrieval-* models, polish-reranker-*-ranknet/mse models and polish-splade have Apache-2.0. stella-pl has MIT.
- Qwen3-Embedding-0.6B: Apache-2.0, 0.6B parameters, up to 1024 dimensions (MRL from 32 to 1024), 32K context, 100+ languages. Queries use "Instruct: {task}\nQuery: {text}". The MTEB Multilingual mean task score is 64.33 for 0.6B, 69.45 for 4B and 70.58 for 8B. The paper date is 5 Jun 2025. — [HF Qwen/Qwen3-Embedding-0.6B](https://huggingface.co/Qwen/Qwen3-Embedding-0.6B)
- BGE-M3: MIT, 1024 dimensions, 8192 tokens. One pass gives dense, sparse (lexical weights) and multi-vector (ColBERT) outputs. It supports 100+ languages and needs no query instruction. The card recommends hybrid retrieval and a reranker for RAG. — [HF BAAI/bge-m3](https://huggingface.co/BAAI/bge-m3)
- EmbeddingGemma-300m: Gemma license, gated (the user must accept the terms). It has 768 dimensions (MRL 512/256/128) and 2048 tokens. The MTEB Multilingual v2 mean is 61.15 at 768 dimensions. It needs fp32 or bf16, not fp16. — [HF google/embeddinggemma-300m](https://huggingface.co/google/embeddinggemma-300m)
- jina-embeddings-v3 has the CC BY-NC 4.0 license. A paid commercial license exists only for organizations with 1-100 employees or less than USD 5 million revenue. — [HF API jinaai/jina-embeddings-v3](https://huggingface.co/api/models/jinaai/jina-embeddings-v3); [Jina commercial license terms](https://jina.ai/COMMERCIAL-LICENSE-TERMS.pdf)
- Licenses and sizes from the HF API (2026-11-21): — [HF API](https://huggingface.co/api/models/intfloat/multilingual-e5-base)
  - intfloat/multilingual-e5-small / base / large / large-instruct: MIT, 118M / 278M / 560M / 560M parameters
  - nomic-ai/nomic-embed-text-v2-moe: Apache-2.0, 475M
  - Snowflake/snowflake-arctic-embed-m-v2.0 / l-v2.0: Apache-2.0, 305M / 568M
  - Alibaba-NLP/gte-multilingual-base: Apache-2.0, 305M
  - ibm-granite/granite-embedding-278m-multilingual: Apache-2.0
  - minishlab/potion-multilingual-128M (static Model2Vec embeddings): MIT
  - BAAI/bge-multilingual-gemma2: Gemma, 9.2B
  - jinaai/jina-embeddings-v4: no license tag in the API (3.75B)
- The PL-MTEB paper names snowflake-arctic-embed-m-v2.0 as the best average in the "base" size group. — [PL-MTEB arXiv v2](https://arxiv.org/html/2405.10138)
- The sentence-transformers efficiency guide recommends `openvino-qint8` on CPU if a small accuracy loss is acceptable. Otherwise it recommends OpenVINO on Intel CPUs and ONNX on other CPUs. It does not give one universal CPU speedup number in the text. — [SBERT efficiency docs](https://sbert.net/docs/sentence_transformer/usage/efficiency.html)
- sentence-transformers 6.3.0 (2026-11-11) has Apache-2.0. FlagEmbedding has MIT. fastembed 0.9.1 has Apache-2.0. model2vec 0.10.0 has MIT. onnxruntime 1.32.0 has MIT. — [PyPI sentence-transformers](https://pypi.org/project/sentence-transformers/); [GitHub FlagOpen/FlagEmbedding](https://github.com/FlagOpen/FlagEmbedding); [PyPI fastembed](https://pypi.org/project/fastembed/)

### Inferences
- Recommended default: `sdadas/mmlw-retrieval-roberta-large` (Apache-2.0) for the search index. PL-MTEB retrieval is 57.23 and the license is clean. Use `mmlw-roberta-base` (124M) if CPU time is a problem. Its retrieval score is 3.6 points lower, but it is about 3.5 times smaller.
- For cross-lingual Polish-English matching (patents, CORDIS), the mmlw models are a reasonable choice, because the distillation aligns Polish to an English teacher space. No cross-lingual score for mmlw was found (see Gaps). bge-m3 and Qwen3-Embedding-0.6B are documented as cross-lingual and have permissive licenses. They are the safer choice for long English abstracts (8K and 32K tokens). mmlw models are RoBERTa models with a 512-token limit.
- Use the Kaggle T4 to embed the full corpus in batches. The server must embed queries with the same model on CPU. Thus the query model must be small enough for CPU. Qwen3-Embedding-4B/8B give the best PL-MTEB averages, but they are too slow for CPU queries.
- Expected quality: PIRB NDCG@10 of about 53-58 for dense retrieval alone with Apache models. A reranker adds about 5-8 points (see section 2).
- Do not use the Gemma-licensed sdadas v2/v3 models in the EUPL deliverable. Their quality gain is only 1-4 points, and the Gemma terms add use restrictions (see section 10).

### Gaps
- No Polish PL-MTEB or PIRB score for bge-m3, EmbeddingGemma or nomic v2 was found in a primary source. The PIRB leaderboard (https://huggingface.co/spaces/sdadas/pirb) is a Space and was not read.
- No measured CPU throughput (texts per second) for these models on a typical laptop was found. Measure it on the team machine.
- The license of jina-embeddings-v4 was not confirmed (no tag in the HF API).

## 2. Rerankers for Polish

### Takeaway
Use `sdadas/polish-reranker-large-ranknet` (Apache-2.0, 435M, 512 tokens) or `BAAI/bge-reranker-v2-m3` (Apache-2.0, 568M, 8192 tokens). `sdadas/polish-reranker-roberta-v3` is the best Polish reranker, but it has the Gemma license.

### Cited Findings
- polish-reranker-roberta-v3: license "gemma", 443M parameters, 8192 tokens, base PKOBP/polish-roberta-8k. The teacher is bge-reranker-v2.5-gemma2-lightweight. The training data includes about 40,000 public administration passages from Polish government and municipal web pages. PIRB NDCG@10 is 66.21 with the bge-multilingual-gemma2 retriever. — [HF sdadas/polish-reranker-roberta-v3](https://huggingface.co/sdadas/polish-reranker-roberta-v3)
- The v3 card gives NDCG@10 on public administration datasets (opi-urzedowe / ezd-qa / ezd-ir-chunked, bge-m3 retriever): — [HF sdadas/polish-reranker-roberta-v3](https://huggingface.co/sdadas/polish-reranker-roberta-v3)
  - polish-reranker-large-ranknet (435M, 512 tokens): 85.0 / 78.2 / 79.9
  - polish-reranker-roberta-v2 (435M, 512 tokens): 86.0 / 78.2 / 83.0
  - Qwen3-Reranker-4B: 77.4 / 72.4 / 80.6
  - Qwen3-Reranker-8B: 84.1 / 79.9 / 84.8
  - bge-reranker-v2-m3 (568M, 8192 tokens): 78.8 / 74.3 / 85.4
  - polish-reranker-roberta-v3 (443M, 8192 tokens): 86.3 / 80.1 / 86.7
- polish-reranker-roberta-v2: license "gemma", PIRB NDCG@10 65.30. — [HF sdadas/polish-reranker-roberta-v2](https://huggingface.co/sdadas/polish-reranker-roberta-v2)
- polish-reranker-large-ranknet, polish-reranker-large-mse and the base variants have Apache-2.0. — [HF API sdadas models](https://huggingface.co/api/models?author=sdadas)
- bge-reranker-v2-m3 has Apache-2.0 (568M). Qwen3-Reranker-0.6B has Apache-2.0 (596M). — [HF API BAAI/bge-reranker-v2-m3](https://huggingface.co/api/models/BAAI/bge-reranker-v2-m3); [HF API Qwen/Qwen3-Reranker-0.6B](https://huggingface.co/api/models/Qwen/Qwen3-Reranker-0.6B)
- A 2024 study evaluated Polish and multilingual rerankers on PIRB. It reports that the best sdadas model beats models with up to 30 times more parameters. — [Dadas & Grębowiec, arXiv 2402.14318](https://arxiv.org/html/2402.14318v1)
- The PIRB paper reports that sparse-dense hybrid retrievers with a light rescoring model improve on dense models alone. — [PIRB, arXiv 2402.13350](https://arxiv.org/abs/2402.13350)

### Inferences
- For short records (title + problem + solution, fewer than 512 tokens), polish-reranker-large-ranknet is the best Apache-licensed option. For long English patent text, use bge-reranker-v2-m3.
- On CPU, rerank only the top 20-50 candidates. The latency is probably 1-3 seconds per query for a 435M cross-encoder (estimate, not measured).

### Gaps
- No PIRB score for polish-reranker-large-ranknet in the same table as v3 was read. The PIRB leaderboard was not read.

## 3. LLMs for extraction, classification and summary (Polish and multilingual)

### Takeaway
Use Bielik-11B-v3.x-Instruct (Apache-2.0) through Ollama/llama.cpp GGUF Q4_K_M (6.7 GB) for the best Polish quality. Use Bielik-4.5B-v3.0, PLLuM-4B-2512 or Qwen3/3.5-4B (all Apache-2.0) if CPU speed is the limit. Do not use the PLLuM "-nc" models (non-commercial) or the Llama-PLLuM models (Llama 3.1 license) in the EUPL deliverable.

### Cited Findings
- Bielik-11B-v3.0-Instruct: Apache-2.0, 11B. The base is Bielik-11B-v3-Base-20250730 (SpeakLeash and ACK Cyfronet AGH). It covers 32 European languages and uses the ChatML format. The HF repository is gated. — [HF speakleash/bielik-11b-v3.0-instruct](https://huggingface.co/speakleash/bielik-11b-v3.0-instruct)
- The Bielik 11B v3 technical report (arXiv 2601.11579, 30 Dec 2025) describes a depth up-scaled Mistral 7B v0.2. The native context is 32,768 tokens, and YaRN extends it to 131,072 tokens. — [arXiv 2601.11579](https://arxiv.org/abs/2601.11579); [PDF](https://arxiv.org/pdf/2601.11579v1)
- Open PL LLM Leaderboard, instruction-tuned models, 5-shot average (self-reported in the Bielik v3 report, Table 5): — [arXiv 2601.11579 PDF](https://arxiv.org/pdf/2601.11579v1)
  - Mistral-Large-Instruct-2411: 69.84
  - Llama-3.3-70B-Instruct: 66.40
  - Bielik-11B-v3.0-Instruct: 65.93
  - Bielik-11B-v2.3-Instruct: 65.71
  - Qwen3-32B: 64.24
  - Mistral-Small-24B-Instruct-2501: 62.97
  - Qwen3-14B: 62.24
  - gemma-3-12b-it: 62.20
  - Bielik-4.5B-v3.0-Instruct: 56.13
  - Qwen3-8B: 55.78
  - Qwen3-4B: 55.49
  - Mistral-Nemo-Instruct-2407: 55.27
  - Qwen2.5-7B-Instruct: 54.93
  - EuroLLM-9B-Instruct: 50.07
- PLCC (Polish linguistic and cultural competency), Table 9 of the same report: — [arXiv 2601.11579 PDF](https://arxiv.org/pdf/2601.11579v1)
  - Bielik-11B-v3.0-Instruct: 71.83%
  - PLLuM-12B-nc-chat-250715 (non-commercial): 69.67%
  - Bielik-11B-v2.6-Instruct: 65.50%
  - Gemma-3-27B: 47.33%
  - PLLuM-12B-chat: 47.00%
  - Mistral-Small-3.1-24B-2503: 43.33%
- CPTUB (complex Polish text understanding, scale 0-5), Table 7: Bielik-11B-v3.0 3.73; Mistral-Small-24B-2501 3.71; pllum-12b-nc-chat-250715 3.67; Bielik-4.5B-v3.0 3.38; PLLuM-12B-chat 3.14; Qwen2.5-7B 3.07. — [arXiv 2601.11579 PDF](https://arxiv.org/pdf/2601.11579v1)
- A third-party aggregator shows the same Open PL LLM Leaderboard values (snapshot about May 2026). It notes small inconsistencies. — [Codesota Open PL LLM Leaderboard](https://www.codesota.com/benchmark/open-pl-llm-leaderboard)
- Bielik-11B-v3.1-Instruct: Apache-2.0, 56 languages (the metadata says 58). It has a `reasoning_effort` control and tool calls through the speakleash/bielik-tools parsers (vLLM 0.24 or newer). The card says that the argument schema of a tool call is not enforced and that `response_format` does not reach the chat template. — [HF speakleash/Bielik-11B-v3.1-Instruct](https://huggingface.co/speakleash/Bielik-11B-v3.1-Instruct)
- Ollama has `SpeakLeash/bielik-11b-v3.1-instruct` with these tags: Q4_K_M 6.7 GB, Q5_K_M 7.9 GB, Q6_K 9.2 GB, Q8_0 about 11-12 GB, bf16 22 GB. All tags have a 32K context. — [Ollama SpeakLeash/bielik-11b-v3.1-instruct](https://ollama.com/SpeakLeash/bielik-11b-v3.1-instruct)
- The GGUF repositories `speakleash/Bielik-11B-v3.0-Instruct-GGUF` (created 2025-12-30) and `speakleash/Bielik-11B-v3.1-Instruct-GGUF` (created 2026-11-10) have Apache-2.0 and are not gated. The base HF repositories have `gated=auto`. — [HF API speakleash](https://huggingface.co/api/models?author=speakleash)
- New SpeakLeash GGUF repositories appeared on 2026-11-20: `Bielik-PL-11B-v3.0-Instruct-GGUF` and `Bielik-PL-Minitron-7B-v3.0-Instruct-GGUF` (Apache-2.0). An April 2026 paper describes tokenizer optimization in a Bielik v3 7B and 11B series. — [HF API speakleash](https://huggingface.co/api/models?author=speakleash); [arXiv 2604.10799](https://arxiv.org/html/2604.10799v1)
- Bielik-4.5B-v3.0-Instruct: Apache-2.0, gated, 4.6B parameters, ChatML. Bielik-1.5B-v3.0-Instruct: Apache-2.0. — [HF speakleash/Bielik-4.5B-v3.0-Instruct](https://huggingface.co/speakleash/Bielik-4.5B-v3.0-Instruct); [HF API Bielik-1.5B](https://huggingface.co/api/models/speakleash/Bielik-1.5B-v3.0-Instruct)
- PLLuM licenses (model card table for release 2412): — [HF CYFRAGOVPL/PLLuM-12B-chat](https://huggingface.co/CYFRAGOVPL/PLLuM-12B-chat)
  - Llama-PLLuM-8B and Llama-PLLuM-70B: Llama 3.1 license
  - PLLuM-12B (Mistral-Nemo base) and PLLuM-8x7B (Mixtral base): Apache-2.0
  - PLLuM-12B-nc and PLLuM-8x7B-nc: CC-BY-NC-4.0 (non-commercial)
- The newest PLLuM release is 2512 (January-February 2026). PLLuM-4B-base/instruct/chat-2512 and PLLuM-12B-base/instruct/chat-2512 have Apache-2.0. Llama-PLLuM-8B-2512 and Llama-PLLuM-70B-2512 have llama3.1. The pllum-12b-nc-*-2507 models have "other". — [HF API CYFRAGOVPL](https://huggingface.co/api/models?author=CYFRAGOVPL)
- The Llama-PLLuM chat and instruct models were trained for RAG. The card gives a Polish RAG prompt with numbered citations such as [0]. — [HF CYFRAGOVPL/PLLuM-12B-chat](https://huggingface.co/CYFRAGOVPL/PLLuM-12B-chat)
- Gemma 4 (released 2 Apr 2026: E2B, E4B, 26B MoE, 31B dense) has Apache-2.0. It is the first Gemma under an OSI license. — [Google Open Source Blog](https://opensource.googleblog.com/2026/03/gemma-4-expanding-the-gemmaverse-with-apache-20.html); [HF API google/gemma-4-E4B-it](https://huggingface.co/api/models/google/gemma-4-E4B-it)
- Gemma 3 (for example gemma-3-12b-it) still has the Gemma license and manual gating. — [HF API google/gemma-3-12b-it](https://huggingface.co/api/models/google/gemma-3-12b-it)
- Qwen3.5-4B and Qwen3.5-9B (created 2026-02-27), Qwen3-4B-Instruct-2507 and Qwen3-8B have Apache-2.0. Qwen3.6 (35B-A3B, 27B, April 2026) has Apache-2.0. Qwen3.6-Max is closed. — [HF API Qwen/Qwen3.5-4B](https://huggingface.co/api/models/Qwen/Qwen3.5-4B); [HF unsloth/Qwen3.6-35B-A3B-GGUF](https://huggingface.co/unsloth/Qwen3.6-35B-A3B-GGUF)
- Mistral-Small-3.2-24B-Instruct-2506 and Ministral-3-8B-Instruct-2512 have Apache-2.0. Ministral-8B-Instruct-2410 has "other" (Mistral research license). EuroLLM-9B-Instruct has Apache-2.0. — [HF API mistralai](https://huggingface.co/api/models/mistralai/Ministral-3-8B-Instruct-2512); [HF API utter-project/EuroLLM-9B-Instruct](https://huggingface.co/api/models/utter-project/EuroLLM-9B-Instruct)
- Llama 3.1 license, main clauses: — [Llama 3.1 Community License](https://dev.meta.ai/llama/llama3_1/license)
  - A distributor must give a copy of the agreement and "prominently display 'Built with Llama'".
  - A distributed model trained on Llama outputs must have a name that starts with "Llama".
  - Products with more than 700 million monthly active users need a separate license.
  - The Acceptable Use Policy is part of the agreement.
  - California law governs the agreement.
- The OSI says that the Llama licenses are not open source. — [OSI blog](https://opensource.org/blog/metas-llama-2-license-is-not-open-source)
- CPU speed: one source gives about 14 tokens/s for Llama 3.1 8B Q4_K_M with 12 threads on an AMD EPYC. Another page gives about 26 tokens/s for Qwen3-8B Q4_K_M on a 16-thread Ryzen 9, but it labels the figure as "expected output". Both sources are weak. — [markaicode Ollama CPU benchmark](https://markaicode.com/benchmarks/tool-cpu-benchmark/); [markaicode llama.cpp benchmark](https://markaicode.com/benchmarks/llamacpp-tokens-per-second-benchmark/)
- Hosted EU options:
  - Scaleway Generative APIs: serverless endpoints in Paris data centers, GDPR, OpenAI-compatible. The catalog includes Gemma-4-26b-a4b, Qwen3.6-35b-a3b and Mistral-medium-3.5. Dedicated deployment can serve any Hugging Face model. No Bielik endpoint was found. — [Scaleway Generative APIs](https://www.scaleway.com/en/generative-apis/); [Scaleway model catalog](https://www.scaleway.com/en/docs/generative-apis/reference-content/model-catalog/)
  - Mistral La Plateforme: the help center says that API inputs and outputs are not used for training. Zero data retention is available on paid plans. Labs and Preview models are an exception. EU hosting by default comes only from secondary sources. — [Mistral help center](https://help.mistral.ai/en/articles/323757-do-you-use-my-user-data-to-train-your-artificial-intelligence-models); [Mistral ZDR docs](https://docs.mistral.ai/admin/monitor-comply/zero-data-retention); [Mistral DPA](https://legal.mistral.ai/terms/data-processing-addendum)
  - NVIDIA build.nvidia.com hosts Bielik 11B v2.3 and v2.6 endpoints (a US service). — [build.nvidia.com Bielik v2.6](https://build.nvidia.com/speakleash/bielik-11b-v2_6-instruct/modelcard)

### Inferences
- Bielik 11B v3.x is the best Polish open model below 20B parameters with a clean Apache-2.0 license, based on the self-reported benchmarks. The benchmarks do not include Gemma 4, Qwen3.5/3.6 or Ministral 3. Test 2-3 models on 20 real records before the final choice.
- CPU latency estimate: 11B Q4_K_M generation is limited by memory bandwidth. Expect about 3-8 tokens/s on a laptop. A JSON record of about 300 output tokens can thus take 40-100 seconds. A 4B model is about 2-3 times faster. For a live demo, extract records in advance (batch on Kaggle T4 with vLLM or llama.cpp), and run the CPU LLM only for one new submission.
- Use the GGUF repositories, because they are not gated. Then the server can download the model without a Hugging Face account.
- Avoid the PLLuM-nc models in spite of the high PLCC score (69.67%). CC-BY-NC is not safe for a public deployment or for the EUPL deliverable.

### Gaps
- No independent (not self-reported) Polish benchmark of Gemma 4, Qwen3.5/3.6, Ministral 3 or PLLuM-4B-2512 was found.
- No reliable measured CPU tokens/s for Bielik 11B Q4_K_M on a laptop was found. Measure it with `llama-bench`.
- The Scaleway data retention FAQ returned HTTP 403. Its exact retention terms were not read.
- The official Open PL LLM Leaderboard page (SpeakLeash) was not read directly.

## 4. Search and storage, hybrid search, Polish lemmatization

### Takeaway
For a 20-hour prototype, use one SQLite file with FTS5 (BM25) and sqlite-vec, and join the results with Reciprocal Rank Fusion. Lemmatize Polish text before FTS5 indexing with simplemma (MIT), Morfeusz 2 (BSD-2) or pystempel (Apache-style). Do not use Elasticsearch (AGPL/SSPL/ELv2) or the spaCy Polish models (GPL-3.0).

### Cited Findings
- sqlite-vec 0.1.9 (2026-03-31) has MIT OR Apache-2.0. — [PyPI sqlite-vec](https://pypi.org/project/sqlite-vec/); [GitHub asg017/sqlite-vec](https://github.com/asg017/sqlite-vec)
- An SQL pattern for hybrid search uses one CTE for sqlite-vec KNN, one CTE for FTS5 and a final RRF CTE. RRF ranks by position and does not compare raw scores. — [Simon Willison, Oct 2024](https://simonwillison.net/2024/Oct/4/hybrid-full-text-search-and-vector-search-with-sqlite/)
- RRF with k=60 is a common default. — [Google Spanner hybrid search docs](https://docs.cloud.google.com/spanner/docs/hybrid-full-text-vector-search)
- Other stores and their licenses (checked 2026-11-21):
  - pgvector extension: PostgreSQL License (GitHub LICENSE file); pgvector Python client 0.5.1: MIT — [GitHub pgvector](https://github.com/pgvector/pgvector); [PyPI pgvector](https://pypi.org/project/pgvector/)
  - Qdrant server: Apache-2.0; qdrant-client 1.20.0: Apache-2.0 — [GitHub qdrant](https://github.com/qdrant/qdrant); [PyPI qdrant-client](https://pypi.org/project/qdrant-client/)
  - faiss-cpu 1.16.0: MIT — [PyPI faiss-cpu](https://pypi.org/project/faiss-cpu/)
  - lancedb 0.41.0: Apache-2.0 — [PyPI lancedb](https://pypi.org/project/lancedb/)
  - chromadb 1.5.9: Apache-2.0 — [PyPI chromadb](https://pypi.org/project/chromadb/)
  - duckdb 2.0.1: MIT; usearch 2.26.6: Apache-2.0; tantivy (Python) 0.27.0: MIT; bm25s 0.3.16: MIT; rank-bm25 0.2.2: Apache-2.0 (last release 2022) — [PyPI duckdb](https://pypi.org/project/duckdb/); [PyPI bm25s](https://pypi.org/project/bm25s/)
  - Meilisearch: MIT AND OpenLDAP AND BUSL-1.1 (the Enterprise Edition parts have BUSL) — [GitHub meilisearch LICENSE](https://github.com/meilisearch/meilisearch)
  - Typesense: GPL-3.0 — [GitHub typesense](https://github.com/typesense/typesense)
- Elastic added AGPLv3 as an option next to SSPL 1.0 and ELv2 (announced 29 Aug / 3 Sep 2024). OpenSearch stays Apache-2.0 and moved to the Linux Foundation in September 2024. — [Elastic press release](https://businesswire.com/news/home/20240829537786/en); [InfoQ](https://www.infoq.com/news/2024/09/elastic-open-source-agpl); [GitHub OpenSearch](https://github.com/opensearch-project/OpenSearch)
- BGE-M3 gives sparse lexical weights in the same pass as the dense vector. This can replace BM25 for hybrid search. — [HF BAAI/bge-m3](https://huggingface.co/BAAI/bge-m3)
- The PIRB paper reports that hybrid sparse-dense retrieval improves on dense retrieval for Polish. — [PIRB arXiv 2402.13350](https://arxiv.org/abs/2402.13350)
- Morfeusz 2: the program and the inflection data for morphological analysis (SGJP, Polimorf) have the 2-clause BSD license. The full SGJP dictionary is not under BSD. morfeusz2 1.99.15 was released 2026-06-01. — [Morfeusz license](http://morfeusz.sgjp.pl/doc/license/); [PyPI morfeusz2](https://pypi.org/project/morfeusz2/)
- pystempel 2.0.0 (2024-07-10): most code has the Egothor Open Source License (Apache-style), the rest has Apache-2.0. The default stemming table has Apache-2.0. The Polimorf table has BSD-2. With the default table, different forms of one lemma give different stems 63% of the time. With the Polimorf table, the value is 13%. — [PyPI pystempel](https://pypi.org/project/pystempel/)
- simplemma 2.0.0 (2026-08-12) has MIT. — [PyPI simplemma](https://pypi.org/project/simplemma/)
- spaCy pl_core_news_md 3.8.0 has the "GNU GPL 3.0" license. It was trained on UD Polish PDB and NKJP. Lemma accuracy is 93.71, POS accuracy 97.66, NER F 82.92, size 47 MB. The spaCy library 3.8.18 itself has MIT. — [spaCy models release pl_core_news_md-3.8.0](https://github.com/explosion/spacy-models/releases/tag/pl_core_news_md-3.8.0); [PyPI spacy](https://pypi.org/project/spacy/)
- Stanza 1.15.1 (2026-11-14) code has Apache-2.0. — [GitHub stanza LICENSE](https://github.com/stanfordnlp/stanza); [PyPI stanza](https://pypi.org/project/stanza/)

### Inferences
- SQLite + FTS5 + sqlite-vec needs no server and keeps all data in one file. A corpus of 10,000-100,000 records fits easily. Brute-force KNN in sqlite-vec is fast enough at this size.
- Qdrant is a good second choice if the team wants built-in sparse+dense hybrid search and payload filters. It needs a Docker container.
- GPL-3.0 is in the EUPL appendix. Thus the spaCy Polish model is not forbidden. But it can force GPL-3.0 on a combined work and complicates the transfer to the public institution. Morfeusz 2 (BSD-2) or simplemma (MIT) avoid this problem.
- Index both the lemmatized text (FTS5) and the raw text (for display and for quote checks).

### Gaps
- The licenses of the Stanza Polish models were not checked. They depend on the UD treebank licenses.
- No Polish lemmatization accuracy for simplemma was found.

## 5. Deduplication, entity resolution, topic clustering and knowledge graph

### Takeaway
Use a two-level duplicate check. MinHash LSH (datasketch, MIT) finds copied text (Jaccard about 0.8). Embedding cosine finds the same idea in different words (threshold about 0.85-0.92, calibrate on own data). Match institutions with rapidfuzz plus the ROR dump (CC0). Use BERTopic (MIT) for topic clusters and NetworkX (BSD-3) for a simple graph.

### Cited Findings
- datasketch 2.0.0 (2026-07-05): MIT. — [PyPI datasketch](https://pypi.org/project/datasketch/)
- MinHash with a Jaccard threshold of 0.8 is common in practice:
  - A 2025 paper uses 112 hashes, 14 bands of 8 rows, a 0.8 threshold and Union-Find clustering. — [arXiv 2512.18834](https://arxiv.org/pdf/2512.18834)
  - Lee et al. (2021) verify candidates with exact Jaccard above 0.8. — [arXiv 2107.06499](https://arxiv.org/pdf/2107.06499)
- NeMo Curator SemDeDup marks pairs as duplicates when the cosine similarity is at least 1 - eps. The text examples use eps = 0.07 (cosine 0.93). The docs tell the user to examine the similarity distribution before the choice of eps. — [NeMo Curator SemDeDup](https://docs.nvidia.com/nemo/curator/main/curate-text/process-data/deduplication/semdedup.md); [NeMo Curator dedup](https://docs.nvidia.com/nemo/curator/latest/curate-video/process-data/dedup)
- Entity resolution libraries: rapidfuzz 3.14.6 MIT; splink 5.0.0 MIT; dedupe 3.0.3 MIT; recordlinkage 0.16 BSD-3 (last release 2023). — [PyPI rapidfuzz](https://pypi.org/project/rapidfuzz/); [PyPI splink](https://pypi.org/project/splink/); [PyPI dedupe](https://pypi.org/project/dedupe/); [PyPI recordlinkage](https://pypi.org/project/recordlinkage/)
- ROR data has CC0 1.0. ROR code has MIT. ROR location data comes from GeoNames and has CC BY 4.0. The `?affiliation=` API parameter returns the most probable match with `chosen: true`. A new single-search strategy was announced on 2025-12-02. The full dump is on Zenodo (JSON and CSV). The v1 API was deprecated on 8 Dec 2025. — [ROR about](https://ror.org/about/); [ROR matching docs](https://ror.readme.io/v2/docs/matching); [ROR blog 2025-12-02](https://ror.org/blog/2025-12-02-announcing-a-new-affiliation-matching-strategy/); [ROR dump on Zenodo](https://zenodo.org/records/18500703)
- ROR suggests local matching against the dump, or a local ROR API in Docker. — [ROR matching docs](https://ror.readme.io/v2/docs/matching)
- Clustering: BERTopic 0.17.4 MIT; hdbscan 0.8.44 BSD; umap-learn 0.6.0 BSD; scikit-learn 1.9.1 BSD-3. — [PyPI bertopic](https://pypi.org/project/bertopic/); [PyPI hdbscan](https://pypi.org/project/hdbscan/); [PyPI umap-learn](https://pypi.org/project/umap-learn/); [PyPI scikit-learn](https://pypi.org/project/scikit-learn/)
- Graph options:
  - NetworkX 3.7: BSD-3 — [PyPI networkx](https://pypi.org/project/networkx/)
  - Neo4j (Community repository): GPL-3.0 — [GitHub neo4j](https://github.com/neo4j/neo4j)
  - Kuzu: MIT, but the repository was archived on 10 Oct 2025 (last release 0.11.3). The forks are LadybugDB (MIT, active on 2026-11-21) and RyuGraph. — [The Register](https://www.theregister.com/2025/10/14/kuzudb_abandoned/); [GitHub kuzudb/kuzu](https://github.com/kuzudb/kuzu); [GitHub LadybugDB/ladybug](https://github.com/LadybugDB/ladybug)
  - RDF: rdflib 7.6.0 BSD-3; pyoxigraph 0.5.13 MIT OR Apache-2.0 — [PyPI rdflib](https://pypi.org/project/rdflib/); [PyPI pyoxigraph](https://pypi.org/project/pyoxigraph/)

### Inferences
- The duplicate check is a main feature of the task ("reduce duplicate work"). Show the similar records with the similarity score and the shared key terms. Let the user mark "duplicate / related / different". These labels then calibrate the threshold.
- For people and institutions: normalize names (lowercase, remove "Sp. z o.o.", diacritics folding only for matching), block by city or region, then compare with rapidfuzz `token_set_ratio`. Use ROR only for universities and research institutes. Most schools and local governments are not in ROR.
- A graph database is not necessary in 20 hours. Store the edges (creator - institution - innovation - sector) in SQLite and build a NetworkX graph in memory for the view. Do not use Kuzu, because the project is archived.

### Gaps
- No published cosine threshold for Polish near-duplicates with the mmlw models was found. Calibrate it on a small labeled set.
- REGON (GUS BIR API) and KRS API terms were not checked in this note. Other notes cover the data sources.

## 6. Matching and recommendation (innovation to need, partner or investor)

### Takeaway
Use the same retrieval stack in two directions: innovation to needs, and needs to innovations. Score each pair with a transparent weighted sum: semantic similarity, shared sector and TRL fit, shared region, and shared key terms. Show each component next to the result.

### Cited Findings
- BGE-M3 gives lexical token weights in the same pass as the dense vector. Thus the shared high-weight tokens can explain a match. — [HF BAAI/bge-m3](https://huggingface.co/BAAI/bge-m3)
- RRF combines rankings by position. It does not need calibrated scores from the different retrievers. — [Simon Willison, Oct 2024](https://simonwillison.net/2024/Oct/4/hybrid-full-text-search-and-vector-search-with-sqlite/)
- A cross-encoder reranker gives a better relevance order than the bi-encoder alone (see section 2). — [HF sdadas/polish-reranker-roberta-v3](https://huggingface.co/sdadas/polish-reranker-roberta-v3)

### Inferences
- Explainable match score (proposal): `score = 0.5 * cosine + 0.2 * sector match + 0.15 * TRL fit + 0.15 * region match`. Show the top 3-5 shared lemmas from FTS5 or the BGE-M3 sparse weights. Keep the weights in one module with a short reason for each.
- Two-sided matching (a stable-matching algorithm) is not necessary for a prototype. A ranked list for each side with the explanation is enough for the jury.
- Use the LLM only to write a short reason ("why this partner"), and give it the computed fields as input. Do not let the LLM compute the score.

### Gaps
- No Polish evaluation of explainable matching methods was found. The design above is an engineering proposal.

## 7. Automatic detection pipeline (crawl, clean, detect language, classify, detect changes)

### Takeaway
Use httpx (BSD-3) or Scrapy (BSD-3) with protego for robots.txt, trafilatura (Apache-2.0) for main text, htmldate and courlan for dates and URLs, feedparser for RSS, lingua or py3langid for language. Then use a two-step "is this an innovation?" filter: a cheap embedding or NLI score, and then an LLM check for the top candidates.

### Cited Findings
- Library versions and licenses (checked 2026-11-21):
  - scrapy 2.20.0: BSD-3; httpx 0.28.1: BSD-3; protego 0.7.0 (robots.txt parser): BSD-3 — [PyPI scrapy](https://pypi.org/project/scrapy/); [PyPI httpx](https://pypi.org/project/httpx/); [PyPI protego](https://pypi.org/project/protego/)
  - trafilatura 2.3.1: Apache-2.0; courlan 1.5.0: Apache-2.0; htmldate 1.11.0: Apache-2.0 — [PyPI trafilatura](https://pypi.org/project/trafilatura/); [GitHub adbar/trafilatura](https://github.com/adbar/trafilatura); [PyPI courlan](https://pypi.org/project/courlan/); [PyPI htmldate](https://pypi.org/project/htmldate/)
  - readability-lxml 0.9: Apache-2.0; justext 3.0.2: BSD-2 — [PyPI readability-lxml](https://pypi.org/project/readability-lxml/); [PyPI justext](https://pypi.org/project/justext/)
  - feedparser 6.0.14: BSD-2 — [PyPI feedparser](https://pypi.org/project/feedparser/)
  - lingua-language-detector 2.2.0: Apache-2.0; py3langid 0.4.0: BSD-3; langdetect 1.0.9: last release 2021 — [PyPI lingua-language-detector](https://pypi.org/project/lingua-language-detector/); [PyPI py3langid](https://pypi.org/project/py3langid/); [PyPI langdetect](https://pypi.org/project/langdetect/)
  - PDF: pypdf 7.1.1 BSD-3; docling 2.151.1 MIT; markitdown 0.1.9 MIT; PyMuPDF 2.1: AGPL-3.0 or commercial — [PyPI pypdf](https://pypi.org/project/pypdf/); [PyPI docling](https://pypi.org/project/docling/); [PyPI pymupdf](https://pypi.org/project/pymupdf/)
  - apscheduler 3.11.3 (schedule for re-crawl): MIT — [PyPI apscheduler](https://pypi.org/project/apscheduler/)
- Trafilatura evaluation (2026-10-02, 990 documents): — [trafilatura evaluation](https://trafilatura.readthedocs.io/en/latest/evaluation.html)
  - trafilatura 2.3.0 standard: F1 0.926 (precision 0.906, recall 0.946)
  - justext 3.0.2: F1 0.862
  - readability-lxml 0.9: F1 0.853
  - goose3: F1 0.810; newspaper4k: F1 0.803
- Zero-shot classification: `MoritzLaurer/mDeBERTa-v3-base-xnli-multilingual-nli-2mil7` has MIT and lists Polish in its training languages (279M parameters). — [HF mDeBERTa-v3-base-xnli-multilingual-nli-2mil7](https://huggingface.co/MoritzLaurer/mDeBERTa-v3-base-xnli-multilingual-nli-2mil7)
- polish-reranker-roberta-v3 was trained with Polish government and municipal web pages. This shows that the domain (BIP, municipal pages) is relevant for Polish retrieval models. — [HF sdadas/polish-reranker-roberta-v3](https://huggingface.co/sdadas/polish-reranker-roberta-v3)

### Inferences
- Change detection: store a SHA-256 hash of the extracted main text and the `ETag` / `Last-Modified` headers for each URL. Use conditional GET. Process a page again only if the hash changes.
- Rate limit: one request per second for each domain, a clear User-Agent with a contact URL, and robots.txt checks with protego. Scrapy has AutoThrottle and robots.txt support. For a small list of sources, httpx with asyncio is simpler.
- "Is this an innovation?" filter, three steps:
  1. Keep only Polish or English text with a minimum length.
  2. Compute a cheap score: cosine to 10-20 prototype descriptions of innovations, or the NLI score for the label "opis nowego rozwiązania / innowacji".
  3. Send only the top candidates to the LLM with a JSON schema that has `is_innovation`, `confidence` and an evidence quote.
- Do not use PyMuPDF (AGPL). pypdf or docling are enough.

### Gaps
- No Polish accuracy figure for the mDeBERTa NLI zero-shot classifier was found.
- The license of the fastText language identification model was not checked. lingua and py3langid avoid this question.

## 8. LLM structured output, guardrails against hallucination, evaluation

### Takeaway
Define the record as a Pydantic model. Use constrained decoding with the JSON schema (Ollama `format`, or the llama.cpp JSON schema grammar). Require an evidence quote for each field, and check in code that each quote is a substring of the source text. Evaluate on 20-50 hand-labeled records.

### Cited Findings
- The Ollama `format` field accepts a JSON schema. The Python example passes `Model.model_json_schema()` and validates the reply with `model_validate_json`. The OpenAI-compatible endpoint uses `response_format` with type `json_schema`. The docs recommend temperature 0. Ollama Cloud does not support structured outputs. — [Ollama structured outputs docs](https://docs.ollama.com/capabilities/structured-outputs); [Ollama blog](https://ollama.com/blog/structured-outputs)
- Library licenses (checked 2026-11-21):
  - pydantic 2.14.4: MIT — [PyPI pydantic](https://pypi.org/project/pydantic/)
  - instructor 1.17.0: MIT — [PyPI instructor](https://pypi.org/project/instructor/)
  - outlines 1.3.3: Apache-2.0 — [PyPI outlines](https://pypi.org/project/outlines/)
  - xgrammar 0.2.9: Apache-2.0; lm-format-enforcer 0.11.3: MIT — [PyPI xgrammar](https://pypi.org/project/xgrammar/); [PyPI lm-format-enforcer](https://pypi.org/project/lm-format-enforcer/)
  - ollama (server): MIT; ollama Python 0.6.3: MIT — [GitHub ollama](https://github.com/ollama/ollama); [PyPI ollama](https://pypi.org/project/ollama/)
  - llama.cpp: MIT; llama-cpp-python 0.3.37: MIT — [GitHub llama.cpp](https://github.com/ggml-org/llama.cpp); [PyPI llama-cpp-python](https://pypi.org/project/llama-cpp-python/)
  - vLLM: Apache-2.0 — [GitHub vllm](https://github.com/vllm-project/vllm)
  - openai Python 3.57.0: Apache-2.0 (client for any OpenAI-compatible endpoint) — [PyPI openai](https://pypi.org/project/openai/)
- Bielik 11B v3.1 does not enforce the tool-call argument schema in vLLM, and `response_format` does not reach its chat template. The card says to treat tool arguments as untrusted input. — [HF speakleash/Bielik-11B-v3.1-Instruct](https://huggingface.co/speakleash/Bielik-11B-v3.1-Instruct)
- The Bielik model cards say that the models have no moderation and can give incorrect facts. — [HF speakleash/bielik-11b-v3.0-instruct](https://huggingface.co/speakleash/bielik-11b-v3.0-instruct)
- The PLLuM RAG prompt asks for numbered citations and a fixed refusal text when the documents have no answer. — [HF CYFRAGOVPL/PLLuM-12B-chat](https://huggingface.co/CYFRAGOVPL/PLLuM-12B-chat)

### Inferences
- Schema proposal: for each field (problem, solution, type, sector, TRL, creators, institution, region) store `value`, `evidence` (a short quote from the source) and `confidence`. Set the field to `null` if the quote check fails. This makes the "transparent" requirement of the task visible to the jury.
- Use closed lists (enum) for type of innovation, sector and region (16 voivodeships). Constrained decoding then cannot invent a new category. Compute TRL with a rule table from keywords and the LLM answer, not only from the LLM.
- Evaluation: label 30-50 records by hand. Report field-level exact match for enum fields, a token-F1 for free-text fields, and the rate of failed quote checks. For search, report recall@10 on 20 queries with known answers.
- Use Ollama `format` directly. instructor adds retries and validation, but it is one more dependency.

### Gaps
- No published accuracy of Polish LLMs on structured extraction (JSON) tasks was found.

## 9. Web layer

### Takeaway
Use FastAPI with Jinja2 templates, htmx and Alpine.js from static files, Leaflet with OSM tiles for the map, and Chart.js for charts. This stack needs no build step. Streamlit and Gradio are faster for a demo, but they give less control over the user interface.

### Cited Findings
- fastapi 0.165.0 (2026-11-18): MIT; uvicorn 0.54.0: BSD-3; jinja2 3.1.6: BSD; sqlmodel 0.0.50: MIT. — [PyPI fastapi](https://pypi.org/project/fastapi/); [PyPI uvicorn](https://pypi.org/project/uvicorn/); [PyPI jinja2](https://pypi.org/project/jinja2/); [PyPI sqlmodel](https://pypi.org/project/sqlmodel/)
- htmx: Zero-Clause BSD (0BSD), from the LICENSE file. Alpine.js: MIT. Leaflet: BSD-2-Clause. Chart.js: MIT. — [GitHub htmx LICENSE](https://github.com/bigskysoftware/htmx); [GitHub alpinejs](https://github.com/alpinejs/alpine); [GitHub Leaflet](https://github.com/Leaflet/Leaflet); [GitHub Chart.js](https://github.com/chartjs/Chart.js)
- streamlit 1.67.0: Apache-2.0; gradio 6.32.0: Apache-2.0. — [PyPI streamlit](https://pypi.org/project/streamlit/); [PyPI gradio](https://pypi.org/project/gradio/)
- OSM tile usage policy: — [OSMF tile usage policy](https://operations.osmfoundation.org/policies/tiles/)
  - Show "© OpenStreetMap contributors" on the map.
  - Send a specific User-Agent, and let the browser send a valid Referer.
  - Obey the cache headers (minimum 7 days if the headers are not read).
  - Do not bulk download or prefetch tiles.
  - There is no SLA. The OSMF can block heavy use without notice.
- OSM data has ODbL (the tile policy page refers to OSM attribution; ODbL details are on the OSM copyright page, not read here). — [OSMF tile usage policy](https://operations.osmfoundation.org/policies/tiles/)

### Inferences
- Plain FastAPI + htmx agrees with a "no build step" rule and gives a better jury demo (custom forms, map, cards with explanation).
- Streamlit or Gradio can make an internal admin page in 1-2 hours. But they add a second UI framework.
- For the map, a choropleth of the 16 voivodeships or powiats from a static GeoJSON file avoids heavy tile use.

### Gaps
- The license of the boundary GeoJSON (PRG from GUGiK) was not checked in this note.

## 10. EUPL-1.2 compatibility of code and model weights

### Takeaway
Permissive code licenses (MIT, BSD, Apache-2.0, 0BSD, PostgreSQL) can go into an EUPL-1.2 work without a problem. Copyleft licenses in the EUPL appendix (GPL, AGPL, LGPL, MPL-2.0, EPL-1.0) are legally "compatible", but they can force the combined work to the other license. Avoid them where possible. For model weights, use only Apache-2.0 or MIT. Avoid Gemma (before Gemma 4), Llama and CC-BY-NC weights.

### Cited Findings
- The EUPL-1.2 appendix lists these "Compatible Licences" (verbatim list): — [EUPL-1.2 text](https://ctan.math.washington.edu/tex-archive/graphics/minim-hatching/EUPL-1.2-EN.txt); [EUPL v1.2 PDF, EC](https://interoperable-europe.ec.europa.eu/sites/default/files/distribution/access_url/2024-10/e5101c2c-40b6-428e-a16e-a6ac9ade8a01/EUPL_v1.2.pdf)
  - GNU GPL v. 2, v. 3
  - GNU AGPL v. 3
  - OSL v. 2.1, v. 3.0
  - EPL v. 1.0
  - CeCILL v. 2.0, v. 2.1
  - MPL v. 2
  - GNU LGPL v. 2.1, v. 3
  - CC BY-SA 3.0 Unported, for works other than software
  - EUPL v. 1.1, v. 1.2
  - LiLiQ-R and LiLiQ-R+
- Article 5 says that a derivative work of the EUPL work and a work under a compatible license can be distributed under the compatible license. If the obligations conflict, the obligations of the compatible license prevail. — [EUPL-1.2 text](https://ctan.math.washington.edu/tex-archive/graphics/minim-hatching/EUPL-1.2-EN.txt)
- The EUPL definition of "Distribution or Communication" includes online access to the essential functions of the work. Thus the EUPL also covers SaaS use. — [EUPL-1.2 text](https://ctan.math.washington.edu/tex-archive/graphics/minim-hatching/EUPL-1.2-EN.txt)
- The EC compatibility matrix says that MIT, BSD and Apache code can be incorporated and linked statically or dynamically into EUPL work ("OK"). It says that "Nothing in the MIT license restricts redistribution under any other license". For GPL and AGPL, incorporation is "NO (exceptions exist)", but dynamic linking is "OK". The matrix covers only OSI licenses. — [EC matrix of EUPL compatible licences](https://interoperable-europe.ec.europa.eu/collection/eupl/matrix-eupl-compatible-open-source-licences)
- The FSF still lists EUPL-1.2 as GPL-incompatible, because the appendix gives compatibility in one direction only. — [Interoperable Europe news "Better late than never"](https://interoperable-europe.ec.europa.eu/collection/eupl/news/better-late-never)
- Gemma Terms of Use (last modified 1 Apr 2026): — [Gemma Terms of Use](https://gemma.ai/resources/terms/)
  - "Model Derivatives" include models made by distillation or by training on synthetic Outputs of Gemma.
  - A distributor must make the use restrictions enforceable downstream and give a copy of the terms.
  - Google "reserves the right to restrict (remotely or otherwise) usage".
  - Gemma 4 is not under these terms. It has a separate Apache-2.0 license.
- The sdadas v2/v3 models list the Gemma license. Their training used a Gemma-based teacher (bge-reranker-v2.5-gemma2-lightweight). — [HF sdadas/polish-reranker-roberta-v3](https://huggingface.co/sdadas/polish-reranker-roberta-v3); [HF sdadas/mmlw-retrieval-roberta-large-v2](https://huggingface.co/sdadas/mmlw-retrieval-roberta-large-v2)
- Llama 3.1 license: "Built with Llama" display, "Llama" name prefix for derived models, Acceptable Use Policy, 700M MAU clause, California law. The OSI says that it is not open source. — [Llama 3.1 license](https://dev.meta.ai/llama/llama3_1/license); [OSI](https://opensource.org/blog/metas-llama-2-license-is-not-open-source)
- PLLuM "-nc" models have CC-BY-NC-4.0 and are for non-commercial use. — [HF CYFRAGOVPL/PLLuM-12B-chat](https://huggingface.co/CYFRAGOVPL/PLLuM-12B-chat)

### Inferences
- The EUPL governs the repository code. The application downloads model weights at run time and does not merge them into the code. Thus the weight license applies to the use and redistribution of the weights, not to the code license. But the public institution (PCI) must obey the weight license when it runs the system. Thus the weight license matters for the transfer.
- Safe weights (Apache-2.0 or MIT): mmlw-* v1 models, polish-reranker-*-ranknet/mse, stella-pl (MIT), multilingual-e5, bge-m3, bge-reranker-v2-m3, Qwen3-Embedding, Qwen3-Reranker, Bielik v2.x/v3.x, PLLuM-4B/12B (not -nc), Gemma 4, Qwen3/3.5/3.6, Mistral Small 3.2, Ministral 3, EuroLLM.
- Weights to avoid: Gemma-licensed models (EmbeddingGemma, Gemma 3, bge-multilingual-gemma2, sdadas v2/v3 and stella-pl-retrieval*), Llama 3.x and Llama-PLLuM, CC-BY-NC models (jina-embeddings-v3, PLLuM -nc), Ministral-8B-2410 (license "other").
- Code to avoid or isolate: PyMuPDF (AGPL), spaCy Polish models (GPL-3.0), Neo4j Community (GPL-3.0), Typesense (GPL-3.0), Elasticsearch (AGPL/SSPL/ELv2), Meilisearch Enterprise parts (BUSL-1.1).
- Record each library and model with its license in README.md, as AGENTS.md requires.

### Gaps
- No official EC guidance on model weights under EUPL was found. The weights inference above is not legal advice.
- The CC BY-SA 3.0 entry covers "works other than software". Its effect on datasets such as Wikipedia extracts was not analyzed.

## 11. Recommended architecture for 20 hours on CPU, and expected quality

### Takeaway
One Python process (FastAPI) with one SQLite file (records, FTS5, sqlite-vec). Use Apache/MIT models: mmlw-retrieval-roberta-large or mmlw-roberta-base for embeddings, polish-reranker-large-ranknet for reranking, and Bielik-11B-v3.1 (or a 4B model) through Ollama with a JSON schema. Run the bulk crawl, extraction and embedding on Kaggle T4 in advance, and load the results into SQLite.

### Cited Findings
- Retrieval quality for Polish with Apache models: PL-MTEB retrieval 53.60 (mmlw-roberta-base) to 57.23 (mmlw-retrieval-roberta-large). — [PL-MTEB arXiv v2](https://arxiv.org/html/2405.10138)
- Reranker quality on Polish public administration data: NDCG@10 78-85 with Apache models (polish-reranker-large-ranknet 85.0 / 78.2 / 79.9; bge-reranker-v2-m3 78.8 / 74.3 / 85.4). — [HF sdadas/polish-reranker-roberta-v3](https://huggingface.co/sdadas/polish-reranker-roberta-v3)
- Polish LLM quality: Bielik-11B-v3.0 has an Open PL LLM average of 65.93 and PLCC 71.83%. Bielik-4.5B-v3.0 has 56.13. Qwen3-4B has 55.49 (self-reported by the Bielik team). — [arXiv 2601.11579 PDF](https://arxiv.org/pdf/2601.11579v1)
- Main text extraction: trafilatura F1 0.926. — [trafilatura evaluation](https://trafilatura.readthedocs.io/en/latest/evaluation.html)
- The Bielik 11B v3.1 GGUF Q4_K_M needs 6.7 GB. — [Ollama SpeakLeash/bielik-11b-v3.1-instruct](https://ollama.com/SpeakLeash/bielik-11b-v3.1-instruct)

### Inferences
- Pipeline:
  1. Collect: httpx + protego + trafilatura + feedparser, and the submission form.
  2. Filter: language check, minimum length, embedding similarity to prototype descriptions.
  3. Extract: Bielik with a Pydantic JSON schema and evidence quotes. Check each quote in code.
  4. Score: a rule module with transparent criteria (TRL, novelty = 1 - max similarity to existing records, market need, team, region).
  5. Index: lemmatized FTS5 + sqlite-vec. Search with RRF, then the reranker on the top 30.
  6. Duplicates: MinHash (Jaccard 0.8) + cosine threshold, shown as "similar solutions".
  7. Match: weighted, explainable score to needs, partners and investors.
  8. Show: FastAPI + htmx + Leaflet + Chart.js.
- Time budget estimate: about 4 h for crawl and extraction, 3 h for search and duplicates, 3 h for scoring and matching, 5 h for the user interface, 3 h for evaluation and README, 2 h for buffer.
- Expected quality (estimate): search with reranker gives a good top-5 for most Polish queries. Extraction of enum fields (sector, type, region) is probably correct in most cases. TRL and the creator names need human review. Show the confidence and the evidence quote so that the jury sees the uncertainty.
- Fallback if the CPU LLM is too slow on demo day: a hosted EU OpenAI-compatible endpoint (for example Scaleway or Mistral) behind the same client. Do not send personal data to it.

### Gaps
- No end-to-end benchmark of such a pipeline for Polish innovation records exists in the sources. All quality expectations for the full pipeline are estimates.
