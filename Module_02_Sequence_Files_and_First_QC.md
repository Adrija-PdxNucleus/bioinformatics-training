# Module 02: Meet sequencing data — FASTA, FASTQ and a first quality check

**For:** life science trainees beginning bioinformatics  
**Time:** about 60–90 minutes  
**You need:** a Windows laptop with Ubuntu on WSL, the commands from [Module 01](WSL_and_Linux_Basics.md), a browser, and [the practice data ZIP](Module_02_Practice_Data.zip).

## What you will learn

By the end, you should be able to identify FASTA and FASTQ records, match R1 and R2 reads, inspect a compressed FASTQ file, and write a short interpretation of a sequencing quality control (QC) report.

The practice files contain **invented sequences**, not patient or research data. Their eight read pairs are for learning file structure. Use the larger published FastQC reports below to practise QC interpretation; do not use the toy reads to judge the quality of a sequencing run.

## 1. Get the practice files into Ubuntu

1. On the GitHub page for this repository, click `Module_02_Practice_Data.zip`, then click **Download raw file** (the download icon). Your browser will normally save it in Windows **Downloads**.
2. Open **Ubuntu** from the Windows Start menu and enter these commands, one line at a time:

```bash
mkdir -p ~/bioinformatics_training/module_02
cd ~/bioinformatics_training/module_02
explorer.exe .
```

3. Windows File Explorer opens at your Ubuntu practice folder. In another File Explorer window, open **Downloads**. Copy `Module_02_Practice_Data.zip` from Downloads into the Ubuntu folder you just opened.
4. Return to the Ubuntu terminal:

```bash
ls
unzip Module_02_Practice_Data.zip
ls
```

You should see `sample_reference.fasta`, `SampleA_R1.fastq`, `SampleA_R2.fastq`, and `run_notes.txt`. If `unzip` says the file is missing, check that you copied it into the folder opened by `explorer.exe .`.

> **Where am I?** `pwd` prints the current folder. `~` means your Ubuntu home folder. Commands in the rest of this worksheet assume you are inside `~/bioinformatics_training/module_02`.

## 2. Meet a FASTA file

FASTA stores sequence records. A line beginning with `>` identifies a sequence; the following line or lines contain its bases. Inspect our small example:

```bash
cat sample_reference.fasta
grep '^>' sample_reference.fasta
```

The `^>` pattern means “a `>` at the start of a line.” The sample reference is an invented teaching sequence, not an actual reference genome.

**Write down:** How many sequence records are present? What are their names? Does this file contain a quality score for every base?

## 3. Meet a FASTQ file

FASTQ stores both bases and one encoded quality value for each base. In this exercise, each read has four lines:

1. An identifier beginning with `@`.
2. The base sequence.
3. A `+` separator.
4. Quality characters, one per base.

```bash
head -n 8 SampleA_R1.fastq
less SampleA_R1.fastq
```

The first eight lines show **two** reads. In `less`, press **Space** to move down and **q** to quit. For this teaching example, the quality strings use Phred+33: `I` represents a high score and `!` a very low score. A quality character is **not** a DNA base. Quality encodings can vary in older data, so check the dataset's provenance before interpreting unfamiliar FASTQ files.

**Write down:** What is the first read's ID? How many bases does it have? How many characters are on its quality line? Which read in the file has visibly low quality near its end?

## 4. Count reads and match pairs

These two files represent paired reads from the same invented sample. `R1` and `R2` each hold one end of every pair, in the same order. Here the IDs share the same number and end in `/1` or `/2`.

```bash
wc -l SampleA_R1.fastq SampleA_R2.fastq
head -n 4 SampleA_R1.fastq
head -n 4 SampleA_R2.fastq
tail -n 4 SampleA_R1.fastq
tail -n 4 SampleA_R2.fastq
```

`wc -l` counts **lines**. Because these particular files have exactly four lines per read, divide a file's line count by four to get its read count. This shortcut assumes conventional four-line FASTQ records; do not count reads with `grep '^@'`, because a quality line may itself begin with `@`.

**Write down:** How many reads are in R1? How many in R2? What are the first and last pair IDs? Are they in matching order?

## 5. Read a compressed FASTQ file

Real FASTQ files are commonly stored as `.fastq.gz` to save space. Make a compressed **copy** of R1, keeping the original file:

```bash
gzip -c SampleA_R1.fastq > SampleA_R1.fastq.gz
ls -lh SampleA_R1.fastq*
zcat SampleA_R1.fastq.gz | head -n 8
```

`gzip -c` writes compressed data to the output after `>`; `zcat` displays decompressed text; `|` passes that text to `head`. On a tiny example, the `.gz` file may be no smaller because compression has overhead.

**Write down:** Did the original `.fastq` remain? Are the first two reads from `zcat` the same as before?

## 6. Inspect real FastQC example reports

Open these two reports **in your Windows browser**; nothing needs to be installed for this step:

- [FastQC example: good Illumina data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html)
- [FastQC example: bad Illumina data](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html)

For **each** report, look at **Basic Statistics**, **Per base sequence quality**, **Overrepresented sequences**, and **Adapter Content**. The green, orange, and red icons point you toward sections to investigate; their meaning depends on the experiment and library design. On the per-base plot, the horizontal axis is position along a read and higher quality scores on the vertical axis mean more confident base calls.

Fill in this table in your notes:

| Observation | Good example | Bad example |
| --- | --- | --- |
| Total sequences | | |
| Sequence length | | |
| What happens to quality across read positions? | | |
| Are overrepresented sequences listed? | | |
| What would you investigate before further analysis? | | |

You can zoom in with your browser if the plots are hard to read. Do not assume a report named “good” has no questions worth asking, or that every warning makes data unusable.

## 7. Submit a short QC note

Write **4–6 sentences** answering:

1. How are FASTA and FASTQ different?
2. What do `R1` and `R2` mean in this dataset, and how many read pairs did you inspect?
3. Which FastQC example needs closer investigation? Name **one observation from its report** and **one sensible next check**. Distinguish what you observed from what you suspect.

Share your answers and any questions with your trainer. In the next practical, you can work with a suitable real dataset and run QC software yourself.

## Further reading

- [NCBI SRA file format guide: FASTQ and paired-end FASTQ](https://www.ncbi.nlm.nih.gov/sra/docs/submitformats/)
- [FastQC: purpose and example reports](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
- [FastQC help: reading the summary icons](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/2%20Basic%20Operations/2.2%20Evaluating%20Results.html)
- [FastQC help: per-base sequence quality](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/2%20Per%20Base%20Sequence%20Quality.html)
