# QUICK REFERENCE CARD — YouTube Video Design System
## The 8 Golden Rules (Memorize These)

1. **NO ORPHANS** — Every element must be in a group of 3+ items
2. **SAFE AREA** — 96px margin on ALL sides (1920x1080)
3. **SPACING ONLY** — Use 8px / 20px / 40px. Nothing else.
4. **ICON SIZING** — 48px small / 64px medium / 80px large. Always square.
5. **TEXT HIERARCHY** — Max 3 levels: Title > Subtitle > Body
6. **FILL THE FRAME** — Content must occupy 70-85% of safe area
7. **GROUP THEN PLACE** — Build groups first, position second
8. **VALIDATE BEFORE RENDER** — Run checks every time, no exceptions

---

## Spacing Quick Reference

| Use Case | Value |
|----------|-------|
| Between text lines | 8px |
| Between related elements | 20px |
| Between sections/groups | 40px |
| Safe area margin | 96px |

**NEVER use:** 10px, 15px, 24px, 30px, 32px, 50px

---

## Icon Sizing Rules

| Context | Size |
|---------|------|
| Inline / small callout | 48x48px |
| Standard card / section | 64x64px |
| Hero / feature | 80x80px |

**Always:** Square bounding box. Centered in container. Consistent size per frame.

---

## Text Hierarchy Rules

| Level | Size | Weight | Usage |
|-------|------|--------|-------|
| Title | 64-80px | Bold | One per frame max |
| Subtitle | 36-48px | SemiBold | 1-2 per frame |
| Body | 24-32px | Regular | Supporting content |

**NEVER use more than 3 font sizes in one frame.**

---

## The Grouping Rule

BEFORE placing any element, ask:
- Does it have at least 2 other elements near it?
- Is the spacing between them 8px, 20px, or 40px?
- Does the group fill its zone without dead space?

If NO to any — redesign before rendering.

---

## Frame Zone System (1920x1080)

```
+--[96px margin]------------------------+
|  HEADER ZONE    (rows 96-200)        |
|  CONTENT ZONE   (rows 200-880)       |
|  FOOTER ZONE    (rows 880-984)       |
+---------------------------------------+
   96px                            96px
   margin                         margin
```

Content zone = 1728px wide x 680px tall. Fill 70-85% of it.

---

## Before You Render — Checklist

- [ ] Every element is in a group of 3+
- [ ] No element is within 96px of frame edge
- [ ] All spacing is 8/20/40px only
- [ ] Icons are 48/64/80px (square)
- [ ] Max 3 font sizes used
- [ ] Content fills 70-85% of safe area
- [ ] No isolated text blocks
- [ ] Header, content, footer zones all occupied

---

## Common Failures & Fixes

| Failure | Fix |
|---------|-----|
| Orphaned headline | Add icon + subtitle to make a group |
| Dead space bottom | Add footer bar or extend content group |
| Icon too small | Upgrade to next size tier |
| Too many font sizes | Consolidate — pick 3 and stick to them |
| Text touching edge | Add 96px margin enforcement |
| Uneven columns | Use equal-width grid with 40px gutters |

---

## Color Usage

- **Primary** = Brand color (CTAs, headlines, key icons)
- **Secondary** = Supporting color (accents, dividers)
- **Neutral** = Background, body text
- Max **3 colors** active per frame
- Background ≠ pure white (#F8F8F8 minimum) or pure black (#111111 max)

---

*Part of the Social Alignment YouTube Video Design System*
*Use with: YOUTUBE_VIDEO_DESIGN_SYSTEM.md | CLAUDE_SYSTEM_PROMPT_VIDEO.md*
