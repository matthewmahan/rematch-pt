# Copilot Instructions for Rematch PT

## Project Overview
**Rematch Physical Therapy** is a static marketing website for a physical therapy practice. Pure HTML, CSS, and vanilla JavaScript with no frameworks, build tools, or dependencies.

**Stack**: HTML + CSS + vanilla JS  
**Deployment**: Static hosting (GitHub Pages, Netlify, Vercel)  
**Architecture**: Single-page HTML files with embedded styles and scripts—no separation of concerns or bundling needed.

## Page Structure
Five core pages (each is a standalone HTML file):
- `index.html` — Homepage hero & overview
- `injuries.html` — Injury types & treatment services  
- `methodologies.html` — PT process & philosophy
- `about.html` — Dr. Allie bio & credentials
- `contact.html` — Free consultation form

All pages share the same CSS custom properties (variables) and JavaScript patterns.

## CSS Conventions

### Design System (CSS Variables)
All color values defined in `:root` at the top of each page:
```css
--black: #0a0a0a;
--cream: #f5f2ed;
--accent: #2b8fd9;
--accent-dim: #005ea8;
--gray-{900,800,700,400,300}: ...
--font-display: 'Bebas Neue';
--font-body: 'DM Sans';
```

**When modifying colors**: Update the `:root` block in ALL five pages to keep the palette consistent. Use `var(--color-name)` everywhere.

### Typography
- **Display font** (`--font-display`): Bold headers, section titles
- **Body font** (`--font-body`): All text content, with weights from 300 to 700
- Use `.container` utility class (max-width: 1280px, responsive padding via clamp)
- Section headings use `.section-eyebrow` class (small caps, accent color, horizontal line before)

### Layout Sections
Most pages follow this pattern:
```html
<section>
  <div class="container">
    <h2 class="section-eyebrow">Label</h2>
    <h1>Main Heading</h1>
    <div class="reveal"><!-- content --></div>
  </div>
</section>
```

The `.reveal` class triggers scroll-based animations (see JS section below).

## JavaScript Patterns

All pages share three behaviors defined in `<script>` tags at document end:

### 1. Nav Scroll Effect
```js
const nav = document.getElementById('mainNav');
window.addEventListener('scroll', () => {
  nav.classList.toggle('scrolled', window.scrollY > 40);
});
```
Adds `scrolled` class to nav when user scrolls past 40px. CSS responds with blurred background.

### 2. Mobile Menu Toggle
```js
const toggle = document.getElementById('mobileToggle');
const navLinks = document.getElementById('navLinks');
toggle.addEventListener('click', () => {
  navLinks.classList.toggle('open');
});
```
Hamburger menu button (`#mobileToggle`) toggles `open` class on nav links container. Hide/show via CSS media queries.

### 3. Scroll Reveal Animation
```js
const reveals = document.querySelectorAll('.reveal');
const observer = new IntersectionObserver((entries) => {
  entries.forEach((entry, i) => {
    if (entry.isIntersecting) {
      setTimeout(() => entry.target.classList.add('visible'), i * 80);
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.1, rootMargin: '0px 0px -40px 0px' });
reveals.forEach(el => observer.observe(el));
```
**Pattern**: Add `.reveal` class to any element to trigger fade-in + stagger animation on scroll. Delay stacks with `i * 80ms` per element.

## Adding New Content

1. **Copy an existing page** as a template (index.html is simplest)
2. **Update `<title>`, page hero, sections** with new content
3. **Reuse `.reveal` classes** for scroll animations
4. **Test mobile responsiveness**: All media queries use `max-width: 1024px` breakpoint for mobile

## Deployment Notes
- No build step required; push files directly to Git
- GitHub Pages: Enable in repo Settings → Pages (root folder)
- Netlify: Drag project folder to [Netlify Drop](https://app.netlify.com/drop)
- All styles are inline in `<style>` tags—no external CSS files

## Key Files to Reference
- [index.html](../index.html) — Full example with all sections, hero, scroll reveals
- [contact.html](../contact.html) — Form structure example
- All pages — Same CSS variables block and JS script patterns
