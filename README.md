# BIC 2.0 — Result Announcement

This is a **safe demonstration**, not an official results release. The three supplied team codes work. All email addresses in the public files are fictional `example.invalid` addresses. Team names and statuses are demonstration data supplied by the organiser.

The supplied design, embedded India-landmarks artwork, cream hero, logo text, tagline and orange band are retained. The popup supports all three statuses, selected-team confetti, code validation, connection errors, keyboard navigation and reduced-motion preferences. Its subtitle now says Round 2.

## Files

- `index.html`: the complete page. It loads `results.json` using `fetch()`.
- `results.json`: public demo records and connection settings.
- `.nojekyll`: tells GitHub Pages to serve the files directly.

Upload only this `public` folder's contents. Do not upload the parent package, original attachment or any private results file.

## Create and publish on GitHub Pages

1. Sign in to GitHub and open https://github.com/new.
2. Set the repository name to `bic-results-demo`. Select Public and click Create repository.
3. Click **uploading an existing file** (or **Add file > Upload files**).
4. Upload `index.html`, `results.json`, `README.md` and `.nojekyll` from this folder, directly into the repository root. Click **Commit changes**.
5. Open **Settings > Pages**. Under **Build and deployment**, select **Deploy from a branch**.
6. Select **main**, then **/(root)**. Click **Save**.
7. Wait for the Pages deployment to finish. Refresh Settings > Pages and use **Visit site**. The usual URL is `https://YOUR-ACCOUNT.github.io/bic-results-demo/`.
8. If a change does not appear immediately, wait for the latest deployment and refresh the page.

Official instructions: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site

## Update demo teams

Edit `results.json` on GitHub and commit the changes. Add a new object to its `teams` array. Keep commas between objects; JSON does not allow comments or trailing commas. Codes must be unique and match `BIC2-XXX-0000`. Allowed statuses: `selected`, `waitlist`, `not_selected`.

```json
{
  "code": "BIC2-NEW-0001",
  "team": "New team",
  "school": "School name",
  "theme": "Theme name",
  "status": "selected",
  "mentor": {"name": "Mentor name", "email": "mentor@example.invalid"},
  "students": [
    {"name": "Student name", "email": "student@example.invalid"}
  ]
}
```

**Never put real contact details in the public JSON, HTML, README, Git history or public ZIP.** Anybody can download public JSON. Hashing the short team code does not encrypt the record or provide reliable access control.

## Switch to real results later

GitHub Pages serves static files and cannot keep a database secret. Use a separate private service. A prepared Cloudflare Worker is included in the separate backend download; its setup instructions are in `SETUP.md` there.

After deploying that service, replace the entire public `results.json` with:

```json
{
  "mode": "api",
  "endpoint": "https://YOUR-WORKER.YOUR-SUBDOMAIN.workers.dev/result"
}
```

The page will then request only the matching team's result, using its code and a private access link. No private records belong in the public JSON. Send each private link only to its team's mentor through an approved channel. Treat it like a password: anybody with the link and code can view that team's record. A forwarded link remains usable until its key is rotated. The page removes the key from the address bar and keeps it only in memory, so reopening/refreshing requires the private link again. For stricter identity checks, use an institutional sign-in or email verification instead.

The visible countdown is presentation only; it does not lock or release results. The Worker has a separate `RESULTS_RELEASED` switch. Confirm the release date/time with the organiser before enabling real results. The date and participation figures are retained from the supplied attachment and have not been independently certified.

## Add this to the LPU website

Use your actual published URL in place of the example below. An LPU website administrator must add the code to the official website.

Link button (recommended; works well on phones):

```html
<a href="https://YOUR-ACCOUNT.github.io/bic-results-demo/"
   target="_blank" rel="noopener noreferrer"
   style="display:inline-block;padding:16px 28px;background:#ff6a2b;color:#fff;border-radius:14px;text-decoration:none;font-weight:700">
  Check Your Result
</a>
```

Embedded page:

```html
<iframe src="https://YOUR-ACCOUNT.github.io/bic-results-demo/"
        title="Bharat Innovation Challenge 2.0 results"
        style="width:100%;height:1000px;border:0"
        loading="lazy" referrerpolicy="no-referrer"></iframe>
```

The iframe scrolls independently and its popup stays inside the frame. In real-results mode, a generic iframe link does not contain a team's private access key; use the private mentor link or an institution-authenticated integration. LPU's content-security policy may need its administrator to allow the Pages origin in `frame-src`.

## Preview on your computer

Opening the HTML by double-click may block `fetch()` of JSON. Use the live GitHub Pages URL or a local web server. Keep both files in the same directory.
