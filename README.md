# Story-generation-process

Generation pipeline: 19 large language models asked to rewrite 16 classical fairy tales. Each notebook corresponds to one source tale from Project Gutenberg.

The texts produced here feed into [LLM_readability_score_analysis](https://github.com/MarinaBaklanova17/LLM_readability_score_analysis) for statistical evaluation. Part of a Master's research project at Whitireia New Zealand (2025).

## Purpose

Build a controlled corpus of LLM-generated narratives against a fixed human-written baseline, such that readability and accuracy assessments can be run under identical prompts and identical source texts across models.

## Design

Each notebook:

1. Loads one classical fairy tale as input
2. Submits the same generation prompt to 19 LLMs from major providers (Anthropic, OpenAI, Google, Meta, Mistral, and others)
3. Collects model outputs into a tabular format
4. Computes `textstat` readability metrics on each output

Cross-model dispatch was originally done through the Unify.ai routing service; see the historical note below.

## Source tales

16 notebooks, one per tale:

| Notebook | Source tale |
|---|---|
| `HOW_JACK_WENT_TO_SEEK_HIS_FORTUNE.ipynb` | How Jack Went to Seek His Fortune |
| `Henny_Penny.ipynb` | Henny Penny |
| `JACK_HANNAFORD.ipynb` | Jack Hannaford |
| `JOHNNY_CAKE.ipynb` | Johnny Cake |
| `Lazy_Jack.ipynb` | Lazy Jack |
| `MASTER_OF_ALL_MASTERS_readability_score.ipynb` | Master of All Masters |
| `MR_MIACCA.ipynb` | Mr Miacca |
| `MR_VINEGAR.ipynb` | Mr Vinegar |
| `TEENY_TINY.ipynb` | Teeny Tiny |
| `THE_CAT_AND_THE_MOUSE_fairy_tale.ipynb` | The Cat and the Mouse |
| `THE_MAGPIE'S_NEST.ipynb` | The Magpie's Nest |
| `THE_MASTER_AND_HIS_PUPIL.ipynb` | The Master and His Pupil |
| `THE_OLD_WOMAN_AND_HER_PIG.ipynb` | The Old Woman and Her Pig |
| `THE_ROSE_TREE.ipynb` | The Rose Tree |
| `THE_STORY_OF_THE_THREE_LITTLE_PIGS.ipynb` | The Story of the Three Little Pigs |
| `THE_THREE_SILLIES.ipynb` | The Three Sillies |

## Historical note: Unify.ai

The original pipeline used **Unify.ai** — a unified-API routing service that consolidated access to LLMs from Anthropic, OpenAI, Google, Meta, Mistral, AWS Bedrock, Fireworks, Together, Anyscale, DeepInfra, and others behind a single SDK (`from unify import Unify`).

**Unify.ai was discontinued in 2024.** The `UNIFY_KEY` variable is preserved in the notebooks as a historical reference but no longer resolves. To re-run the pipeline today, the generation step must be rewritten against each provider's native SDK (`openai`, `anthropic`, `google-generativeai`, and so on).

## Reproduce (requires porting)

```bash
pip install textstat pandas openpyxl jupyter
# plus the native SDKs for each provider you have access to
jupyter notebook
```

Replace the `unify = Unify(model_name)` + `unify.generate(prompt, temperature)` calls in the third cell of each notebook with equivalent calls against your preferred provider SDK.

## Limitations

- Unify.ai decommissioning means strict reproducibility of the original results requires access to the same model snapshots, which is not guaranteed after model versioning updates at each provider
- Only one generation per (model × tale) — no temperature-sweep or multi-seed averaging
- Prompt is held constant; prompt-sensitivity of readability is not studied here

## Related

- [LLM_readability_score_analysis](https://github.com/MarinaBaklanova17/LLM_readability_score_analysis) — statistical analysis of the texts generated here
- Master's research: *The comparative analysis of human-written and large language model generated children's tales: Readability and accuracy assessments of LLM-generated texts*, Whitireia New Zealand (2025)

## License

No license is specified; for commercial use, please open an issue.

---

*Language:* **English** · [Русский](README.ru.md)
