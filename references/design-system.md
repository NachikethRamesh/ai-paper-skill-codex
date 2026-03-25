# Design System — Paper to Lesson

Visual identity: A scholarly yet approachable reading companion — feels like annotated margin notes from someone who genuinely understood the paper. Clean, spacious, and intellectually serious without being sterile. Distinct from tutorial-style interfaces: this is for understanding research, not learning to code.

---

## Color Palette

Copy this entire `:root` block into your HTML `<style>`:

```css
:root {
  /* === Backgrounds === */
  --color-bg: #F8F6F1;                /* warm parchment */
  --color-bg-alt: #F2EEE5;            /* slightly deeper, for alternating modules */
  --color-bg-code: #1B1D2E;           /* deep navy for code/math blocks */

  /* === Text === */
  --color-text: #2B2926;              /* rich charcoal */
  --color-text-secondary: #6A645E;    /* warm gray */
  --color-text-muted: #9D978F;        /* soft muted */
  --color-text-inverse: #F8F6F1;      /* light text on dark bg */

  /* === Borders & Surfaces === */
  --color-border: #E3DDD3;            /* subtle warm border */
  --color-surface: #FFFFFF;           /* card backgrounds */
  --color-surface-warm: #FBF8F2;      /* warm card variant */

  /* === Primary Accent (choose ONE per lesson) === */
  --color-accent: #2E6B8A;            /* scholarly blue — authoritative, research-oriented */
  /* Alternatives per paper topic:
     NLP papers:     #C05230 (burnt sienna)
     Vision papers:  #8B6BAE (soft violet)
     RL papers:      #C4883A (warm amber)
     Bio/Med papers: #3A8C5E (sage green)
     Math-heavy:     #7A4E8C (deep plum)
  */
  --color-accent-light: color-mix(in srgb, var(--color-accent) 12%, white);
  --color-accent-muted: color-mix(in srgb, var(--color-accent) 40%, var(--color-text-muted));

  /* === Semantic Colors === */
  --color-success: #3A8C5E;
  --color-success-light: #E6F4EC;
  --color-error: #C43A3A;
  --color-error-light: #FDE6E6;
  --color-info: #2E7D8C;
  --color-info-light: #E2F0F4;
  --color-warning: #C4883A;
  --color-warning-light: #FDF3E4;

  /* === Component Colors (for architecture diagrams, flowcharts) === */
  --color-component-1: #2E6B8A;       /* scholarly blue */
  --color-component-2: #8B6BAE;       /* soft violet */
  --color-component-3: #C4883A;       /* warm amber */
  --color-component-4: #3A8C5E;       /* sage green */
  --color-component-5: #C05230;       /* burnt sienna */
  --color-component-6: #6B5B73;       /* muted mauve */

  /* === Typography === */
  --font-display: 'Bricolage Grotesque', system-ui, sans-serif;
  --font-body: 'Source Serif 4', Georgia, serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', monospace;

  /* Type scale (1.25 ratio) */
  --text-xs: 0.75rem;      /* 12px — labels, badges */
  --text-sm: 0.875rem;     /* 14px — secondary text, code */
  --text-base: 1rem;       /* 16px — body */
  --text-lg: 1.125rem;     /* 18px — lead paragraphs */
  --text-xl: 1.25rem;      /* 20px — screen headings */
  --text-2xl: 1.5rem;      /* 24px — sub-module titles */
  --text-3xl: 1.875rem;    /* 30px — module subtitles */
  --text-4xl: 2.25rem;     /* 36px — module titles */
  --text-5xl: 3rem;        /* 48px — hero text */
  --text-6xl: 3.75rem;     /* 60px — module numbers */

  /* Line heights */
  --leading-tight: 1.15;
  --leading-snug: 1.3;
  --leading-normal: 1.6;
  --leading-loose: 1.8;

  /* === Spacing Scale === */
  --space-1: 0.25rem;   /* 4px */
  --space-2: 0.5rem;    /* 8px */
  --space-3: 0.75rem;   /* 12px */
  --space-4: 1rem;      /* 16px */
  --space-5: 1.25rem;   /* 20px */
  --space-6: 1.5rem;    /* 24px */
  --space-8: 2rem;      /* 32px */
  --space-10: 2.5rem;   /* 40px */
  --space-12: 3rem;     /* 48px */
  --space-16: 4rem;     /* 64px */
  --space-20: 5rem;     /* 80px */
  --space-24: 6rem;     /* 96px */

  /* === Layout === */
  --content-width: 800px;
  --content-width-wide: 1000px;
  --nav-height: 50px;

  /* === Border Radius === */
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --radius-full: 9999px;

  /* === Shadows (warm-tinted, never pure black) === */
  --shadow-sm: 0 1px 2px rgba(43, 41, 38, 0.05);
  --shadow-md: 0 4px 12px rgba(43, 41, 38, 0.08);
  --shadow-lg: 0 8px 24px rgba(43, 41, 38, 0.1);
  --shadow-xl: 0 16px 48px rgba(43, 41, 38, 0.12);

  /* === Motion === */
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
  --duration-fast: 150ms;
  --duration-normal: 300ms;
  --duration-slow: 500ms;
  --stagger-delay: 120ms;
}
```

---

## Google Fonts

Include in `<head>`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,400;12..96,600;12..96,700;12..96,800&family=Source+Serif+4:ital,opsz,wght@0,8..60,300;0,8..60,400;0,8..60,500;0,8..60,600;0,8..60,700;1,8..60,400;1,8..60,500&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
```

---

## Typography Rules

| Element | Size | Font | Weight | Line Height |
|---------|------|------|--------|-------------|
| Module numbers | text-6xl | display | 800 | tight |
| Module titles | text-4xl | display | 700 | tight |
| Screen headings | text-xl / text-2xl | display | 600 | snug |
| Body text | text-base / text-lg | body | 400 | normal |
| Code / math | text-sm | mono | 400 | normal |
| Labels & badges | text-xs | mono | 500 | tight |

- Module numbers: use accent color at 15% opacity for large decorative numbers
- Labels/badges: uppercase, `letter-spacing: 0.05em`

---

## Module Layout

```css
/* Alternating module backgrounds */
.module:nth-child(even) { background: var(--color-bg); }
.module:nth-child(odd)  { background: var(--color-bg-alt); }

.module {
  min-height: 100dvh;
  min-height: 100vh; /* fallback */
  scroll-snap-align: start;
  padding: var(--space-16) var(--space-6);
  padding-top: calc(var(--nav-height) + var(--space-12));
  border-top: 2px solid color-mix(in srgb, var(--color-accent) 20%, transparent);
}

.module-content {
  max-width: var(--content-width);
  margin: 0 auto;
}

/* Wide layouts (side-by-side comparisons, math translations) */
.module-content--wide {
  max-width: var(--content-width-wide);
}
```

---

## Scroll & Navigation

```css
html {
  scroll-snap-type: y proximity;  /* NEVER mandatory */
  scroll-behavior: smooth;
}

body {
  background: var(--color-bg);
  background-image: radial-gradient(
    ellipse at 20% 50%,
    color-mix(in srgb, var(--color-accent) 3%, transparent) 0%,
    transparent 50%
  );
}

/* Custom scrollbar */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb {
  background: var(--color-border);
  border-radius: var(--radius-full);
}
```

---

## Scroll-Triggered Animations

```css
.animate-in {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity var(--duration-slow) var(--ease-out),
              transform var(--duration-slow) var(--ease-out);
}
.animate-in.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Stagger children */
.stagger-children > .animate-in {
  transition-delay: calc(var(--stagger-index, 0) * var(--stagger-delay));
}
```

```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      observer.unobserve(entry.target);
    }
  });
}, { rootMargin: '0px 0px -10% 0px', threshold: 0.1 });

document.querySelectorAll('.animate-in').forEach(el => observer.observe(el));
```

---

## Syntax Highlighting (for code blocks on dark background)

```css
.code-keyword   { color: #CBA6F7; }   /* purple — def, class, import, return */
.code-string    { color: #A6E3A1; }   /* green — string literals */
.code-function  { color: #89B4FA; }   /* blue — function names */
.code-comment   { color: #6C7086; }   /* gray — comments */
.code-number    { color: #FAB387; }   /* peach — numeric literals */
.code-variable  { color: #F9E2AF; }   /* yellow — variables, properties */
.code-operator  { color: #94E2D5; }   /* teal — operators */
.code-builtin   { color: #F38BA8; }   /* pink — built-in functions */
.code-decorator { color: #F9E2AF; }   /* yellow — decorators */
.code-type      { color: #89DCEB; }   /* sky — type annotations */
```

---

## Math Rendering

For equations, use Unicode math symbols and styled `<span>` elements rather than external libraries like MathJax or KaTeX (keeps the file self-contained).

```css
.math-block {
  font-family: var(--font-mono);
  font-size: var(--text-lg);
  color: var(--color-text);
  padding: var(--space-6);
  background: var(--color-surface);
  border-radius: var(--radius-md);
  text-align: center;
  letter-spacing: 0.02em;
}

.math-inline {
  font-family: var(--font-mono);
  font-size: 0.95em;
  background: var(--color-surface-warm);
  padding: 0.1em 0.3em;
  border-radius: var(--radius-sm);
}

/* Subscript/superscript styling */
.math-block sub { font-size: 0.7em; vertical-align: sub; }
.math-block sup { font-size: 0.7em; vertical-align: super; }
```

Common Unicode math symbols reference:
- Summation: `&#8721;` (∑)
- Product: `&#8719;` (∏)
- Square root: `&#8730;` (√)
- Infinity: `&#8734;` (∞)
- Partial derivative: `&#8706;` (∂)
- Element of: `&#8712;` (∈)
- Arrow: `&#8594;` (→)
- Approximately: `&#8776;` (≈)
- Not equal: `&#8800;` (≠)
- Less/greater or equal: `&#8804;` (≤) / `&#8805;` (≥)
- Multiplication dot: `&#183;` (·)
- Times: `&#215;` (×)
- Greek: α β γ δ ε θ λ μ σ φ ω (use literal Unicode characters)

---

## Responsive Breakpoints

```css
/* Tablet: 768px and below */
@media (max-width: 768px) {
  :root {
    --text-4xl: 1.875rem;
    --text-5xl: 2.25rem;
    --text-6xl: 3rem;
  }
  .module {
    padding: var(--space-12) var(--space-4);
    padding-top: calc(var(--nav-height) + var(--space-8));
  }
  /* Math translation blocks stack vertically */
  .math-translation { grid-template-columns: 1fr; }
  .comparison-grid { grid-template-columns: 1fr; }
}

/* Mobile: 480px and below */
@media (max-width: 480px) {
  :root {
    --text-3xl: 1.5rem;
    --text-4xl: 1.5rem;
    --text-5xl: 1.875rem;
    --text-6xl: 2.25rem;
  }
  .module {
    padding: var(--space-8) var(--space-3);
    padding-top: calc(var(--nav-height) + var(--space-6));
  }
  /* Flow arrows go vertical */
  .flow-arrow { transform: rotate(90deg); }
  .flow-diagram { flex-direction: column; }
}
```
