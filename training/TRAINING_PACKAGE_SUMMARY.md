# Training Package Summary
## YouTube Video Design System — Social Alignment

**Version:** 1.0 | **Created:** May 2026 | **Repo:** Social-Alignment-Git/power-design

---

## WHAT THIS PACKAGE SOLVES

Your Claude Code + Hyperframes + video-use workflow was producing frames with:
- Orphaned elements (text/icons with no visual grouping)
- Dead space (bottom 30-40% of frames unused)
- Wrong icon sizes (32px instead of 64px)
- Off-system spacing (12px, 15px instead of 8/20/40px)
- Missing header and footer zones
- Inconsistent font hierarchies

This training package installs guardrails at the **code level**, **prompt level**, and **workflow level** so Claude Code cannot produce those failures anymore.

---

## THE 6 FILES — WHAT EACH ONE DOES

### 1. QUICK_REFERENCE_CARD.md
**Use it:** Every day. Print it or keep it open.
**What it contains:**
- The 8 Golden Rules (memorize these)
- Spacing quick reference table
- Icon sizing rules
- Typography hierarchy
- Pre-render checklist
- Common failures & fixes table

**When to use:** Before every render session. Check the checklist before you submit any frame for render.

---

### 2. YOUTUBE_VIDEO_DESIGN_SYSTEM.md
**Use it:** When onboarding new workflows or debugging design decisions.
**What it contains:**
- 10-part complete design bible
- The 4 failure modes and their root causes
- Canvas specs, zone breakdown, column grids
- Typography scale, spacing system, icon system
- Color rules, layout patterns A/B/C/D
- Animation rules, brand integration rules
- Complete pre-render validation checklist

**When to use:** Reference when you're building a new frame type or when Claude produces something unexpected.

---

### 3. CLAUDE_SYSTEM_PROMPT_VIDEO.md
**Use it:** PASTE INTO EVERY CLAUDE CODE SESSION for video work.
**What it contains:**
- Complete system prompt (copy the code block)
- The 8 non-negotiable rules written for Claude to follow
- Zone system, column grids, layout patterns
- Output format requirements (Claude must validate before rendering)
- Escalation prompts for when Claude makes mistakes
- Instructions for adding to CLAUDE.md

**When to use:** Start of every Claude Code session. Also add to your project's CLAUDE.md file so it loads automatically.

**Quick copy command:**
```bash
cat training/CLAUDE_SYSTEM_PROMPT_VIDEO.md | pbcopy
```

---

### 4. CLAUDE_CODE_HYPERFRAMES_GUIDE.md
**Use it:** When writing or reviewing frame generation code.
**What it contains:**
- Wrong vs. correct code patterns (side by side)
- Canvas constants (copy into every frame file)
- YouTubeVideoFrame class (full implementation)
- Pattern A and Pattern D templates (copy-paste ready)
- Validation helper functions
- Hyperframes integration wrapper (safe_render)
- Copy-paste snippets for header, footer, icon points

**When to use:** Copy the YouTubeVideoFrame class into your project. Use the snippets as boilerplate for every new frame.

---

### 5. PRACTICAL_EXAMPLE_DIANA_FRAME.md
**Use it:** As a reference for your real-world frame fixes.
**What it contains:**
- The broken Diana Dikes code (annotated with every error)
- The fixed Diana Dikes code (full implementation with comments)
- Expected validation output when it passes
- Adaptation guide for other guests/personas
- Key lessons table mapping failures to fixes

**When to use:** Use as a template for any Person Card (Pattern D) frame. Adapt the PERSON and POINTS variables, keep everything else identical.

---

### 6. TRAINING_PACKAGE_SUMMARY.md (this file)
**Use it:** Orientation and onboarding.
**What it contains:** This overview, the implementation roadmap, troubleshooting guide, and expected results.

---

## IMPLEMENTATION ROADMAP

### Day 1 — Install the System Prompt (30 min)
```bash
# 1. Clone the repo if you haven't
git clone https://github.com/Social-Alignment-Git/power-design.git
cd power-design

# 2. Add system prompt to your video project's CLAUDE.md
cat training/CLAUDE_SYSTEM_PROMPT_VIDEO.md >> your-video-project/CLAUDE.md

# 3. Or copy to clipboard for manual paste
cat training/CLAUDE_SYSTEM_PROMPT_VIDEO.md | pbcopy
```

### Day 2 — Add the Base Class (1 hour)
```bash
# Copy the YouTubeVideoFrame class to your project
mkdir -p your-video-project/src
# Open CLAUDE_CODE_HYPERFRAMES_GUIDE.md
# Copy the YouTubeVideoFrame class
# Paste into your-video-project/src/frame_base.py
# Add your Hyperframes render call to _render()
```

### Day 3 — Rebuild One Broken Frame (1-2 hours)
- Open PRACTICAL_EXAMPLE_DIANA_FRAME.md
- Use the fixed code as your template
- Adapt the PERSON and POINTS data to your actual frame
- Run validate() before render()
- Compare output to your old version

### Day 4 — Validate Your Existing Library (2-3 hours)
- Run audit_frame() on each existing frame definition
- Fix any groups with fewer than 3 elements
- Fix any empty header/footer zones
- Fix any off-system spacing or icon sizes
- Re-render corrected frames

### Day 5 — Establish the Workflow (ongoing)
- Start every Claude Code session with the system prompt
- Check QUICK_REFERENCE_CARD.md before every render
- Use render_with_validation() instead of render() everywhere
- Log any new failure patterns and add them to the card

---

## THE 3 CODE CHANGES THAT FIX EVERYTHING

### Change 1: Replace render() with render_with_validation()
```python
# BEFORE (broken)
frame.render(output_path)

# AFTER (safe)
frame.render_with_validation(output_path)
```

### Change 2: Replace individual add_text/add_icon with add_grouped_element()
```python
# BEFORE (broken)
frame.add_text("Name", x=100, y=200, size=48)
frame.add_text("Title", x=100, y=260, size=24)

# AFTER (safe)
frame.add_grouped_element('identity', [
    {'type': 'text', 'content': 'Name', 'size': 72, 'weight': 'bold'},
    {'type': 'text', 'content': 'Title', 'size': 36, 'weight': 'semibold'},
    {'type': 'text', 'content': 'Context', 'size': 28}
], zone='content', direction='vertical', spacing=20)
```

### Change 3: Always populate all 3 zones
```python
# REQUIRED: Every frame must have these 3 calls
frame.add_grouped_element(..., zone='header', ...)  # 3+ elements
frame.add_grouped_element(..., zone='content', ...)  # 3+ elements  
frame.add_grouped_element(..., zone='footer', ...)  # 3+ elements
```

---

## EXPECTED RESULTS

| Metric | Before | After Week 1 |
|--------|--------|-------------|
| Orphaned elements | ~35% of frames | 0% |
| First-render approval | ~60% | 90%+ |
| Missing footer zones | ~50% of frames | 0% |
| Off-system spacing | ~40% of frames | 0% |
| Wrong icon sizes | ~60% of frames | 0% |
| Time to fix bad frame | 15-30 min | 2-5 min |
| Render confidence | Low | High |

---

## TROUBLESHOOTING

### "Claude keeps ignoring the rules"
Solution: Paste the CLAUDE_SYSTEM_PROMPT_VIDEO.md system prompt at the START of the session, before any other messages. Don't rely on the CLAUDE.md alone for first sessions.

### "validate() is passing but the frame still looks bad"
Solution: The validator checks structure, not aesthetics. Check:
- Are the right elements in the right columns?
- Is the content zone actually 70-85% filled or just technically occupied?
- Are font sizes legible at 1080p (minimum 20px)?

### "My Hyperframes render API doesn't match the class interface"
Solution: Override the `_render()` method in a subclass with your actual Hyperframes API call. The base class handles validation; you handle render.

### "I have a frame type that doesn't fit A/B/C/D patterns"
Solution: Use the closest pattern as a base, then adapt. The zone structure (header/content/footer) and grouping rules are non-negotiable. The internal layout of the content zone can flex.

### "A team member is producing bad frames again"
Solution: Have them read QUICK_REFERENCE_CARD.md (10 min), then show them PRACTICAL_EXAMPLE_DIANA_FRAME.md. The before/after is the fastest way to build intuition.

---

## REPOSITORY LOCATION

```
https://github.com/Social-Alignment-Git/power-design/tree/main/training

training/
├── QUICK_REFERENCE_CARD.md          ← Start here daily
├── YOUTUBE_VIDEO_DESIGN_SYSTEM.md  ← Full design bible
├── CLAUDE_SYSTEM_PROMPT_VIDEO.md   ← Paste into Claude
├── CLAUDE_CODE_HYPERFRAMES_GUIDE.md ← Code implementation
├── PRACTICAL_EXAMPLE_DIANA_FRAME.md ← Before/after example
└── TRAINING_PACKAGE_SUMMARY.md     ← This file
```

**To pull latest updates:**
```bash
cd power-design
git pull origin main
```

---

*Social Alignment YouTube Video Design System v1.0*
*Built for: Claude Code + Hyperframes + video-use*
*Maintained by: Social-Alignment-Git organization*
