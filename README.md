# Nebula Cloud website

Source for [nebulacloud.ca](https://nebulacloud.ca), the website for Nebula Cloud, an AWS cloud consulting business.

It is a static, single-page site with no build step: plain HTML, CSS and a small amount of JavaScript.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole site: Home, Services, About, Contact, Terms & Conditions and Privacy Notice |
| `favicon.svg` | Browser tab icon (Nebula Cloud monogram) |

## Pages

Pages are switched with the URL hash, so every page is served by `index.html`:

| Page | URL |
|---|---|
| Home | `https://nebulacloud.ca/` |
| Services | `https://nebulacloud.ca/#services` |
| About | `https://nebulacloud.ca/#about` |
| Contact | `https://nebulacloud.ca/#contact` |
| Terms & Conditions | `https://nebulacloud.ca/#terms` |
| Privacy Notice | `https://nebulacloud.ca/#privacy` |

Individual services can be linked directly, for example `#svc-migration` or `#svc-security`.

## Preview locally

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Deploy

### Option A: Amazon S3 + CloudFront

1. Create a private S3 bucket and upload `index.html` and `favicon.svg`.
2. Create a CloudFront distribution with the bucket as origin, using Origin Access Control, with `index.html` as the default root object.
3. Request an ACM certificate in `us-east-1` for `nebulacloud.ca` and `www.nebulacloud.ca` and attach it to the distribution.
4. Point the domain at CloudFront (Route 53 alias records, or CNAME/ALIAS at your DNS provider).

To publish updates:

```bash
aws s3 sync . s3://<bucket-name> --exclude ".git/*" --exclude "README.md" --exclude ".gitignore"
aws cloudfront create-invalidation --distribution-id <distribution-id> --paths "/*"
```

### Option B: GitHub Pages

1. In the repository, go to **Settings → Pages** and publish from the `main` branch, root folder.
2. Add a file named `CNAME` containing `nebulacloud.ca`.
3. Configure DNS for the domain as described in GitHub's custom domain documentation.

## Before launch

- **Legal name:** the Terms & Conditions and Privacy Notice refer to "Nebula Cloud". Update them with the registered corporate name once incorporation is complete, and have both pages reviewed by a lawyer.
- **Contact form:** the form currently prepares an email in the visitor's mail app, addressed to support@nebulacloud.ca. To receive submissions directly, connect it to a form backend (for example API Gateway + Lambda + Amazon SES, or a hosted form service).
- **Fonts:** Inter and Space Grotesk load from Google Fonts. The Privacy Notice discloses this. If you self-host the fonts, update section 9 of the Privacy Notice.
- **Business details:** check the hours (Mon–Fri, 9–5 ET), the one-business-day reply target and the 24-month inquiry retention period in the Privacy Notice.

## Brand

- Colours: violet `#6D28D9`, indigo `#1E1B4B`, cyan accent `#22D3EE`, text `#0F172A`
- Typefaces: Space Grotesk (headings, wordmark), Inter (body)
- Tagline: SECURE. RESILIENT. OPERATIONAL.

---

Amazon Web Services, AWS and related marks are trademarks of Amazon.com, Inc. or its affiliates.
