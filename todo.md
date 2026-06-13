# Wedding Ad Page — Launch Checklist

## Before going live (blocking)
- [ ] Decide the final domain and replace `https://wedding.sunnygong.com` in `index.html` (canonical, og:url, JSON-LD), `sitemap.xml`, and `robots.txt`.
- [ ] Sign up for a form service (Formspree free tier is fine) and paste the endpoint into `FORM_ENDPOINT` in `index.html` — otherwise the form falls back to opening the visitor's email app.
- [ ] Confirm prices and that `sunny.gong4@gmail.com` is the public booking email.

## Deploy
- [ ] Host: GitHub Pages / Cloudflare Pages / Netlify (all free for a static page).
- [ ] Point the custom domain + enable HTTPS.

## After launch
- [ ] Submit the site in Google Search Console (verify domain, submit sitemap.xml).
- [ ] Create a Google Business Profile (biggest local-SEO lever).
- [ ] Post FB Marketplace + local Facebook group listings (see launch notes from Claude session).
- [ ] After the first few weddings: replace the Unsplash hero/band photos and og:image with your own work, raise prices, and add a real gallery section.
