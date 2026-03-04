# Advania Presentation Creator

Create an Advania-branded presentation based on the user's request.

## Workflow

### Step 1: Gather Context

Ask for (skip what's already provided):

- Topic / title
- Audience (executives, technical team, client, internal)
- Purpose (inform, persuade, propose, educate)
- Duration (5 min, 15 min, 30 min, 60 min)
- Key message / takeaway
- Language (Swedish or English)
- Output format: **HTML** (recommended, richest visuals), **PPTX** (editable in PowerPoint), or **Outline** (markdown only)

### Step 2: Slide Count Guidelines

| Duration | Slides |
|----------|--------|
| 5 min | 3-5 |
| 15 min | 8-12 |
| 30 min | 15-20 |
| 60 min | 25-35 |

### Step 3: Generate Outline

Create a structured slide-by-slide outline:

```
## Slide [N]: [Title]
**Type:** [title | section | content | two-column | three-column | end]
**Key points:**
- Point 1
- Point 2
**Speaker notes:** [What to say]
```

#### Standard Structure

1. Title slide with context
2. Agenda / overview
3. Problem or situation
4. Solution or proposal (main content)
5. Benefits / value
6. Timeline or next steps
7. Summary / key takeaway
8. End slide (Advania branded)

### Step 4: Generate Output

Present the outline for review, then generate the chosen format.

---

## HTML Mode (recommended)

Create a single-file HTML slideshow. All CSS, JS, and content in one `.html` file. No external dependencies.

### Full HTML Template

Use this complete template as your starting point. Replace the slide content but keep the CSS, JS, and structure intact.

```html
<!DOCTYPE html>
<html lang="sv">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>[PRESENTATION TITLE] | Advania</title>
<style>
  :root {
    /* ── Advania Brand Colors ── */
    --orange: #d54328;
    --pink: #cb0084;
    --purple: #98139c;
    --indigo: #4f0077;
    --green: #45b649;
    --blue: #2abad9;
    --dark: #30302f;
    --light: #eceff0;
    --white: #ffffff;

    /* ── Approved Gradients (always 135deg) ── */
    --grad-hero: linear-gradient(135deg, var(--indigo) 0%, var(--purple) 50%, var(--pink) 100%);
    --grad-warm: linear-gradient(135deg, var(--orange) 0%, var(--pink) 100%);
    --grad-cool: linear-gradient(135deg, var(--blue) 0%, var(--green) 100%);
    --grad-deep: linear-gradient(135deg, var(--purple) 0%, var(--indigo) 100%);
    --grad-accent: linear-gradient(135deg, var(--pink) 0%, var(--purple) 100%);

    /* ── Typography ── */
    --font-display: Georgia, 'Times New Roman', serif;
    --font-body: Arial, Helvetica, sans-serif;

    /* ── Slide Dimensions (16:9) ── */
    --slide-w: 1280px;
    --slide-h: 720px;
  }

  *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: #1a1a19;
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    font-family: var(--font-body);
    overflow: hidden;
  }

  /* ── Slide Deck Container ── */
  .deck {
    position: relative;
    width: var(--slide-w);
    height: var(--slide-h);
    overflow: hidden;
    box-shadow: 0 4px 60px rgba(0,0,0,.6);
    border-radius: 4px;
    border: 1px solid rgba(255,255,255,.06);
  }

  .slide {
    position: absolute;
    inset: 0;
    display: flex;
    flex-direction: column;
    opacity: 0;
    pointer-events: none;
    overflow: hidden;
    transition: opacity .5s cubic-bezier(.4,0,.2,1), transform .5s cubic-bezier(.4,0,.2,1);
    transform: translateX(80px);
  }

  .slide.active {
    opacity: 1;
    transform: translateX(0);
    pointer-events: auto;
    z-index: 2;
  }

  .slide.exit-left {
    opacity: 0;
    transform: translateX(-80px);
  }

  /* ── Hexagon SVG Decoration ── */
  .hex-deco {
    position: absolute;
    pointer-events: none;
    z-index: 0;
  }
  .hex-deco svg { display: block; }

  /* ══════════════════════════════════════
     SLIDE TYPES
     ══════════════════════════════════════ */

  /* ── Title Slide ── */
  .slide-title {
    background: var(--grad-hero);
    color: var(--white);
    justify-content: center;
    align-items: flex-start;
    padding: 80px 90px;
  }

  .slide-title h1 {
    font-family: var(--font-display);
    font-size: 56px;
    font-weight: 400;
    line-height: 1.15;
    letter-spacing: -0.5px;
    max-width: 700px;
    position: relative;
    z-index: 1;
  }

  .slide-title h1 span {
    display: block;
    font-style: italic;
    font-size: 32px;
    margin-top: 16px;
    opacity: .85;
  }

  .slide-title .subtitle-line {
    font-size: 18px;
    margin-top: 40px;
    letter-spacing: 2px;
    text-transform: uppercase;
    opacity: .7;
    position: relative;
    z-index: 1;
  }

  .slide-title .brand-mark {
    position: absolute;
    bottom: 40px;
    right: 60px;
    font-size: 14px;
    letter-spacing: 4px;
    text-transform: uppercase;
    opacity: .5;
    z-index: 1;
  }

  /* ── Section Divider ── */
  .slide-section {
    background: var(--grad-deep);
    color: var(--white);
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 60px 90px;
  }

  .slide-section h2 {
    font-family: var(--font-display);
    font-size: 48px;
    font-weight: 400;
    position: relative;
    z-index: 1;
  }

  .slide-section .section-sub {
    font-size: 20px;
    margin-top: 16px;
    opacity: .7;
    position: relative;
    z-index: 1;
  }

  .slide-section .section-num {
    font-size: 28px;
    margin-top: 30px;
    opacity: .5;
    position: relative;
    z-index: 1;
  }

  /* ── Content Slide (single column) ── */
  .slide-content {
    background: var(--white);
    padding: 60px 90px;
    justify-content: center;
  }

  .section-label {
    font-family: var(--font-body);
    font-size: 11px;
    letter-spacing: 4px;
    text-transform: uppercase;
    color: var(--purple);
    margin-bottom: 12px;
  }

  .slide-content h2 {
    font-family: var(--font-display);
    font-size: 40px;
    color: var(--dark);
    margin-bottom: 30px;
    letter-spacing: -0.3px;
  }

  .slide-content ul {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  .slide-content li {
    display: flex;
    align-items: flex-start;
    gap: 16px;
    font-size: 20px;
    color: var(--dark);
    line-height: 1.5;
  }

  .dot {
    display: inline-block;
    width: 8px;
    height: 8px;
    background: var(--purple);
    clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
    flex-shrink: 0;
    margin-top: 8px;
  }

  /* ── Content Slide (dark variant) ── */
  .slide-dark {
    background: var(--dark);
    color: var(--white);
    padding: 60px 90px;
    justify-content: center;
  }

  .slide-dark h2 {
    font-family: var(--font-display);
    font-size: 40px;
    margin-bottom: 30px;
  }

  .slide-dark .section-label { color: var(--blue); }

  .slide-dark ul {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  .slide-dark li {
    display: flex;
    align-items: flex-start;
    gap: 16px;
    font-size: 20px;
    line-height: 1.5;
  }

  .slide-dark .dot { background: var(--blue); }

  /* ── Two-Column Slide ── */
  .slide-two-col {
    background: var(--white);
    padding: 60px 90px;
    justify-content: center;
  }

  .slide-two-col h2 {
    font-family: var(--font-display);
    font-size: 36px;
    color: var(--dark);
    margin-bottom: 30px;
  }

  .two-cols {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 60px;
  }

  .col h3 {
    font-size: 18px;
    color: var(--purple);
    text-transform: uppercase;
    letter-spacing: 2px;
    margin-bottom: 20px;
  }

  .col ul {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .col li {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    font-size: 17px;
    color: var(--dark);
    line-height: 1.5;
  }

  /* ── Three-Column Slide ── */
  .slide-three-col {
    background: var(--white);
    padding: 60px 70px;
    justify-content: center;
  }

  .slide-three-col h2 {
    font-family: var(--font-display);
    font-size: 36px;
    color: var(--dark);
    margin-bottom: 30px;
  }

  .three-cols {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 40px;
  }

  /* ── Agenda Slide ── */
  .slide-agenda {
    background: var(--white);
    padding: 60px 90px;
    justify-content: center;
  }

  .slide-agenda h2 {
    font-family: var(--font-display);
    font-size: 44px;
    color: var(--dark);
    margin-bottom: 50px;
  }

  .agenda-list {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 0;
  }

  .agenda-list li {
    display: flex;
    align-items: center;
    gap: 28px;
    padding: 22px 0;
    border-bottom: 1px solid var(--light);
    font-size: 20px;
    color: var(--dark);
  }

  .agenda-list li:last-child { border-bottom: none; }

  .agenda-num {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 44px;
    height: 44px;
    background: var(--grad-hero);
    color: var(--white);
    font-weight: 700;
    font-size: 16px;
    clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
    flex-shrink: 0;
  }

  /* ── End Slide ── */
  .slide-end {
    background: var(--grad-hero);
    color: var(--white);
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 60px;
  }

  .end-tagline .line1 {
    font-family: var(--font-display);
    font-size: 42px;
    font-weight: 400;
    opacity: .9;
  }

  .end-tagline .line2 {
    font-family: var(--font-display);
    font-style: italic;
    font-size: 42px;
    opacity: .7;
    margin-top: 8px;
  }

  .end-logo {
    font-family: var(--font-body);
    font-size: 20px;
    letter-spacing: 8px;
    text-transform: uppercase;
    opacity: .4;
    margin-top: 60px;
  }

  .end-footer {
    position: absolute;
    bottom: 30px;
    font-size: 13px;
    opacity: .4;
  }

  /* ── kbd styling (for technical slides) ── */
  kbd, .kbd {
    display: inline-block;
    background: var(--light);
    color: var(--dark);
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 2px 8px;
    font-family: 'SF Mono', Consolas, monospace;
    font-size: 0.85em;
  }

  /* ── Controls ── */
  .controls {
    position: fixed;
    bottom: 24px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    align-items: center;
    gap: 16px;
    z-index: 100;
  }

  .controls button {
    background: rgba(255,255,255,.1);
    border: 1px solid rgba(255,255,255,.15);
    color: rgba(255,255,255,.6);
    width: 40px;
    height: 40px;
    border-radius: 50%;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all .2s;
  }

  .controls button:hover { background: rgba(255,255,255,.2); color: #fff; }
  .controls button:disabled { opacity: .3; cursor: default; }

  .slide-counter {
    color: rgba(255,255,255,.4);
    font-size: 13px;
    font-family: var(--font-body);
    min-width: 60px;
    text-align: center;
  }

  /* ── Progress Bar ── */
  .progress-bar {
    position: fixed;
    top: 0;
    left: 0;
    height: 3px;
    background: var(--grad-warm);
    transition: width .4s ease;
    z-index: 100;
  }

  /* ── Fullscreen ── */
  .deck:fullscreen,
  .deck:-webkit-full-screen {
    width: 100vw !important;
    height: 100vh !important;
    border-radius: 0;
    border: none;
  }

  /* ── Print ── */
  @media print {
    body { background: white; }
    .controls, .progress-bar { display: none !important; }
    .deck {
      box-shadow: none;
      border: none;
      width: 100%;
      height: auto;
    }
    .slide {
      position: relative !important;
      opacity: 1 !important;
      transform: none !important;
      pointer-events: auto !important;
      page-break-after: always;
      min-height: 100vh;
    }
  }
</style>
</head>
<body>

<div class="progress-bar" id="progress"></div>

<div class="deck" id="deck">

  <!-- ═══ SLIDE 1: Title ═══ -->
  <div class="slide slide-title active" data-slide="0">
    <div class="hex-deco top-right">
      <svg width="300" height="346" viewBox="0 0 300 346" fill="none"><path d="M150 0L300 86.5V259.5L150 346L0 259.5V86.5L150 0Z" fill="rgba(255,255,255,.12)"/></svg>
    </div>
    <h1>[Presentation Title]<span>[Subtitle]</span></h1>
    <div class="subtitle-line">[Client/Context] | [Date]</div>
    <div class="brand-mark">advania</div>
  </div>

  <!-- ═══ SLIDE 2: Content example ═══ -->
  <div class="slide slide-content" data-slide="1">
    <div class="section-label">Section Label</div>
    <h2>Slide Title</h2>
    <ul>
      <li><span class="dot"></span> First point</li>
      <li><span class="dot"></span> Second point</li>
      <li><span class="dot"></span> Third point</li>
    </ul>
  </div>

  <!-- ═══ SLIDE 3: Two-column example ═══ -->
  <div class="slide slide-two-col" data-slide="2">
    <div class="section-label">Section Label</div>
    <h2>Two Column Title</h2>
    <div class="two-cols">
      <div class="col col-left">
        <h3>Left</h3>
        <ul>
          <li><span class="dot"></span> Point A</li>
          <li><span class="dot"></span> Point B</li>
        </ul>
      </div>
      <div class="col col-right">
        <h3>Right</h3>
        <ul>
          <li><span class="dot"></span> Point C</li>
          <li><span class="dot"></span> Point D</li>
        </ul>
      </div>
    </div>
  </div>

  <!-- ═══ SLIDE 4: Section divider example ═══ -->
  <div class="slide slide-section" data-slide="3">
    <div class="hex-deco" style="bottom:-80px;right:-40px;opacity:.1;">
      <svg width="400" height="460" viewBox="0 0 400 460" fill="none"><path d="M200 0L400 115V345L200 460L0 345V115L200 0Z" fill="white"/></svg>
    </div>
    <h2>Section Title</h2>
    <div class="section-sub">Section subtitle</div>
  </div>

  <!-- ═══ LAST SLIDE: End ═══ -->
  <div class="slide slide-end" data-slide="4">
    <div class="hex-deco" style="top:-80px;left:-60px;">
      <svg width="420" height="485" viewBox="0 0 420 485" fill="none">
        <path d="M210 0L420 121V363L210 485L0 363V121L210 0Z" fill="rgba(79,0,119,.5)"/>
      </svg>
    </div>
    <div class="end-tagline">
      <div class="line1">The tech company</div>
      <div class="line2">with people at heart</div>
    </div>
    <div class="end-logo">advania</div>
    <div class="end-footer"><span>advania.se</span></div>
  </div>

</div>

<!-- Controls -->
<div class="controls">
  <button id="prevBtn" aria-label="Previous slide" disabled>
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><polyline points="15 18 9 12 15 6"/></svg>
  </button>
  <span class="slide-counter" id="counter">1 / 5</span>
  <button id="nextBtn" aria-label="Next slide">
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><polyline points="9 18 15 12 9 6"/></svg>
  </button>
</div>

<script>
  const slides = document.querySelectorAll('.slide');
  const total = slides.length;
  let cur = 0;

  function go(n) {
    if (n < 0 || n >= total) return;
    const dir = n > cur ? 1 : -1;
    slides[cur].classList.remove('active');
    slides[cur].classList.add(dir > 0 ? 'exit-left' : '');
    setTimeout(() => slides[cur === 0 && dir < 0 ? 0 : cur].classList.remove('exit-left'), 500);
    cur = n;
    slides.forEach(s => { s.classList.remove('active','exit-left'); });
    slides[cur].classList.add('active');
    document.getElementById('counter').textContent = `${cur + 1} / ${total}`;
    document.getElementById('prevBtn').disabled = cur === 0;
    document.getElementById('nextBtn').disabled = cur === total - 1;
    document.getElementById('progress').style.width = `${((cur + 1) / total) * 100}%`;
  }

  document.getElementById('prevBtn').addEventListener('click', () => go(cur - 1));
  document.getElementById('nextBtn').addEventListener('click', () => go(cur + 1));

  document.addEventListener('keydown', e => {
    if (e.key === 'ArrowRight' || e.key === ' ') { e.preventDefault(); go(cur + 1); }
    if (e.key === 'ArrowLeft') { e.preventDefault(); go(cur - 1); }
    if (e.key === 'f' || e.key === 'F') {
      const d = document.getElementById('deck');
      if (!document.fullscreenElement) d.requestFullscreen?.() || d.webkitRequestFullscreen?.();
      else document.exitFullscreen?.() || document.webkitExitFullscreen?.();
    }
    if (e.key === 'p' || e.key === 'P') window.print();
  });

  // Touch swipe
  let tx = 0;
  document.getElementById('deck').addEventListener('touchstart', e => { tx = e.touches[0].clientX; });
  document.getElementById('deck').addEventListener('touchend', e => {
    const dx = e.changedTouches[0].clientX - tx;
    if (Math.abs(dx) > 50) go(cur + (dx < 0 ? 1 : -1));
  });

  go(0);
</script>
</body>
</html>
```

### Available Slide Types

Use these CSS classes for different slide types. Add slides inside the `.deck` div with incrementing `data-slide` values.

| Type | CSS Class | Use For |
|------|-----------|---------|
| Title | `slide-title` | Opening slide with gradient background |
| Content | `slide-content` | Single-column bullet points (white bg) |
| Dark Content | `slide-dark` | Single-column on dark background |
| Two-Column | `slide-two-col` | Side-by-side comparison |
| Three-Column | `slide-three-col` | Three areas with `.three-cols` grid |
| Section Divider | `slide-section` | Chapter break with gradient background |
| Agenda | `slide-agenda` | Numbered agenda with hexagon markers |
| End | `slide-end` | Closing slide with tagline |

### Hexagon Decorations

Add 1-3 hexagons per slide as background decoration. Use on gradient-background slides only.

```html
<div class="hex-deco" style="top:-40px;right:-20px;opacity:.12;">
  <svg width="300" height="346" viewBox="0 0 300 346" fill="none">
    <path d="M150 0L300 86.5V259.5L150 346L0 259.5V86.5L150 0Z" fill="rgba(255,255,255,.12)"/>
  </svg>
</div>
```

### Bullet Points

Use hexagon-shaped dots for bullet points:

```html
<li><span class="dot"></span> Your bullet text here</li>
```

### Keyboard Shortcuts

The template includes: Arrow keys + Space (navigate), F (fullscreen), P (print).

---

## PPTX Mode (optional, requires setup)

PPTX generation requires two additional files in the project:
1. `scripts/generate-pptx.py` - Python script for PPTX generation
2. `templates/presentations/advania-template.pptx` - Branded PowerPoint template

If these files exist, create a JSON slide spec and run:

```bash
python3 scripts/generate-pptx.py slides.json output.pptx
```

JSON format:

```json
{
  "title": "Presentation Title",
  "subtitle": "Optional subtitle",
  "slides": [
    { "layout": "title-clean", "title": "Title", "subtitle": "Subtitle" },
    { "layout": "content", "title": "Slide Title", "body": "Line 1\nLine 2\nLine 3" },
    { "layout": "two-column", "title": "Title", "left": "Left text", "right": "Right text" },
    { "layout": "three-column", "title": "Title", "col1": "Col 1", "col2": "Col 2", "col3": "Col 3" },
    { "layout": "section", "title": "Section Header", "subtitle": "Subtitle" },
    { "layout": "end" }
  ]
}
```

Available layouts: `title`, `title-clean`, `content`, `content-alt`, `two-column`, `three-column`, `section`, `title-only`, `blank`, `end`

---

## Outline Mode (markdown only)

Create a markdown slide-by-slide outline with speaker notes. Useful for drafting.

---

## Brand Reference (Advania)

### Colors

| Name | Hex | Usage |
|------|-----|-------|
| Orange | `#d54328` | Primary accent, CTAs |
| Pink | `#cb0084` | Secondary accent |
| Purple | `#98139c` | Section headers, emphasis |
| Indigo | `#4f0077` | Deep accent, headings |
| Green | `#45b649` | Success, growth |
| Blue | `#2abad9` | Technology, trust |
| Dark Grey | `#30302f` | Body text, dark backgrounds |
| Light Grey | `#eceff0` | Light backgrounds, dividers |

### Gradients (always 135deg)

| Name | CSS |
|------|-----|
| Hero (3-stop) | `linear-gradient(135deg, #4f0077, #98139c, #cb0084)` |
| Warm | `linear-gradient(135deg, #d54328, #cb0084)` |
| Accent | `linear-gradient(135deg, #cb0084, #98139c)` |
| Deep | `linear-gradient(135deg, #98139c, #4f0077)` |
| Cool | `linear-gradient(135deg, #45b649, #2abad9)` |

### Typography

| Role | Font |
|------|------|
| Headlines / Display | Georgia, serif |
| Body / UI | Arial, sans-serif |

### Hexagon Rules

- Max 2-3 hexagons per slide
- Use as decorative elements, not for critical content
- Hexagon clip-path: `polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%)`

### End Slide Tagline

Always close with: "The tech company / with people at heart"

## Writing Style

- Combine related sentences into proper paragraphs. Avoid one-sentence-per-line.
- Never use American-style em-dashes. Use short dashes or rewrite.
- Adapt formality to audience (executives = high-level, technical = detailed).
- Include data points and examples where possible. Avoid generic slides.
