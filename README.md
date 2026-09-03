# resume2human

**English** · [Русский](README.ru.md)

The tool does exactly one thing: turn your resume into **a list of well-matched
vacancies → 1-3 real people behind each of them, people you can write to
yourself**.

No automated outreach, no CRM, no application statuses, and not a single API
key. Just search, scoring, storage and an output you can copy an address out of
straight into a letter you write by hand.

It runs on your machine: a window instead of a console, and the data in a folder
next to the exe.

---

## Download

Windows 10/11, 64-bit. You do not need Python or anything else installed.

**→ [resume2human-windows.zip](https://github.com/ialakey/resume2human/releases/download/latest/resume2human-windows.zip)** — always the latest build.

1. Unpack the **whole** archive. What is inside is a folder, not a single file:
   the exe will not start without the files next to it.
2. Run `JobHunter\JobHunter.exe`.
3. On the first run SmartScreen will say "Windows protected your PC".
   "More info" → "Run anyway". It will keep doing that until the build is
   signed by a certificate from a public certificate authority — that costs
   money, and a reputation with SmartScreen takes time. If you want to be sure
   the file was not tampered with, compare the archive's SHA-256 with the one
   printed in the build log on
   [GitHub Actions](https://github.com/ialakey/job-hunter/actions).

Numbered releases (`v1.2.0`) live on the
[releases page](https://github.com/ialakey/resume2human/releases); `latest` is
the same thing, rebuilt on every change.

To check that a build is alive without opening the window:

```powershell
JobHunter.exe --selftest      # the verdict lands in data\selftest.log
```

The interface comes up in Russian and switches to English in **⚙ Settings →
Interface → Interface language**. The window, the settings and the report all
follow it; no restart needed.

## How to use it

1. **Resume.** Drop in a file (PDF, DOCX, TXT, MD, RTF) or paste the text. A
   plain description of what you are after works too: "Senior Java, Kafka, Riga
   or remote". The app shows what it understood straight away — the stack, the
   grade, the role — and that is worth a look, because the profile is
   multiplied by every vacancy in the run.
2. **Search parameters.** Keywords, locations, job titles, the match threshold,
   the "Fully remote vacancies only" box and the choice of sources. An empty
   field means "take it from the resume"; every source ticked means "no
   restriction".
3. **LinkedIn — optional.** "Sign in to LinkedIn" opens a browser, you sign in
   yourself (with your own 2FA and captcha) and the app picks up the finished
   session. **No password is ever typed into the app or stored by it.** Without
   a session, contacts are looked for in open sources only — there will simply
   be fewer of them.
4. **"Find vacancies".** Progress goes to the status bar and to the "Log" tab;
   "Stop" interrupts the run at any point.
5. **The result** is on the "Vacancies" tab: a table with the columns "%",
   "Vacancy", "Company", "Location", "People" and "Source". Any heading sorts
   it, and the row colour shows how strong the match is. Under the table is the
   card for the selected vacancy and three buttons:
   * **Open the vacancy** (or double-click the row);
   * **Copy the link**;
   * **Copy the card** — the percentage, what matched, why it fits and the
     people found with their addresses: the draft of a first letter.

   The text report for the whole run is on the "Report" tab and in
   `data\reports\*.txt`.

## Where the vacancies come from

Every source is public, and they are queried in parallel:

* **company ATS boards directly** — Greenhouse, Lever, Ashby, Recruitee,
  SmartRecruiters, Workable;
* **remote aggregators** — RemoteOK, We Work Remotely, Arbeitnow, Remotive,
  Jobicy, Working Nomads, Himalayas, The Muse, Empllo, and the "Who is hiring"
  thread on Hacker News;
* **single-market boards** — NoFluffJobs (Poland), getmatch, Landing.jobs (EU),
  Djinni and DOU (Ukraine and the remote work around it), Habr Career, GeekJob;
* **LinkedIn** — vacancies (guest access) and "we are hiring" posts (through
  your session);
* **posts** — public Telegram channels and threads.com;
* **web search** — Bing and DuckDuckGo without a key, as a fallback channel.

## How the match is calculated

The scoring is deterministic and explainable: no ML, and no calls to anybody
else's model.

* The resume becomes a profile: must-have skills, nice-to-have skills, grade,
  years of experience, domains, locations, deal breakers.
* A cheap prefilter on skill overlap drops the bulk of the vacancies, and only
  the survivors are scored in full: **match 0-100**, whether the grade fits,
  what is missing from the must-haves, what tripped a deal breaker, and why the
  vacancy fits.
* Half of the final score is must-have coverage, which is why that list is kept
  short: the language and the framework, not the whole stack. Everything else
  goes into nice-to-have, where it adds points but never blocks.
* The report shows both what matched and what was missing — so you can argue
  with the score instead of taking it on faith.

## How the people are found

For every vacancy above the threshold the tool climbs a ladder and stops as
soon as it has enough contacts:

1. contacts written into the vacancy text itself;
2. the company's own pages — careers, team, about, people;
3. LinkedIn profiles: the roles are chosen **from the vacancy** (Engineering
   Manager, Head of, VP, CTO, the recruiter for that discipline) rather than
   "whoever comes up";
4. web search, as the last rung.

What comes out is a name, a role and an address. The letter is yours to write.

## What the tool does not do

* it does not send messages and does not apply on your behalf — and it will
  not: it takes you to the line "here is a person and here is their address",
  and stops there;
* it does not track application statuses and is not a CRM;
* it needs no API key at all, and sends your resume to no service.

## Privacy

Your resume, your profile, the vacancy database and the LinkedIn session stay
on your computer — in the same folder you unpacked the archive into:

```
JobHunter\
├── JobHunter.exe
├── job_hunter.db          vacancies, verdicts, contacts
├── profile\search.json    search parameters
├── linkedin_cookies.json  the LinkedIn session (if you signed in)
└── data\
    ├── reports\*.txt      one report per run
    └── desktop.log        the log, in place of a console
```

Delete the folder and you have deleted all of it. The only thing that leaves
your machine is the requests to the job sources themselves.

The app does not carry a browser for LinkedIn around with it — that would be
another 150 MB per copy. It takes the first one available: Playwright's
Chromium if you already have it downloaded, otherwise an installed Chrome or
Edge, otherwise a "Download Chromium" button appears in the window.

## Limitations

* The scoring is a weighted keyword algorithm. It is explainable, but it does
  not understand context: "Java" in the list of requirements and "Java" in a
  sentence about legacy code are the same thing to it.
* Web search is public scraping without a key. Run it often enough and the
  search engine starts answering with a captcha; the engine is then put on
  pause and the query goes to the next one. The report says so in its own
  warning, so that an empty list of people is not read as "there is nobody at
  these companies".
* Parsing posts (Telegram, Threads, LinkedIn, Hacker News) is heuristic: people
  write freehand, so the company name is sometimes guessed wrong. When no
  employer is named at all, the report shows the author of the post — which is
  more honest than an invented name.
* Searching LinkedIn posts needs a live session and cannot filter by geography;
  without a session the source quietly returns an empty list.
* LinkedIn's markup, and the search engines', change. Every parser returns an
  empty result and writes a warning to the log when it breaks, rather than
  taking the run down with it.

## Source code

This repository is the storefront: a README and the releases. Development
happens in a private repository, where every build run executes the tests,
builds the exe, runs `--selftest` inside the built file, and only then updates
the release here.

Questions and bug reports go to
[Issues](https://github.com/ialakey/resume2human/issues). A bug report is much
more useful with `data\desktop.log` attached.
