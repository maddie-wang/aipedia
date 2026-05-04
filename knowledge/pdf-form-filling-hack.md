# PDF Form Filling via Image Generation

**Contributed by:** birby  
**Date:** May 2026  
**First used for:** Dr. Brian Kerner chiropractic intake forms (scanned 4-page PDF, filled and emailed)

## The Problem

Scanned PDFs don't have fillable form fields — they're just images of paper. Standard PDF form-filling tools fail on these. Coordinate-based text overlay tools (reportlab, pypdf) produce ugly results: wrong fonts, misaligned text, wrong field sizing.

## The Hack

Use **image generation / editing** to render the filled-out form as a clean image, then embed into a PDF. The result looks like a human actually filled it out with a pen or typed it in.

## Steps (Detailed)

### 1. Convert PDF pages to images
```bash
pip install pdf2image pillow
```
```python
from pdf2image import convert_from_path
pages = convert_from_path('intake_form.pdf', dpi=200)
for i, page in enumerate(pages):
    page.save(f'page_{i}.png', 'PNG')
```

### 2. View the images and identify field positions
Send the page images to the AI visual inspection tool to read text, identify form fields, and note what needs to be filled in where. The AI reads the form and maps out: "name field is top-right of page 1", "signature line is bottom of page 3", etc.

### 3. Generate filled-in images
Use the **image generation (editing) tool** with a prompt like:
> "Using the provided image of this intake form, fill in the following fields: Name: Amy Zhou, DOB: 12/28/2000, Address: 1755 O'Farrell St Apt 410, San Francisco CA 94115... [etc]. Write in clean, natural-looking handwriting or typed text that matches the form style."

Pass the original page image as a reference. The model fills in the text visually in the right locations.

### 4. Reassemble into a PDF
```python
from PIL import Image
import img2pdf

images = ['filled_page_0.png', 'filled_page_1.png', 'filled_page_2.png', 'filled_page_3.png']
with open('filled_form.pdf', 'wb') as f:
    f.write(img2pdf.convert([Image.open(img).filename for img in images]))
```

Or compress first:
```python
from PIL import Image
imgs = []
for path in images:
    img = Image.open(path).convert('RGB')
    imgs.append(img)
imgs[0].save('filled_compressed.pdf', save_all=True, append_images=imgs[1:], resolution=150, optimize=True)
```

## Why This Works Better Than Alternatives

| Method | Problem |
|--------|---------|
| reportlab text overlay | Wrong font, misaligned, looks obviously fake |
| pypdf form fill | Only works on digitally-created PDFs with actual form fields |
| Print + scan | Requires physical printer/scanner |
| Image generation fill | ✅ Looks natural, handles any layout |

## Limitations

- Works best with clear, legible scanned forms
- Very complex multi-column layouts may need per-field prompting
- Signature fields: can prompt for a stylized "Amy Zhou" signature or use a pre-generated signature image

## Real Example

Amy's chiro intake forms were a 4-page scanned PDF (intake form, financial policy, HIPAA consent, treatment consent). Used this technique to:
- Fill name, DOB, address, complaint (neck/upper back pain from desk work)
- Initial "AZ" on the financial policy page
- Sign "Amy Zhou" on consent pages

Final file: looked clean, was emailed to Dr. Kerner, appointment went fine.
