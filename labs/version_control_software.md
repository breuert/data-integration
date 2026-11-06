# Lab 3 · Version Control Software

> [!NOTE]
> In the lecture, you made your first commits in a folder called `desk` and watched branches split and merge in a graph. In this lab, you use Git for real. You predict commit graphs before a visualizer draws them. You bring your lab repository to your computer: so far, you made every commit in the browser. You put your download script from lab 2 under version control, one small step at a time, and rescue it when something goes wrong. You merge branches and resolve a conflict in a data file. You read the history of a real dataset that changes every month. Finally, you write your team's roadmap the way you will work all semester: through issues and pull requests.
>
> Your roadmap, milestone 1, is due on 13.11.26. On the same day, you let a coding agent work in a Git repository for the first time, and Git is your safety net.

## What you practise

| Task | What you do | Slides to look at again |
| --- | --- | --- |
| 1 | Predict commit graphs, then check them in a visualizer | *Git’s commit history as a DAG* · *What is merging in Git?* · *Demo: branching & merging* · *Undoing things* |
| 2 | Configure Git, clone your lab repository, push your first commit | *Before you start: configure Git once* · *Cloning a repository* · *Authenticating with GitHub* · *Viewing your history with git log* · *Pushing your commits* |
| 3 | Put your download script under version control, one small commit at a time | *Git is like a desk* · *Ignoring files with .gitignore* · *Best practices* · *Version control for data projects* |
| 4 | Rescue your work with the right undo command | *Undoing things* · *What is a commit hash?* |
| 5 | Branch, merge and resolve a conflict in a CSV file | *What is a branch?* · *Git branch commands overview* · *Handling merge conflicts* · *How to resolve merge conflicts* |
| 6 | Read the history of a dataset | *Viewing your history with git log* · *What is a commit hash?* · *Version control for data projects* |
| 7 | Write your roadmap with your team, through issues and pull requests | *Common workflow* · *Syncing with remote repositories* · *Pull requests and forks* · *Git workflow example* · *Best practices for branching and merging* |

## Before you start

- **Git and a shell.** You checked `git --version` in lab 2. On Windows, work in Git Bash; on macOS and Linux, in your terminal.
- **An editor.** Git opens an editor for some of its messages, and conflicts are easier to resolve in one. [VS Code](https://code.visualstudio.com/) is free and marks conflicts with buttons. You connect it to Git in Task 2.
- **A login for the command line.** GitHub does not accept your account password in the terminal (*Authenticating with GitHub*). On macOS and Linux, install the [GitHub CLI](https://cli.github.com/): its website has an installer for macOS and instructions for Linux, and `brew install gh` works, too. On Windows, Git Credential Manager comes with Git for Windows and opens the browser at your first push. If you prefer the GitHub CLI there, too, `winget install --id GitHub.cli` installs it.
- **Your repository from labs 1 and 2.** You need your lab repository `data-integration-labs`, with `labs/02/get_data.sh` in it. No lab repository yet? Create it as in lab 1, Task 2. No script? Task 3 has a template.
- **One folder, side by side.** Clone every repository next to each other. Never clone a repository into another one.
- **From now on, you hand in with `git push`**, not by uploading files.
- Chatbots and coding agents are allowed, as everywhere in this course: try it yourself first, then ask, and log what you used in your `AI_USAGE.md`. Read every Git command before you run it, as in lab 2's Task 5: `git restore`, `git reset --hard` and `git push --force` can throw work away for good.
- Every commit stores a name and an e-mail address, and `git push` publishes both. Task 2 shows you how to keep your address private.
- You may write your answers in English or German.

## Part A · The commit graph

### Task 1 · Predict, then run

Open [Visualizing Git](https://git-school.github.io/visualizing-git), the tool from the slide *Demo: branching & merging*, and type commands into the field *enter git command* at the bottom left. It differs from Git on your computer in four ways:

- It calls the first branch `master`. Read it as `main`.
- It knows `git checkout`, but not `git switch`.
- Branch names with a slash, such as `feature/clean`, break it: the branch never moves. Use names without a slash here.
- `git reset` always works like `git reset --hard`.

`undo` takes back your last command, and `clear` starts over.

Play the four rounds in order, without `clear` in between. Before each round, write down your prediction: which commits will appear, which of them will have two parents, and where `master`, the other branches and `HEAD` will point. Then type the commands, compare, and answer the questions. Keep your predictions and answers in a file `graph.md` for now. In Task 2, it moves into your repository.

**Round 1 · A straight line**

```text
git commit -m "Download the data"
git checkout -b clean
git commit -m "Drop empty rows"
git commit -m "Parse dates"
git checkout master
git merge clean
```

Did `git merge clean` create a new commit? What did the tool answer, and where do `master` and `clean` point now?

**Round 2 · Two lines meet**

```text
git checkout -b plot
git commit -m "Plot cyclists per month"
git checkout master
git commit -m "Describe the data in the README"
git merge plot
git tag v1.0
```

Which commit has two parents? Name both by their messages. Why was this merge not a fast-forward? What would a tag like `v1.0` mark in your project?

**Round 3 · Two ways back**

```text
git commit -m "Delete outliers"
git revert HEAD
git reset HEAD~2
```

What did `git revert HEAD` add to the graph, and what did `git reset HEAD~2` change? Commits that no branch reaches are drawn with dashed lines: are they gone? Which of the two commands may you use for commits that you have already pushed, and why?

**Round 4 · Lost and found**

```text
git checkout v1.0~1
git commit -m "Try a log scale"
git checkout master
git reflog
```

What does the line above the graph say after `git checkout v1.0~1`? What happens to *Try a log scale* when you switch back to `master`? Find its hash in the output of `git reflog`, and keep the commit with `git branch logscale <hash>`.

When you have finished, take a screenshot of the whole graph and save it as `git-graph.png`.

<details>
<summary>Hint: what do <code>HEAD~2</code> and <code>v1.0~1</code> mean?</summary>

`~1` means one commit back, `~2` two commits back, always following the first parent. `HEAD~2` is the commit two steps before the one you are on, and `v1.0~1` the commit before the one tagged `v1.0`. Git on your computer understands the same notation.

</details>

<details>
<summary>Hint: I cannot find the hash in the reflog</summary>

The reflog lists every move of `HEAD`, the newest first. The tool shows no commit messages there, but the entry *checkout: moving from … to master* names the commit you just left.

</details>

## Part B · Your lab repository on your computer

### Task 2 · Clone, commit, push

1. **Keep your address private.** On GitHub, open **Settings** → **Emails** and select **Keep my email addresses private** and **Block command line pushes that expose my email**. GitHub then shows your `noreply` address, in the form `12345678+username@users.noreply.github.com`. It links your commits to your profile without publishing your address.
2. **Configure Git once**, as on the slide *Before you start: configure Git once*, with two more settings:

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "12345678+username@users.noreply.github.com"
   git config --global init.defaultBranch main
   git config --global pull.rebase false
   git config --global core.editor "code --wait"
   git config --global --list
   ```

   With `pull.rebase false`, `git pull` does what the slide *Syncing with remote repositories* says: fetch, then merge. Without it, Git refuses to pull once your branch and the remote have both moved on. `code --wait` makes VS Code the editor that Git opens for its messages. On a Mac, first run *Shell Command: Install 'code' command in PATH* from VS Code's Command Palette. Prefer to stay in the terminal? Write `nano` instead.
3. **Log in.** Run `gh auth login`: choose GitHub.com and HTTPS, let it authenticate Git with your GitHub credentials, and log in with your browser. `gh auth status` shows whether it worked. On Windows without the GitHub CLI, skip this step: Git opens the browser at your first push.
4. **Clone.** Copy the HTTPS URL of your lab repository from the green **Code** button and clone it to your disk, e.g, in `~`:

   ```bash
   cd ~
   git clone https://github.com/<username>/data-integration-labs.git
   cd data-integration-labs
   ```

5. **Read your history.** Look around with `ls -a`, `git status`, `git remote -v` and `git log --oneline`. Then create the folder `labs/03` with `mkdir labs/03`, and in it a file `git.md`. Answer there, in a section *Setup*, with the commands and their output:
   - How many commits does your repository have, and where did you make them?
   - GitHub wrote your commit messages, such as *Add files via upload*. Which messages do you find? Rewrite two of them so that they say what changed and why. Write the new messages into `git.md`, but do not change the history.
   - Which e-mail address do your commits carry? `git log --format="%h %ae %s"` shows it. If it is not your `noreply` address, it is now public, in every clone of your repository. What does step 1 change, and what can it not change?
   - Which files did you change most often? Combine `git log --name-only --format=""` with `sort` and `uniq -c` from lab 2.
6. **Your first commit from your computer.** Move `graph.md` and `git-graph.png` from Task 1 into `labs/03/`. Run `git status`, `git add labs/03` and `git status` again, then commit with a message that says what you added. Run `git status`, `git push` and `git status` once more, and copy what `git status` said before and after the push into `git.md`. Is your avatar next to your commit on GitHub?

<details>
<summary>Hint: stuck in <code>git log</code>, or in an editor you cannot leave</summary>

`git log` shows long output page by page: press the space bar to scroll and `q` to quit. If Git opens Vim, press `Esc`, type `:wq` and press `Enter` to save and close. Then set `core.editor` as in step 2, so it does not happen again.

</details>

<details>
<summary>Hint: Git asks for a password, or GitHub rejects my push</summary>

GitHub does not accept your account password for Git. Log in with `gh auth login` as in step 3, or use a personal access token as your password (*Authenticating with GitHub*).

If GitHub rejects your push because it would publish a private e-mail address, your `user.email` is not the `noreply` address. GitHub checks only your most recent commit. Set the `noreply` address as in step 2, give that commit the new address, and push again:

```bash
git commit --amend --reset-author --no-edit
git push
```

</details>

<details>
<summary>Hint: SSH instead of HTTPS</summary>

Create a key pair with `ssh-keygen -t ed25519` and add the public key, the file `~/.ssh/id_ed25519.pub`, on GitHub under **Settings** → **SSH and GPG keys**. Then clone with the SSH URL from the **Code** button, which starts with `git@github.com:`. The file without `.pub` is your private key: remember command 8 of lab 2's Task 5, and never show it to anyone.

</details>

<details>
<summary>Hint: no avatar next to my commit</summary>

GitHub links a commit to your profile by its e-mail address. Compare `git log -1 --format=%ae` with the `noreply` address in your settings. If you have not pushed yet, fix it with `git commit --amend --reset-author --no-edit`.

</details>

<details>
<summary>Hint: <code>git status</code> lists a file <code>.DS_Store</code></summary>

The Finder on a Mac leaves a hidden file `.DS_Store` in folders that you open. It does not belong in your repository: create a `.gitignore` at the top of your repository with the line `.DS_Store`, as on the slide *Ignoring files with .gitignore*, and commit it. If you have already committed a `.DS_Store`, remove it from the repository with `git rm --cached labs/03/.DS_Store`.

</details>

### Task 3 · Commit step by step

Git is made for text: your scripts go into the repository, your raw data does not (*Version control for data projects*). Work in your lab repository.

1. **Copy your script** from lab 2 and commit it unchanged:

   ```bash
   cp labs/02/get_data.sh labs/03/
   cd labs/03
   chmod +x get_data.sh
   git add get_data.sh
   git commit -m "Copy the download script from lab 2"
   ```

   Your upload in lab 2 lost the permission to execute the script, and `chmod +x` gives it back.
2. **Keep the raw data out.** Run `./get_data.sh`, then `git status`: Git sees `data/`. Create the file `labs/03/.gitignore` with the single line `data/raw/` and run `git status` again. `git status --ignored` lists what Git now ignores, and `git check-ignore -v data/raw/*` names the rule. Commit the `.gitignore` on its own.
3. **One change per commit.** Make two unrelated changes to `get_data.sh` before you commit:
   - at the top, improve a comment: what the script does, or how to run it,
   - at the end, print a checksum of the downloaded file. It changes whenever the content of the file changes, so you can tell whether your source has changed since your last download:

     ```bash
     echo "== Checksum"; git hash-object --no-filters "$out"
     ```

     Use the name of your own variable instead of `out`. `git hash-object` computes the same kind of hash that Git computes for every file you commit (*What is a commit hash?*).

   Run the script, and look at both changes with `git diff`. Then commit them separately, as the slide *Best practices* asks. `git add -p get_data.sh` shows one change at a time and asks whether to stage it: answer `y` for the comment and `n` for the checksum. Check with `git diff --staged`, and commit. Then stage and commit the checksum.
4. **Say how to rebuild the data.** Write `labs/03/README.md`: what this folder contains, and how anyone can rebuild `data/raw/`, with the command, the source and its licence. Commit it, and push.
5. In `git.md`, in a section *Step by step*, add the output of `git log --oneline -- .` and of `git status --ignored`, both run in `labs/03`. Answer: why does the `.gitignore` belong in the repository, but `data/raw/` does not? What does a teammate need to get your raw data?

<details>
<summary>Hint: no <code>get_data.sh</code> from lab 2</summary>

Start from this script. It downloads the bike counts of Cologne in 2022, the example from labs 1 and 2:

```bash
#!/bin/bash
# get_data.sh: downloads the bike counts of Cologne in 2022.
# Usage: ./get_data.sh
#
# Source:  Stadt Köln, Fahrrad Verkehrsdaten Köln 2022,
#          https://www.offenedaten-koeln.de/dataset/fahrrad-verkehrsdaten-koeln-2022
# Licence: Datenlizenz Deutschland – Zero – Version 2.0

url="https://www.offenedaten-koeln.de/sites/default/files/distribution/Radverkehr%2520f%25C3%25BCr%2520Offene%2520Daten%2520K%25C3%25B6ln%25202022.csv"
out="data/raw/koeln_2022.csv"

mkdir -p data/raw
curl -fL -o "$out" "$url" || exit 1
echo "$(date +"%Y-%m-%d %H:%M") $url -> $out" >> data/raw/SOURCES.txt
```

Save it as `labs/03/get_data.sh`, and continue in step 1 with `cd labs/03`. The URL is spelled exactly like this on the dataset page.

</details>

<details>
<summary>Hint: one <code>.gitignore</code> for the whole repository?</summary>

A pattern with a slash at the start or in the middle counts from the folder of its `.gitignore`. So `data/raw/` in a `.gitignore` at the top of your repository ignores `data/raw/` there, but not `labs/03/data/raw/`. Either put a `.gitignore` next to your data, as in this task, or write `**/data/raw/` at the top: `**/` stands for any number of folders.

</details>

<details>
<summary>Hint: <code>git add -p</code> offers both changes at once</summary>

Git groups changes that are close together into one hunk. Press `s` to split the hunk into smaller ones. If that does not work, your two changes are too close: move one of them further away.

</details>

<details>
<summary>Hint: warnings about LF and CRLF on Windows</summary>

Windows ends lines with two characters (CRLF), macOS and Linux with one (LF). Git for Windows converts between them when you commit and check out files, and tells you so. The warning is harmless.

</details>

### Task 4 · Rescue your work

Go through the slide *Undoing things* row by row, on purpose. Work in `labs/03`, and commit your other changes first, so that `git status` says *nothing to commit*. Record each rescue in `git.md`, in a section *Undo*: the commands, what `git status` or `git log --oneline -3` showed, and one sentence on what happened.

1. **Deleted by accident.** Delete your script with `rm get_data.sh`, check `git status`, and bring it back with `git restore get_data.sh`. In lab 2, there was no trash can. Which files could Git not have brought back?
2. **Staged by accident.** Create a file `notes.txt` with a line of text and run `git add .`. Take the file out of the staging area with `git restore --staged notes.txt`. Is it still there? Then delete it.
3. **A typo in the message.** Make a small change, for example in your `README.md`, and commit it with a typo in the message. Fix the message with `git commit --amend -m "…"`. Compare the hash before and after: why did it change, and why is `--amend` safe only before you push?
4. **A bad commit, already pushed.** Break your script: change its download URL to one that does not exist, for example by changing a year in it. Commit and push the change, then run the script: it stops with an error. Undo the commit with `git revert HEAD`, run the script again, and push. What does `git log --oneline -3` show now? Why is `git revert` the right command here, and not `git reset --hard HEAD~1`?

<details>
<summary>Hint: <code>git revert</code> opened my editor</summary>

Git suggests a message, *Revert "…"*, and waits for you to accept it. Save and close the editor. `git revert --no-edit HEAD` accepts the message without opening the editor.

</details>

<details>
<summary>Hint: my script does not stop when the download fails</summary>

Then it saves an error page under the name of your data file. Add `-f` to `curl` and `|| exit 1` after it, as in lab 2's Task 4, in a commit of its own. Then break the URL again.

</details>

## Part C · Branches and history

### Task 5 · Branch, merge, resolve a conflict

Your team joins two sources by country name, but names change. Since 2016, the Czech Republic has had the short name Czechia; Swaziland became Eswatini in 2018, Macedonia North Macedonia in 2019, and Turkey Türkiye in 2022. Two of you work on the same mapping table at the same time. Play both roles, on two branches, in `labs/03`.

1. On `main`, create `countries.csv` and commit it:

   ```csv
   name,iso3
   Czech Republic,CZE
   Germany,DEU
   Macedonia,MKD
   Swaziland,SWZ
   Turkey,TUR
   ```

2. **A fast-forward.** Create a branch with `git switch -c feature/iso2`, add a column `iso2` with the codes `CZ`, `DE`, `MK`, `SZ` and `TR`, and commit. Switch back to `main` and merge the branch. Which word in Git's answer tells you that this was a fast-forward?
3. **Two lines of work.** Create a second branch, `feature/new-names`, give the four countries their new names, and commit. Switch back to `main` and add a column `numeric` with the ISO numeric codes `203`, `276`, `807`, `748` and `792`. Commit. Look at both branches with `git log --oneline --graph --all`.
4. **The conflict.** Merge `feature/new-names` into `main`. Before you fix anything, copy the file as Git left it into `labs/03/conflict.md`, in a code block: outside one, Markdown turns the line above `=======` into a heading. Then look at the file with `cut -d, -f1 countries.csv`. What would a program that reads this CSV file see?
5. **Resolve it.** Edit `countries.csv` so that it has the new names and all four columns, and no conflict markers. Check it: `grep -c -E "^(<<<<<<<|=======|>>>>>>>)" countries.csv` counts the lines that start with a marker and must print 0, and `cut -d, -f4 countries.csv` must show a code in every row. Then complete the merge with `git add countries.csv` and `git commit`. Delete both branches with `git branch -d` and push.
6. In `conflict.md`, add the output of `git log --oneline --graph -8` and answer: the two branches changed different columns, so why did Git report a conflict? Why are *Germany* and the header part of the conflict, although only one branch changed them? How could your team avoid conflicts like this one in a shared data file?

<details>
<summary>Hint: resolving the conflict in VS Code</summary>

VS Code marks the two versions and offers buttons above them. *Accept Current Change* keeps the version of `main`, *Accept Incoming Change* the version of the branch. *Accept Both Changes* puts one under the other, and for a CSV file that is wrong: you would get two headers and ten rows. Here, you have to combine the two versions line by line.

</details>

<details>
<summary>Hint: see what the file looked like before both changes</summary>

`git config --global merge.conflictStyle zdiff3` adds a third section to every conflict, between `|||||||` and `=======`: the lines as they were before both branches changed them. That shows you what each side changed. To see it for the current conflict, run `git checkout --conflict=zdiff3 countries.csv`. This overwrites the file, including anything you have fixed so far.

</details>

<details>
<summary>Hint: I want to start over, or Git refuses to switch branches</summary>

`git merge --abort` takes you back to the state before `git merge`. If Git refuses to switch branches because of uncommitted changes, commit them first. Not ready to commit? `git stash` puts your changes aside, and `git stash pop` brings them back.

</details>

### Task 6 · Read the history of a dataset

Data changes, too, and not only by new rows at the end. [datasets/co2-ppm](https://github.com/datasets/co2-ppm) publishes the monthly mean CO₂ concentration measured at Mauna Loa, Hawaii, since 1958, with data from NOAA. A bot updates it on the first day of every month, and Git has kept every version. Clone it to you disk, next to your lab repository and not inside it. It is less than 1 MB:

```bash
cd ~
git clone https://github.com/datasets/co2-ppm.git
cd co2-ppm
```

Answer in `labs/03/history.md` in your lab repository, with the commands and their output. Shorten long outputs with `head`. All questions are about the file `data/co2-mm-mlo.csv`.

1. **Versions.** How many commits have changed the file? Who made them, and in which years? In which years did nobody update the file, and which was the latest month in it during those years?
2. **The day the data disappeared.** One update deleted almost the whole file. Find it with `git log --stat -- data/co2-mm-mlo.csv`. Show the file as that commit left it, with `git show <hash>:data/co2-mm-mlo.csv`: what would your `get_data.sh` have downloaded that day? Who repaired it, and how many days later? Look at the repair with `git show <hash> -- scripts/process.sh`: which lesson from lab 2 does it apply?
3. **Old numbers change.** Look at the update of 1 October 2026, commit `8c40972`, with `git show --stat 8c40972` and `git diff 8c40972~1 8c40972 -- data/co2-mm-mlo.csv`. How many months did it add, and how many existing months did it change? Which is the oldest month that it changed? Add `--word-diff --word-diff-regex='[^,]+'` to the diff to see which values changed. What does that mean for numbers that you computed from a download in September?
4. **A header that no longer fits.** Compare the first two lines of the file in the version of 1 October 2026 with those of the version of 13 March 2017, commit `af2c0a6`. How many names does the header have, and how many values does a row have? Since which commit has it been that way? On GitHub, find out whether the problem is known, and whether it has been fixed since.
5. **Pin your version.** Write down a URL that will always return the file as it was on 1 October 2026. Take `https://raw.githubusercontent.com/datasets/co2-ppm/main/data/co2-mm-mlo.csv` and replace `main` with the full hash of the commit, which `git rev-parse 8c40972` prints. Test it with `curl -fsSL <url> | tail -n 1`. Then check out the version of the day the data disappeared with `git checkout <hash>`, count its lines, and return with `git switch main`. What does Git call the state in between?

End `history.md` with three sentences: how do you note in your project README which version of a dataset you used? What if the data comes from a portal without Git, as in lab 1?

<details>
<summary>Hint: commands for this task</summary>

| Command | What it does |
| --- | --- |
| `git log --oneline -- <file>` | only the commits that changed this file |
| `git log --format="%h %ad %an %s" --date=short` | hash, date, author and message, one commit per line |
| `git log --stat` | adds to each commit how many lines of which files it changed |
| `git show <hash>:<file>` | the file as it was in this commit |
| `git diff <hash1> <hash2> -- <file>` | the changes between two versions of a file |
| `<hash>~1` | the commit before |

Combine them with `grep`, `cut`, `sort`, `uniq -c` and `wc -l` from lab 2. Lines that a diff removed start with `-`, lines that it added with `+`.

</details>

## Part D · Your team

### Task 7 · Your roadmap, through pull requests

Milestone 1 is your roadmap in the README of your team's repository: topic, data and licences, questions and a plan. Write it the way your team will work all semester (*Common workflow*): nobody pushes to `main`. Every change arrives on a branch, as a pull request that a teammate has read. The grading criterion *Version control and collaboration* looks for exactly this: frequent, meaningful commits and messages; branches, issues and pull requests.

1. **Plan in issues.** Open an issue for each piece of the roadmap and assign it: for example, one for each dataset that you profiled in lab 1, one for the research questions and one for the plan.
2. **Branch.** Each of you clones the team's repository and creates a branch for an issue, for example `git switch -c feature/3-bike-counts` for issue 3.
3. **Commit and push.** Add your dataset from lab 1 as a new row at the end of the table *Data* in `README.md`. Commit it with a message that says what and why, and push your branch with `git push -u origin feature/3-bike-counts`.
4. **Open a pull request.** GitHub offers *Compare & pull request* right after your push. Describe what you changed and why, write `Closes #3` in the description, and request a review from a teammate under *Reviewers*.
5. **Review.** Review at least one pull request of a teammate. Under *Files changed*, comment on at least one line: does the link work, is the licence right? Then finish with *Review changes* and *Approve* or *Request changes*.
6. **Merge.** Once a teammate has approved, the author merges with *Merge pull request*, as a merge commit, and deletes the branch. Check that GitHub has closed the issue.
7. **The conflict.** After the first merge, the other pull requests that added a row at the same place show *This branch has conflicts that must be resolved*. Resolve the conflict on your computer:

   ```bash
   git switch main
   git pull
   git switch feature/3-bike-counts
   git merge main
   # edit README.md: keep all rows, remove the markers
   git add README.md
   git commit
   git push
   ```

   Your pull request updates itself. Ask your reviewer to look again, then merge.
8. **Catch up.** When all pull requests are merged, each of you runs `git switch main`, `git pull` and `git branch -d` with the name of their branch.

In `labs/03/team.md` in your own lab repository, write down the links to your issue, your pull request and the pull request you reviewed, and the output of `git log --oneline --graph -15` in the team's repository. Add three to five sentences: what did your review comment say? Which conflict did you resolve, or why did you have none? Which rule do you agree on as a team?

<details>
<summary>Hint: your README has no roadmap yet</summary>

One of you starts it with this skeleton, the only commit straight to `main`. Everyone else waits for it before they create their branches:

```markdown
# Project title

One sentence: which question does your project answer?

## Team

## Topic

## Data

| Dataset | Publisher | Licence | Formats | Profiled by |
| --- | --- | --- | --- | --- |

## Research questions

## Plan
```

</details>

<details>
<summary>Hint: let GitHub enforce the rule</summary>

Under **Settings** → **Rulesets** → **New ruleset** → **New branch ruleset**, target the default branch and select *Require a pull request before merging*. From then on, GitHub rejects pushes straight to `main`, even yours.

</details>

## Optional challenges

**A · Who renamed Germany?** [datasets/country-codes](https://github.com/datasets/country-codes) puts the country codes of ISO, the UN, FIFA, the IOC and others in one table, a typical lookup table for joining sources. For a while, its column *CLDR display name* did not say *Germany*, but *Jerman*. Clone the repository next to the others and find out when this began and when it ended, with `git log -S Jerman --oneline -- data/country-codes.csv`. `-S` lists the commits that change how often a text occurs. The search finds four commits: what happened in each of them? For how many months was the column wrong?

**B · Contribute through a fork, from the command line.** In lab 1, you could propose a change to this [awesome list](https://github.com/irgroup-classrooms/awesome-data-modeling) in the browser. Now do it the way open source projects work (*Pull requests and forks*): fork the list, clone your fork, and add the original as a second remote with `git remote add upstream <url>`. Create a branch, add a resource from this lab that helped you, push the branch to your fork, and open a pull request to the original. Before you push, run `git fetch upstream` and `git log --oneline main..upstream/main`: has the original moved on since you forked it?

**C · Rebase instead of merge.** The lecture defines *rebase*, but you have not used it yet. In the visualizer, run `clear` and play rounds 1 and 2 of Task 1 again, but replace `git merge plot` with `git checkout plot`, `git rebase master`, `git checkout master` and `git merge plot`. Compare the graph with your screenshot. What happened to *Plot cyclists per month*, and which hash does it have now? Why does the rule *never rewrite history that others have* apply to `git rebase`, too?

## Submission

> [!IMPORTANT]
> Push these files to your lab repository:
>
> ```text
> labs/03/
> ├── graph.md        # Task 1: predictions and answers
> ├── git-graph.png   # Task 1: your graph after round 4
> ├── git.md          # Tasks 2–4: setup, step by step, undo
> ├── .gitignore      # Task 3: contains data/raw/
> ├── README.md       # Task 3: what is here, how to rebuild data/raw/
> ├── get_data.sh     # Tasks 3–4
> ├── countries.csv   # Task 5, merged
> ├── conflict.md     # Task 5
> ├── history.md      # Task 6
> ├── team.md         # Task 7
> └── data/raw/       # your downloads: not committed
> ```
>
> In your team's repository: your row of the roadmap, merged through your pull request.
>
> Before you finish, check that:
>
> - `git status` in your lab repository says *nothing to commit, working tree clean* and *Your branch is up to date with 'origin/main'*,
> - `git log --oneline -- labs/03` shows one commit per step, each with a message that says what and why,
> - neither `data/raw/` nor a clone of `co2-ppm` or of your team's repository has ended up in your lab repository,
> - `grep -c -E "^(<<<<<<<|=======|>>>>>>>)" labs/03/countries.csv` prints 0,
> - `team.md` links your issue, your merged pull request and the pull request you reviewed,
> - your `AI_USAGE.md` has an entry if you used AI tools.
>
> Finish the lab before 13.11.26: your roadmap is due that day, and the Agentic Coding lab starts from a Git repository.

## Resources

- [Pro Git](https://git-scm.com/book/en/v2), the free Git book: chapters 2 (Git basics) and 3 (branching) · [Software Carpentry: Version Control with Git](https://swcarpentry.github.io/git-novice/)
- GitHub Docs: [Setting your commit email address](https://docs.github.com/en/account-and-profile/how-tos/email-preferences/setting-your-commit-email-address) · [About pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) · [Linking a pull request to an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/linking-a-pull-request-to-an-issue) · [Reviewing proposed changes in a pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/reviewing-proposed-changes-in-a-pull-request) · [Resolving a merge conflict using the command line](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-using-the-command-line)
- [GitHub CLI](https://cli.github.com/), with the manual of [`gh auth login`](https://cli.github.com/manual/gh_auth_login) · [VS Code: merge conflicts](https://code.visualstudio.com/docs/sourcecontrol/merge-conflicts)
- [gitignore documentation](https://git-scm.com/docs/gitignore) · [gitignore templates](https://github.com/github/gitignore)
- Practise in the browser: [Visualizing Git](https://git-school.github.io/visualizing-git/) · [Learn Git Branching](https://learngitbranching.js.org/) · [GitHub Skills](https://skills.github.com/), free hands-on courses
- When things go wrong: [Dangit, Git!?!](https://dangitgit.com/), undo recipes in plain language
- The data of Task 6: [NOAA: Trends in atmospheric carbon dioxide](https://gml.noaa.gov/ccgg/trends/) · [datasets/co2-ppm](https://github.com/datasets/co2-ppm), published under ODC-PDDL-1.0
