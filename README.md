# plinio.me — Deploy Guide

Your personal CV site. One file: `index.html`. Everything else is optional.

---

## What's already done

Your CV content is **already in the site**:
- Hero with your name and location
- Full About section pulled from your CV's summary
- Four work experiences (Betaramps, Cellulant, EBANX x2) with the full bullets
- Education (UT Austin, HBS CORe, University of Kent)
- Skills, tools, languages
- Contact block with `dioufplinio@gmail.com`

**You only need to do three small things before deploying** (see Step 1 below).

---

## 1. Small tweaks before deploy (5 min)

Open `index.html` in Claude Code or any text editor and do a Find & Replace for these:

**a) Add your LinkedIn URL.** Search for `YOUR-LINKEDIN` and replace it with your actual LinkedIn vanity URL (whatever comes after `linkedin.com/in/`).

**b) Drop in your CV PDF.** Rename your CV PDF to `plinio-diouf-cv.pdf` and place it in the same folder as `index.html`. The "Download CV" button already points at that filename.

→ You have two CVs (Payment and Product). Recommendation: use the **Product** version as the default download — it's the broader of the two and aligns with the PM roles at the top of your target list. Rename `Product_-_CV_Plinio_-_USA.pdf` to `plinio-diouf-cv.pdf`.

**c) (Optional) Change the email.** If you want a `hello@plinio.me` or `plinio@plinio.me` address later, use Find & Replace to swap `dioufplinio@gmail.com` with the new address. For now, Gmail is totally fine — it's what recruiters will expect and it's what's on your CV.

That's it. Now you're ready to deploy.

---

## 2. Put it on GitHub (10 min)

Vercel deploys from GitHub. If you don't have an account, make one at github.com (free).

In **Mac Terminal**, run these grouped commands:

```bash
# move the site folder somewhere sensible
mkdir -p ~/Sites
mv ~/Downloads/plinio-site ~/Sites/plinio-site
cd ~/Sites/plinio-site

# initialize git
git init
git add .
git commit -m "Initial site"
```

Then on **github.com**: click **New repository** → name it `plinio-site` → leave it public, don't add README/license → **Create repository**.

GitHub will show you commands to push. They look like this (substitute your actual username):

```bash
git remote add origin https://github.com/YOUR-USERNAME/plinio-site.git
git branch -M main
git push -u origin main
```

Run those in Mac Terminal. Refresh the GitHub page — your files should appear.

---

## 3. Deploy to Vercel (5 min)

- Go to **vercel.com** → **Sign Up with GitHub**
- Click **Add New → Project**
- Find and import your `plinio-site` repo
- Framework Preset: **Other** (Vercel auto-detects static HTML — no config needed)
- Click **Deploy**

In ~30 seconds you'll get a live URL like `plinio-site-abc123.vercel.app`. **Open it, test it.** Check that the CV downloads, the LinkedIn link works, the layout looks right on your phone.

---

## 4. Connect plinio.me (10 min + DNS wait)

**In Vercel:**
- Open your project → **Settings → Domains**
- Add `plinio.me`
- Add `www.plinio.me` (Vercel will auto-redirect one to the other)
- Vercel shows you DNS records to add — usually an `A` record pointing to `76.76.21.21` and a `CNAME` for `www` pointing to `cname.vercel-dns.com`

**In GoDaddy:**
- Log in → **My Products** → find **plinio.me** → click **DNS**
- Delete any existing `A` record pointing to a parked page
- Add the records Vercel gave you (exactly as shown — GoDaddy's interface has an "Add Record" button)
- Save

**Then wait.** DNS propagation takes anywhere from 5 minutes to a few hours. You can check progress at dnschecker.org. Once it propagates, `plinio.me` loads your site and Vercel auto-provisions a free SSL cert (so you get `https://` automatically).

---

## Updating the site later

Want to tweak a bullet, add a new role, swap out the CV?

```bash
cd ~/Sites/plinio-site
# edit index.html in Claude Code or your editor
git add .
git commit -m "Updated experience"
git push
```

Vercel auto-deploys in ~20 seconds. Live.

---

## FAQ

**Do I need to pay for anything?**
No. Vercel is free for personal sites (under 100GB bandwidth/month — you won't come close). GitHub is free. You already paid for the domain.

**What about email (hello@plinio.me)?**
The site uses your Gmail (`dioufplinio@gmail.com`) for now. If you want a `@plinio.me` address later, two free options:
- **GoDaddy email**: click the "Activate Email" button on your GoDaddy dashboard (comes free with some domains)
- **Cloudflare Email Routing**: free forwarding — anything sent to `hello@plinio.me` lands in your Gmail inbox

**Can I add more sections later (projects, writing, case studies)?**
Yes. The HTML has clear section comments (`<!-- ABOUT -->`, `<!-- EXPERIENCE -->`, etc.). Copy an existing `section` block and modify it. Or just ask Claude to do it.

**Should I use the Payment CV or the Product CV as the download?**
My vote: **Product CV** as the default download. Your target roles lean PM. Recruiters scanning LinkedIn and applying for PM roles expect that framing. The only reason to host the Payment CV would be if you're specifically applying to roles like "Payments Ops" or "Payment Strategy" — in which case, swap the download temporarily for that application.

---

## Design notes

- **Fonts**: Fraunces (serif, for headings) + Inter Tight (sans, for body). Both free via Google Fonts.
- **Colors**: Warm off-white paper (`#f4f1ea`) with near-black ink. No accent color — kept it restrained per your "genuine, simple, clean" direction. All defined as CSS variables at the top of `<style>` — change them once, the whole site updates.
- **Layout**: Single page, responsive. Mobile collapses the year column into a stacked list.
- **Animations**: Subtle fade-in on scroll. Nothing flashy.
