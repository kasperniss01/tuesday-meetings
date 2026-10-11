# Tuesday Meeting website

A public website for a weekly Tuesday meeting with talks, reading sessions and discussions:

- shows the coming Tuesdays with type, speaker or leader, title, abstract and paper link
- lets anyone request a Tuesday (name, email, type, title, short description, paper link, date)
- lets presenters upload slides or notes once their session is confirmed
- shows earlier Tuesdays with their slides

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

1. **Create the sheet.** Go to <https://sheets.new>. Click "Untitled spreadsheet" in the top left and name it, for example, `Tuesday Meeting`.

2. **Open the script editor.** In the menu choose **Extensions → Apps Script**. A new tab opens with a file called `Code.gs` that contains a few lines.

3. **Paste the script.** Select everything in that editor and delete it. Open `Code.gs` from this folder, copy all of it, and paste it in. Click the **Save** icon (the floppy disk). At the top left, rename the project from "Untitled project" to `Tuesday Meeting`.

4. **Run the setup once.** In the toolbar there is a dropdown next to "Debug". Choose **setup**, then click **Run**.
   - Google asks for permission. Click **Review permissions** and choose your account.
   - You will see "Google hasn't verified this app". That is normal for your own scripts. Click **Advanced**, then **Go to Tuesday Meeting (unsafe)** (or whatever you named the project), then **Allow**.
   - When the log says "Execution completed", go back to the sheet tab. You now have two tabs, **Talks** and **Settings**, and a new folder in your Google Drive whose name ends in "– slides".

5. **Fill in Settings.** In the **Settings** tab, fill in the meeting name, usual time, usual room and an organizer contact (for example `Kasper, kn@math.ku.dk`). "Email new requests to" already has your Gmail address. Add co-organizers' addresses separated by commas if they should also get emails. Leave "Website address" empty for now.

6. **Publish the script as a web app.** Back in the Apps Script tab:
   - Click **Deploy → New deployment**.
   - Click the gear icon next to "Select type" and choose **Web app**.
   - Description: `Meeting website`.
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

- **Approved**: the session appears on the website, and the speaker gets an email with a personal link for uploading slides or notes.
- **Declined**: the speaker gets a short, polite email, and the date becomes free again.

To move a talk to another Tuesday, change its Date. To turn emails to speakers off, set "Email presenters…" to `No` in Settings.

### Session types
The **Type** column says what happens that Tuesday: **Talk**, **Reading session**, **Discussion** or **Other**. People choose it when they send a request; empty means Talk. On the website, reading sessions and discussions show "Led by …" instead of the speaker's name. Put a link to the paper being read in **Paper / material link**, and it appears as a "Paper ↗" button.

### Adding a session yourself, for example an invited speaker or a reading session
Add a row with Date, Status **Approved**, Type, Speaker (the person leading it) and Title, and optionally Abstract, Time, Room, Note and Paper / material link. The Email column is optional; with an address there, the speaker gets the confirmation email. To get the slides-upload link for the speaker, select the row and use the menu **Meeting → Show slides-upload link for selected session**. That menu appears after you reload the sheet once.

### Removing a session
Right-click the row number on the left of the sheet and choose **Delete row**. The talk disappears from the website and nobody gets an email. If you set the Status to **Declined** instead, the speaker gets the polite "not scheduled" email.

### Earlier Tuesdays
Approved sessions with a date in the past are shown under **Earlier Tuesdays** on the website, newest first, together with their slides. The section only appears once there is at least one. To add old sessions, add a row with the past Date, Status **Approved**, Type, Speaker, Title and optionally Abstract, Slides and Paper / material link. Leave **Email** empty for these rows; otherwise the speaker gets a "your talk is confirmed" email.

### Weeks without a meeting
Add a row with the Date, Status **No meeting** and an optional Note, such as `Autumn break`.

### Adding slides yourself (organizer mode)
The Settings tab has an **Organizer key**, a random code the script creates. On the website, scroll to the bottom, click **Organizer login** and enter that key. The browser remembers it. Every approved session, coming and earlier, then gets an **Add slides** button where you can upload a file or paste a link. Give the key only to co-organizers. To change it, replace it in Settings with a new code of at least 8 characters.

### Slides from presenters
After approval, speakers see an upload box in "Your sessions" on the website. It appears in the browser they used to send the request, or when they open the link from their confirmation email. They can upload a PDF, PowerPoint, Keynote or ODP file up to 25 MB, or paste a link. Uploaded files go to the "– slides" folder in your Drive, set to "anyone with the link can view". To add slides yourself, paste a link into the **Slides** column.

### Co-organizers
Click **Share** in the Google Sheet and add them as **Editors**. They need a Google account; they can create a free one with their KU email at <https://accounts.google.com/signup> by choosing "Use my current email address instead". When they change a status, the emails are still sent from your Gmail account.

### What is public and what is private
- **Public on the website:** approved sessions (date, type, speaker, title, abstract, time, room, note, slides, paper link), weeks without a meeting, and which dates have a pending request. Pending requests show only the date, never the name or topic.
- **Private, in the sheet only:** email addresses, pending and declined requests, and the hidden Token column. Don't share the tokens, because they work as upload keys.

---

## If something goes wrong

- **The website says "The programme couldn't be loaded".** Check that `config.js` has the full URL ending in `/exec`, and that the deployment's access is set to **Anyone**.
- **Updating to a new version of `Code.gs`.**
  1. In the sheet, open **Extensions → Apps Script**, select all the code, delete it and paste the new `Code.gs`. Click **Save**.
  2. Choose **setup** in the dropdown and click **Run**. This adds any new columns and keeps your data and settings.
  3. Click **Deploy → Manage deployments**, then the pencil icon. Under Version choose **New version**, then click **Deploy**. The web app address stays the same, so `config.js` doesn't change.
- **No emails arrive.** Run `setup` once more from the Apps Script editor. This reinstalls the email trigger and keeps your data. Also check your Gmail spam folder.
- **The website shows example data.** `config.js` on GitHub still has an empty `apiUrl`.
- **The page doesn't update after a GitHub change.** GitHub Pages can take a minute or two. Then reload with Shift held down.
