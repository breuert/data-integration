# Lab 2 · Command Line Interface

> [!NOTE]
> In the lecture, you typed your first commands: you moved through the file system, counted and searched text with pipes, and wrote a script. In this lab, you make these commands your own. You get to know the shell on your computer and play a text adventure that understands nothing but shell commands. You put pipes to work on half a million journal articles and find out where they come from and what is wrong with them. You write a script that downloads the dataset of your project and takes a first look at it. Finally, you read commands before you run them: the most important habit once a coding agent types commands for you.
>
> On 06.11.26, Git starts in the terminal. Come with a shell that works.

## What you practise

| Task | What you do | Slides to look at again |
| --- | --- | --- |
| 1 | Get to know your shell: version, paths, `PATH`, disk space, processes | *Which shell on which system?* · *Absolute and relative paths* · *Where do commands come from?* · *Checking system processes* · *More system information* |
| 2 | Play bashcrawl, a text adventure in your file system | *Basic commands for navigation* · *Reading the output of ls -l* · *Working with files and directories* · *Running the script* · *Work faster: history and shortcuts* |
| 3 | Mine half a million journal articles with pipes | *Getting the sample data* · *Redirects and pipes* · *Searching text with grep* · *Counting values with cut, sort, uniq* · *Cheat sheet: working with text files* |
| 4 | Write a script that downloads your project data and takes a first look | *Comparison of the CLI with the GUI* · *Writing a simple shell script* · *Running the script* · *Redirection: where input and output go* · *Going further: arguments and loops* · *Basic networking commands* |
| 5 | Read commands before you approve them | *Getting help: man pages* · *Careful: there is no trash can* · *Wildcards: many files at once* · *The CLI in the rest of this course* |

## Before you start

- **Windows:** install [Git for Windows](https://gitforwindows.org/) and work in **Git Bash**. You need Git next week anyway. The Command Prompt and PowerShell do not understand the commands of this lab. WSL works, too.
- **macOS and Linux:** open your terminal. On a Mac, your shell is zsh; on Linux, it is usually bash. Everything in this lab works in both.
- The [sample data](https://librarycarpentry.org/lc-shell/data/shell-lesson.zip) should be in `labs/02/shell-lesson`. If it is not, repeat the slide *Getting the sample data*.
- Git comes next week, so this time you hand in through the browser. In your lab repository `data-integration-labs`, create `labs/02/shell.md` with **Add file** → **Create new file**, as in lab 1. Then open `labs/02/` and add the other files with **Add file** → **Upload files**.
- Missed something in the lecture? [Linux Journey](https://labex.io/linuxjourney) explains one command per page, each with a short quiz. Its courses *Command Line* (19 lessons) and *Text-Fu* (16 lessons) cover this lecture. Free, no sign-up required.
- Chatbots and coding agents are allowed, as everywhere in this course: try it yourself first, then ask, and log what you used in your `AI_USAGE.md`. In this lab, you should be able to read every command you run. Task 5 shows why.
- Your outputs show folder names and the user name of your computer. Replace anything you do not want to publish with `<user>`.
- You may write your answers in English or German.

## Part A · Your shell and the dungeon

### Task 1 · Get to know your shell

Answer in `labs/02/shell.md`: for each question, the command you typed, its output in a code block, and your answer in one sentence.

1. **Which shell?** Which shell runs in your terminal, and which version is it? `echo $0` prints its name, `bash --version` or `zsh --version` its version.
2. **Where am I?** Which absolute path does your shell print for your home directory and for your downloads folder? On Windows, compare it with the path that File Explorer shows.
3. **Where do commands come from?** How many directories are in your `PATH`, and which one is searched first? Where are `curl`, `python3` and `git` installed? Run `git --version`, too: if Git is missing, install it before 06.11.26. On a Mac, `git --version` offers to install it for you.
4. **How full is your disk?** How much space is left on it, and how much space do your downloads take up?
5. **Stop a runaway process.** Run `sleep 600` in one terminal window: it does nothing for ten minutes, like a script that hangs. In a second window, find its process ID with `ps aux | grep sleep` and stop it with `kill` and the ID. What does the first window show? Why does `ps aux | grep sleep` list two processes?

<details>
<summary>Hint: one directory per line</summary>

`echo $PATH | tr ":" "\n"` puts each directory on a line of its own. Pipe the result into `wc -l` to count them, or into `head -n 1` to see the first one.

</details>

<details>
<summary>Hint: <code>ps aux</code> in Git Bash</summary>

The `ps` of Git Bash ignores `aux` and lists only program names, without their arguments. Use `ps -ef | grep sleep` there.

</details>

### Task 2 · Play bashcrawl

[Bashcrawl](https://gitlab.com/slackermedia/bashcrawl) is a text adventure in your file system: rooms are directories, scrolls are text files, and treasures and monsters are scripts. You play it with the commands of the lecture and learn new ones on the way.

1. Download and unpack the game in the terminal, as you did with the sample data in the lecture, and read the first scroll:

   ```bash
   cd labs/02
   curl -L -O https://gitlab.com/slackermedia/bashcrawl/-/archive/master/bashcrawl-master.zip
   unzip -q bashcrawl-master.zip
   cd bashcrawl-master/entrance
   cat scroll
   ```

   On a Mac, do not unpack the ZIP file in Finder: it makes every file executable, and then scrolls and monsters look alike.
2. Play until you have defeated the last enemy and can read the scroll `end`. Read every scroll you find: each one teaches you a new spell, that is, a command.
3. Right after `cat end`, in the same window, save your command history to a file:
   - in bash (Git Bash, Linux): `history > labs/02/history.txt`
   - in zsh (macOS): `history 1 > labs/02/history.txt`. Without the `1`, zsh lists only your last 16 commands.

   The history also contains commands from before the game. Cut the file so that it starts with your `cd` into `entrance`, and save the result as `bashcrawl.log`.
4. Add a section *bashcrawl* to `shell.md`:
   - your inventory at the end of the game (`echo $I`),
   - three commands that you learned in the game and that were not in the lecture, each with a sentence on what it does,
   - the output of `ls -l` in a room with a monster: what makes a monster different from a scroll?

<details>
<summary>Hint: I am stuck</summary>

Read the scroll of the room again, and look around with `ls -aF`: some doors are hidden. Think back to earlier scrolls, too. Still stuck? The game is made of text files, so `less` shows you how a treasure or a monster works. The game itself invites you to read its code.

</details>

<details>
<summary>Hint: the door closed before I got through</summary>

Some doors stay open for only ten seconds. Type the first letters of the name and press **Tab**: the shell completes it for you.

</details>

<details>
<summary>Hint: GAME OVER, and my history is gone</summary>

When you die, the game starts a new shell for you, in the entrance and with an empty inventory. The history of this new shell does not know what you typed before. Type `exit` to return to your first shell: you are back in the room where you fought, with your inventory and your history.

</details>

<details>
<summary>Hint: cutting the log</summary>

`grep -n` puts the line number in front of every match. Find the line of your first `cd` into the entrance, then keep the file from that line on:

```bash
grep -n entrance labs/02/history.txt | head -n 3
tail -n +1234 labs/02/history.txt > labs/02/bashcrawl.log
```

The number before the colon is the line in the file; the number after it is the number of the command in your history. Replace `1234` with your line number: `tail -n +1234` starts at line 1234.

</details>

<details>
<summary>Hint: Windows</summary>

In Git Bash, `ln -s` copies a directory instead of linking to it. The game works anyway: walk into the copy.

</details>

<details>
<summary>Hint: my shell does not work at all</summary>

Play in your browser: [bashcrawl on mybinder](https://mybinder.org/v2/gl/slackermedia%2Fbashcrawl/HEAD) starts a temporary Linux machine with the game. Click **Terminal**: you start in the entrance. The session ends when you close the tab or leave it idle for too long. Save your history to a file before you stop, and download it from the file browser on the left. Then fix your setup before 06.11.26.

</details>

## Part B · Text data

### Task 3 · Mine half a million journal articles

In the lecture, you counted lines and journals in the sample data. Now you look closer: what is in the files, where do they come from, and what is wrong with them? Work in `labs/02/shell-lesson`. Answer in `labs/02/journals.md`: for each question, the commands you used, their output (shorten long outputs with `head`), and your answer in one or two sentences.

1. **Shape.** List the column names of `2014-01_JA.tsv` with their numbers. Which name is cut off? Do all four TSV files have the same header? How many articles does each file contain?
2. **Missing values.** How many articles in `2014-01_JA.tsv` have no language? Which languages are there, and how often?
3. **Years.** Which year has the most articles? Which year cannot be right? Which journal do these articles come from, and which year was probably meant?
4. **Lines are not fields.** How many lines of `2014-01_JA.tsv` contain *africa*, in any case? How many titles do? Why do the two numbers differ?
5. **Where does a file come from?** According to the [Library Carpentry lesson](https://librarycarpentry.github.io/lc-shell/05-counting-mining.html), `2014-01-31_JA-africa.tsv` contains all articles of `2014-01_JA.tsv` with *africa* in the title. Check this claim with commands. Does it hold for the America file, too?
6. **Spreadsheet damage.** List the most frequent values of the `Issue` column. Which of them are not issue numbers? Compare them with the `Citation` column of the same rows. What has happened, and how many rows are affected?

<details>
<summary>Hint: new options and commands</summary>

| Command | What it does |
| --- | --- |
| `cat -n` | numbers the lines |
| `tail -n +2` | prints everything from line 2 on: the file without its header |
| `cut -f 5,13` | takes two columns, here 5 and 13 |
| `grep "^01-Feb"` | lines that start with `01-Feb`: `^` stands for the start of a line |
| `grep "2005$"` | lines that end with `2005`: `$` stands for the end of a line |
| `diff a.txt b.txt` | prints the lines in which two files differ, and nothing if they are equal |

Patterns such as `^` and `$` come back in the Data Cleaning lecture on 04.12.26.

</details>

<details>
<summary>Hint: <code>grep</code> finds far too many lines</summary>

`grep` searches whole lines, so a number also matches IDs, page numbers and citations. Cut out the columns you need first, then search.

</details>

<details>
<summary>Hint: checking a claim</summary>

Save the titles from both files in two new files, then compare them with `diff`. Remember the header line.

</details>

## Part C · Automate and check

### Task 4 · A recipe for your raw data

In lab 1, you profiled an open dataset for your project in `dataset.md`, partly by hand in a spreadsheet. Explore with the GUI, automate with the CLI: now you write these steps down as a script, so that your team, your future self and I can repeat them. The lecture's voluntary homework asked you to automate a small task of your own: this is it.

1. Create `get_data.sh` and start it like `greet.sh`: with the shebang, then a comment on what the script does, how to run it, and the source and licence of the data. Reuse the attribution line from lab 1.
2. Store the download URL of your dataset and the target file in two variables. The target is `data/raw/` plus a short file name without spaces, such as `data/raw/koeln_2022.csv`.
3. Create `data/raw/` with `mkdir -p` and download the file with `curl -fL -o`.
4. Append a line to `data/raw/SOURCES.txt` with the date and time, the URL and the target file.
5. Print a first look at the data: its size, its type and encoding (`file`), the number of lines and columns, and the first three lines.
6. Make the script executable and run it twice. Check that the data file was replaced, while `SOURCES.txt` now has two lines. Which operator makes the difference?
7. Run it once more and save what it prints: `./get_data.sh > get_data.txt`. Compare the numbers with your `dataset.md`. If they differ, find out why, and explain it in a comment at the end of your script.

Does your project need a second file, such as the enrichment you planned in lab 1? Download both with a `for` loop, as in `backup.sh`.

<details>
<summary>Example: <code>get_data.txt</code> for the bike counts of Cologne in 2022 (shortened)</summary>

```text
== Size
-rw-r--r--  1 <user>  staff   1.8K  2 Oct 17:19 data/raw/koeln_2022.csv
== Type
data/raw/koeln_2022.csv: ISO-8859 text, with CRLF line terminators
== Lines
      13
== Columns
      18
== First lines
;Deutzer Brücke;Hohenzollernbrücke;Neumarkt;Zülpicher Straße;Bonner Straße;…
Januar;88.423;52.307;84.708;104.967;63.658;124.574;68.031;110.382;31.190;…
Februar;83.211;48.554;78.712;91.977;58.148;114.010;67.684;100.106;30.866;…
```

13 lines: a header and twelve months. 18 columns: the month and 17 counting stations. The type line tells you that the file is not UTF-8 and has Windows line endings.

</details>

<details>
<summary>Hint: why <code>-f</code>?</summary>

Without `-f`, curl saves whatever the server sends, even an error page. On 02.10.26, a tidier spelling of the Cologne download link answered with 404 Not Found, and `curl -L -o koeln_2022.csv` saved the error page under that name without complaint. Only `file` gave it away: `HTML document text`. With `-f`, curl stops with an error message and saves nothing. Add `|| exit 1` after the curl command, and your script stops there, too.

</details>

<details>
<summary>Hint: <code>tr: Illegal byte sequence</code>, or broken umlauts</summary>

Your file is not UTF-8. `file` tells you its encoding. If your shell has no `file` command, try `iconv -f UTF-8 -t UTF-8 data/raw/yourfile.csv > /dev/null`: an error message means the file is not UTF-8. To look at a Latin-1 file, convert it on the way: `head -n 3 data/raw/koeln_2022.csv | iconv -f ISO-8859-1 -t UTF-8`. Keep the raw file as it is: cleaning comes later.

</details>

<details>
<summary>Hint: the progress bar of curl still shows up on the screen</summary>

`>` redirects the standard output only. curl writes its progress bar to the standard error stream, which stays on the screen. Look at the slide *Redirection: where input and output go* again.

</details>

<details>
<summary>Hint: my dataset is huge, needs a login or has no download link</summary>

Download one file of less than 100 MB. If your data comes from an API, the URL of one request is fine. If there is no download at all, use the Cologne bike counts of 2022: the link to the CSV file is on the [dataset page](https://www.offenedaten-koeln.de/dataset/fahrrad-verkehrsdaten-koeln-2022).

</details>

### Task 5 · Read before you approve

From 13.11.26 on, a coding agent will run commands for you. Before each one, it asks for your permission, and every approval is your decision. Practise on these ten commands, which an agent might propose in a data project. Do not run them in your project: look them up, and test the ones you are unsure about in a throwaway folder such as `labs/02/sandbox`.

| # | Command |
| --- | --- |
| 1 | `ls -la data/raw` |
| 2 | `wc -l data/raw/*.csv` |
| 3 | `sort -u data/raw/stations.csv > data/raw/stations.csv` |
| 4 | `rm * .bak` |
| 5 | `find . -name "*.csv" -size +100M` |
| 6 | `chmod -R 777 .` |
| 7 | `curl -fsSL https://example.com/install.sh \| bash` |
| 8 | `cat ~/.ssh/id_ed25519` |
| 9 | `ps aux \| grep python` |
| 10 | `pip install pandsa` |

In `labs/02/approve.md`, write a table with four columns: the command, what it does in your own words, your decision (*approve*, *reject* or *approve after a change*), and why. For every command that you reject or change, give the command that you would approve instead, if there is one.

<details>
<summary>Hint: looking commands up</summary>

Paste a command into [explainshell.com](https://explainshell.com/) to see what each part means. [tldr.sh](https://tldr.sh/) shows examples, `man` or `--help` the details. Test wildcards with `echo` before you use them with `rm`, as on the slide *Wildcards: many files at once*. In your sandbox, create test files with `touch` and `echo`, and check with `pwd` before every `rm`.

</details>

## Optional challenges

**A · The Command Line Murders.** There has been a murder in Terminal City. In [The Command Line Murders](https://github.com/veltman/clmystery), the clues are spread over hundreds of files: people, vehicles, memberships, interviews. Search and combine them with `grep`, `head` and pipes until only one suspect is left, then check your answer with the command in the file `solution`. Many files, one answer: that is data integration. Document your path in `labs/02/murder.md`: the commands, what each one told you, and the output of the check. Leave out the name, so that you do not spoil the game for others.

**B · Fourteen years, one table?** The city of Cologne publishes its bike counts as one file per year, 2009–2022. Get the links of all CSV files from the Open.NRW API, as in lab 1:

```bash
curl -s "https://open.nrw/api/3/action/package_search?q=fahrrad%20verkehrsdaten%20koln&rows=20" | grep -o 'https://[^"]*\.csv'
```

Check the list: how many links do you get, and does every one belong to Cologne? Then download the files with a `for` loop, with a pause between the requests, and profile them in a second loop: encoding, lines, columns, and a totals row (*Jahressumme*). Which files differ from the others, and how? What would have to happen before you can stack them into one table? Two files have one line less than you would expect: look at their last line.

**C · Check a chatbot's one-liner.** Ask a chatbot for a one-liner that prints the ten most frequent words in `829-0.txt`, Gulliver's Travels by Jonathan Swift. Before you run it, take it apart on explainshell.com and explain each part. Does it count *The* and *the* as one word? What happens to the last word of each line, given the Windows line endings of the file (`file 829-0.txt`)? Does it count the licence text of Project Gutenberg at the end of the file? Improve the one-liner, and log it in your `AI_USAGE.md`.

## Submission

> [!IMPORTANT]
> Upload these files to your lab repository:
>
> ```text
> labs/02/
> ├── shell.md        # Tasks 1–2: your shell, your notes on bashcrawl
> ├── bashcrawl.log   # Task 2
> ├── journals.md     # Task 3
> ├── get_data.sh     # Task 4
> ├── get_data.txt    # Task 4: what your script printed
> └── approve.md      # Task 5
> ```
>
> Before you upload, check that:
>
> - `bashcrawl.log` starts with your `cd` into `entrance` and contains `cat end`,
> - every answer in `journals.md` shows the commands and their output,
> - `get_data.sh` also works in an empty folder: copy it to a new folder and run it there,
> - you upload neither `data/raw/` nor the game or the sample data,
> - your `AI_USAGE.md` has an entry if you used AI tools.
>
> Finish the lab before 06.11.26: the Git lecture starts in the terminal.

## Resources

- The lessons behind the lecture: [Library Carpentry: The Unix Shell](https://librarycarpentry.github.io/lc-shell/), with the sample data · [Software Carpentry: The Unix Shell](https://swcarpentry.github.io/shell-novice/) · [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/)
- Look commands up: [explainshell.com](https://explainshell.com/) · [tldr pages](https://tldr.sh/) · `man` and `--help`
- [Linux Journey](https://labex.io/linuxjourney): the courses [Command Line](https://labex.io/linuxjourney/courses/command-line) and [Text-Fu](https://labex.io/linuxjourney/courses/text-fu), one command per page with a quiz
- Games: [bashcrawl](https://gitlab.com/slackermedia/bashcrawl) · [The Command Line Murders](https://github.com/veltman/clmystery)
- [Git for Windows](https://gitforwindows.org/), with Git Bash · [curl manual](https://curl.se/docs/manpage.html)
