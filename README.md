# ua-telegram-manipulation-dataset
Annotated dataset of Ukrainian Telegram posts for manipulation detection (span identification & technique classification), developed using a hybrid human-AI annotation pipeline with strict QA validation.

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC_BY--NC--SA_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
[![Language: Ukrainian](https://img.shields.io/badge/Language-Ukrainian-blue.svg)]()
[![Task: NLP / Manipulation Detection](https://img.shields.io/badge/Task-Manipulation_Detection-green.svg)]()

## Overview
This repository contains an annotated corpus of Ukrainian-language Telegram posts designed for detecting and analyzing manipulative discourse in social media. The dataset supports both **span-level identification** of manipulative text fragments and **multilabel classification** of specific rhetorical and psychological manipulation techniques.

The corpus was developed as a research deliverable of the **Socio-Communicative Internship in Computational Linguistics** at Taras Shevchenko National University of Kyiv. The dataset was created by a student team comprising **O. Solodka, M. Yurutkin, A. Rudiuk, D. Dudnik, T. Androshchuk, D. Bidochka, Ye. Morhunova, K. Nesterenko, Ya. Oliinyk, O. Olkhovyk, and K. Savron**, under the academic supervision of **O. Zuban, V. Robeyko, Ya. Khodakivskya**.

## Task Description
The dataset addresses two core NLP tasks:
1. **Span Identification:** Pinpointing exact token/character boundaries of the minimal continuous text fragments where manipulative language occurs within a post.
2. **Technique Classification:** Identifying which specific manipulation techniques are present in a given post or span (multilabel classification across 8 binary label columns).

### Manipulation Taxonomy
The dataset covers **8** manipulation techniques (adapted from the UNLP 2025 Shared Task framework):
* **`emotives` (Емоціоген)** — Use of profanity, insults, derogatory labels, or strongly negatively charged lexical items aimed at discrediting or humiliating a person or group.
* **`fud` (СНС / Fear, Uncertainty, and Doubt)** — Rhetoric that induces or amplifies panic, anxiety, helplessness, or doubt regarding safety, stability, or institutional capability (beyond neutral reporting of real threats).
* **`spin` (Маніпуляція інформацією)** — Selective presentation, distortion, exaggeration, or decontextualization of facts to impose a one-sided or misleading interpretation of an event or position.
* **`cliche` (Кліше)** — Overused thought-terminating formulas offering oversimplified answers to complex issues, as well as political irony, sarcasm, and mockery used to dismiss an argument or subject.
* **`whataboutism` (Сам дурень)** — Deflecting criticism or an argument by shifting focus to the opponent's alleged hypocrisy, flaws, or past actions rather than addressing the original claim.
* **`euphoria` (Ейфорія)** — Triumphalist, overly optimistic, or emotionally elevated framing of events designed to induce premature relief or heightened morale.
* **`generalization` (Генералізація)** — Unjustified extrapolation from isolated incidents or personal experience to an entire social, national, or professional group.
* **`appeal_to_people` (Апеляція до читача)** — Direct or indirect calls to action, clickbait framing, or exploitation of collective values (*freedom*, *patriotism*, *justice*) and empathy to engage or mobilize the audience.

*(Full definitions, span boundary criteria, and edge-case rules are detailed in `docs/Final_Instruction_Extended.pdf` and `docs/Final_Table_Instruction.pdf`.)*

## Annotation Methodology & Pipeline
To ensure dataset homogeneity and scientific validity, the corpus was built through a multi-stage workflow across five independent annotator pairs:
1. **Data Collection & Deduplication:** Automated scraping of 7 Ukrainian Telegram channels (`truexanewsua`, `kievinfo_kyiv`, `times_ukraina`, `voynareal`, `oleksiihoncharenko`, `reiters`, `ssternenko`) covering September 15, 2025 – September 15, 2026 (10–80 words per post), filtered via regex cleaning and an 80% character-sequence similarity threshold (`tg_parser.ipynb`).
2. **Guideline Standardization:** Iterative refinement of annotation manuals (`Final_Instruction_Extended.pdf` and `Final_Table_Instruction.pdf`) to unify span boundary rules and establish a 12-column paired table schema (`id`, 8 binary technique labels, `span`, `explanation`, and `annotator`).
3. **Hybrid Annotation (Human + AI):** Parallel human markup (`H`) and structured LLM pre-annotation with mandatory explanations (`AI`, powered by Gemini via `docs/Final_AI_Prompt.pdf`), enabling direct human-AI comparison.
4. **Edge-Case Resolution (`N/I` Cases):** Centralized collection, UUID verification against raw data, and resolution of ambiguous or out-of-scope (`N/I`) instances (`NI_Cases.xlsx`).
5. **QA Audit & Harmonization:** Weekly cross-checks, adjudication of disagreements, and final dataset proofreading conducted by annotation leads (`docs/Annotation_Lead_Review.pdf`).

## Dataset Statistics & Quality Control
* **Total annotated posts:** **2,055** Ukrainian Telegram posts (collected September 29 – November 10, 2026), split into two methodological phases:
  * **Phase 1 (Initial Guidelines, Sep 29 – Oct 20, 2026):** 1,109 posts
  * **Phase 2 (Updated Guidelines, Oct 21 – Nov 10, 2026):** 946 posts
* **Span-level annotations:** **1,210** analyzed span pairs across the full corpus, achieving a weighted average **Intersection over Union (IoU) of 0.734** when both annotators identified a manipulative fragment.
* **Overall Agreement:** **Gwet's AC1** reached **0.877–0.997** in Phase 1 and **0.848–0.997** in Phase 2, while **Krippendorff's Alpha ($\alpha$)** improved across 5 of the 8 categories following the guideline revision (most notably in `cliche` and `fud`).

| Manipulation Technique | Krippendorff's $\alpha$ (Phase 1) | Krippendorff's $\alpha$ (Phase 2) | $\Delta \alpha$ | Span IoU (Full) | Span F1-score (Full) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **`emotives`** | 0.640 | 0.685 | +0.044 | 0.860 | 0.896 |
| **`fud`** | 0.414 | 0.563 | +0.149 | 0.667 | 0.737 |
| **`spin`** | 0.362 | 0.401 | +0.039 | 0.702 | 0.767 |
| **`cliche`** | 0.511 | 0.788 | +0.277 | 0.736 | 0.785 |
| **`whataboutism`** | 0.719 | 0.674 | -0.045 | 0.914 | 0.949 |
| **`euphoria`** | 0.498 | 0.397 | -0.101 | 0.731 | 0.778 |
| **`generalization`** | 0.473 | 0.236 | -0.238 | 0.593 | 0.685 |
| **`appeal_to_people`** | 0.497 | 0.554 | +0.057 | 0.714 | 0.768 |

*(Full statistical breakdowns, pairwise annotator comparisons, and qualitative error analyses are available in `docs/IAA_&_QA_Report.pdf`.)*

## Repository Structure
```text
├── data/
│   ├── Annotated_Dataset.xlsx        # Final harmonized and validated dataset
│   ├── Manipulation_Dataset_Raw.xlsx # Initial unannotated text pool (UUID-indexed)
│   └── NI_Cases.xlsx                 # Resolved ambiguous / out-of-scope samples
├── docs/
│   ├── Final_Instruction_Extended.pdf
│   ├── Final_Table_Instruction.pdf
│   ├── Final AI Prompt.pdf
│   ├── Annotation_Lead_Review.pdf
│   ├── IAA_&_QA_Report.pdf
│   └── Code_Documentation.pdf
├── scripts/
│   ├── tg_parser.ipynb               # Telegram scraping, regex cleaning & deduplication
│   ├── table_manipulation.ipynb      # Sheet segmentation, merging & UUID validation
│   └── table_join.ipynb              # Pairwise merging & layout sanitization
└── README.md
```

## Scripts & Reproducibility
All data collection, table normalization, and validation pipelines were implemented in **Python 3.12** (`telethon`, `pandas`, `numpy`). Key utilities include automated Cyrillic-to-Latin character sanitization (`Н`/`А`/`І` $\rightarrow$ `H`/`A`/`I`), UUID cross-verification across subsets, and pairwise missing-ID diagnostics. Full technical specifications are documented in `docs/Code_Documentation.pdf`.

## License & Acknowledgments
This repository's content is licensed under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)** license.

*Acknowledgments:* Task formulation and manipulation taxonomy were adapted from the **UNLP 2025 Shared Task on Detecting Social Media Manipulation** (organized by Texty.org.ua & UNLP).
