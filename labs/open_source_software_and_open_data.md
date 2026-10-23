# Lab 1 · Open Source Software and Open Data

> [!NOTE]
> In the lecture, you met the two pillars of this course: open source software and open data, and the licences that make them open. In this lab, you put both to work. You create your GitHub profile and the repository for all your labs, give it a licence, and learn Markdown by writing it. You look inside a real open source project. Then you find an open dataset for your project, check how open and how FAIR it is, and plan what to combine it with.
>
> Your dataset profile is the first section of your roadmap, milestone 1, due on 13.11.26.

## What you practise

| Task | What you do | Slides to look at again |
| --- | --- | --- |
| 1 | Write your GitHub profile README | *Lab: your GitHub profile in Markdown* · *What is GitHub?* |
| 2 | Set up your lab repository and give it a licence | *Anatomy of a data project repository* · *No licence, no permission* · *Choosing a licence* |
| 3 | Build your Markdown cheat sheet | *What is Markdown?* · *Markdown in one slide* |
| 4 | Look inside an open source project | *How open source projects work* · *Who keeps open source alive?* · *Permissive or copyleft?* · *Put the licence where machines can find it* |
| 5 | Find an open dataset for your project | *Where to find open data?* · *Portals have APIs, too* |
| 6 | Profile the dataset | *Is this dataset any good?* · *The 5-star open data scheme* · *Creative Commons licences* · *Licences made for data* · *Give credit: title, author, source, licence* |
| 7 | Review it with the FAIR principles | *The FAIR principles* · *To be findable and accessible* · *To be interoperable and reusable* · *Make your project dataset FAIR* |
| 8 | Plan what to combine it with | *Project ideas: enrich an open dataset* · *The recipe: base + enrichment + analysis* |

## Before you start

- A browser is all you need: everything happens on github.com. The command line follows on 30.10.26, Git on your own computer on 06.11.26.
- New to Markdown? Take the [10-minute tutorial](https://commonmark.org/help/tutorial/) first.
- To edit a file on GitHub, click the pencil icon. The **Preview** tab shows the result before you commit. To edit several files at once, press `.` in your repository: [VS Code for the Web](https://docs.github.com/en/codespaces/the-githubdev-web-based-editor) opens in your browser, free for everyone.
- Chatbots and coding agents are allowed, as everywhere in this course: try it yourself first, then ask, and log what you used in your `AI_USAGE.md` (Task 2). A chatbot explains Markdown well. Check everything it tells you about a licence or a dataset against the source.
- Everything you create in this lab is public. Share only what you are happy for anyone to read: no address, no phone number, no student ID.
- You may write your answers in English or German.

## Part A · Your place on GitHub

### Task 1 · Your profile README

1. Sign up at [github.com](https://github.com/) if you do not have an account yet. Choose a username you would put on a job application: it is part of every link to your work.
2. Create a new public repository named exactly like your username, with **Add README** switched on. This repository is special: GitHub shows its `README.md` at the top of your profile page.
3. Edit the README and introduce yourself in Markdown: a heading with your full name, a few sentences, a list (what you study, which data you are curious about) and at least one link. Check the **Preview** tab, then commit.
4. Open `https://github.com/<username>` and check that your profile shows the README.

Planning to use GitHub Copilot later in the course? Apply for [GitHub Education](https://education.github.com/pack) with your TH Köln e-mail address now: GitHub checks your student status before your benefits start.

<details>
<summary>Hint: parts of my README do not show up</summary>

Look for `<!--` and `-->`: everything between them is an HTML comment and stays invisible. GitHub's template may wrap its suggestions in one. Delete the comment markers, or the whole comment.

</details>

### Task 2 · Your lab repository

You hand in all labs of this semester in one repository. Give it the two files the lecture asks for in every repository: a README and a licence.

1. Create a new public repository named `data-integration-labs`, with a description and **Add README** switched on. Then add a few topics with the gear icon next to *About*. Topics do for a repository what metadata does for a dataset: they make it findable.
2. Add a licence, as shown in the lecture: **Add file** → **Create new file**, name the file `LICENSE`, click **Choose a license template**, pick a licence, fill in the year and your name, click **Review and submit** and commit. The lecture recommends MIT for code.
3. Write the README in Markdown:
   - a title and one sentence on what the repository is for,
   - your name or username, with links to your profile, to the [course repository](https://github.com/breuert/data-integration) and, once it exists, to your team's project repository,
   - a table of the labs with a relative link to each lab folder, starting with `labs/01/`,
   - a section *Licence*: what your `LICENSE` covers, and that data from other sources keeps its own licence.
4. Create `labs/01/cheatsheet.md` for Task 3: **Add file** → **Create new file**, then type the whole path. Each `/` creates a folder.
5. Invite `breuert` as a collaborator: **Settings** → **Collaborators** → **Add people**. That is how I find your repository and check your labs for the bonus points. Then add a link to your lab repository to your profile README.

When you use AI tools in a lab, log them in `AI_USAGE.md` at the top level of this repository. Write one entry per task: the date and the task, the tool and model, your prompt, what you accepted, changed or rejected, and how you checked it.

<details>
<summary>Hint: one licence for code and texts?</summary>

MIT was written for software and its documentation. For texts and data, the lecture recommends CC BY 4.0 (*Code, data, text: the right licence for each*). A common setup: MIT in `LICENSE`, and a sentence in your README that your texts are under CC BY 4.0. Machines read only the `LICENSE` file, though: remember the slide *Put the licence where machines can find it*.

</details>

### Task 3 · Your Markdown cheat sheet

You will write Markdown all semester: READMEs, issues, notebook text, your `AI_USAGE.md`. Build your own reference, and find out which features belong to the Markdown standard and which work only on GitHub.

1. In `labs/01/cheatsheet.md`, write one entry for each feature in the table below: a sentence on what it does, the Markdown source in a code block, and the rendered result right below it.
2. Test every entry in the [CommonMark dingus](https://spec.commonmark.org/dingus/), the reference implementation of the Markdown standard, and on GitHub. Label each entry **CommonMark** if it works in both, or **GitHub** if it works on GitHub only.
3. Group the entries as in the table, and start with a table of contents that links to your sections.
4. End with a section *Pitfalls*: at least three things that did not work the way you expected, and how you fixed them.

| Group | Features |
| --- | --- |
| Text | headings · paragraphs and line breaks · italic, bold and both · strikethrough · inline code · escaping a character such as `*` |
| Lists | unordered, ordered and nested lists · task lists |
| Links and images | inline and reference-style links · a relative link to another file in your repository · a link to a section · an image with alt text · an image that is a link |
| Code | fenced code blocks with syntax highlighting for Python, Bash and JSON |
| Quotes | blockquotes, nested and with formatting · alerts, like the boxes in this lab |
| Tables | a table with aligned columns, and a cell that contains a link or a `\|` |
| Layout | horizontal rules · collapsed sections, like the hints in this lab · comments that do not render |
| GitHub extras | footnotes · emoji shortcodes · @mentions and references to issues · a Mermaid diagram · a formula |

Keep your cheat sheet growing: it is your reference for the rest of the semester.

<details>
<summary>Example entry</summary>

````markdown
### Strikethrough

**GitHub** · Two tildes on each side cross text out.

```markdown
~~no longer true~~
```

~~no longer true~~
````

</details>

<details>
<summary>Hint: how do I show the source of a code block?</summary>

Put a longer fence around it: four backticks around a block with three.

`````markdown
````markdown
```python
print("Hello")
```
````
`````

</details>

<details>
<summary>Hint: my line break disappears</summary>

In a `.md` file, GitHub joins the lines of a paragraph. End a line with a backslash or two spaces, or leave a blank line. In issues, pull requests and comments, GitHub keeps every line break.

</details>

<details>
<summary>Hint: my mentions and issue references do not become links</summary>

Test them in two places: in your file, and in a comment on a new issue in your lab repository (**Issues** → **New issue**). Write down where they work.

</details>

## Part B · Open source

### Task 4 · Look inside an open source project

Choose one project from the course stack and find out how it works: who builds it, who pays for it, and what its licence lets you do. Answer in `labs/01/open-source.md`, with a link to where you found each answer.

| Project | Repository |
| --- | --- |
| pandas | [pandas-dev/pandas](https://github.com/pandas-dev/pandas) |
| NumPy | [numpy/numpy](https://github.com/numpy/numpy) |
| Requests | [psf/requests](https://github.com/psf/requests) |
| JupyterLab | [jupyterlab/jupyterlab](https://github.com/jupyterlab/jupyterlab) |
| Matplotlib | [matplotlib/matplotlib](https://github.com/matplotlib/matplotlib) |
| Git | [git/git](https://github.com/git/git) |
| Python | [python/cpython](https://github.com/python/cpython) |
| VS Code | [microsoft/vscode](https://github.com/microsoft/vscode) |

1. **Licence.** Which licence does the licence file name? Is it permissive or copyleft? What must you do if you copy code from the project into your own?
2. **What machines see.** Open `https://api.github.com/repos/OWNER/REPO` in your browser and find `spdx_id` under `license`. Do the same for your lab repository and for `owid/co2-data` from the lecture. Explain each of the three results in one sentence.
3. **Community files.** Which of `README`, `LICENSE`, `CONTRIBUTING` and `CODE_OF_CONDUCT` does the project have? GitHub lists them under **Insights** → **Community standards**.
4. **People.** How many contributors does it have? Who decides what gets merged?
5. **Money.** Who pays for the work: volunteers, companies, a foundation, donations? Look for a *Sponsor* button, a funding page or the project's website.
6. **Activity.** When was the latest release? How many issues and pull requests are open?
7. **Your way in.** According to its contributing guide, how does a change get in? Does the project mark issues for newcomers, for example with the label *good first issue*?

<details>
<summary>Hint: <code>NOASSERTION</code> or <code>null</code>?</summary>

`null` means that GitHub found no licence file it could read. `NOASSERTION`, which the API calls *Other*, means that GitHub found a licence file but could not match it to a known licence. Open the file to see why. Either way, the licence text counts, not GitHub's label.

</details>

## Part C · Open data for your project

Your project starts from an open dataset. If you already have a team, each of you profiles a different candidate, so you can compare them for milestone 1. If not, pick a topic you care about and bring your profile to your future team. Answer Tasks 5–8 in `labs/01/dataset.md`.

### Task 5 · Find a dataset

1. Search for data on your topic in at least two kinds of sources from the lecture: a city or state portal (Offene Daten Köln, Open.NRW), a national or international portal (GovData, data.europa.eu, Eurostat, World Bank, Our World in Data), or a dataset search engine or repository (Google Dataset Search, Zenodo). Note where you searched and with which terms.
2. **Search like a machine.** Send one search to the API of GovData or Open.NRW, right in your browser. Replace `fahrrad` with your own search term:

   ```text
   https://www.govdata.de/ckan/api/3/action/package_search?q=fahrrad&rows=1
   https://open.nrw/api/3/action/package_search?q=fahrrad&rows=1
   ```

   Note the number of hits (`count`), the title of the first hit, and where its licence is given: for the whole dataset (`license_id`) or for each file (`license` under `resources`).
3. Choose one dataset that you can download in a machine-readable format. If you found it on Kaggle or in a list, follow it back to its original publisher, and work with that source.

<details>
<summary>Hint: reading the API's answer</summary>

Firefox shows JSON as a tree that you can fold and filter. Other browsers may show plain text: look for a pretty-print option. And do not trust every field: on 02.10.26, Open.NRW marked all 499 datasets of the city of Cologne as `"isopen": false`, including those under CC BY 4.0. Read the licence yourself.

</details>

### Task 6 · Profile the dataset

1. **Facts.** Start `dataset.md` with a table: title, publisher (and the original publisher, if different), link or identifier, formats, size (files, rows, columns), coverage (period, area, granularity), last update and update frequency, licence.
2. **Stars.** How many stars does the dataset earn on the 5-star scheme? What is missing for the next star?
3. **Open the data.** Metadata can be wrong, so download one file and look at it, for example in [LibreOffice Calc](https://www.libreoffice.org/), an open source spreadsheet. For two numeric columns, give the minimum, maximum and mean. For one column, count the empty cells. Note anything odd: encoding, separators, units, totals rows. Name the tool you used.
4. **Data dictionary.** Describe five columns: name, meaning, type and unit, an example value. Does the publisher document its columns? Link the documentation, or write that there is none.
5. **Licence.** Is the dataset open according to the Open Definition? What must you do when you reuse it? Write the attribution line (TASL: title, author, source, licence) for your project README.
6. **People.** Does the data contain personal data? If so, how will you avoid or minimise it?

<details>
<summary>Hint: German CSV files</summary>

Many German files are not encoded in UTF-8, use `;` as separator, and write numbers with a decimal comma and a dot for thousands. If umlauts look broken or numbers arrive as text, import the file again and set the character set, the separator and the language in LibreOffice's *Text Import* dialog. For example, the 2022 file of the Cologne bike counters is encoded in Latin-1 and writes 88,423 cyclists as `88.423`.

</details>

<details>
<summary>Hint: which licences are open?</summary>

CC0, CC BY and CC BY-SA, the Open Data Commons licences (PDDL, ODC-By, ODbL) and the Datenlizenz Deutschland 2.0, both Zero and Namensnennung, [conform to the Open Definition](https://opendefinition.org/licenses/). Licences with NC or ND do not, and neither do terms that forbid redistribution. Data under [dl-de/by-2-0](https://www.govdata.de/dl-de/by-2-0) needs a source note with the provider, the licence and a link to the dataset, and changed data must say that it was changed.

</details>

<details>
<summary>Example: the start of a profile</summary>

| | Fahrrad Verkehrsdaten Köln 2022 |
| --- | --- |
| Publisher | Stadt Köln, on [Offene Daten Köln](https://www.offenedaten-koeln.de/dataset/fahrrad-verkehrsdaten-koeln-2022), harvested by [Open.NRW](https://open.nrw/dataset/fahrrad-verkehrsdaten-koln-2022-k) |
| Formats | CSV, plus a link to an online map |
| Size | 1 CSV file of 1,864 bytes: 12 rows (months), one column per counting station (17) |
| Coverage | Cologne, 2022, monthly totals per counting station. One dataset per year from 2009 to 2022, none after that |
| Licence | Datenlizenz Deutschland – Zero – Version 2.0, stated for the dataset and for each file |
| Stars | ★★★: openly licensed, structured, in an open format. Open.NRW's API gives it 3 as well (`opennrw_openness_score` under `extras`) |

</details>

### Task 7 · Review it with the FAIR principles

Rate your dataset against each principle with ✓ (yes), ~ (partly) or ✗ (no), and give your evidence: a link, a quote, or what you tried.

| Principle | Question |
| --- | --- |
| F1 | Does it have a globally unique and persistent identifier, such as a DOI, or at least a stable URL? |
| F2 | Is it described with rich metadata: title, description, publisher, dates, keywords? |
| F4 | Is it indexed in a searchable resource? Where can you find it? |
| A1 | Can you download it with a standard protocol, such as HTTPS or an API? Without a login, or with clear rules for access? |
| I1 | Is it in an open, machine-readable format? |
| I2, I3 | Does it use standard codes, such as ISO 8601 dates, ISO 3166 country codes or official municipality keys? Does it link to related data or to its sources? |
| R1 | Is it described well enough to reuse: a data dictionary, a method? |
| R1.1 | Is the licence clear, and is it in machine-readable metadata? |
| R1.2 | Is its provenance documented: who collected the data, how and when? Is it the original or a copy? |
| R1.3 | Does it follow a standard of its community, such as DCAT-AP, the metadata standard of European data portals? |

Then answer in a few sentences: is your dataset FAIR, is it open, or both? Name three things you will do better when you publish your own dataset for milestone 2.

<details>
<summary>Example: two rows for the Cologne bike counters</summary>

| Principle | Rating | Evidence |
| --- | --- | --- |
| F1 | ~ | A stable URL for each year, such as [offenedaten-koeln.de/dataset/fahrrad-verkehrsdaten-koeln-2022](https://www.offenedaten-koeln.de/dataset/fahrrad-verkehrsdaten-koeln-2022), but no DOI |
| R1.1 | ✓ | The licence URI is in the metadata of the dataset and of each file, for example `license_id` in the [Open.NRW API](https://open.nrw/api/3/action/package_show?id=fahrrad-verkehrsdaten-koln-2022-k) |

</details>

### Task 8 · Plan what to combine it with

Real questions need more than one dataset. Plan how you would enrich yours.

1. **Second source.** Name another open dataset, or web data you could scrape later in the course (for example, a table on Wikipedia), with its link and licence.
2. **Shared key.** Which columns link the two sources: a place, a date, a code? Show two or three key values from each source in a table. Do they match as published? What must happen before they do?
3. **Granularity.** Do both sources have the same level of detail, such as day or month, district or city? How would you align them?
4. **Questions.** Write two or three research questions that you can answer only with both sources.
5. **Licences.** May you combine the two and publish the result? Under which licence, and whom must you credit?
6. **Next steps.** List the steps from the two downloads to the joined table. A Mermaid flowchart from your cheat sheet works well here.
7. **Verdict.** In three sentences: would you build your project on this dataset? What is the biggest risk?

<details>
<summary>Example: one month, two spellings</summary>

The bike counts of the city of Cologne and the monthly climate data of the Deutscher Wetterdienst for station 2667, Köln/Bonn, share the month, but not its spelling:

| Source | The month as published | After cleaning |
| --- | --- | --- |
| Fahrrad Verkehrsdaten Köln 2022 | `Januar`, with the year only in the file name | `2022-01` |
| [DWD monthly climate data](https://opendata.dwd.de/climate_environment/CDC/observations_germany/climate/monthly/kl/historical/), station 2667 | `MESS_DATUM_BEGINN` = `20220101` | `2022-01` |

The bike counts are under Datenlizenz Deutschland Zero, the weather data under CC BY 4.0. You may combine them and publish the result, as long as you credit the Deutscher Wetterdienst.

</details>

## Optional challenges

**A · Contribute to open source.** This [awesome list](https://github.com/irgroup-classrooms/awesome-data-modeling) collects tutorials, tools and datasets. Found a portal or a tool in this lab that is missing there? Edit the list on GitHub and propose your change as a pull request: GitHub creates the fork for you. Explain in the pull request what you added and why.

**B · Let a machine check your FAIR review.** [F-UJI](https://www.f-uji.net/) assesses the FAIRness of a dataset automatically, from its DOI or the URL of its landing page. Run it on your dataset and compare its scores with your review in Task 7. Where do you disagree, and why? On 02.10.26, F-UJI rated Our World in Data's CO₂ repository on GitHub *initial* and a Cologne dataset with a Zenodo DOI *advanced*. What does that tell you about FAIR and about the quality of the data?

**C · Let a chatbot profile your dataset.** Once your own profile is done, give a chatbot the link to your dataset and ask for its licence, formats and a FAIR review. Compare its answer with yours. Did it get the licence right? Did it invent a DOI, a column or a date? Log it in your `AI_USAGE.md`.

## Submission

> [!IMPORTANT]
> Your results live in two repositories:
>
> ```text
> <username>/<username>/        # your profile
> └── README.md                 # Task 1
>
> <username>/data-integration-labs/
> ├── README.md                 # links, table of labs, licence
> ├── LICENSE
> ├── AI_USAGE.md               # if you used AI tools
> └── labs/01/
>     ├── cheatsheet.md         # Task 3
>     ├── open-source.md        # Task 4
>     └── dataset.md            # Tasks 5–8
> ```
>
> Before you finish, check that:
>
> - every entry of your cheat sheet renders on GitHub, and every link in its table of contents works,
> - `dataset.md` credits every source with title, author, source and licence,
> - your `AI_USAGE.md` has an entry if you used AI tools.
>
> Set up your lab repository before the next lab on 30.10.26: you will commit all labs there. Your dataset profile is the first section of your roadmap, due on 13.11.26.

## Resources

- GitHub Docs: [Managing your profile README](https://docs.github.com/en/account-and-profile/how-tos/profile-customization/managing-your-profile-readme) · [Basic writing and formatting syntax](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) · [Working with advanced formatting](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting) · [Adding a license to a repository](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/adding-a-license-to-a-repository) · [Inviting collaborators](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/repository-access-and-collaboration/inviting-collaborators-to-a-personal-repository)
- Markdown: [CommonMark reference and tutorial](https://commonmark.org/help/) · [CommonMark dingus](https://spec.commonmark.org/dingus/) · [GitHub Flavored Markdown spec](https://github.github.com/gfm/) · [Markdown Tutorial](https://www.markdowntutorial.com/), with more exercises
- Licences: [choosealicense.com](https://choosealicense.com/) and its page on [code without a licence](https://choosealicense.com/no-permission/) · [SPDX licence list](https://spdx.org/licenses/) · [Creative Commons licences](https://creativecommons.org/cc-licenses/) · [Recommended practices for attribution](https://wiki.creativecommons.org/wiki/Recommended_practices_for_attribution) · [Open Definition: conformant licences](https://opendefinition.org/licenses/) · [Datenlizenz Deutschland](https://www.govdata.de/informationen/lizenzen)
- Open data: [5-star open data](https://5stardata.info/en/) · [FAIR principles](https://www.gofair.foundation/fair-principles) and the [original paper](https://doi.org/10.1038/sdata.2016.18) · [CKAN API guide](https://docs.ckan.org/en/latest/api/) · the course's [awesome list](https://github.com/irgroup-classrooms/awesome-data-modeling)
- [Open Source Guides: How to contribute to open source](https://opensource.guide/how-to-contribute/)
