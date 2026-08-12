# Research Project Instructions

This repository is being used to reconstruct and improve a research paper for journal submission.

## Original Project

The `original/` directory is the immutable baseline.

NEVER modify, delete, rename, overwrite, or reorganize files inside `original/`.

All future work must be performed in:

- `RESEARCH_REWRITE/`
- `prototype/`
- `evaluation/`
- `REVISED_PAPER/`

The original manuscript must remain available for comparison throughout the project.

## Scientific Integrity

NEVER fabricate:

- experimental results
- benchmark results
- statistics
- participant information
- ethics approvals
- datasets
- citations
- DOI information
- implementation results
- security guarantees
- deployment results

If evidence is unavailable, explicitly state that it is unavailable.

Never convert a proposed architecture into a claimed implementation.

Never convert future work into a claimed contribution.

Never strengthen a claim beyond the available evidence.

## Evidence

Every quantitative claim in the revised manuscript must be traceable to:

1. an actual experiment
2. an executable test
3. a reproducible calculation
4. a verified dataset
5. or a verified scholarly source

## Research Reconstruction

Distinguish carefully between:

- what the original manuscript claims
- what the original project actually implements
- what can be reproduced
- what must be newly implemented
- what must be experimentally evaluated
- what should be removed from the paper

Do not silently reconcile contradictions. Flag them.

## Literature

Never fabricate references.

Verify bibliographic information before adding references.

Do not retain placeholder references in the final manuscript.

## Manuscript Consistency

The revised paper must maintain consistency between:

Research questions
→ contributions
→ architecture
→ implementation
→ methodology
→ experiments
→ results
→ discussion
→ conclusions.

Do not claim that a feature is experimentally evaluated unless an actual experiment supports it.

## Reproducibility

Whenever experiments are created, record:

- software versions
- compiler versions
- network configuration
- hardware/environment
- experiment configuration
- workload
- random seeds where applicable
- commands used
- output files
- analysis procedures

## Coding

Prefer executable tests and reproducible scripts over manually entered results.

Never report a test as passing unless it was actually executed.

## Editing

Prefer creating new artifacts in:

- `RESEARCH_REWRITE/`
- `prototype/`
- `evaluation/`
- `REVISED_PAPER/`

rather than modifying the original project.
