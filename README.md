
# Amazon ML Challenge 2026 – S3 Blocking & Candidate Generation

## Contribution

This repository contains my contribution to the Amazon ML Challenge 2026 entity matching task, focusing on:

- Source 3 (S3) blocking
- S1 → S3 candidate generation
- Training-data blocking evaluation

## Objective

The goal of blocking is to reduce the number of possible S1–S3 entity pairs before the final machine learning matching stage.

Instead of comparing every Source 1 record with every Source 3 record, multiple blocking rules are used to generate a smaller candidate set.

## S3 Blocking Rules

The final blocking strategy uses the following three rules:

### Rule 1 – Country + Name Prefix + Address Prefix

- Country
- First 4 characters of normalized business name
- First 4 characters of normalized business address

### Rule 2 – Country + Exact Normalized Business Name

- Country
- Exact normalized business name

### Rule 3 – Country + Name Token

- Country
- Individual tokens from the normalized business name
- Tokens occurring more than 1000 times in S3 are excluded

The candidate sets from these rules are combined and deduplicated.

## Blocking Results

- S1 records: 2,206,821
- S1 → S3 ground-truth matches: 3,944,746
- Final blocking recall: 71.58%
- Final candidate pairs: 527,901,663
- Average candidates per S1: 239.21
- Candidate file size: approximately 13.17 GB

## Additional Rule Tested

A fourth rule using:

- Country + first 6 characters of normalized business name

was evaluated but did not provide meaningful additional recall, so it was not included in the final blocking strategy.

## Files

`S3_Blocking.ipynb`

Contains the complete Colab notebook used for:

- Data loading
- Text normalization
- S3 blocking index construction
- Blocking-rule evaluation
- Candidate generation
- Training-set recall evaluation

Large generated candidate files and raw training datasets are not stored in this GitHub repository.

## Output

The final S1–S3 candidate pairs were generated as:

`final_s3_blocking.tsv`

This file contains:

- `source1_entity_id`
- `source3_entity_id`

The generated candidate file is stored separately because of its large size.

## Next Step

The generated S1–S3 candidate pairs can be used as input to the downstream entity-matching / machine-learning stage.
