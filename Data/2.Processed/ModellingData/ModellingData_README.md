# Modelling Data Overview

This folder contains various Parquet files used for advanced modeling tasks (e.g., topic modeling, sentiment analysis, network analysis). Below is a brief description of each file:

---

## `P0H_tokens_abstract.parquet`
- **Contains**: Initial tokens extracted from abstracts using a Hugging Face pipeline.
- **Usage**: Early exploration of HF subword tokenization.

## `P1H_tokens_title.parquet`
- **Contains**: Title-level tokens using a Hugging Face pipeline.
- **Usage**: Analysis of short text tokens in article titles.

## `P0_simple_tokens_abstract.parquet`
- **Contains**: Abstract-level tokens from a simpler, rule-based approach (lowercase, minimal cleaning).
- **Usage**: Baseline tokens for direct frequency analysis or minimal EDA.

## `P1_simple_tokens_title.parquet`
- **Contains**: Title-level tokens from a simpler, rule-based approach.
- **Usage**: Baseline tokens in titles for minimal EDA or comparison with HF tokens.

## `P2_abstract.parquet`
- **Contains**: Processed abstracts with additional fields (date parsing).
- **Usage**: General advanced EDA or minimal feature engineering.

## `P2_title.parquet`
- **Contains**: Processed titles with additional fields.
- **Usage**: Focused analysis on article titles (e.g., disease mentions in short text).

## `P3_bc5cdr_results_abstract.parquet`
- **Contains**: Abstract tokens or entities identified using a BC5CDR (BioCreative V CDR) spaCy model.
- **Usage**: Disease/chemical entity recognition on abstract text.

## `P3_bc5cdr_results_mesh.parquet`
- **Contains**: MeSH-related tokens or entity matches from the BC5CDR model.
- **Usage**: Detailed MeSH term analysis or entity-level EDA.

## `P4_abstract.parquet`
- **Contains**: Another iteration of processed abstracts (additional cleaning or token merging).
- **Usage**: Next step in pipeline after P3 for refining tokens or metadata.

## `P4_title.parquet`
- **Contains**: Another iteration of processed titles, possibly merging multiple token sources.
- **Usage**: Next step in pipeline after P3 for refining short-text tokens.

## `P5_final_merged.parquet`
- **Contains**: A more consolidated dataset, combining multiple token columns and deduplications.
- **Usage**: Often used for large-scale analyses (topic modeling, classification).

## `P5_final_new.parquet`
- **Contains**: Similar to `P5_final_merged` but with incremental improvements or new columns (e.g., refined tokens).
- **Usage**: Potentially the main dataset for advanced tasks (sentiment, NER, topic modeling).

## `P6_final_merged.parquet`
- **Contains**: The newest version of the data, including all fields from previous versions plus a new column with merged tokens (e.g., `merged_all_tokens`).
- **Usage**: **Recommended** dataset for current modeling tasks (e.g., LDA, NMF, or classification). It has all prior enhancements, tokens, and merges.

---

### Notes
- **Naming Conventions**: 
  - `P#_` indicates iterative processing stages.
  - `_tokens_abstract` vs `_tokens_title` differentiates where tokens originated (abstract vs title).
  - `bc5cdr` references a specialized spaCy model for disease/chemical entity extraction.
- **Recommended File**: 
  - `P6_final_merged.parquet` is the most up-to-date, containing comprehensive token merges and fields for advanced modeling.

