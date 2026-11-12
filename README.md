# ReferralForge

One file, `ReferralForge.html`, that fills in other teams' referral forms on
an iPad (or any computer) and gives you back a PDF of the team's own form with
your answers typed in. Each PDF is built from the team's original page, in the
form's own fonts, so it looks like the Word document filled in and saved as
PDF.

It holds two forms:

* **ECAS / CRT Referral Form** (Cardiff and Vale, July 2016), one page.
* **Cardiff CLDT Referral Form** (Community Learning Disability Team), three
  pages.

## Using it

1. Open `ReferralForge.html` in a browser (Safari, Edge or Chrome).
2. Choose the form in the box under the name ReferralForge, top left. Each
   form keeps its own referral in progress, so changing form loses nothing.
3. Fill in the boxes on the left. The right-hand side shows the pages exactly
   as they will print and updates as you type. Tap anything on a page to jump
   to its box. On an iPad held upright, **Page** switches to the pages and
   **Form** switches back.
4. Tap **Share** to send the PDF straight to Mail, Outlook or Files, or
   **Save PDF** to put it in Downloads.
5. Tap **New** to clear the patient before the next referral.

Your name is picked from the Physician Response Unit list (or typed under
"Other"); the unit's address and telephone number are filled in already. Tick
"Remember my details" to keep your choices on the device (each form remembers
its own).

The referral you are writing is kept on the device for 24 hours after your
last change, so switching apps or a Safari reload does not lose it. New clears
it at once. A PDF you save to Files stays there until you delete it.

### What the boxes do

* A box that needs more lines than the form printed grows, and the rows below
  move down, as they would in Word. A row that no longer fits on the page
  breaks and carries on at the top of the next page, again as Word does it.
  The app tells you under the box when this happens.
* A one-line box (phone numbers, dates, hospital number, postcodes) cannot
  grow. If you type too much the app says so in red, a banner at the top names
  the box, and Save and Share explain why they will not make the PDF yet.
* ECAS / CRT: ticked boxes print as Word's checkbox character. CLDT: the form
  has empty cells to tick, so a tick is typed into the cell (the Wingdings
  tick Word's Insert Symbol gives).
* "Delete as appropriate" choices, and the CLDT form's "Yes / No" answers,
  remove the other words from the form.
* Typing over a dotted line removes the dots, as you would in Word.
* Welsh letters (ŵ, ŷ), Greek letters, arrows and signs such as ≥ print as
  typed. Anything none of the form's fonts has prints as "?" and the app tells
  you which character.
* Date (and, on ECAS / CRT, Time) of Referral fill themselves in and follow
  the clock until you type your own; they are stamped again when you share or
  save.
* The PDF is named for the form ("ECAS CRT referral", "Cardiff CLDT
  referral") with the referral date and the time.
* Nothing on either form is required. Before saving, the app reminds you of
  the few things each form marks as essential or that are easy to miss (GP
  informed and the team on ECAS / CRT; the client's name on CLDT).
* The receiving team's own "office use" boxes are never filled in.

## On the iPad: the web copy

An iPad will not run an HTML file opened from the Files app, so the app lives
on GitHub Pages. The copy to upload is `publish/index.html` (the build writes
it beside `ReferralForge.html`; they are the same file). To update the web
copy after a change: on the repository's page on github.com, Add file, Upload
files, drag the new `publish/index.html` in, Commit changes. It goes live in
a minute or two.

The web address opens on the ECAS / CRT form. The same address with `#cldt`
on the end opens on the CLDT form, so each form can have its own bookmark.

The page fetches nothing and sends nothing, so what is typed never leaves the
iPad. The published page does contain the unit's doctor names, address and
telephone number, as anyone with the link could see in its code.

## Files

    ReferralForge.html            the app (built, never edited by hand)
    publish/index.html            the same file, named for the web copy
    sources/                      each team's form, as received (and Word's PDF of the CLDT .docx)
    forms/ecas_crt/form.yaml      where each answer goes on the ECAS / CRT page
    forms/cldt/form.yaml          where each answer goes on the CLDT pages
    tools/shell.html              the app's source
    tools/extract.py              reads a form's pages
    tools/flow.py                 finds the rows of a form of several pages
    tools/build.py                writes ReferralForge.html
    tests/harness.py              the checks for ECAS / CRT
    tests/harness_cldt.py         the checks for CLDT
    tests/safari_check.py         has Safari on this Mac build the same PDFs and compares them
    handover/                     notes for rebuilding the ECAS / CRT form elsewhere (Microsoft List, Power Automate, Power BI)

## Commands

    python3 tools/build.py            # rebuild ReferralForge.html
    python3 tools/build.py --check    # is the built file up to date?
    python3 tools/build.py --runs     # list each form's text runs
    python3 tools/flow.py forms/cldt/form.yaml   # list the rows found in a form of several pages
    python3 tests/harness.py          # run the ECAS / CRT checks (writes tests/out/)
    python3 tests/harness_cldt.py     # run the CLDT checks
    python3 tests/safari_check.py     # opens two tabs in Safari; close them afterwards

## If a team issues a new version of its form, or another form is wanted

Put the team's file in `sources/`. A Word file is first saved as PDF by Word
itself (File, Save As, PDF), so the page is Word's own. Point a `form.yaml` at
the PDF and ask Claude to map the boxes (the run numbers in `form.yaml` come
from the page, so they change with it). Then rebuild and run the checks. The
first check of each harness proves the blank output is identical to the
team's form, pixel for pixel.
