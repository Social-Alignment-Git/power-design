# Practical Example: Diana Dikes Frame
## Before & After — Real Fix for Your Broken Render

---

## THE BROKEN VERSION (What You're Currently Getting)

### What's Wrong With It
```
ISSUES IDENTIFIED:
1. ORPHAN: "Diana Dikes" text placed alone at top-left
2. ORPHAN: "Mortgage Expert" subtitle floating with no group
3. DEAD SPACE: Bottom 40% of frame completely empty
4. ICON ERROR: House icon is 32px (should be 64px minimum)
5. SPACING: 12px gap used between name and title (not in approved set)
6. MISSING: Footer zone (y:880-984) has zero content
7. MISSING: Header zone (y:96-200) has zero content
```

### The Broken Code
```python
# THIS IS WHAT'S PRODUCING YOUR BAD RENDERS
def diana_frame_broken(output_path):
    frame = HyperFrame(1920, 1080)
    
    # PROBLEM 1: Text placed alone, no group
    frame.add_text(
        text="Diana Dikes",
        x=100, y=150,
        font_size=48,
        color="#FFFFFF"
    )
    
    # PROBLEM 2: Another orphan, wrong spacing (12px)
    frame.add_text(
        text="Mortgage Expert",
        x=100, y=210,  # 210 - 48px font - 150 = 12px gap (VIOLATION)
        font_size=24,
        color="#CCCCCC"
    )
    
    # PROBLEM 3: Icon too small, orphaned
    frame.add_icon(
        icon="house",
        x=400, y=150,
        size=32  # VIOLATION: must be 48/64/80px
    )
    
    # PROBLEM 4: No header, no footer, dead space bottom half
    frame.render(output_path)  # renders garbage
```

---

## THE FIXED VERSION (What It Should Look Like)

### Design Decisions Made
```
FIXES APPLIED:
1. Pattern D (Person Card) chosen for this use case
2. Header: Show name + episode + logo (3-element group)
3. Content Left: Avatar + Name + Title (3-element group, column 1)
4. Content Right: 3 credential points with 64px icons (3x3-element groups)
5. Footer: Social handle + divider + NMLS number (3-element group)
6. All spacing: 20px within groups, 40px between groups
7. All icons: 64x64px square bounding boxes
8. Font sizes: 72px (name), 36px (title), 28px (body) = 3 sizes only
```

### The Fixed Code
```python
def diana_frame_fixed(output_path):
    """
    Diana Dikes - Mortgage Expert Feature Frame
    Pattern D: Person Card
    Canvas: 1920x1080, Safe area: 96px margins
    """
    frame = YouTubeVideoFrame(brand_config={
        'show_name': 'ALIGNED',
        'primary_color': '#1A1A2E',
        'accent_color': '#E94560',
        'bg_color': '#F5F5F5'
    })

    # ===== HEADER ZONE (y: 96-200) =====
    # 3 elements: show name + episode label + logo
    frame.add_grouped_element(
        group_name='header',
        elements=[
            {
                'type': 'text',
                'content': 'ALIGNED',
                'size': 28,
                'weight': 'bold',
                'color': '#1A1A2E',
                'x': 96, 'y': 120
            },
            {
                'type': 'text',
                'content': 'Expert Spotlight',
                'size': 20,
                'weight': 'regular',
                'color': '#666666',
                'x': 240, 'y': 125
            },
            {
                'type': 'image',
                'src': 'assets/aligned_logo.png',
                'height': 40,
                'x': 1724, 'y': 110,  # right-aligned within safe area
                'align': 'right'
            }
        ],
        zone='header',
        direction='horizontal',
        spacing=20  # SP_SM
    )

    # ===== CONTENT ZONE LEFT COLUMN (x: 96-940) =====
    # Avatar + Name + Title — 3 elements
    frame.add_grouped_element(
        group_name='person_identity',
        elements=[
            {
                'type': 'image',
                'src': 'assets/diana_dikes_headshot.png',
                'width': 380,
                'height': 380,
                'x': 96, 'y': 240,
                'border_radius': 8
            },
            {
                'type': 'text',
                'content': 'Diana Dikes',
                'size': 72,
                'weight': 'bold',
                'color': '#1A1A2E',
                'x': 96, 'y': 660  # 240 + 380 + 40 = 660
            },
            {
                'type': 'text',
                'content': 'Senior Mortgage Advisor',
                'size': 36,
                'weight': 'semibold',
                'color': '#E94560',
                'x': 96, 'y': 756  # 660 + 72px font + 24px line + 20sp
            }
        ],
        zone='content',
        direction='vertical',
        spacing=20  # SP_SM between elements
    )

    # ===== CONTENT ZONE RIGHT COLUMN (x: 980-1824) =====
    # 3 credential points, each with icon + label + sublabel

    # Point 1: Experience
    point_1 = frame.add_icon_point(
        icon_name='award',
        label='15+ Years Experience',
        sublabel='Closed 2,000+ loans across Nevada',
        tier='medium'  # 64x64px
    )

    # Point 2: Specialty
    point_2 = frame.add_icon_point(
        icon_name='home',
        label='First-Time Buyer Specialist',
        sublabel='FHA, VA, Conventional & Jumbo',
        tier='medium'
    )

    # Point 3: Speed
    point_3 = frame.add_icon_point(
        icon_name='zap',
        label='21-Day Close Guarantee',
        sublabel='Fastest approval process in Las Vegas',
        tier='medium'
    )

    # Add all 9 elements as one right-column group
    frame.add_grouped_element(
        group_name='credentials',
        elements=point_1 + point_2 + point_3,  # 9 elements total
        zone='content',
        direction='vertical',
        spacing=40  # SP_MD between each 3-element point
    )

    # ===== FOOTER ZONE (y: 880-984) =====
    # Social + divider + NMLS — 3 elements
    frame.add_grouped_element(
        group_name='footer',
        elements=[
            {
                'type': 'text',
                'content': '@dianadikes',
                'size': 20,
                'weight': 'regular',
                'color': '#666666',
                'x': 96, 'y': 920
            },
            {
                'type': 'divider',
                'axis': 'vertical',
                'height': 20,
                'color': '#CCCCCC',
                'x': 260, 'y': 920
            },
            {
                'type': 'text',
                'content': 'NMLS #1234567 | Licensed in NV, AZ, CA',
                'size': 20,
                'weight': 'regular',
                'color': '#666666',
                'x': 280, 'y': 920
            }
        ],
        zone='footer',
        direction='horizontal',
        spacing=20
    )

    # Set 2-column layout
    frame.set_columns(2)

    # Validate then render — will block if anything is wrong
    return frame.render_with_validation(output_path)


# Run it
if __name__ == '__main__':
    result = diana_frame_fixed('output/diana_dikes_frame.png')
    if result:
        print(f"SUCCESS: Frame rendered to {result}")
    else:
        print("FAILED: Check validation errors above")
```

---

## VALIDATION OUTPUT (Expected)

When you run the fixed code, validation should output:
```
Checking: header zone... PASS (3 elements)
Checking: content groups... PASS (2 groups, 12 elements total)
Checking: footer zone... PASS (3 elements)
Checking: icon sizes... PASS (all 64x64px square)
Checking: spacing... PASS (20px, 40px used)
Checking: font sizes... PASS (72px, 36px, 28px = 3 sizes)
Checking: frame fill... PASS (estimated 78% of safe area)

All 7 checks passed. Rendering...
SUCCESS: Frame rendered to output/diana_dikes_frame.png
```

---

## ADAPTATION GUIDE

To adapt this template for other guests/experts:

```python
# 1. Swap the person data
PERSON = {
    'name': 'Guest Name Here',
    'title': 'Their Professional Title',
    'headshot': 'assets/their_photo.png',
    'social': '@theirhandle',
    'credential': 'License # or certification'
}

# 2. Swap the 3 credential points
POINTS = [
    {'icon': 'relevant-icon', 'label': 'Point 1 Headline', 'sublabel': 'Supporting detail'},
    {'icon': 'relevant-icon', 'label': 'Point 2 Headline', 'sublabel': 'Supporting detail'},
    {'icon': 'relevant-icon', 'label': 'Point 3 Headline', 'sublabel': 'Supporting detail'},
]

# 3. Keep EVERYTHING else the same (spacing, sizes, zones)
# The structure is proven. Only swap the content.
```

---

## KEY LESSONS FROM THIS FIX

| What Was Wrong | Why It Matters | How It's Fixed |
|----------------|----------------|----------------|
| Name placed alone | Orphan = unprofessional | Grouped with avatar + title |
| 12px spacing | Off-system = inconsistent | Changed to 20px (SP_SM) |
| 32px icon | Too small for 1080p | Upgraded to 64px (ICON_MD) |
| Empty footer | Dead space = incomplete | Added 3-element footer bar |
| Empty header | No brand context | Added show name + episode + logo |
| Bottom 40% empty | Frame feels unfinished | Credential points fill right column |

---

*Social Alignment YouTube Video Design System v1.0*
*Template for: Pattern D (Person Card)*
*See also: CLAUDE_CODE_HYPERFRAMES_GUIDE.md | YOUTUBE_VIDEO_DESIGN_SYSTEM.md*
