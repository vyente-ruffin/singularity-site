# Singularity Site

Landing page variations for Singularity.tech - AI integration consulting

## Project Overview

- **Name**: singularity-site
- **Type**: Landing Page / Marketing Site (13 design variations)
- **Domain**: singularity.tech
- **Hosting**: GitHub Pages at `vyente-ruffin.github.io/singularity-site`
- **Repository**: https://github.com/vyente-ruffin/singularity-site

## Tech Stack

### Core Technologies
- **HTML5**: Semantic markup, accessibility-focused
- **Tailwind CSS**: Via CDN (`https://cdn.tailwindcss.com`) - no build step required
- **Vanilla JavaScript**: ES6+, no framework dependencies

### Animation Libraries
- **GSAP 3.12.5**: Core animation engine (CDN)
- **ScrollTrigger**: GSAP plugin for scroll-based animations
- **ScrollSmoother**: GSAP plugin for smooth scrolling (premium, some versions)
- **SplitText**: GSAP plugin for text animations (premium, some versions)

### 3D/WebGL (v2 only)
- **Three.js**: 3D rendering and WebGL abstraction

### Testing & Automation
- **Playwright MCP**: Browser automation for testing and screenshots

## File Structure

```
singularity-site/
├── index.html          # Neobrutalist (root design)
├── og-image.png        # Root OG preview image
├── v1/index.html       # Bento Grid
├── v2/index.html       # 3D/WebGL (Three.js)
├── v3/index.html       # Kinetic Scroll
├── v4/index.html       # Glassmorphism
├── v5/index.html       # Monospace/Dev
├── v6/index.html       # Parallax Storytelling
├── v7/index.html       # Claymorphism
├── v8/index.html       # Editorial/Magazine
├── v9/index.html       # Swiss Minimalist
├── v10/index.html      # Split Screen
├── v11/index.html      # Maximalist
├── v12/index.html      # Retro Futurism
└── v*/og-image.png     # OG preview images (1200x630)
```

---

## Learnings & Best Practices

### What Worked Well

1. **CDN-based Tailwind**: No build step = fast iteration, easy deployment to GitHub Pages
2. **Single-file architecture**: Each version is self-contained `index.html` with embedded CSS/JS
3. **Parallel sub-agent execution**: Building 4 versions simultaneously via Task tool
4. **Playwright MCP for testing**: Reliable cross-viewport testing and screenshot capture
5. **Mobile-first design**: Starting with 375px viewport prevented most responsive issues

### GSAP Animation Patterns That Work

```javascript
// CORRECT: Register plugins BEFORE any GSAP code
gsap.registerPlugin(ScrollTrigger);

// CORRECT: Wait for DOM and fonts
window.addEventListener('load', () => {
  // Initialize animations here
});

// CORRECT: Use data attributes for scroll triggers
<div data-speed="0.5">Content</div>

// CORRECT: Refresh ScrollTrigger after dynamic content
ScrollTrigger.refresh();
```

---

## Pitfalls & Error Patterns

### 1. GSAP Plugin Load Order (CRITICAL)

**Error**: `gsap.registerPlugin is not a function` or animations not triggering

**Cause**: Plugins loaded before GSAP core, or `registerPlugin()` called before plugins loaded

**Solution**:
```html
<!-- CORRECT ORDER -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<script>
  // Register AFTER both scripts loaded
  gsap.registerPlugin(ScrollTrigger);
</script>
```

### 2. ScrollTrigger Not Firing on Page Load

**Error**: Animations work on scroll but initial viewport elements don't animate

**Cause**: ScrollTrigger initialized before DOM fully ready

**Solution**:
```javascript
// Use load event, not DOMContentLoaded
window.addEventListener('load', () => {
  gsap.registerPlugin(ScrollTrigger);

  // Small delay ensures layout is stable
  setTimeout(() => {
    initAnimations();
    ScrollTrigger.refresh();
  }, 100);
});
```

### 3. Horizontal Scroll on Mobile

**Error**: Page scrolls horizontally, breaking mobile experience

**Causes**:
- Elements with `width: 100vw` (includes scrollbar width)
- Absolute positioned elements extending beyond viewport
- Negative margins without overflow hidden

**Solution**:
```css
/* On body or main container */
overflow-x: hidden;

/* Use 100% instead of 100vw */
width: 100%;
max-width: 100%;

/* Or clip viewport */
html, body {
  overflow-x: clip;
}
```

### 4. OG Meta Tag Inconsistencies

**Error**: sed replacement only updated 7 of 12 files

**Cause**: Different versions had varying OG tag structures (some missing, some different patterns)

**Solution**: Always verify meta tags individually when bulk editing:
```bash
# Check which files have the expected pattern
grep -l "og:image" v*/index.html

# Verify the actual content
grep "og:url" v*/index.html
```

### 5. Playwright Screenshot Path Restrictions

**Error**: Cannot save screenshots to arbitrary paths

**Cause**: Playwright MCP restricts output to `.playwright-mcp/` directory

**Solution**:
```bash
# Save to allowed directory, then copy
# Screenshots go to .playwright-mcp/
# Then use bash to move them
for i in {1..12}; do
  cp .playwright-mcp/v${i}-og-image.png v${i}/og-image.png
done
```

### 6. CSS Animation Performance

**Error**: Janky animations on mobile devices

**Cause**: Animating expensive properties (width, height, top, left)

**Solution**:
```css
/* Animate transform and opacity only */
.animate-element {
  will-change: transform, opacity;
}

/* Use transform instead of position */
/* BAD */
.element { left: 100px; }

/* GOOD */
.element { transform: translateX(100px); }
```

---

## Tools Reference

### Highly Effective

| Tool | Use Case | Notes |
|------|----------|-------|
| **Playwright MCP** | Browser testing, screenshots | Reliable viewport testing, OG image capture |
| **Task tool** | Parallel sub-agent work | 4x faster for building multiple versions |
| **Context7** | Library documentation | Essential for GSAP, Three.js patterns |
| **Tailwind CDN** | Styling | No build = instant deployment |

### Situational

| Tool | Use Case | Limitations |
|------|----------|-------------|
| **sed** | Bulk text replacement | Pattern matching can be fragile |
| **grep** | Finding patterns | Good for verification, not modification |
| **Three.js** | 3D effects | Heavy, use only when essential |

### OG Image Workflow

```bash
# 1. Set viewport to OG dimensions
# Playwright: 1200x630

# 2. Navigate and screenshot each version
# Save to .playwright-mcp/v{N}-og-image.png

# 3. Copy to version folders
for i in {1..12}; do
  cp .playwright-mcp/v${i}-og-image.png v${i}/og-image.png
done

# 4. Add OG meta tags to each index.html
<meta property="og:image" content="https://vyente-ruffin.github.io/singularity-site/v{N}/og-image.png">
```

---

## Design Variation Reference

| Version | Style | Key Features | Complexity |
|---------|-------|--------------|------------|
| root | Neobrutalist | Bold borders, raw aesthetic | Medium |
| v1 | Bento Grid | Card layout, asymmetric grid | Medium |
| v2 | 3D/WebGL | Three.js particles, 3D scene | High |
| v3 | Kinetic Scroll | GSAP ScrollTrigger, parallax | High |
| v4 | Glassmorphism | Blur effects, gradients | Medium |
| v5 | Monospace/Dev | Terminal aesthetic, code blocks | Low |
| v6 | Parallax Storytelling | Pin sections, scroll narrative | High |
| v7 | Claymorphism | Soft shadows, rounded forms | Medium |
| v8 | Editorial/Magazine | Typography-focused, serif fonts | Low |
| v9 | Swiss Minimalist | Grid precision, whitespace | Low |
| v10 | Split Screen | 50/50 layouts, contrast | Medium |
| v11 | Maximalist | Dense, layered, high energy | High |
| v12 | Retro Futurism | 80s neon, CRT effects | Medium |

---

## Deployment Checklist

- [ ] All versions responsive (test 375px and 1440px)
- [ ] No horizontal scroll on mobile
- [ ] GSAP animations firing on load
- [ ] OG meta tags present with correct URLs
- [ ] OG images exist (1200x630)
- [ ] No console errors
- [ ] Links working (Book a Demo, Learn More)
- [ ] Git committed and pushed

## GitHub Pages URLs

```
https://vyente-ruffin.github.io/singularity-site/      # Neobrutalist
https://vyente-ruffin.github.io/singularity-site/v1/   # Bento Grid
https://vyente-ruffin.github.io/singularity-site/v2/   # 3D/WebGL
https://vyente-ruffin.github.io/singularity-site/v3/   # Kinetic Scroll
https://vyente-ruffin.github.io/singularity-site/v4/   # Glassmorphism
https://vyente-ruffin.github.io/singularity-site/v5/   # Monospace/Dev
https://vyente-ruffin.github.io/singularity-site/v6/   # Parallax Storytelling
https://vyente-ruffin.github.io/singularity-site/v7/   # Claymorphism
https://vyente-ruffin.github.io/singularity-site/v8/   # Editorial/Magazine
https://vyente-ruffin.github.io/singularity-site/v9/   # Swiss Minimalist
https://vyente-ruffin.github.io/singularity-site/v10/  # Split Screen
https://vyente-ruffin.github.io/singularity-site/v11/  # Maximalist
https://vyente-ruffin.github.io/singularity-site/v12/  # Retro Futurism
```

---

## Future Session Quick Start

When working on this project again:

1. **Test changes**: Use Playwright MCP to verify at 375px and 1440px viewports
2. **GSAP issues**: Check plugin load order first (most common cause)
3. **Mobile overflow**: Add `overflow-x: hidden` to body
4. **OG updates**: Verify each file individually, don't assume sed worked
5. **New versions**: Copy existing version as template, maintain structure
