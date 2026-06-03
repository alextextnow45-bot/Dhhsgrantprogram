# DHHS Grant Portal — Setup Guide

## 1. Custom Domain (Netlify)

### Option A — Free subdomain (already live)
Your site is at: `https://dhhs-grant-portal.netlify.app`

### Option B — Custom domain (e.g. dhhsgrants.org)
1. Buy a domain at [Namecheap](https://namecheap.com) or [Google Domains](https://domains.google)
2. In Netlify → **dhhs-grant-portal** → **Domain management** → **Add custom domain**
3. Enter your domain (e.g. `dhhsgrants.org`) → **Verify**
4. Netlify will show you DNS records to add. Go to your domain registrar and add:
   - **Type:** CNAME
   - **Name:** www
   - **Value:** dhhs-grant-portal.netlify.app
5. Also add the Netlify nameservers for the apex domain (@)
6. Wait 10–30 minutes for DNS to propagate
7. Netlify auto-provisions a free SSL certificate ✓

---

## 2. Email Notifications (EmailJS)

### Setup (free — 200 emails/month)
1. Go to [emailjs.com](https://emailjs.com) → Create account
2. **Email Services** → Add Service → Connect Gmail or Outlook
   - Copy your **Service ID**
3. **Email Templates** → Create Template → Paste this:

```
Subject: DHHS Grant Application Received – {{app_id}}

Dear {{to_name}},

Your DHHS grant application has been successfully received.

──────────────────────────────
Application ID : {{app_id}}
Reference #    : {{ref_number}}
Organization   : {{org_name}}
Project Title  : {{project_title}}
Amount         : {{amount}}
Submitted      : {{submitted_date}}
──────────────────────────────

Track your application status here:
{{status_url}}

If you have questions, reply to this email.

The DHHS Grant Review Team
Department of Health and Human Services
```

   - Set the **To Email** field to: `{{to_email}}`
   - Copy your **Template ID**

4. **Account** → General → Copy your **Public Key**

5. Open `apply.html` and replace lines at the top of the script:
```js
const EMAILJS_PUBLIC_KEY  = 'YOUR_PUBLIC_KEY';   // ← paste here
const EMAILJS_SERVICE_ID  = 'YOUR_SERVICE_ID';   // ← paste here
const EMAILJS_TEMPLATE_ID = 'YOUR_TEMPLATE_ID';  // ← paste here
```

6. Commit and push to GitHub → Netlify auto-deploys ✓

---

## 3. Admin Dashboard

### Access
- URL: `https://dhhs-grant-portal.netlify.app/admin.html`
- Default login: `admin` / `dhhs2026`
- Reviewer login: `reviewer` / `review2026`

### Change passwords
Open `admin.html` and find this block near the top of the script:
```js
const ADMIN_USERS = {
  'admin': 'dhhs2026',       // ← change this
  'reviewer': 'review2026',  // ← change this
};
```

### Features
- View all submitted applications
- Update application status (Pending → Under Review → Approved / Not Selected)
- Search and filter by status
- Export all applications to CSV
- Auto-ingests any new submission made in the same browser

### Production upgrade
For a real multi-user admin with server-side storage:
1. Enable **Netlify Forms** — add `data-netlify="true"` to the form in `apply.html`
2. All submissions appear in Netlify dashboard under **Forms**
3. Set up **Netlify email notifications** to alert you on each submission

---

## 4. Files in this package

| File | Purpose |
|------|---------|
| `index.html` | Landing page |
| `apply.html` | 4-step grant application form |
| `status.html` | Application status tracker |
| `admin.html` | Password-protected admin dashboard |
| `SETUP.md` | This guide |

---

## 5. Update the site

Any time you make changes:
1. Edit the HTML files
2. Go to your GitHub repo → upload the changed file
3. Netlify auto-deploys within 30 seconds ✓
