# Assignment 01: Linux commands and sequence files

**Level:** beginner life science trainee  
**Suggested time:** 90–120 minutes  
**Total:** 30 questions, 60 marks (2 marks each)

This assignment follows [Module 01: WSL and Linux basics](WSL_and_Linux_Basics.md) and [Module 02: sequence files and first QC](Module_02_Sequence_Files_and_First_QC.md). Use the synthetic files from [Module 02 practice data](Module_02_Practice_Data.zip). They are for learning file structure and commands, not for assessing a real sequencing run.

## Before you begin

Open **Ubuntu on WSL**. The Module 02 ZIP should already be extracted in `~/bioinformatics_training/module_02`. If it is not, follow the setup in Module 02 first. Work only on these practice files.

For each command question, submit **the command you ran and the relevant result**. For explanation questions, answer in one or two sentences unless a question asks for more. If a command gives an error, record it and explain how you fixed it. You may refer to both modules while working. Do not paste the question text into the terminal.

## Part A — Set up and navigate (Questions 1–7)

1. Go to `~/bioinformatics_training/module_02`. Which command shows your current directory? Run it and record the path shown.
2. List the files in this folder with human-readable sizes. Write the names of the FASTA file and both FASTQ files you see.
3. Create a directory named `assignment_work` inside `module_02`. Show a command that confirms it exists.
4. Copy `sample_reference.fasta`, `SampleA_R1.fastq`, `SampleA_R2.fastq`, and `run_notes.txt` into `assignment_work`. Show a command that lists the copies. Leave the originals in place.
5. Enter `assignment_work`. How would you return to its parent directory with one command? Return to `assignment_work` afterward.
6. Display all of `run_notes.txt`. According to the file, why should you avoid drawing conclusions about sequencing-run quality from these reads?
7. Use `nano` to create `analysis_notes.txt` in `assignment_work`. Put these two lines in it, save, exit, and show the command you use to confirm the saved text:

   ```text
   Module 02 assignment
   Synthetic sequence practice
   ```

**Stay inside `assignment_work` for Questions 8–28.**

## Part B — Inspect and search FASTA (Questions 8–13)

8. Display the complete `sample_reference.fasta`. Identify the character that starts each sequence header.
9. Use `grep` to print **only the header lines** from `sample_reference.fasta`. Write the three header names.
10. Use `grep` to count the header lines. How many sequences does this file contain?
11. Display only the **first two lines** of `sample_reference.fasta`. Which sequence record is shown?
12. Display only the **last two lines** of the same file. Which sequence record is shown?
13. Search the FASTA for the motif `GATTACA` and show the matching line number. In which record is the motif found? Explain how you determined that from the file.

## Part C — Inspect FASTQ and paired reads (Questions 14–21)

14. Show the first **four lines** of `SampleA_R1.fastq`. Label the identifier, bases, separator, and quality line. How many bases are in this read?
15. Count the lines in both FASTQ files. Given four lines per read in these files, how many reads are in each file, and how many read **pairs** are there?
16. Display the first record from **each** FASTQ file. Write both identifiers and explain which part of the identifiers shows that they belong to the same pair.
17. Display the last record from each FASTQ file. Write both identifiers. Do the pair identifiers still match?
18. For the first R1 record, count the bases and quality characters. What must be true about these two lengths in a valid FASTQ record?
19. Search `SampleA_R1.fastq` for the motif `GATTACA`, showing the line number. Which read contains it? What does the matching line represent?
20. Search `SampleA_R1.fastq` for `!!!!`, showing the line number. Which read contains it, and what do these characters illustrate in this teaching dataset?
21. Open `SampleA_R1.fastq` with `less`, search inside it for `SampleA_read004`, and return to the terminal. Write the command and the keys you pressed to search and quit.

## Part D — Manage files, compression, and QC (Questions 22–30)

22. Make a copy of `SampleA_R1.fastq` named `R1_backup.fastq`. Show that both files exist.
23. Rename `R1_backup.fastq` to `R1_working.fastq`. Which command did you use? Does `SampleA_R1.fastq` still exist?
24. Compress **only** `R1_working.fastq` with `gzip` using its default behavior. What is the resulting filename? Is the uncompressed `R1_working.fastq` still present?
25. Restore that compressed working file with `gunzip`. Show the resulting filename. Does its `.gz` version still exist?
26. Create `SampleA_R1.fastq.gz` from the original R1 file **without removing the original**. Then display the first four lines of the compressed file in the terminal without extracting it. Write both commands.
27. Create a ZIP archive called `assignment_bundle.zip` containing `sample_reference.fasta`, both original FASTQ files, and `analysis_notes.txt`. List the archive contents without extracting it. How many files are inside?
28. Create a folder named `submission_check`, extract `assignment_bundle.zip` into it, and list the extracted files. Does this step remove the originals in `assignment_work`?
29. In the two [FastQC example reports linked in Module 02](Module_02_Sequence_Files_and_First_QC.md#6-inspect-real-fastqc-example-reports), record the **total sequences** and **sequence length** for each report. Which report lists overrepresented sequences?
30. Write **four to six sentences** explaining how FASTA differs from FASTQ, what R1 and R2 mean here, one observation that makes the “bad” FastQC example worth investigating, and one next check you would make before deciding on any action. Separate what you observed from what you suspect.

## What to submit

Submit a document with answers numbered **1–30**. Include commands and the small relevant outputs for command questions; for Question 30, include your short interpretation. You can also submit `analysis_notes.txt` if your trainer asks for it. The practice reads contain no real sample information.
