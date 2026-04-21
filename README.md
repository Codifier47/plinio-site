# plinio.me — Deploy Guide

Your personal CV site. One file: `index.html`. That's it.

---

## What you need to do (in order)

### 1. Personalize the content (15 min)
Open `index.html` in any text editor (Claude Code works great). Use **Find & Replace** to update these:

- Replace `Plinio Drumond` with your full name
- Update the `<h1>` tagline if you want different wording
- Update the **Experience** section — I put placeholders like `[UK Payments Co.]` and `[Company]`. Swap in real company names, roles, and years from your cv.md.
- Update the **About** section paragraphs
- Replace `plinio-diouf` in the LinkedIn link with your actual LinkedIn handle
- Replace `dioufplinio@gmail.com` with whatever email you want to use (you can set this up later through GoDaddy email)
- CV link points to Google Drive: `https://drive.google.com/file/d/1lTYNBsEaNjb2P0tUfHYwGA1zWWB0oklR/view?usp=sharing`

### 2. Put it on GitHub (10 min)
Vercel deploys from GitHub. If you don't have an account, make one at github.com.

In **Mac Terminal**, run these grouped commands:

```bash
cd ~/Downloads
# move the site folder somewhere sensible
mv plinio-site ~/Sites/plinio-site 2>/dev/null || mkdir -p ~/Sites && mv plinio-site ~/Sites/
cd ~/Sites/plinio-site

# init git
git init
git add .
git commit -m "Initial site"
```

Then on github.com: **New repository** → name it `plinio-site` → don't add README/license → create. GitHub will show you two commands to push; they look like:

```bash
git remote add origin https://github.com/YOUR-USERNAME/plinio-site.git
git branch -M main
git push -u origin main
```

Run those in Mac Terminal.

### 3. Deploy to Vercel (5 min)
- Go to **vercel.com** → sign up with your GitHub account
- Click **Add New → Project**
- Import your `plinio-site` repo
- Framework Preset: **Other** (Vercel will auto-detect static HTML)
- Click **Deploy**

In ~30 seconds you'll have a live URL like `plinio-site-abc123.vercel.app`. **Test it.**

### 4. Connect plinio.me (10 min)
In the Vercel project dashboard:
- Go to **Settings → Domains**
- Add `plinio.me` and `www.plinio.me`
- Vercel will show you **2 DNS records** to add (an A record and a CNAME, or a set of nameservers)

Then in **GoDaddy**:
- Log in → **My Products** → find plinio.me → **DNS**
- Add the records Vercel gave you (delete conflicting default records — GoDaddy usually has a parked-page A record you'll need to remove)
- Save

DNS propagation takes **5 min to a few hours**. Once it propagates, plinio.me loads your site. Vercel automatically provisions a free SSL cert, so you'll get https:// too.

---

## Updating the site later

Any time you want to change something:

```bash
cd ~/Sites/plinio-site
# edit index.html
git add .
git commit -m "Update experience section"
git push
```

Vercel auto-deploys in ~20 seconds. That's it.

---

## Questions you might have

**Do I need to pay for anything?**
No. Vercel is free for personal sites. GitHub is free. You already paid for the domain.

**What about email (dioufplinio@gmail.com)?**
Vercel handles the website, but email needs a separate service. Easiest option: activate email through GoDaddy (the "Activate Email" button you saw on your dashboard) — comes free with some domain purchases. Or use Cloudflare Email Routing for free forwarding to your Gmail.

**What if I want to add more sections (projects, writing, etc.) later?**
The HTML is organized by section with clear comments (`<!-- ABOUT -->`, `<!-- EXPERIENCE -->`, etc.). Copy an existing section and modify it. Or just ask me.

---

## Design notes (so you know what to tweak)

- **Fonts**: Fraunces (serif, for headings) + Inter Tight (sans, for body). Both free via Google Fonts, already loaded.
- **Colors**: Warm off-white paper (`#f4f1ea`) with near-black ink and a burnt-orange accent (`#c8441f`). All defined as CSS variables at the top of `<style>` — change them once, affects the whole site.
- **Layout**: Single page, responsive. Mobile version collapses the experience grid into a stacked list.
- **Animations**: Subtle fade-in on scroll, hover states on experience rows and project cards. Nothing flashy.
