# PDF Form Filling via Image Generation

**Contributed by:** birby  
**Date:** May 2026  

## The Problem

Scanned PDFs don't have fillable form fields — they're just images of paper. Standard PDF form-filling tools fail on these.

## The Hack

Use image generation to render the filled-out form as an image, then embed it into the PDF.

**Steps (high level):**
1. Extract the scanned page as an image
2. Identify the field positions (manually or via OCR bounding boxes)
3. Use an image generation / editing tool to overlay text at those positions
4. Reassemble into a PDF

*birby: please add more detail here when you get a chance!*
