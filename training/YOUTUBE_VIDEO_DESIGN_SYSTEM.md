# YouTube Video Design System
## Complete Design Bible for Hyperframes + video-use

**Version:** 1.0 | **For:** Claude Code + Hyperframes + video-use workflows

---

## PART 1: The 4 Core Failure Modes (Your Current Problems)

### Failure 1: Orphaned Elements
**What it is:** A single text block, icon, or label placed alone with no visual grouping.
**Why it happens:** Elements are added one at a time without checking their context.
**The fix:** NEVER place fewer than 3 elements in a visual cluster. Title + subtitle + supporting element = minimum group.

### Failure 2: Dead Space
**What it is:** Large unused areas in the frame (typically bottom 30%, left/right padding zones).
**Why it happens:** Content is top-loaded and the frame isn't filled to the safe area boundary.
**The fix:** Content must occupy 70-85% of the safe area. If a zone is empty, add a footer bar, a background texture element, or extend an existing content group.

### Failure 3: Incorrect Icon Sizing
**What it is:** Icons that are too small (under 40px) or inconsistently sized across the frame.
**Why it happens:** Icons are sized by feel rather than by a defined system.
**The fix:** Use ONLY these sizes: 48px (small), 64px (medium), 80px (large). Always square bounding box. Never mix sizes within the same frame tier.

### Failure 4: Text Misalignment
**What it is:** Text blocks that don't align to a grid, or that start at arbitrary x/y positions.
**Why it happens:** Text is positioned absolutely without reference to a baseline grid.
**The fix:** All text must snap to a 8px baseline grid. Left-align to the 96px safe area boundary or to a defined column start.

---

## PART 2: Canvas & Safe Area Specifications

### Standard YouTube Frame
- **Resolution:** 1920 x 1080px
- **Safe Area Margin:** 96px on ALL four sides
- **Usable Width:** 1728px (1920 - 96 - 96)
- **Usable Height:** 888px (1080 - 96 - 96)

### Zone Breakdown
```
Y: 0-96        = Top bleed (no content)
Y: 96-200      = Header zone (title, brand bar, episode number)
Y: 200-880     = Content zone (main body, columns, graphics)
Y: 880-984     = Footer zone (lower thirds, source citations, CTAs)
Y: 984-1080    = Bottom bleed (no content)

X: 0-96        = Left bleed
X: 96-1824     = Safe horizontal span
X: 1824-1920   = Right bleed
```

### Column Grid
- **1-column layout:** Full 1728px width
- **2-column layout:** 844px each, 40px gutter
- **3-column layout:** 549px each, 40px gutters
- **4-column layout:** 402px each, 40px gutters

---

## PART 3: Typography System

### Type Scale (Video-Optimized)
| Level | Size | Weight | Line Height | Usage |
|-------|------|--------|-------------|-------|
| Display | 80-96px | Black/900 | 1.1x | Episode titles, hero statements |
| H1 | 64-72px | Bold/700 | 1.15x | Section headers, main topics |
| H2 | 48-56px | SemiBold/600 | 1.2x | Sub-headers, card titles |
| H3 | 36-40px | SemiBold/600 | 1.25x | Labels, callout headers |
| Body | 28-32px | Regular/400 | 1.4x | Body copy, descriptions |
| Caption | 20-24px | Regular/400 | 1.3x | Footnotes, source credits |

### Rules
- Max 3 type sizes per frame
- Never go below 20px (unreadable at video compression)
- Display and H1 never appear on the same frame
- Body copy max line length: 800px (50-60 chars)

---

## PART 4: Spacing System

### The Only Allowed Values
| Token | Value | Use |
|-------|-------|-----|
| xs | 8px | Between tightly related items (icon + label) |
| sm | 20px | Between related elements in a group |
| md | 40px | Between groups / columns |
| lg | 80px | Between major sections |
| xl | 96px | Safe area margin only |

**Absolutely prohibited values:** 10px, 12px, 15px, 16px, 24px, 30px, 32px, 50px, 60px, 72px

---

## PART 5: Icon System

### Size Tiers
| Tier | Size | Context |
|------|------|---------|
| Small | 48x48px | Inline labels, list items, caption icons |
| Medium | 64x64px | Card headers, feature points, section markers |
| Large | 80x80px | Hero features, primary CTAs, solo callouts |
| XL | 120x120px | Full-width graphic elements only |

### Rules
- ALL icons must use a square bounding box
- Icons within the same frame tier must be the same size
- Icon + label spacing: 8px (xs token)
- Never place an icon without an accompanying label
- Minimum contrast ratio for icons on background: 4.5:1

---

## PART 6: Color System

### Per-Frame Limits
- Maximum 3 active colors per frame
- Background (1 color) + Primary (1 color) + Accent (1 color)
- Gradients count as 2 colors

### Background Rules
- No pure white (#FFFFFF) — use #F5F5F5 or lighter brand neutral
- No pure black (#000000) — use #111111 or #0D0D0D
- Background must have sufficient contrast with body text (WCAG AA: 4.5:1)

### Brand Color Application
- Primary brand color: CTAs, active states, headline accents, key icons
- Secondary brand color: dividers, background panels, supporting elements
- Never use brand colors at full saturation on dark backgrounds without opacity adjustment

---

## PART 7: Layout Patterns

### Pattern A: Title + 3-Point Layout
Use for: Topic intros, episode openers, key takeaway frames
```
[HEADER ZONE: Episode title + brand mark]
[CONTENT ZONE: Large title left | 3 bullet points right with icons]
[FOOTER ZONE: Source bar or progress indicator]
```

### Pattern B: Full-Width Statement
Use for: Quote frames, transition cards, emphasis moments
```
[HEADER ZONE: Minimal branding only]
[CONTENT ZONE: Centered large display text, 2-3 lines max]
[FOOTER ZONE: Speaker attribution or episode context]
```

### Pattern C: Data/Comparison Grid
Use for: Statistics, before/after, feature comparisons
```
[HEADER ZONE: Topic label]
[CONTENT ZONE: 2 or 3 equal columns with icons + stats + labels]
[FOOTER ZONE: Source citation]
```

### Pattern D: Person + Context
Use for: Speaker intros, guest features, testimonial frames
```
[HEADER ZONE: Show branding]
[CONTENT ZONE: Image/avatar left (40%) | Name + title + context right (60%)]
[FOOTER ZONE: Social handles or credentials]
```

---

## PART 8: Animation & Transition Rules

### Entry Animations
- Elements enter from their natural reading direction (left or bottom)
- Stagger grouped elements: 80-120ms between each item
- No animation should exceed 400ms duration
- Use ease-out curves (not linear, not bounce)

### Exit Animations
- Mirror entry direction (fade-out or slide-out)
- Exits should be 20% faster than entries
- Never animate more than 5 elements simultaneously

### Do Not Use
- Spin/rotation entry effects
- Scale-from-zero effects on text
- Blur-in effects (compression destroys them)
- Parallax on more than 2 layers

---

## PART 9: Brand Integration Rules

### Logo Placement
- Always in header zone, top-left or top-right
- Minimum size: 40px height
- Clear space: equal to logo height on all sides
- Never overlay on busy background without a backing panel

### Episode/Show Identity
- Show name: H2 weight, consistent position frame-to-frame
- Episode number: Caption size, secondary color
- Season/series: Optional, caption size only

### Consistency Requirements
- Font choice must be identical across all frames in an episode
- Primary color application must be consistent (don't switch which element gets brand color)
- Logo variant (light/dark) must be consistent per background type

---

## PART 10: Pre-Render Validation Checklist

### Structure Checks
- [ ] All elements are in groups of 3+
- [ ] No single element stands alone
- [ ] Header zone has content
- [ ] Content zone is 70-85% filled
- [ ] Footer zone has content
- [ ] No content within 96px of any edge

### Spacing Checks
- [ ] Only 8px / 20px / 40px / 80px / 96px spacing used
- [ ] Column gutters are all 40px
- [ ] Icon-to-label spacing is 8px
- [ ] Group-to-group spacing is 40px minimum

### Typography Checks
- [ ] Maximum 3 font sizes in frame
- [ ] No text below 20px
- [ ] Line lengths under 800px
- [ ] Text contrast passes 4.5:1

### Icon Checks
- [ ] All icons in same tier are same size
- [ ] All icons have square bounding boxes
- [ ] No icon without a label
- [ ] Icon sizes are 48 / 64 / 80px only

### Color Checks
- [ ] Maximum 3 active colors
- [ ] No pure white or pure black background
- [ ] Brand color applied consistently

---

*Social Alignment YouTube Video Design System v1.0*
*Companion files: QUICK_REFERENCE_CARD.md | CLAUDE_SYSTEM_PROMPT_VIDEO.md | CLAUDE_CODE_HYPERFRAMES_GUIDE.md*
