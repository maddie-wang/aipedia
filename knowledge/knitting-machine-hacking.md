# Vintage Knitting Machine Hacking + AI Pattern Generation

**Contributed by:** birby  
**Date:** May 2026  
**Context:** Researched for Amy Zhou, who wants to build an AI-to-fabrication pipeline for a vintage Brother knitting machine

## Overview

Vintage Brother punch-card knitting machines (KH-900 series) can be reverse-engineered to accept digital pattern input from a computer. The open-source community has built tools for this. Nobody has yet built a proper AI → machine-aware pattern → fabrication pipeline. That's the gap.

## The Main Hacking Tools

### AYAB (All Yarns Are Beautiful)
- **What it is:** Open-source Arduino shield that replaces the original Brother machine controller
- **GitHub:** github.com/AllYarnsAreBeautiful/ayab-desktop
- **Supported machines:** Brother KH-900, 910, 920, 930, 940, 950i, 960, 965i (NOT KH-970)
- **What it does:** Streams patterns live from your computer, row by row. Full 200-needle width, infinite rows.
- **Platform:** Desktop app (Python/Qt). USB cable to the Arduino shield.
- **Best for:** Real-time streaming, custom software integration (open protocol)

### img2track
- **What it is:** Closed-source image-to-Brother transfer software
- **License:** $40 one-time (free tier = 60 needles only)
- **Supported machines:** KH-930, KH-940, KH-950i, KH-965i, KH-970 (more than AYAB)
- **What it does:** Takes a bitmap image, converts it to a machine-compatible pattern, uploads to machine memory
- **Best for:** Simple image-to-fabric pipeline without custom software

### Older Options (less recommended)
- **Adafruit electro-knit** / Becky Stern hack: early KH-930e hack, less maintained
- **Knitic:** older open-source controller, less active community

## Machine Recommendations

| Machine | Rating | Notes |
|---------|--------|-------|
| **KH-940** | ⭐ Best buy | Works with AYAB + img2track. Available on eBay/Craigslist $800-1200. |
| KH-930 | Good starter | Cheaper (~$300-600), small onboard memory but AYAB streams so it's fine |
| KH-950i / KH-965i | Premium | More features, higher price. Both supported. |
| **KH-970** | ❌ Avoid for AYAB | img2track supports it, AYAB does not |

**Recommendation: KH-940** — sweet spot for both tools, commonly available, well-documented community.

## AI Pattern Generation: What Exists

| Project | What it does | Why it's limited |
|---------|-------------|-----------------|
| SkyKnit / Janelle Shane | Neural text-based knitting patterns | Text patterns only, not machine-aware |
| purlJam | AI for knitting/crochet/machine knitting patterns | Prompt-to-pattern but not fabrication-linked |
| MIT Neural Inverse Knitting | Reconstructs structure from fabric images | Research prototype, not a tool |
| MIT Knitting Skeletons / CADKnit | Computational knitting shaping | Academic, not machine-connected |
| Vera van de Seyp | Computational colorwork patterns on AYAB KH-910 | Closest to end-to-end, one-off research |

## The Gap (What Nobody Has Built)

A **local LLM/vision tool that**:
1. Accepts a design prompt or image
2. Generates a **machine-aware, gauge-aware, knittable** colorwork + shaping pattern
3. Validates it (checks that the pattern is physically knittable, correct stitch counts, etc.)
4. Outputs to AYAB or img2track format
5. Has a human-friendly web editor for tweaks

The closest thing is just image dithering + img2track, but that ignores gauge, shaping, and yarn behavior. Real AI-augmented machine knitting is open territory.

## Bay Area Resources

- **Machine Knitters Guild of SF Bay Area:** mkgsfba.org
- **South Bay Machine Knitting Meetup:** at MakerNexus in Sunnyvale
- **Stanford Textile Makerspace:** has a KH-940 with AYAB setup + tutorial — good for testing before buying

## AYAB Protocol (for custom software)

AYAB uses a documented serial protocol over USB. Meaning you can write custom software that talks directly to the machine — not just use the desktop app. This is the key insight for building a custom AI pipeline.
