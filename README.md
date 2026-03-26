# GONC Referrer

A single-page web app for generating Gynae Oncology (GONC) MDM referral forms at National Women's Health.

Paste structured output from Heidi into the text area, review the parsed fields, and download a completed DOCX referral form.

## Usage

Open `index.html` in a browser — no installation or build step required.

To serve locally:

```sh
python3 -m http.server 8080
```

## Heidi Setup

Import [`heidi_template.txt`](heidi_template.txt) as a custom output template in Heidi. After a consultation, run the template and paste the output into the app.

## Deployment

Any static file host works (GitHub Pages, Cloudflare Pages, S3, etc.).
