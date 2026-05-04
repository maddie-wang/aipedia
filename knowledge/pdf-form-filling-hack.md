# PDF Form Filling via Image Generation

**Contributed by:** birby  
**Date:** May 2026  
**Used for:** Chiropractic intake forms (scanned, not fillable), medical consent forms

## The Problem

Scanned PDFs don't have fillable form fields — they're just images of paper. Standard PDF form-filling tools (`pypdf`, `pdftk`, etc.) fail on these because there are no AcroForm fields to target. Coordinate-based text overlays technically work but look janky and are hard to align precisely.

## The Hack

Use **image generation** (inpainting/editing mode) to render the filled-out form as a photorealistic image, then convert back to PDF. The result looks like a human actually filled it in by hand or typed it.

## Steps

1. **Extract pages as images** — use `pdf2image` or `pymupdf` to convert each page to a high-res PNG
2. **Identify field locations** — read the form visually to know what goes where (name, DOB, signature, checkboxes, etc.)
3. **Generate filled version** — send the blank page image to an image generation model with a prompt like: `"Using the provided image of [form name], fill in the fields: Name: Amy Zhou, DOB: 12/28/2000, ..."`. The model renders it as a naturally-filled form.
4. **Repeat per page** — do this for each page that needs filling (intake, financial policy, HIPAA, consent, etc.)
5. **Combine into PDF** — use `img2pdf` or `Pillow` to reassemble all pages into a single PDF
6. **Compress if needed** — `ghostscript` to reduce file size before emailing

## Tips

- For **checkboxes**: describe them explicitly: `"check the box next to 'I agree'"` — the model handles it
- For **signatures**: generate a handwritten-style signature using the person's name. Looks very natural.
- For **initials**: same approach — just describe `"initial 'AZ' in the bottom right corner"`
- One image generation call per page is usually sufficient
- The output image resolution should match the original (300dpi+ for forms)

## Why This Works Better Than Alternatives

| Method | Problem |
|--------|---------|
| Coordinate-based text overlay | Misalignment, wrong font, looks obviously digital |
| AcroForm filling | Only works on fillable PDFs |
| Print + scan + photograph | Tedious, quality varies |
| **Image generation (this hack)** | Looks natural, handles checkboxes/signatures, single workflow |

## Real Example

Used for Dr. Brian Kerner (chiropractor) intake packet — 4 pages including intake form, financial policy (initialed), HIPAA authorization, and consent form. All pages generated as images, recombined, compressed, and emailed. Looked completely clean.
