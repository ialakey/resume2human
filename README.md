# resume2human

**English** · [Русский](README.ru.md)

The tool does one thing: turn your resume into **a list of well-matched
vacancies → 1-3 real people behind each of them, people you can write to
yourself** — and then remember which of those letters got an answer.

No automated outreach, no API key, nothing sent on your behalf. Search,
scoring, contacts, and a record of what you did with them.

It runs on your machine: a window instead of a console, and the data in a
folder next to the exe.

<p align="center">
  <img src="images/en/app-main.png" width="900"
       alt="The main window: the search panels on the left, the scored vacancies with their applied status on the right, and the card of the selected one below">
</p>

---

## Download

Windows 10/11, 64-bit. You do not need Python or anything else installed.

**→ [resume2human-windows.zip](https://github.com/ialakey/resume2human/releases/download/latest/resume2human-windows.zip)** — always the latest build.

1. Unpack the **whole** archive. What is inside is a folder, not a single
   file: the exe will not start without the files next to it.
2. Run `JobHunter\JobHunter.exe`.

### Why Windows will warn you, and what you can check instead

On the first run SmartScreen says "Windows protected your PC" →
"More info" → "Run anyway". It will keep saying that until the build is signed
by a certificate from a public certificate authority: that costs money, and a
reputation with SmartScreen takes downloads and time. Pretending otherwise
would be the untrustworthy part.

What you *can* verify, without trusting anything written on this page:

* **The SHA-256 of the archive is printed in the release notes**, next to the
  download link. It is computed by the build itself, from the same file the
  release then uploads. Compare it after downloading:

  ```powershell
  Get-FileHash resume2human-windows.zip -Algorithm SHA256
  ```

  A different hash means the file you have is not the file that was built.
* **The build is not made by hand.** Every push runs the tests, builds the exe
  with PyInstaller, runs `--selftest` *inside the built file*, and only then
  publishes the release here. A build that cannot open its own parsers and
  start a browser never reaches this page.
* **The program says the same thing about itself.** Before trusting the
  window, ask the exe:

  ```powershell
  JobHunter.exe --selftest      # the verdict lands in data\selftest.log
  ```

* **Nothing is uploaded.** No API key, no account, no telemetry. The only
  outbound traffic is the requests to the job sources themselves — the same
  addresses you would open in a browser. See [Privacy](#privacy).

Numbered releases (`v1.2.0`) live on the
[releases page](https://github.com/ialakey/resume2human/releases); `latest` is
the same thing, rebuilt on every change.

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
   "Vacancy", "Company", "Location", "People", "Source" and "Applied". Any
   heading sorts it, and the row colour shows how strong the match is. Under
   the table is the card for the selected vacancy and the buttons:

   * **Open the vacancy** (or double-click the row);
   * **Copy the link**;
   * **Copy the card** — the percentage, what matched, why it fits and the
     people found with their addresses;
   * **Letter** — a first message built from a template and this card;
   * **Mark** — what you did about it.

   The text report for the whole run is on the "Report" tab and in
   `data\reports\`.

## What happens after the report

The search used to end at the report, and the tool knew nothing after that.
Now it does.

**Mark.** "Mark" → "Written" with a channel (email, LinkedIn, Telegram, a
form, through somebody you know), then "Replied", "Screening", "Interview",
"Offer", "Rejected". The row changes colour, the status shows in the "Applied"
column, and it is printed in the report too.

You cannot walk back down the ladder — there is no un-sending a letter. If you
marked the wrong row, "Clear the mark" puts it back to where it started, dates
and all.

**Silence is counted, not typed in.** An application becomes "no answer" by
itself once the configured number of days has passed without a reply. A status
that has to be set by hand is set by nobody, and the funnel quietly lies.

**Letter.** "Letter" assembles a first message out of a template and the card:
the person's name, the company, the role, and the "why I fit this particular
vacancy" line the tool already computes on every run. There are two templates
from the start, and that is not an accident — a "by message version" breakdown
needs more than one version to compare. They are edited in
`data\templates.json`, from **Letter → Edit the templates…**; the list of
available substitutions is written inside the file.

When you press "Written" after "Copy the letter", the tool remembers which
version it was written with. Otherwise there would be nothing to compare.

**The dashboard** is the "📈 Applications" button in the header. Two tabs:

<p align="center">
  <img src="images/en/app-funnel.png" width="760"
       alt="The funnel tab: found, over the threshold, written, replied, screening, interview, offer, with the conversion at each step, and a list of what needs doing today">
</p>

*The funnel* — found → over the threshold → written → replied → screening →
interview → offer, with the conversion at every step. Next to it, what needs
doing today: how many are waiting for an answer, who is due a follow-up, who
has been silent past the threshold, how many letters went out in the last 7 and
30 days.

<p align="center">
  <img src="images/en/app-slices.png" width="760"
       alt="The what-works tab: response rate by vacancy source, with the number of letters each percentage is counted from and a confidence interval, and 'not enough data' where there are too few">
</p>

*What works* — response rate by the vacancy's source, the channel, the message
version, the match band, the kind of role and the day it was sent.

The percentages there are honest. Each one says how many letters it is counted
from, and its interval: "25% of 12 (9–53%)". Below eight letters no percentage
is shown at all — "not enough data" instead. At five letters the difference
between two sources does not exist, and a dashboard that pretends it does is
worse than no dashboard.

## Reports you can open again

<p align="center">
  <img src="images/en/app-reports.png" width="700"
       alt="The reports window: a list of past runs with their date, vacancy count, how many had people, the threshold and the file format">
</p>

Every search saves two files under the same name: the document you read
(`.txt`, `.csv` or `.xlsx` — **Settings → Report format**) and a `.json` beside
it. A person reads the first; the program reopens the second.

* **Reports** lists everything this computer has found: the date, how many
  vacancies, how many of them had people, the threshold and the file format.
  Double-click a row and a run from last week is back in the table, contacts
  and all. **Import a file…** in the same window opens a report from another
  machine exactly the same way.
* **Save as…** writes the run on screen in another format — text, CSV or Excel.
  The format is the extension you choose in the save dialog.
* **Open the file** hands the document to whatever your system opens it with.

The archive carries neither the text of your resume nor the descriptions of the
vacancies: no report prints either, and report files are the ones people send
each other.

The table formats (`.csv`, Excel) are one row per person rather than per
vacancy — the vacancy repeats next to each contact found, because the only
reason to open a report in Excel is to filter and sort it. A vacancy with
nobody found still gets its row.

Statuses are **not** stored in the archive. The archive is a snapshot of a run
and is honest precisely because it does not change; the status lives in the
database and is laid over the run every time it is opened or exported. Open
last week's report and you see today's truth, not "written" above a vacancy
that turned you down since.

## On a schedule

<p align="center">
  <img src="images/en/app-settings-schedule.png" width="760"
       alt="The schedule tab in settings: a switch, the time of day, daily or weekdays, and two options for the background run">
</p>

**Settings → Schedule**: a checkbox, a time, every day or on weekdays. The app
creates the Windows scheduled task itself, and removes it when the box is
cleared. The task runs `JobHunter.exe --scan` — the same run, without a window.

Three things worth knowing about that task:

* it runs **while you are logged in**. The "whether or not the user is logged
  on" option needs your account password stored, and running as `SYSTEM` breaks
  both the paths and the browser;
* if you move the app's folder, the task keeps pointing at the old place. The
  app notices at startup and offers to recreate it;
* LinkedIn is off in a background run. The browser rung needs a live session and
  behaves like a person at a computer — at night, with nobody there, it is only
  a risk to the account.

The result reaches you three ways: `data\scheduled.log`, Telegram if
notifications are on (**Settings → Telegram**: a switch, a bot token and a chat),
and a "while you were away: 3 new vacancies" line the next time you open the
window.

Once a day is the ceiling. Hourly polling looks like scraping to the sources,
and the app keeps its own minimum interval between runs regardless.

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

There is one more rung, **off by default** (Settings → Applications → "Guess
likely addresses"): building an address out of a name and a domain when nobody
published the person's email. Those are marked as unverified in the report,
because a letter to a guessed address can bounce, and bounces cost the sender's
reputation.

## What the tool does not do

* it does not send messages and does not apply on your behalf — and it will
  not: it takes you to the line "here is a person and here is their address",
  and stops there. "Letter" fills your clipboard, never an outbox;
* it records what *you* say you did — one click per step — and does not watch
  your mail to find out;
* it needs no API key at all, and sends your resume to no service.

## Privacy

Your resume, your profile, the vacancy database, the applications and the
LinkedIn session stay on your computer — in the same folder you unpacked the
archive into:

```
JobHunter\
├── JobHunter.exe
├── job_hunter.db          vacancies, verdicts, contacts, applications
├── profile\search.json    search parameters
├── linkedin_cookies.json  the LinkedIn session (if you signed in)
└── data\
    ├── reports\           one document per run, plus the .json beside it
    ├── templates.json     your letter templates
    ├── settings.json      everything the settings window changed
    ├── scheduled.log      what the background run did
    └── desktop.log        the log, in place of a console
```

Delete the folder and you have deleted all of it. The only thing that leaves
your machine is the requests to the job sources themselves — and, if you turn
the switch on yourself, the finished report to your own Telegram chat.

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
* The funnel is only as good as your clicking. A letter you sent and never
  marked is not in it.

## Source code

This repository is the storefront: a README, the screenshots and the releases.
Development happens in a private repository, where every build run executes the
tests, builds the exe, runs `--selftest` inside the built file, and only then
updates the release here.

Questions and bug reports go to
[Issues](https://github.com/ialakey/resume2human/issues). A bug report is much
more useful with `data\desktop.log` attached.
