# Deploy Guide — Palm & Place
### Netlify + Squarespace Domain

---

## A. Deploy on Netlify via GitHub

### Step 1 — Create a GitHub repository

1. Go to [github.com](https://github.com) and sign in (create a free account if needed).
2. Click **New repository** (green button, top-right).
3. Name it: `palmandplace` (or any name you prefer).
4. Set it to **Public** or **Private** — both work fine with Netlify.
5. Click **Create repository**.

### Step 2 — Upload your files

**Option A — Using GitHub's web interface (easiest, no terminal needed):**

1. Inside your new repo, click **Add file → Upload files**.
2. Drag and drop your entire `palmandplace/` folder (which contains `index.html` and this `DEPLOY.md`).
3. Scroll down, write a commit message like `"Initial site upload"`, and click **Commit changes**.

**Option B — Using Git (terminal):**
```bash
cd path/to/palmandplace
git init
git add .
git commit -m "Initial site upload"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/palmandplace.git
git push -u origin main
```

### Step 3 — Connect GitHub to Netlify

1. Go to [app.netlify.com](https://app.netlify.com) and sign in (create a free account if needed).
2. Click **Add new site → Import an existing project**.
3. Select **Deploy with GitHub**.
4. Authorize Netlify to access your GitHub account.
5. Search for and select your `palmandplace` repository.

### Step 4 — Configure build settings

This is a pure static site — **no build tool needed**.

| Setting | Value |
|---|---|
| **Base directory** | *(leave empty)* |
| **Build command** | *(leave empty)* |
| **Publish directory** | `.` *(or leave as default)* |

Click **Deploy site**.

### Step 5 — Verify the live site

1. Netlify will assign you a random URL like `https://graceful-tesla-abc123.netlify.app`.
2. Open it in your browser — your site should be live within 30–60 seconds.
3. Every time you push a new commit to GitHub, Netlify auto-redeploys in seconds.

---

## B. Connect Your Squarespace Domain

You have two options. **Option 1 (recommended)** hands full DNS control to Netlify — simpler long-term management.

---

### Option 1 — Use Netlify DNS (Recommended)

#### Step 1 — Add your domain in Netlify

1. In your Netlify site dashboard, go to **Domain management → Add a domain**.
2. Type your domain (e.g. `palmandplace.com`) and click **Verify**.
3. Click **Add domain**.
4. Netlify will show you **4 nameserver addresses** like:
   ```
   dns1.p01.nsone.net
   dns2.p01.nsone.net
   dns3.p01.nsone.net
   dns4.p01.nsone.net
   ```
   Copy all four — you'll need them in the next step.

#### Step 2 — Change nameservers in Squarespace

1. Log in to [account.squarespace.com](https://account.squarespace.com).
2. In the left sidebar, click **Domains**.
3. Click on your domain name.
4. Click **DNS Settings** (or **Advanced Settings**).
5. Look for **Nameservers** and click **Edit / Use Custom Nameservers**.
6. Delete the existing Squarespace nameservers.
7. Paste the 4 Netlify nameservers you copied above (one per field).
8. Save the changes.

> **DNS propagation takes 24–48 hours.** Your site may show intermittently during this window — this is normal.

#### Step 3 — Confirm in Netlify

1. Return to Netlify → **Domain management**.
2. Once propagation is complete, the domain will show as **Active** with a green checkmark.

---

### Option 2 — Point DNS manually from Squarespace (without changing nameservers)

Use this if you have other services (email, etc.) running through Squarespace DNS that you don't want to disrupt.

#### Records to add in Squarespace DNS:

1. Log in to Squarespace → **Domains** → select your domain → **DNS Settings**.
2. Add the following DNS records:

**A Record (apex / root domain):**
| Type | Host | Value | TTL |
|---|---|---|---|
| A | `@` | `75.2.60.5` | 3600 |

**CNAME Record (www subdomain):**
| Type | Host | Value | TTL |
|---|---|---|---|
| CNAME | `www` | `YOUR-NETLIFY-SUBDOMAIN.netlify.app` | 3600 |

> Replace `YOUR-NETLIFY-SUBDOMAIN` with the subdomain Netlify assigned to your site (e.g. `graceful-tesla-abc123`).

3. In Netlify → **Domain management**, add both `palmandplace.com` and `www.palmandplace.com`.
4. Wait 24–48 hours for propagation.

---

## C. HTTPS / SSL Setup

### Enable SSL (automatic — Netlify handles this for free)

1. In Netlify → **Domain management → HTTPS**.
2. Once your domain is connected and DNS has propagated, click **Verify DNS configuration**.
3. If the check passes, click **Provision certificate**. Netlify uses Let's Encrypt and provisions the cert automatically — usually within a few minutes.

### Force HTTPS

1. In the same **HTTPS** section, toggle on **Force HTTPS**.
2. This redirects all `http://` traffic to `https://` automatically.

### Verify everything is working

Run through this checklist:

- [ ] `https://palmandplace.com` loads your site
- [ ] `https://www.palmandplace.com` also loads (redirects properly)
- [ ] Browser shows the padlock icon (valid SSL)
- [ ] `http://` automatically redirects to `https://`
- [ ] No mixed-content warnings in browser console

---

## Quick Reference

| What | Where |
|---|---|
| Netlify dashboard | [app.netlify.com](https://app.netlify.com) |
| Squarespace domains | [account.squarespace.com/domains](https://account.squarespace.com) |
| Check DNS propagation | [dnschecker.org](https://dnschecker.org) |
| Netlify IP for A record | `75.2.60.5` |
| SSL provider | Let's Encrypt (free, auto-renews) |

---

*Generated for Palm & Place — static site, no build step required.*
