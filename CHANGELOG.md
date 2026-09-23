# nf-core/rnasplice: Changelog

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## v1.0.4-flow.3 (Goodwright fork for Flow)

- On Flow, only rMATS runs by default. rnasplice's own defaults switch on DEXSeq,
  edgeR, DEXSeq DTU, SUPPA and MISO sashimi plots; the schema now sets DEXSeq and
  edgeR off (optional toggles) and pins DEXSeq DTU, SUPPA and sashimi plots off.
  Before, sashimi plots ran from BAM input, for two fixed human genes.

## v1.0.4-flow.2 (Goodwright fork for Flow)

- The Flow schema takes the genome FASTA, GTF and transcripts FASTA from a Prepare
  RNA-Seq Genome execution, so no genome files are rebuilt per run. From BAM input,
  rnasplice builds no indexes; the transcripts FASTA was the one costly step left.

## v1.0.4-flow.1 (Goodwright fork for Flow)

Based on v1.0.4.

- Added `flow/schema/rnasplice.json` so the pipeline runs on Flow from the genome BAMs of
  RNA-Seq executions (`--source genome_bam`), with rMATS always on.
- `--source genome_bam` samplesheets accept optional `strandedness` and `single_end`
  columns, and rMATS uses them. Without them, rMATS ran every BAM as unstranded
  paired-end. Backported from nf-core/rnasplice#263, which is fixed on `dev` but
  unreleased.
- The rMATS checks that all samples share read type and strandedness now also run for
  `--source genome_bam`.

## v1.0.4 - 2024-04-21

- Fixed incorrect assignment of cluster groups (Issue #131).

## v1.0.3 - 2024-02-23

- Improved TPM file splitting performance (Issue #120).
- Fixed an issue where R scripts altered sample names upon loading (Issue #122).

## v1.0.2 - 2024-01-08

Patch for run_stager.R (#108) and template update v2.11.1 (#109).

## v1.0.1 - 2023-11-15

Patch for run_drimseq_filter.R to cast command line arguments to numeric. See issue #98 on nf-core/rnasplice.

## v1.0.0 - 2023-05-22

First release of nf-core/rnasplice, created with the [nf-core](https://nf-co.re/) template.

### `Added`

Implemented pipeline:

- Merge re-sequenced FastQ files (cat)
- Read QC (FastQC)
- Adapter and quality trimming (TrimGalore)
- Alignment with STAR:
  - STAR -> Salmon
  - STAR -> featureCounts
  - STAR -> HTSeq (DEXSeq count)
- Sort and index alignments (SAMtools)
- Create bigWig coverage files (BEDTools, bedGraphToBigWig)
- Pseudo-alignment and quantification (Salmon; optional)
- Summarize QC (MultiQC)
- Differential Exon Usage (DEU):
  - HTSeq -> DEXSeq
  - featureCounts -> edgeR
  - Quantification with featureCounts or HTSeq
- Differential exon usage with DEXSeq or edgeR
  - Differential Transcript Usage (DTU):
  - Salmon -> DRIMSeq -> DEXSeq
  - Filtering with DRIMSeq
- Differential transcript usage with DEXSeq
- Event-based splicing analysis:
  - STAR -> rMATS
  - Salmon -> SUPPA2

Updated pipeline:

- Visualization of differential results with edgeR, DEXSeq, and MISO
- Contrasts specified using contrastsheet.csv
- Allow users to specify input data type and start point (e.g., fastq, genome_bam, transcript_bam, salmon_results)
- Pipeline schematic updated
