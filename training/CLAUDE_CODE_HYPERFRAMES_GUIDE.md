# Claude Code + Hyperframes Implementation Guide
## YouTube Video Frame Design — Code Patterns & Architecture

---

## THE CORE PROBLEM & SOLUTION

**Problem:** Elements placed individually = orphans, dead space, bad renders.
**Solution:** Group-first architecture — build groups, then place groups.

### WRONG Pattern (Breaking Your Renders)
```python
# DO NOT DO THIS
frame.add_text("Diana Dikes", x=100, y=200, size=48)
frame.add_text("Mortgage Expert", x=100, y=260, size=24)
frame.add_icon("house", x=500, y=200, size=32)  # orphaned + wrong size
frame.render()  # produces broken output
```

### CORRECT Pattern (Group-First)
```python
# DO THIS INSTEAD
person_group = FrameGroup()
person_group.add(Text("Diana Dikes", size=72, weight="bold"))
person_group.add(Text("Mortgage Expert | NMLS #12345", size=36, weight="semibold"))
person_group.add(Text("Helping families close faster", size=28))
person_group.layout(direction="vertical", spacing=20)

frame.place_group(person_group, zone="content", align="left", x=96, y=200)
frame.validate()  # runs checks before render
frame.render()   # only renders if validation passes
```

---

## CANVAS CONSTANTS (Use These Everywhere)

```python
# Copy these into every frame file
CANVAS_W = 1920
CANVAS_H = 1080
MARGIN = 96

SAFE_X_MIN = 96
SAFE_X_MAX = 1824
SAFE_Y_MIN = 96
SAFE_Y_MAX = 984
SAFE_W = 1728
SAFE_H = 888

HEADER_Y = (96, 200)
CONTENT_Y = (200, 880)
FOOTER_Y = (880, 984)

# Approved spacing only
SP_XS = 8    # icon-to-label
SP_SM = 20   # within group
SP_MD = 40   # between groups
SP_LG = 80   # between sections
SP_XL = 96   # safe area margin

# Icon sizes only
ICON_SM = 48
ICON_MD = 64
ICON_LG = 80
ICON_XL = 120

# Column grids (x_start, x_end per column)
COL_2 = [(96, 940), (980, 1824)]
COL_3 = [(96, 645), (685, 1234), (1274, 1824)]
COL_4 = [(96, 498), (538, 940), (980, 1382), (1422, 1824)]
```

---

## THE YouTubeVideoFrame CLASS

```python
class YouTubeVideoFrame:
    """Base class enforcing design system at architecture level."""

    def __init__(self, brand_config=None):
        self.brand = brand_config or {}
        self.groups = []
        self.header_group = None
        self.content_groups = []
        self.footer_group = None

    def add_grouped_element(self, group_name, elements, zone='content',
                             direction='vertical', spacing=SP_SM):
        """Add a group. Raises if fewer than 3 elements."""
        if len(elements) < 3:
            raise ValueError(
                f"Group '{group_name}' needs 3+ elements, got {len(elements)}. "
                "Add more elements before placing."
            )
        group = {'name': group_name, 'elements': elements,
                 'zone': zone, 'direction': direction, 'spacing': spacing}
        if zone == 'header':
            self.header_group = group
        elif zone == 'footer':
            self.footer_group = group
        else:
            self.content_groups.append(group)
        self.groups.append(group)
        return group

    def add_icon_point(self, icon_name, label, sublabel=None, tier='medium'):
        """Returns 3-element icon+label+sublabel group (safe to add_grouped_element)."""
        sizes = {'small': ICON_SM, 'medium': ICON_MD, 'large': ICON_LG}
        sz = sizes[tier]
        return [
            {'type': 'icon', 'name': icon_name, 'size': (sz, sz)},
            {'type': 'text', 'content': label, 'size': 32,
             'weight': 'semibold', 'margin_top': SP_XS},
            {'type': 'text', 'content': sublabel or '', 'size': 24,
             'weight': 'regular', 'margin_top': SP_XS}
        ]

    def validate(self):
        """Run all checks. Raises DesignError if anything fails."""
        errors = []

        for g in self.groups:
            if len(g['elements']) < 3:
                errors.append(f"ORPHAN: '{g['name']}' has {len(g['elements'])} elements (need 3+)")

        if not self.header_group:
            errors.append("DEAD ZONE: Header (y:96-200) is empty")
        if not self.footer_group:
            errors.append("DEAD ZONE: Footer (y:880-984) is empty")
        if not self.content_groups:
            errors.append("DEAD ZONE: Content zone has no groups")

        for g in self.groups:
            for el in g['elements']:
                if el.get('type') == 'icon':
                    w, h = el.get('size', (0, 0))
                    if w != h:
                        errors.append(f"ICON: '{el.get('name')}' not square ({w}x{h})")
                    if w not in (ICON_SM, ICON_MD, ICON_LG, ICON_XL):
                        errors.append(f"ICON: '{el.get('name')}' is {w}px — use 48/64/80/120")

        if errors:
            raise DesignError("Validation failed:\n" + "\n".join(f"  {e}" for e in errors))
        return True

    def render_with_validation(self, output_path):
        """Validates then renders. Blocks render if validation fails."""
        try:
            self.validate()
        except DesignError as e:
            print(f"RENDER BLOCKED — fix these before rendering:\n{e}")
            return False
        return self._render(output_path)

    def _render(self, output_path):
        raise NotImplementedError("Implement _render() with your Hyperframes call")


class DesignError(Exception):
    pass
```

---

## READY-TO-USE PATTERN TEMPLATES

### Pattern A — Title + 3 Feature Points
```python
def pattern_a(show_name, episode, logo_path, title, eyebrow, subtitle,
              points, source, cta, output_path):
    """
    points = [{icon, label, sublabel}, {icon, label, sublabel}, {icon, label, sublabel}]
    """
    f = YouTubeVideoFrame()

    # HEADER (3 elements)
    f.add_grouped_element('header', [
        {'type': 'text', 'content': show_name, 'size': 28, 'weight': 'semibold'},
        {'type': 'text', 'content': episode, 'size': 20},
        {'type': 'image', 'src': logo_path, 'height': 40, 'align': 'right'}
    ], zone='header', direction='horizontal', spacing=SP_MD)

    # CONTENT LEFT — title block (3 elements)
    f.add_grouped_element('title', [
        {'type': 'text', 'content': eyebrow, 'size': 28},
        {'type': 'text', 'content': title, 'size': 72, 'weight': 'bold'},
        {'type': 'text', 'content': subtitle, 'size': 36, 'weight': 'semibold'}
    ], zone='content', direction='vertical', spacing=SP_SM)

    # CONTENT RIGHT — 3 icon points (9 elements total = 3 groups of 3)
    all_point_els = []
    for p in points[:3]:
        all_point_els += f.add_icon_point(p['icon'], p['label'],
                                           p.get('sublabel'), tier='medium')
    f.add_grouped_element('points', all_point_els, zone='content',
                           direction='vertical', spacing=SP_MD)

    # FOOTER (3 elements)
    f.add_grouped_element('footer', [
        {'type': 'text', 'content': source, 'size': 20},
        {'type': 'divider', 'axis': 'vertical', 'height': 20},
        {'type': 'text', 'content': cta, 'size': 20, 'weight': 'semibold'}
    ], zone='footer', direction='horizontal', spacing=SP_SM)

    f.set_columns(2)
    return f.render_with_validation(output_path)
```

### Pattern D — Person Card
```python
def pattern_d(show_name, logo_path, avatar_path, name, title, context,
              social, credentials, output_path):
    f = YouTubeVideoFrame()

    # HEADER
    f.add_grouped_element('header', [
        {'type': 'image', 'src': logo_path, 'height': 40},
        {'type': 'text', 'content': show_name, 'size': 28, 'weight': 'semibold'},
        {'type': 'text', 'content': 'Featured Guest', 'size': 20}
    ], zone='header', direction='horizontal', spacing=SP_MD)

    # CONTENT LEFT — avatar (3 elements: image + name + title)
    f.add_grouped_element('person_left', [
        {'type': 'image', 'src': avatar_path, 'width': 400, 'height': 400},
        {'type': 'text', 'content': name, 'size': 48, 'weight': 'bold'},
        {'type': 'text', 'content': title, 'size': 28}
    ], zone='content', direction='vertical', spacing=SP_SM)

    # CONTENT RIGHT — context block (3 elements)
    f.add_grouped_element('person_right', [
        {'type': 'text', 'content': context, 'size': 32, 'weight': 'semibold'},
        {'type': 'text', 'content': credentials, 'size': 24},
        {'type': 'text', 'content': social, 'size': 24}
    ], zone='content', direction='vertical', spacing=SP_SM)

    # FOOTER
    f.add_grouped_element('footer', [
        {'type': 'text', 'content': social, 'size': 20},
        {'type': 'divider', 'axis': 'vertical', 'height': 20},
        {'type': 'text', 'content': credentials, 'size': 20}
    ], zone='footer', direction='horizontal', spacing=SP_SM)

    f.set_columns(2)
    return f.render_with_validation(output_path)
```

---

## COPY-PASTE SNIPPETS

### Valid Header (always use this)
```python
header_els = [
    {'type': 'text', 'content': SHOW_NAME, 'size': 28, 'weight': 'semibold'},
    {'type': 'text', 'content': EPISODE_LABEL, 'size': 20},
    {'type': 'image', 'src': LOGO_PATH, 'height': 40}
]
frame.add_grouped_element('header', header_els, zone='header', direction='horizontal')
```

### Valid Footer (always use this)
```python
footer_els = [
    {'type': 'text', 'content': SOURCE, 'size': 20},
    {'type': 'divider', 'axis': 'vertical', 'height': 20},
    {'type': 'text', 'content': CTA, 'size': 20, 'weight': 'semibold'}
]
frame.add_grouped_element('footer', footer_els, zone='footer', direction='horizontal')
```

### Valid Icon Point (use add_icon_point method)
```python
point = frame.add_icon_point(
    icon_name='check-circle',
    label='Key Benefit',
    sublabel='Supporting detail',
    tier='medium'  # = 64x64px square
)
# Returns 3 elements ready for add_grouped_element
```

### Pre-render Validation Call
```python
# Always validate before rendering — never skip this
try:
    frame.validate()
    frame.render(output_path)
except DesignError as e:
    print(e)  # read errors, fix, re-run
```

---

## HYPERFRAMES INTEGRATION WRAPPER

```python
def safe_render(frame_def, output_path, hyperframes_client):
    """
    Drop-in wrapper for all Hyperframes render calls.
    Validates design before spending compute on render.
    """
    audit = validate_frame_def(frame_def)

    if not audit['valid']:
        print("RENDER BLOCKED — Design errors found:")
        for err in audit['errors']:
            print(f"  ERROR: {err}")
        print("\nFix all errors then re-run.")
        return None

    print(f"Design valid. Sending to Hyperframes...")
    result = hyperframes_client.render(frame_def, output=output_path)
    print(f"Rendered: {output_path}")
    return result


def validate_frame_def(frame_def):
    errors = []
    groups = frame_def.get('groups', [])

    for g in groups:
        if len(g.get('elements', [])) < 3:
            errors.append(f"Group '{g['name']}' has fewer than 3 elements")

    zones = [g['zone'] for g in groups]
    for z in ('header', 'content', 'footer'):
        if z not in zones:
            errors.append(f"Zone '{z}' is empty")

    return {'valid': len(errors) == 0, 'errors': errors}
```

---

*Social Alignment YouTube Video Design System v1.0*
*See also: YOUTUBE_VIDEO_DESIGN_SYSTEM.md | CLAUDE_SYSTEM_PROMPT_VIDEO.md*
