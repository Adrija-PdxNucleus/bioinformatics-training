[WSL_and_Linux_Basics_for_Life_Science_Beginners.md](https://github.com/user-attachments/files/32595463/WSL_and_Linux_Basics_for_Life_Science_Beginners.md)
# bioinformatics-training
Beginner-friendly WSL and Linux training for life science students, with genomics examples.
# WSL and Linux Basics for Life Science Beginners

**A Windows laptop guide to installing Ubuntu and working with sequence files**  
Updated 24 September 2026

 You will install Ubuntu through Windows Subsystem for Linux (WSL), learn where your files live, and practise 15 useful commands on a tiny FASTA file. You do not need programming experience.

> **How to read the commands:** Run commands marked **PowerShell** in Windows PowerShell. Run commands marked **Ubuntu** in the Ubuntu terminal. Copy only the text inside the code block, not the surrounding explanation or a sample prompt such as `user@computer:~$`.

## 1. Install WSL and Ubuntu

### Check your laptop

You need Windows 11 or Windows 10 version 2004 (build 19041) or later, an internet connection for installation, and permission to run PowerShell as administrator. Press **Windows + R**, type `winver`, and press **Enter** to see the Windows version. If this is a workplace managed laptop, your IT team may need to allow installation or enable virtualization. [Microsoft's installation guide](https://learn.microsoft.com/en-us/windows/wsl/install) has the current requirements.

1. Open the **Start** menu, search for **PowerShell**, right click it, and select **Run as administrator**.
2. Run this command in **PowerShell**:

   ```powershell
   wsl --install
   ```

3. Restart Windows when prompted. The command normally enables WSL and installs Ubuntu as the default Linux distribution.
4. Open **Ubuntu** from the Start menu. On first launch, create a Linux username and password. The password is separate from your Windows password; **nothing appears while you type it**, which is normal.
5. In **PowerShell**, confirm that Ubuntu is installed and note the value in the `VERSION` column:

   ```powershell
   wsl --list --verbose
   ```

   A new installation normally shows `Ubuntu` with `VERSION` `2`. If you see an issue, use the troubleshooting section below.

6. In **Ubuntu**, update its package list and installed packages:

   ```bash
   sudo apt update
   sudo apt upgrade
   ```

   Enter your *Linux* password when requested. You may be asked to confirm the upgrade with `Y`. Ubuntu package updates are separate from Windows updates.

7. The `zip` and `unzip` programs may need to be installed for the later exercise. In **Ubuntu**:

   ```bash
   sudo apt install zip unzip
   ```

   If Ubuntu says they are already installed, you are ready.

### PowerShell and Ubuntu are different terminals

`wsl --install`, `wsl --list --verbose`, and `wsl --update` are Windows commands. Commands such as `ls`, `cd`, and `grep` below belong in **Ubuntu**. You can reopen Ubuntu from Start whenever you want to practise; closing the terminal does not remove Ubuntu or its files. Type `exit` in Ubuntu to leave the shell.

## 2. Understand folders before using commands

In Ubuntu, your personal folder is `/home/your_linux_username`; `~` is shorthand for that folder. Your Windows `C:` drive is generally visible under `/mnt/c/`. For example, `C:\Users\Asha\Downloads` corresponds to `/mnt/c/Users/Asha/Downloads` in Ubuntu. Linux paths use `/`, and uppercase and lowercase letters matter: `Sample.fasta` and `sample.fasta` can be different files.

For Linux based work, make a project folder under your Ubuntu home folder. Microsoft recommends storing files in the WSL file system when Linux tools will process them, especially for better performance. From Ubuntu, `explorer.exe .` opens your current Linux folder in Windows File Explorer. [Read Microsoft's file system guide](https://learn.microsoft.com/en-us/windows/wsl/filesystems).

The commands below use a **practice** folder and invented sequences. Do not practise moving, renaming, or compressing your only copy of real sequencing data.

## 3. Learn the commands in a useful order

Type each example in **Ubuntu**. A command is followed by any options and then the file or folder it should act on. A space separates them. After each step, look at the result before continuing.

### A. Look around and make a folder

**1. `ls` — list files and folders.** Start by seeing what is in your current location.

```bash
ls
ls -lh
```

`-l` gives a detailed list; `-h` makes sizes easier to read. An empty result from `ls` can simply mean the folder is empty.

**2. `mkdir` — make a directory (folder).** Create your practice folder.

```bash
mkdir linux_practice
ls
```

You should now see `linux_practice` in the list. If it already exists, `mkdir` will say so; you can reuse it or choose another name.

**3. `cd` — change directory.** Enter the folder and move back when needed.

```bash
cd linux_practice
ls
cd ..
cd linux_practice
```

`cd ..` goes one folder up, `cd ~` goes to your home folder, and `cd` alone also returns home. The current location is often shown in the prompt. If you lose track, `pwd` prints the full path; it is a helpful extra command.

### B. Make and read a small FASTA file

First, create a tiny file so every following example has something safe to use. Make sure you are **inside** `linux_practice`, then run this **Ubuntu** command exactly:

```bash
printf '%s\n' '>sample_A' 'ATGCTAGCTA' '>sample_B' 'ATGAAAGCTA' > sequences.fasta
```

The `>` before `sequences.fasta` writes the command's output to a file. The quoted `>sample_A` and `>sample_B` are FASTA header lines; their quotes prevent the shell from treating those signs as output redirection. This invented file contains two records and four lines.

**4. `cat` — show a short file in full.**

```bash
cat sequences.fasta
```

You should see both `>` headers and both sequence lines. For a large FASTQ or report, use `head`, `tail`, or `less` instead of flooding the screen with `cat`.

**5. `head` — show the beginning of a file.**

```bash
head -n 2 sequences.fasta
```

This shows the first two lines: the first FASTA header and its sequence. Without `-n 2`, `head` normally displays the first 10 lines.

**6. `tail` — show the end of a file.**

```bash
tail -n 2 sequences.fasta
```

This shows the second record. On analysis logs, `tail -n 20 run.log` is a quick way to inspect the most recent 20 lines.

**7. `less` — page through a longer text file.**

```bash
less sequences.fasta
```

Use the arrow keys or **Space** to scroll, `/sample_B` followed by **Enter** to search, and **q** to quit. `less` lets you inspect text without editing it.

**8. `nano` — edit a text file.** Make a small note:

```bash
nano notes.txt
```

Type `Practice FASTA with two records.` Press **Ctrl + O**, then **Enter** to save; press **Ctrl + X** to exit. Check it with `cat notes.txt`. In the menu, `^O` means **Ctrl + O**. Avoid editing a FASTQ or other large dataset in a text editor.

### C. Copy, rename, move, and search

**9. `cp` — copy a file, leaving the original in place.**

```bash
cp sequences.fasta sequences_backup.fasta
ls
```

You should see both files. The first path is the source; the second is the new copy. `cp` can overwrite an existing destination, so check the destination name. Use `cp -i source destination` when you want an overwrite prompt.

**10. `mv` — rename or move a file.** First rename the backup, then move it into another folder:

```bash
mv sequences_backup.fasta sequences_copy.fasta
mkdir copies
mv sequences_copy.fasta copies/
ls copies
```

`sequences_copy.fasta` should now be inside `copies`; the original `sequences.fasta` remains in your practice folder. `mv` can overwrite a destination on some systems; `mv -i` asks before an overwrite.

**11. `grep` — find lines containing a pattern.** In FASTA, headers start with `>`:

```bash
grep '^>' sequences.fasta
grep -c '^>' sequences.fasta
grep -n 'AAA' sequences.fasta
```

The first command prints the two headers. The second prints `2`, the number of matching header **lines** in this simple example. The third prints the line containing `AAA` with its line number. `^` means “start of line”; `-c` counts matching lines; `-n` adds line numbers. This is a useful quick check, though real FASTA records may have sequences wrapped across many lines and headers alone do not validate the file.

### D. Work with compressed files

**12. `gzip` — compress one file to `.gz`.** We compress the *copy* so the original remains available:

```bash
gzip copies/sequences_copy.fasta
ls copies
```

You should see `sequences_copy.fasta.gz`. By default, `gzip` replaces the uncompressed input with the compressed file. Many bioinformatics files arrive as `.fastq.gz` or `.fasta.gz`.

**13. `gunzip` — restore a `.gz` file.**

```bash
gunzip copies/sequences_copy.fasta.gz
ls copies
```

The copy is back as `sequences_copy.fasta`. By default, `gunzip` replaces the `.gz` file. A `.gz` file usually compresses a single file; it is different from a `.zip` archive.

**14. `zip` — put files into a `.zip` archive.**

```bash
zip practice_archive.zip sequences.fasta notes.txt
ls -lh practice_archive.zip
```

The archive contains both named files and leaves the originals in place. For a folder and its contents, `zip -r archive.zip folder_name` is available, but the simple two file example is easier to check first.

**15. `unzip` — extract a `.zip` archive.** Extract into a separate folder to avoid filename collisions:

```bash
mkdir extracted
unzip practice_archive.zip -d extracted
ls extracted
```

You should see `sequences.fasta` and `notes.txt` in `extracted`. Use `unzip -l practice_archive.zip` to list archive contents without extracting them. Treat archives from unknown sources cautiously and inspect their contents before extraction.

## 4. One complete practice run

After learning each command, try this short sequence in **Ubuntu**. It starts in your existing practice folder and does not change your real data:

```bash
cd ~/linux_practice
ls
cat sequences.fasta
head -n 2 sequences.fasta
tail -n 2 sequences.fasta
grep -c '^>' sequences.fasta
less sequences.fasta
```

When `less` opens, press **q** to get back to the terminal. The `grep` count should be `2`. Then inspect the copied file and archive:

```bash
ls copies
ls extracted
unzip -l practice_archive.zip
```

If the `ls copies` result still ends in `.gz`, you skipped `gunzip`, which is fine; return to its step when you are ready.

## 5. Habits that prevent common mistakes

- **Check your location and file names first.** Use `pwd` and `ls` before `cp`, `mv`, `gzip`, or `gunzip`. Paths with spaces need quotes, for example `cd "/mnt/c/Users/Asha/My Data"`.
- **Keep an untouched original.** Copy a small input before trying edits or compression. Never overwrite the only copy of raw reads.
- **Remember case and extensions.** `Sample.fastq.gz` and `sample.fastq.gz` may be different files. `gunzip` does not extract a `.zip` archive; `unzip` does.
- **Read an error literally.** “No such file or directory” often means a spelling error or a different working folder. “Permission denied” means your account cannot access that location; do not immediately put `sudo` before a command on project files.
- **Use Tab completion.** Type the first part of a file or folder name and press **Tab** to reduce typing errors. Use the **Up arrow** to revisit an earlier command.
- **Know where WSL files are.** In Ubuntu, run `explorer.exe .` to open the current folder in Windows File Explorer. Do not edit WSL's underlying virtual disk files directly through Windows system folders.

## 6. Installation and first use troubleshooting

- **`wsl --install` shows help text or WSL is already present:** in **PowerShell**, run `wsl --list --online`, then `wsl --install -d Ubuntu` if Ubuntu is absent. [Microsoft installation guide](https://learn.microsoft.com/en-us/windows/wsl/install).
- **The download stalls at 0%:** in **PowerShell**, try `wsl --install --web-download -d Ubuntu` as described by Microsoft. A restart may still be required.
- **Virtualization related error:** check the [Microsoft WSL troubleshooting guide](https://learn.microsoft.com/en-us/windows/wsl/troubleshooting). On a managed laptop, ask IT to confirm virtualization and Windows feature settings; do not change firmware settings blindly.
- **Ubuntu password appears blank while typing:** that is expected. Type it and press **Enter**.
- **`zip: command not found`:** in **Ubuntu**, run `sudo apt update` and `sudo apt install zip unzip`.
- **A command seems stuck inside `less`:** press **q**. Inside `nano`, press **Ctrl + X** to exit; it will ask whether to save unsaved changes.

## 7. Further study

- [Microsoft Learn — Install WSL](https://learn.microsoft.com/en-us/windows/wsl/install): installation, supported Windows versions, and WSL 2 checks.
- [Microsoft Learn — Set up a WSL environment](https://learn.microsoft.com/en-us/windows/wsl/setup/environment): user account, package updates, and how Windows and Linux work together.
- [Microsoft Learn — Working across file systems](https://learn.microsoft.com/en-us/windows/wsl/filesystems): where to store projects and how to open Linux folders in Explorer.
- [Ubuntu — The Linux command line for beginners](https://ubuntu.com/tutorials/command-line-for-beginners): a longer beginner tutorial with examples.
- [Ubuntu Server — Command line cheat sheet](https://ubuntu.com/server/docs/reference/cli-cheatsheet/): a quick reference when you forget a command.
- [GNU gzip manual](https://www.gnu.org/software/gzip/manual/): details on `gzip` and `gunzip`.

For any unfamiliar command, `command --help` is a useful first stop. Ubuntu also includes manual pages for many tools, for example `man grep`; press **q** to close the manual.

## Quick reference for the 15 commands

- **`ls`** — list the files in a sequencing run folder.
- **`mkdir`** — create a folder for one analysis or sample.
- **`cd`** — enter the folder you want to work in.
- **`cat`** — display a very small FASTA or metadata file.
- **`head`** — inspect the first lines of a report or FASTQ.
- **`tail`** — inspect the last lines of an analysis log.
- **`less`** — scroll through a longer text report without editing it.
- **`nano`** — write a short notes file or edit a small text file.
- **`cp`** — make a copy before changing a file.
- **`mv`** — rename a sample file or move it into a folder.
- **`grep`** — find a sample ID, FASTA header, or error message.
- **`gzip`** — compress one file to `.gz`.
- **`gunzip`** — restore a `.gz` file.
- **`zip`** — bundle a few reports into a `.zip` archive.
- **`unzip`** — extract a `.zip` archive into a separate folder.
