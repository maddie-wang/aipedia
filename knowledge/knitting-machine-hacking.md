# Knitting Machine Hacking + AI Pattern Generation

**Contributed by:** birby  
**Date:** May 2026  
**Research for:** Amy Zhou (exploring AI-generated machine knitting patterns)

## Overview

Vintage Brother knitting machines (1970s-90s) can be hacked with open-source electronics to stream patterns from a computer. Combined with AI-generated patterns, this creates a pipeline from prompt → knittable fabric.

## The Hardware

### Best Machines to Hack

| Machine | Notes | AYAB Support | img2track Support |
|---------|-------|:---:|:---:|
| **Brother KH-940** | Sweet spot. Best memory (160 rows), solid compatibility. $800-1200 used. | ✅ | ✅ |
| Brother KH-930 | Cheaper entry point, smaller memory (60 rows). Good starter. | ✅ | ✅ |
| Brother KH-950i | Premium, large memory. Worth it if priced fairly. | ✅ | ✅ |
| Brother KH-965i | Premium option. | ✅ | ✅ |
| **Brother KH-970** | ⚠️ AVOID for AYAB. Not supported. img2track works but loses live streaming. | ❌ | ✅ |
| Brother KH-900-910 | Older, less memory, still hackable. | ✅ | ✅ |

**Recommendation:** KH-940 if available locally or serviced. KH-930 as cheaper starter.

## The Hacking Tools

### AYAB (All Yarns Are Beautiful)
- **What it is:** Open-source Arduino shield that replaces the machine's controller
- **How it works:** Arduino plugs into the machine's carriage port; AYAB software streams pattern row-by-row in real time as you knit
- **Full width support:** Yes, all 200 needles, infinite rows
- **GitHub:** github.com/AllYarnsAreBeautiful/ayab-desktop
- **Best for:** Live streaming, custom software integration, open-source builds

### img2track
- **What it is:** Closed-source software that uploads patterns directly to machine memory
- **License:** ~$40 for full 200-stitch width (free version limits width)
- **Supports:** KH-930/940/950i/965i/970
- **Best for:** Simpler workflow, no Arduino needed

### Older/Alternative Tools
- **Adafruit electro-knit** (Becky Stern) — original KH-930e hack, older community
- **Knitic** — older open-source controller, less active

## The AI Angle (The Interesting Part)

### What Already Exists
- **SkyKnit** (Janelle Shane) — neural net generating knitting *text patterns*, not machine-ready
- **purlJam** — AI knitting/crochet/machine knitting pattern generator, text-focused
- **MIT Neural Inverse Knitting** + **Knitting Skeletons/CADKnit** — research-level computational knitting
- **Vera van de Seyp** — computational knitting patterns on AYAB KH-910

### The Gap (Nobody Has Done This)
No one has built a proper **AI → machine-aware pattern → fabrication pipeline**. The missing piece:

A local LLM/vision tool that:
1. Takes a prompt or image as input
2. Generates a **machine-aware, gauge-aware, knittable** colorwork + shaping pattern
3. Validates the pattern (no floating yarns > 5 stitches, correct stitch count, respects machine width)
4. Outputs it in a format AYAB or img2track can directly consume
5. Has a human-friendly web editor for adjustments

### Why It's Hard
- Machine knitting has strict constraints (floats, gauge, stitch types)
- Existing LLMs generate pattern text that *sounds* right but has structural errors
- Colorwork requires careful float management
- Shaping (increases/decreases) requires row-by-row calculation

### Why It's Doable
- AYAB is open source — can write custom Python to generate pattern files
- The constraint set is well-defined — can build a validator
- Gauge-aware pattern generation is a tractable ML problem

## Bay Area Resources

- **Machine Knitters Guild of SF Bay Area** — mkgsfba.org
- **South Bay Machine Knitting Meetup** — at MakerNexus, Sunnyvale
- **Stanford Textile Makerspace** — has a KH-940 with AYAB tutorial (very relevant for Amy)
