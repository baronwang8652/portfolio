# Ling-Lian Wang — portfolio

Product design portfolio. Hand-written HTML and CSS, no build step, no framework.

**Live:** https://baronwang8652.github.io/portfolio/

## Structure

```
index.html          Home — positioning and the three cases
about.html          About
editor-role.html    Case 1 — The Editor Role (DottedSign)
mobile-id.html      Case 2 — Mobile ID (DottedSign)
css/main.css        Two design systems in one file:
                      · the site (built for reading)
                      · the product UI layer used by the recreated
                        interfaces (built for density)
assets/             Résumé PDF, images
source/             Résumé source (resume.html) and case study drafts
```

## Editing

Open any `.html` file and edit the text directly. To change the site's colours,
edit the `:root` token block at the top of `css/main.css`; to change the recreated
product screens, edit the `.mock` token block further down.

To regenerate the résumé PDF after editing `source/resume.html`:

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --no-pdf-header-footer \
  --print-to-pdf="assets/Ling-Lian-Wang-Resume.pdf" \
  source/resume.html
```

## Note on the recreated interfaces

Every product screen in the case studies was rebuilt from scratch in HTML and CSS
to explain the design. No original product assets or confidential material is used,
and the QR code shown is structurally correct but deliberately not scannable.
