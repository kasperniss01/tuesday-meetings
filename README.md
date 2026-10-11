# Tuesday Seminar website

A public website for a weekly Tuesday seminar:

- shows the coming Tuesdays with speaker, title and abstract
- lets anyone request a Tuesday (name, email, topic, short description, date)
- lets presenters upload their slides once their talk is confirmed
- keeps an archive of past talks with slides

The website is hosted on **GitHub Pages**. Requests, approvals and slides live in a **Google Sheet** that only you and your co-organizers can open.

```
Visitors ──► website (GitHub Pages) ──► Google Apps Script ──► Google Sheet + Drive folder
                                                                  ▲
                                         you approve/decline here ┘
```

Files:

| File | What it is |
|---|---|
| `index.html` | The website |
| `config.js` | One line: the address of your Google script |
| `Code.gs` | The Google script (goes into the Google Sheet, not on GitHub) |
| `README.md` | This guide |

Setup takes about 30 minutes. Do part A first, then part B.

---

## Part A: the Google Sheet (about 15 minutes)

Use your personal Google account.

1. **Create the sheet.** Go to <https://sheets.new>. Click "Untitled spreadsheet" in the top left and name it, for example, `Tuesday Seminar`.

2. **Open the script editor.** In the menu choose **Extensions → Apps Script**. A new tab opens with a file called `Code.gs` that contains a few lines.

3. **Paste the script.** Select everything in that editor and delete it. Open `Code.gs` from this folder, copy all of it, and paste it in. Click the **Save** icon (the floppy disk). At the top left, rename the project from "Untitled project" to `Tuesday Seminar`.

4. **Run the setup once.** In the toolbar there is a dropdown next to "Debug". Choose **setup**, then click **Run**.
   - Google asks for permission. Click **Review permissions** and choose your account.
   - You will see "Google hasn't verified this app". That is normal for your own scripts. Click **Advanced**, then **Go to Tuesday Seminar (unsafe)**, then **Allow**.
   - When the log says "Execution completed", go back to the sheet tab. You now have two tabs, **Talks** and **Settings**, and a new folder in your Google Drive called "Tuesday Seminar – slides".

5. **Fill in Settings.** In the **Settings** tab, fill in the seminar name, usual time, usual room and an organizer contact (for example `Kasper, kn@math.ku.dk`). "Email new requests to" already has your Gmail address. Add co-organizers' addresses separated by commas if they should also get emails. Leave "Website address" empty for now.

6. **Publish the script as a web app.** Back in the Apps Script tab:
   - Click **Deploy → New deployment**.
   - Click the gear icon next to "Select type" and choose **Web app**.
   - Description: `Seminar website`.
   - Execute as: **Me**.
   - Who has access: **Anyone**. Choose "Anyone", not "Anyone with Google account", so visitors don't need to log in.
   - Click **Deploy**. If it asks for permissions again, allow them as in step 4.
   - Copy the **Web app URL**. It looks like `https://script.google.com/macros/s/AKfycb…/exec`. Keep it for part B.

**Check:** paste the Web app URL into a new browser tab. You should see a short text starting with `{"ok":true,"settings":`. If you do, the Google side works.

---

## Part B: the website on GitHub Pages (about 10 minutes)

1. **Create a repository.** Go to <https://github.com/new>.
   - Repository name: `tuesday-seminar`. This becomes part of the web address.
   - Choose **Public**. Free GitHub Pages needs a public repository. That is fine, because the files contain no private data: emails and requests stay in your Google Sheet.
   - Click **Create repository**.

2. **Upload the files.** On the new, empty repository page, click the link **uploading an existing file**. Drag in `index.html`, `config.js` and `README.md`. Do **not** upload `Code.gs`; it only lives in Google. Click **Commit changes**.

3. **Add your script address.** In the repository, click `config.js`, then the pencil icon (Edit). Paste your Web app URL between the quotes:
   ```js
   apiUrl: "https://script.google.com/macros/s/AKfycb…/exec"
   ```
   Click **Commit changes…** twice.

4. **Turn on GitHub Pages.** Go to the repository's **Settings → Pages**. Under "Build and deployment", set Source to **Deploy from a branch**, Branch to **main** and folder **/ (root)**, then click **Save**.

5. **Wait 1–2 minutes** and refresh the Pages settings page. It shows "Your site is live at `https://YOUR-USERNAME.github.io/tuesday-seminar/`". Open it.

6. **Tell the sheet where the website is.** Copy that address into **Website address** in the Settings tab of your Google Sheet. The script uses it for the slides-upload link in confirmation emails.

**Test it:** send yourself a request from the website. It should appear in the Talks tab as Pending, you should get an email, and the website should show that Tuesday as "Requested, awaiting confirmation".

---

## Daily use

### Approving or declining a request
Open the Google Sheet, **Talks** tab. New requests arrive as rows with Status **Pending**. Change the Status dropdown:

- **Approved**: the talk appears on the website, and the speaker gets an email with a personal link for uploading slides.
- **Declined**: the speaker gets a short, polite email, and the date becomes free again.

To move a talk to another Tuesday, change its Date. To turn emails to speakers off, set "Email presenters…" to `No` in Settings.

### Adding a talk yourself, for example an invited speaker
Add a row with Date, Status **Approved**, Speaker and Title, and optionally Abstract, Time, Room and Note. The Email column is optional; with an address there, the speaker gets the confirmation email. To get the slides-upload link for the speaker, select the row and use the menu **Seminar → Show slides-upload link for selected talk**. That menu appears after you reload the sheet once.

### Removing a talk
Right-click the row number on the left of the sheet and choose **Delete row**. The talk disappears from the website and nobody gets an email. If you set the Status to **Declined** instead, the speaker gets the polite "not scheduled" email.

### Earlier Tuesdays
Talks with a date in the past are shown under **Earlier Tuesdays** on the website, newest first, together with their slides. To add old talks, add a row with the past Date, Status **Approved**, Speaker, Title and optionally Abstract and Slides. Leave **Email** empty for these rows; otherwise the speaker gets a "your talk is confirmed" email.

### Weeks without a seminar
Add a row with the Date, Status **No seminar** and an optional Note, such as `Autumn break`.

### Slides
After approval, speakers see an upload box in "Your talks" on the website. It appears in the browser they used to request the talk, or when they open the link from their confirmation email. They can upload a PDF, PowerPoint, Keynote or ODP file up to 25 MB, or paste a link. Uploaded files go to the "Tuesday Seminar – slides" folder in your Drive, set to "anyone with the link can view". To add slides yourself, paste a link into the **Slides** column.

### Co-organizers
Click **Share** in the Google Sheet and add them as **Editors**. They need a Google account; they can create a free one with their KU email at <https://accounts.google.com/signup> by choosing "Use my current email address instead". When they change a status, the emails are still sent from your Gmail account.

### What is public and what is private
- **Public on the website:** approved talks (date, speaker, title, abstract, time, room, note, slides), weeks without a seminar, and which dates have a pending request. Pending requests show only the date, never the name or topic.
- **Private, in the sheet only:** email addresses, pending and declined requests, and the hidden Token column. Don't share the tokens, because they work as upload keys.

---

## If something goes wrong

- **The website says "The programme couldn't be loaded".** Check that `config.js` has the full URL ending in `/exec`, and that the deployment's access is set to **Anyone**.
- **You changed `Code.gs` later.** Changes only go live after **Deploy → Manage deployments → pencil icon → Version: New version → Deploy**. The URL stays the same.
- **No emails arrive.** Run `setup` once more from the Apps Script editor. This reinstalls the email trigger and keeps your data. Also check your Gmail spam folder.
- **The website shows example data.** `config.js` on GitHub still has an empty `apiUrl`.
- **The page doesn't update after a GitHub change.** GitHub Pages can take a minute or two. Then reload with Shift held down.
