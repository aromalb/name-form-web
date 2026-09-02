# NextGreenTech UK website

## Contact form setup (GitHub + Vercel + Resend)

The contact form on this site posts to `/api/contact`, a serverless function
that sends the enquiry via Resend. The Resend API key is never stored in
this code — it's read from an environment variable at runtime.

### 1. Rotate your Resend API key
If you ever pasted your key anywhere outside your own machine (chat, email,
a public repo, etc.), delete it in the Resend dashboard and generate a new
one before going further: https://resend.com/api-keys

### 2. Push this folder to GitHub
```
git init
git add .
git commit -m "Initial site with contact form"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

### 3. Import into Vercel
- Go to https://vercel.com, sign in with GitHub.
- Click "Add New Project" and select this repo.
- Framework preset: "Other" (it's a static site + one API function — Vercel
  detects `/api` automatically).
- Deploy.

### 4. Add the environment variable
In the Vercel project → Settings → Environment Variables, add:

| Name              | Value                  |
|-------------------|------------------------|
| `RESEND_API_KEY`  | your new Resend key    |

Redeploy after adding it (Vercel → Deployments → ⋯ → Redeploy).

### 5. Verify your sending domain in Resend (recommended)
By default `api/contact.js` sends from `onboarding@resend.dev`, which works
immediately but looks less professional and has lower deliverability. Once
you verify `nextgreentech.co.uk` in Resend (Settings → Domains → Add Domain,
then add the DNS records they give you), change the `from` address in
`api/contact.js` to something like:

```
from: 'NextGreenTech UK <hello@nextgreentech.co.uk>'
```

### 6. Test
Open the live site, submit the contact form, and confirm the email arrives
at info@nextgreentech.co.uk. Check the "Functions" tab in Vercel for logs if
anything fails.
