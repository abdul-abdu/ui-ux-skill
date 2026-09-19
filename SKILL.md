---
name: tailwind-plus-ui-ux
description: Visual/UX design-pattern reference mined from 13 Tailwind Plus templates (radiant, salient, studio, protocol, syntax, compass, primer, commit, transmit, pocket, keynote, spotlight, oatmeal) — color strategy, typography pairing, motion/animation patterns, and the concrete variants each template uses for recurring sections (hero, testimonials, pricing, FAQ, nav/header, footer, decorative backgrounds), with real embedded code for every named technique. Use this when making an aesthetic or UX decision — picking a visual treatment for a section, choosing a color/type strategy for a new site, deciding how much motion to add, or wanting a working example of a specific UI pattern (animated counter, marquee, sticky nav, accordion, scroll-driven header) before designing one from scratch. Self-contained: every technique cited here ships with its actual source code inline, so this works in any project without needing access to the original template folders.
---

# Tailwind Plus UI/UX Design Patterns

A design reference distilled from 13 Tailwind Plus Next.js/Tailwind v4 templates. Every technique below includes the real, verified source code (trimmed to the essential lines) rather than just a pointer — this file is meant to be dropped into any project and used standalone.

This doc is about **how these templates look and behave**, not how they're routed or authored. Provenance is noted per snippet (template name + original file path) for context, not because you need to go find that file — the code you need is right here.

## Color strategy

The single most important, least obvious pattern across all 13: **almost none of them define a bespoke brand color palette.** `@theme` blocks redefine type scale, radius, and container tokens constantly, but color tokens are left alone in 11 of 13 templates. "Brand identity" instead comes from picking one existing Tailwind hue and using it consistently for CTAs/links/active-states, or from one recurring gradient reused in 3-4 places.

| Template | Strategy | Details |
|---|---|---|
| radiant | Signature gradient | Warm gold→pink→purple, reused in hero background, testimonial title text, and footer/pricing panels |
| protocol | Signature gradient + accent | Teal→lime decorative hero gradient; functional accent is stock `emerald-500/400` |
| salient | Stock hue | `blue-600`/`blue-400` |
| primer | Stock hue | `blue-600` |
| keynote | Stock hue | `blue-600`/`blue-900`, `indigo-50` hero wash only |
| syntax | Stock hue | `sky-500`/`sky-400` |
| commit | Stock hue | `sky-300` + a blue radial glow |
| pocket | Stock hue + neutral override | `cyan-500/600` accent; also overrides `--color-gray-*` to zero-chroma neutral |
| transmit | Stock hue, minor | pink/violet used sparingly, no site-wide brand utility |
| spotlight | Stock hue | `teal-400/500` |
| compass | None (grayscale) | pure `gray-950`/`gray-700`/white; `blue-500/700` appears only as a focus-ring color |
| studio | None (monochrome) | dark-first `neutral-950`, no accent color anywhere |
| oatmeal | Custom palette (the exception) | full 11-step `olive-*` oklch scale — the only template with a real bespoke named palette |

**Takeaway**: you don't need to invent a color system before you start — pick one accent hue and apply it with discipline (CTAs, active nav state, links, focus rings), or go monochrome and let type/motion carry personality. Reach for a real custom palette only when the brand genuinely needs a hue Tailwind's defaults don't cover.

### Radiant's signature gradient
Reused verbatim in the hero background, testimonial title text, and footer/pricing panels — one gradient string, several contexts (`radiant-ts/src/components/gradient.tsx`):
```tsx
export function Gradient({ className, ...props }: React.ComponentPropsWithoutRef<'div'>) {
  return (
    <div
      {...props}
      className={clsx(
        className,
        'bg-linear-115 from-[#fff1be] from-28% via-[#ee87cb] via-70% to-[#b060ff] sm:bg-linear-145',
      )}
    />
  )
}

export function GradientBackground() {
  return (
    <div className="relative mx-auto max-w-7xl">
      <div
        className={clsx(
          'absolute -top-44 -right-60 h-60 w-xl transform-gpu md:right-0',
          'bg-linear-115 from-[#fff1be] from-28% via-[#ee87cb] via-70% to-[#b060ff]',
          'rotate-[-10deg] rounded-full blur-3xl',
        )}
      />
    </div>
  )
}
```

### Protocol's signature gradient
A teal→lime hero pattern, masked to a soft radial fade and skewed behind a grid overlay (`protocol-ts/src/components/HeroPattern.tsx`):
```tsx
<div className="absolute inset-0 bg-linear-to-r from-[#36b49f] to-[#DBFF75] mask-[radial-gradient(farthest-side_at_top,white,transparent)] opacity-40 dark:from-[#36b49f]/30 dark:to-[#DBFF75]/30 dark:opacity-100">
  <GridPattern
    width={72} height={56} x={-12} y={4}
    squares={[[4, 3], [2, 1], [7, 3], [10, 6]]}
    className="absolute inset-x-0 inset-y-[-50%] h-[200%] w-full skew-y-[-18deg] fill-black/40 stroke-black/50 mix-blend-overlay dark:fill-white/2.5 dark:stroke-white/5"
  />
</div>
```

### Pocket's true-neutral gray override
Tailwind's default gray has a cool (blue) tint; this swaps it for zero-chroma oklch values — a genuinely neutral gray (`pocket-ts/src/styles/tailwind.css`):
```css
@theme {
  --color-gray-50: oklch(0.985 0 0);
  --color-gray-100: oklch(0.97 0 0);
  --color-gray-200: oklch(0.922 0 0);
  --color-gray-300: oklch(0.87 0 0);
  --color-gray-400: oklch(0.708 0 0);
  --color-gray-500: oklch(0.556 0 0);
  --color-gray-600: oklch(0.439 0 0);
  --color-gray-700: oklch(0.371 0 0);
  --color-gray-800: oklch(0.269 0 0);
  --color-gray-900: oklch(0.205 0 0);
  --color-gray-950: oklch(0.145 0 0);
}
```

### Oatmeal's bespoke palette
The only template of the 13 with a real named color scale, plus how it swaps for dark mode at the root (`oatmeal-olive-instrument/tailwind.css`):
```css
@theme {
  --color-olive-50: oklch(98.8% 0.003 106.5);
  --color-olive-100: oklch(96.6% 0.005 106.5);
  --color-olive-200: oklch(93% 0.007 106.5);
  --color-olive-300: oklch(88% 0.011 106.6);
  --color-olive-400: oklch(73.7% 0.021 106.9);
  --color-olive-500: oklch(58% 0.031 107.3);
  --color-olive-600: oklch(46.6% 0.025 107.3);
  --color-olive-700: oklch(39.4% 0.023 107.4);
  --color-olive-800: oklch(28.6% 0.016 107.4);
  --color-olive-900: oklch(22.8% 0.013 107.4);
  --color-olive-950: oklch(15.3% 0.006 107.1);
}

@layer base {
  html {
    background-color: var(--color-olive-100);
    @variant dark {
      background-color: var(--color-olive-950);
    }
  }
}
```

## Typography pairing

Four distinct strategies, in order of how much visual distinctiveness they buy vs. effort/performance cost:

### 1. Two Google fonts via `next/font/google` — cheapest real pairing
Sans for body, a different display face for headlines, with full Next.js font optimization. Salient's setup (Inter + Lexend, `salient-ts/src/app/layout.tsx`):
```tsx
import { Inter, Lexend } from 'next/font/google'

const inter = Inter({ subsets: ['latin'], display: 'swap', variable: '--font-inter' })
const lexend = Lexend({ subsets: ['latin'], display: 'swap', variable: '--font-lexend' })

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={clsx('h-full scroll-smooth bg-white antialiased', inter.variable, lexend.variable)}>
      <body className="flex h-full flex-col">{children}</body>
    </html>
  )
}
```
```css
/* salient-ts/src/styles/tailwind.css */
@theme {
  --font-sans: var(--font-inter);
  --font-display: var(--font-lexend);
}
```
Keynote does the same pairing pattern with Inter+DM Sans, both via `next/font/google` — copy this shape if that's what you want. Syntax pairs Inter (google) with Lexend too, but loads Lexend via `next/font/local` instead (see strategy 2 below) — a hybrid of this strategy and the next one, not a pure example of it.

### 2. One self-hosted variable font, two axes — no second family at all
"Display" is just a different axis setting of the same font file. Studio varies **width**:
```css
/* studio-ts/src/styles/base.css */
@font-face {
  font-family: 'Mona Sans';
  font-weight: 200 900;
  font-display: block;
  font-style: normal;
  font-stretch: 75% 125%;
  src: url('../fonts/Mona-Sans.var.woff2') format('woff2');
}
```
```css
/* studio-ts/src/styles/tailwind.css */
@theme {
  --font-sans: Mona Sans, ui-sans-serif, system-ui, sans-serif;
  --font-display: Mona Sans, ui-sans-serif, system-ui, sans-serif;
  --font-display--font-variation-settings: 'wdth' 125; /* wider stretch for display text, same file */
}
```
Commit uses the same font file but varies **weight** instead, loaded via `next/font/local` rather than a raw `@font-face` (`commit-ts/src/app/layout.tsx`):
```tsx
import localFont from 'next/font/local'

const monaSans = localFont({
  src: '../fonts/Mona-Sans.var.woff2',
  display: 'swap',
  variable: '--font-mona-sans',
  weight: '200 900',
})
```

### 3. External CDN font (Fontshare, not `next/font`) — a specific typeface at the cost of automatic optimization
Radiant loads Switzer via a plain `<link>` tag in `<head>` (`radiant-ts/src/app/layout.tsx`):
```tsx
<head>
  <link rel="stylesheet" href="https://api.fontshare.com/css?f%5B%5D=switzer@400,500,600,700&display=swap" />
</head>
```
Primer mixes both strategies in one template — Inter via `next/font/google` for body, Cabinet Grotesk via Fontshare CDN for display (`primer-ts/src/app/layout.tsx`):
```tsx
import { Inter } from 'next/font/google'
const inter = Inter({ subsets: ['latin'], display: 'swap', variable: '--font-inter' })
// ...
<head>
  <link rel="preconnect" href="https://cdn.fontshare.com" crossOrigin="anonymous" />
  <link rel="stylesheet" href="https://api.fontshare.com/v2/css?f[]=cabinet-grotesk@800,500,700&display=swap" />
</head>
```
**Gotcha**: Radiant's components apply a `font-display` utility class in several places, but its `tailwind.css` never actually defines a `--font-display` token — that class is a silent no-op there. If you copy the CDN-font pattern, make sure you also wire the theme token, not just the `<link>` tag.

### 4. No custom font at all — system stack
Protocol and Spotlight both skip custom fonts entirely — a valid choice for a text-first site where content should carry the design, not the chrome. Nothing to embed here; it's the absence of the above three patterns.

## Shape & spacing tokens

`--radius-4xl`/`--radius-5xl` (2rem/2.5rem) is close to universal for "soft, large" cards and hero panels — treat it as the house convention. `--shadow-*`/`--container-*` overrides are rarer:

**Primer's heavier, tinted shadows** — visibly different from Tailwind's flat default shadows (`primer-ts/src/styles/tailwind.css`):
```css
@theme {
  --shadow-*: initial;
  --shadow-sm: 0 2px 6px rgb(15 23 42 / 0.08);
  --shadow-md: 0 8px 8px rgb(15 23 42 / 0.05), 0 3px 6px rgb(15 23 42 / 0.05);
  --shadow-lg: 0 8px 15px rgb(15 23 42 / 0.08), 0 3px 6px rgb(15 23 42 / 0.08);
  --shadow-xl: 2px 11px 16px rgb(15 23 42 / 0.17), 0 1px 6px rgb(15 23 42 / 0.17), 3px 23px 24px rgb(15 23 42 / 0.17);
}
```

**Protocol's subtle glow + granular container widths** (`protocol-ts/src/styles/tailwind.css`):
```css
@theme {
  --shadow-glow: 0 0 4px rgb(0 0 0 / 0.1);
  --container-lg: 33rem;
  --container-2xl: 40rem;
  --container-3xl: 50rem;
  --container-5xl: 66rem;
}
```

**Oatmeal's one variable driving three layout behaviors** — anchor-scroll offset, sticky-nav height, and sticky-table-header position all read the same custom property (`oatmeal-olive-instrument/tailwind.css` + two component usages):
```css
/* base: sets the default and wires it to the CSS scroll-padding property */
html {
  --scroll-padding-top: 0;
  scroll-padding-top: var(--scroll-padding-top);
}
```
```tsx
// each sticky navbar variant overrides the variable locally, then sizes itself off it
<header className="sticky top-0 z-10 bg-olive-100 dark:bg-olive-950">
  <style>{`:root { --scroll-padding-top: 5.25rem }`}</style>
  <nav>
    <div className="mx-auto flex h-(--scroll-padding-top) max-w-7xl items-center gap-4 px-6 lg:px-10">
      {/* ... */}
    </div>
  </nav>
</header>
```
```tsx
// a sticky comparison-table header row reads the exact same variable for its offset
<th className="sticky top-(--scroll-padding-top) bg-olive-100 py-5 pr-3 dark:bg-olive-950">
  Compare features
</th>
```

## Motion: the spectrum, and a real accessibility gap

Motion intensity varies enormously, and **only one of the animated templates (`studio`) handles reduced motion.** Every other template with framer-motion/`motion` ships animation with zero `useReducedMotion`/`prefers-reduced-motion` guard, including ones with persistent, non-scroll-triggered motion. If you copy any animation code below except studio's, add the guard yourself.

**No motion at all**: keynote, transmit, syntax, oatmeal (all interactivity is native `<dialog>` + Commands API instead), spotlight (no animation library, but see its hand-rolled scroll effect below).

### Radiant — spring count-up, hover-cascaded bento cards, jiggling logo cluster
Count-up on scroll into view (`radiant-ts/src/components/animated-number.tsx`):
```tsx
'use client'
import { motion, useInView, useMotionValue, useSpring, useTransform } from 'framer-motion'
import { useEffect, useRef } from 'react'

export function AnimatedNumber({ start, end, decimals = 0 }: { start: number; end: number; decimals?: number }) {
  let ref = useRef(null)
  let isInView = useInView(ref, { once: true, amount: 0.5 })
  let value = useMotionValue(start)
  let spring = useSpring(value, { damping: 30, stiffness: 100 })
  let display = useTransform(spring, (num) => num.toFixed(decimals))

  useEffect(() => {
    value.set(isInView ? end : start)
  }, [start, end, isInView, value])

  return <motion.span ref={ref}>{display}</motion.span>
}
```
A card whose hover state cascades into its children via a shared variant name, instead of each child managing its own hover listener (`bento-card.tsx`):
```tsx
<motion.div initial="idle" whileHover="active" variants={{ idle: {}, active: {} }} className="group ...">
  <div className="relative h-80 shrink-0">{graphic}</div>
  {/* graphic can itself be a motion component with matching variant keys, e.g. LogoCluster below */}
</motion.div>
```
One logo of the cluster inside that graphic — each has its own idle/active offset so hovering the parent card makes every logo jiggle independently:
```tsx
<motion.img
  variants={{
    idle: { x: 0, y: 0, rotate: 0 },
    active: {
      x: [0, hover.x, 0], y: [0, hover.y, 0], rotate: [0, hover.rotate, 0],
      transition: { duration: 0.75, repeat: Infinity, repeatDelay: 1.25, ease: 'easeInOut', delay: hover.delay },
    },
  }}
  style={{ left, top }}
  className="absolute size-16 rounded-full bg-white shadow-sm ring-1 ring-black/5"
/>
```

### Protocol — scroll-linked header opacity (a continuous fade, not a toggle)
`protocol-ts/src/components/Header.tsx`:
```tsx
import { motion, useScroll, useTransform } from 'framer-motion'

let { scrollY } = useScroll()
let bgOpacityLight = useTransform(scrollY, [0, 72], ['50%', '90%'])
let bgOpacityDark = useTransform(scrollY, [0, 72], ['20%', '80%'])

<motion.div
  className="fixed inset-x-0 top-0 z-50 ... bg-white/(--bg-opacity-light) dark:bg-zinc-900/(--bg-opacity-dark)"
  style={{ '--bg-opacity-light': bgOpacityLight, '--bg-opacity-dark': bgOpacityDark } as React.CSSProperties}
/>
```
The trick: bind a motion value to a CSS custom property via inline `style`, then reference that variable inside a Tailwind arbitrary-value opacity utility (`bg-white/(--bg-opacity-light)`) — lets a JS-driven scroll value control a Tailwind class without re-rendering class names on every scroll tick.

### Studio — the disciplined, reduced-motion-aware implementation
Scroll-reveal that explicitly degrades under reduced motion (`studio-ts/src/components/FadeIn.tsx`):
```tsx
'use client'
import { motion, useReducedMotion } from 'framer-motion'

export function FadeIn(props: React.ComponentPropsWithoutRef<typeof motion.div>) {
  let shouldReduceMotion = useReducedMotion()
  return (
    <motion.div
      variants={{
        hidden: { opacity: 0, y: shouldReduceMotion ? 0 : 24 },
        visible: { opacity: 1, y: 0 },
      }}
      transition={{ duration: 0.5 }}
      initial="hidden"
      whileInView="visible"
      viewport={{ once: true, margin: '0px 0px -200px' }}
      {...props}
    />
  )
}
```
And a global kill-switch so *every* motion component in the app respects the same preference at once (`RootLayout.tsx`):
```tsx
let shouldReduceMotion = useReducedMotion()

<MotionConfig transition={shouldReduceMotion || !isTransitioning ? { duration: 0 } : undefined}>
  {/* entire app tree */}
</MotionConfig>
```
This `MotionConfig` wrapper is the single most valuable thing to steal from this whole reference set — it's a one-line way to make an entire framer-motion app respect `prefers-reduced-motion`, and none of the other animated templates here do it.

### Pocket — morphing hover pill (heaviest motion of the set)
A shared `layoutId` makes a background pill smoothly slide between nav links on hover, rather than each link animating its own background independently (`pocket-ts/src/components/NavLinks.tsx`):
```tsx
'use client'
import { AnimatePresence, motion } from 'framer-motion'

let [hoveredIndex, setHoveredIndex] = useState<number | null>(null)

<Link onMouseEnter={() => setHoveredIndex(index)} onMouseLeave={() => setHoveredIndex(null)} className="relative ...">
  <AnimatePresence>
    {hoveredIndex === index && (
      <motion.span
        className="absolute inset-0 rounded-lg bg-gray-100"
        layoutId="hoverBackground"
        initial={{ opacity: 0 }}
        animate={{ opacity: 1, transition: { duration: 0.15 } }}
        exit={{ opacity: 0, transition: { duration: 0.15 } }}
      />
    )}
  </AnimatePresence>
  <span className="relative z-10">{label}</span>
</Link>
```
The CSS-only keyframes backing its marquee/spin effects (`pocket-ts/src/styles/tailwind.css`):
```css
@theme {
  --animate-fade-in: fade-in 0.5s linear forwards;
  --animate-spin-slow: spin 4s linear infinite;
  --animate-spin-reverse: spin-reverse 1s linear infinite;
  @keyframes fade-in { from { opacity: 0; } to { opacity: 1; } }
  @keyframes spin-reverse { to { transform: rotate(-360deg); } }
}
@theme inline {
  --animate-marquee: marquee var(--marquee-duration) linear infinite;
  @keyframes marquee { 100% { transform: translateY(-50%); } }
}
```

### Commit — persistent twinkling star field (not scroll- or hover-triggered)
Uses the renamed `motion` package's imperative `animate()` API directly on a ref, not the `<motion.div>` component form (`commit-ts/src/components/StarField.tsx`):
```tsx
let groupRef = useRef<React.ElementRef<'g'>>(null) // wraps the circle, fades in once
let ref = useRef<React.ElementRef<'circle'>>(null)  // the star itself, pulses forever

useEffect(() => {
  let delay = Math.random() * 2
  let animations = [
    animate(groupRef.current, { opacity: 1 }, { duration: 4, delay }),
    animate(
      ref.current,
      { opacity: dim ? [0.2, 0.5] : [1, 0.6], scale: dim ? [1, 1.2] : [1.2, 1] },
      { delay, duration: Math.random() * 2 + 2, direction: 'alternate', repeat: Infinity },
    ),
  ]
  return () => animations.forEach((a) => a.cancel())
}, [dim])
```
This runs forever from mount — no scroll listener, no `useInView`, no reduced-motion check.

### Spotlight — scroll-driven header transform via raw CSS custom properties
No animation library at all — plain `scroll`/`resize` listeners write CSS variables that a stylesheet reads, shrinking and translating the avatar into the nav bar as you scroll past it (`spotlight-ts/src/components/Header.tsx`):
```tsx
useEffect(() => {
  function setProperty(property: string, value: string) {
    document.documentElement.style.setProperty(property, value)
  }

  function updateAvatarStyles() {
    let fromScale = 1, toScale = 36 / 64
    let fromX = 0, toX = 2 / 16
    let scrollY = downDelay - window.scrollY
    let scale = clamp((scrollY * (fromScale - toScale)) / downDelay + toScale, fromScale, toScale)
    let x = clamp((scrollY * (fromX - toX)) / downDelay + toX, fromX, toX)
    setProperty('--avatar-image-transform', `translate3d(${x}rem, 0, 0) scale(${scale})`)
  }

  window.addEventListener('scroll', updateStyles, { passive: true })
  window.addEventListener('resize', updateStyles)
}, [isHomePage])
```
The header itself then just reads those variables in inline `style`, e.g. `height: 'var(--header-height)'` — the whole effect is CSS variables driven by a scroll listener, no transform library needed.

## Recurring section patterns

### Hero
- **Centered, plain**: salient (hand-drawn SVG squiggle underline behind a highlighted word), studio (no background at all), keynote (full-bleed decorative background image), oatmeal's `hero-simple-centered`/`hero-centered-with-demo`.
- **Split two-column**: syntax (fake code-editor window + circuit-pattern SVG background), pocket (phone mockup + spinning ring illustration), oatmeal's `hero-two-column-with-photo`/`hero-left-aligned-with-demo`.
- **Left-aligned over a decorative panel**: radiant — headline over a blurred pastel-gradient panel (code above).
- **Asymmetric/bento**: primer — a rotated book-cover image on a solid panel paired with a pull-quote in a 12-col grid.
- Oatmeal ships **8 hero variants** as ready-made choices — check there first if you want a hero shape without building one.

### Testimonials
Four genuinely different UX approaches — pick based on volume and tone:
- **Static grid** (salient, primer): 3-column cards with a decorative oversized quote-mark glyph.
- **Horizontal snap-scroll carousel** (radiant): portrait-photo cards, off-screen cards fade via `useScroll`+spring, dot pagination.
- **Auto-scrolling vertical marquee** (pocket): masonry columns scrolling upward at different speeds, gated by `useInView`.
- **Single large pull-quote** (studio): one centered quote + client logo, reused per page.

### Pricing
Present in salient, primer, radiant, and several oatmeal variants; absent from agency/docs/portfolio-shaped templates (studio, protocol, syntax, spotlight, keynote, transmit, commit, compass).
- **Featured-tier via solid fill**: salient (middle plan gets `order-first bg-blue-600`) and primer (solid `bg-blue-600` + `GridPattern` watermark).
- **No distinct featured styling**: radiant — all cards get the same glassy, inset-shadow bezel treatment.
- Oatmeal ships the most variety: `pricing-hero-multi-tier`, `pricing-multi-tier` (reflows column count by plan count), `pricing-single-tier-two-column`, and a `plan-comparison-table` with a sticky header row (code above) and mobile tab-switcher.

### FAQ
**Gotcha**: sections that look like an FAQ accordion aren't always interactive. Salient's `Faqs.tsx` is a plain static 3-column list with no expand/collapse at all. A true accordion only shows up in oatmeal (`oatmeal-olive-instrument/components/sections/faqs-accordion.tsx`), built on `ElDisclosure` with `aria-expanded`-driven icon swaps:
```tsx
export function Faq({ id, question, answer, ...props }: { question: ReactNode; answer: ReactNode } & ComponentProps<'div'>) {
  let autoId = useId()
  id = id || autoId

  return (
    <div id={id} {...props}>
      <button type="button" id={`${id}-question`} command="--toggle" commandfor={`${id}-answer`} className="flex w-full items-start justify-between gap-6 py-4 text-left">
        {question}
        <PlusIcon className="h-lh in-aria-expanded:hidden" />
        <MinusIcon className="h-lh not-in-aria-expanded:hidden" />
      </button>
      <ElDisclosure id={`${id}-answer`} hidden className="-mt-2 flex flex-col gap-2 pr-12 pb-4 text-sm/7">
        {answer}
      </ElDisclosure>
    </div>
  )
}
```
The button never sets `aria-expanded` itself — `ElDisclosure` manages that attribute on the trigger/panel pair, and the icon swap is pure CSS reading that state via the `in-aria-expanded:`/`not-in-aria-expanded:` variants. Check for actual toggle behavior like this before assuming a "FAQ section" is interactive.

### Header / nav — sticky behavior spectrum
- **Not sticky**: radiant, salient, keynote — plain top bar in normal document flow.
- **Always sticky, static**: syntax's docs header — `sticky top-0 z-50`, permanent shadow, `backdrop-blur-sm` only on scroll in dark mode.
- **Sticky with continuous scroll-linked transform**: protocol (code above).
- **Fully custom scroll-driven transform**: spotlight (code above).
- **Persistent app-shell nav, not really a header**: transmit's fixed sidebar + fixed-bottom player bar; compass's three distinct layout shells (narrow centered auth, collapsible sidebar rail for lessons, full-width navbar for plain pages) — worth studying as a set if a project needs more than one nav paradigm to coexist.

A fourth, hand-rolled scroll-spy variant — Primer's `NavBar` computes the active section from raw `getBoundingClientRect()` comparisons on scroll, no `IntersectionObserver`:
```tsx
function updateActiveIndex() {
  let elements = sections.map(({ id }) => document.getElementById(id)).filter(Boolean)
  let offset = document.body.getBoundingClientRect().top + navBarRef.current.offsetHeight + 1
  let newActiveIndex = null
  for (let index = 0; index < elements.length; index++) {
    if (window.scrollY >= elements[index].getBoundingClientRect().top - offset) newActiveIndex = index
    else break
  }
  setActiveIndex(newActiveIndex)
}
window.addEventListener('scroll', updateActiveIndex, { passive: true })
```

### Decorative backgrounds
Every template with visual flourish invents its own bespoke SVG pattern component rather than sharing one:
- radiant's `plus-grid.tsx` — small "+" marks at grid intersections.
- studio's `GridPattern.tsx` — tessellated house/shield tiles, and uniquely interactive (mouse movement lights up tiles).
- protocol's `GridPattern.tsx` (code above) — square-grid tiles inside a skewed radial-masked gradient.
- primer's `GridPattern.tsx` (grid lines) + separate `Pattern.tsx` (checkerboard of circle/square SVG symbols).
- oatmeal's `Wallpaper` — a gradient block with an inline SVG `feTurbulence` noise texture at `mix-blend-overlay`, used behind screenshots rather than headlines.
- commit's `StarField` (code above) + dotted `Timeline` — the only pair built for a dark, ambient/persistent feel.

## Distinctive one-off techniques

Specific, narrow tricks that solve a particular visual problem well:

### Puzzle-piece avatar frames
Keynote clips speaker photos into a jagged custom SVG `clipPath`, with 3 rotating variants so a grid of avatars doesn't look uniform (`keynote-ts/src/components/Speakers.tsx`):
```tsx
function ImageClipPaths({ id, ...props }: React.ComponentPropsWithoutRef<'svg'> & { id: string }) {
  return (
    <svg aria-hidden="true" width={0} height={0} {...props}>
      <defs>
        <clipPath id={`${id}-0`} clipPathUnits="objectBoundingBox">
          <path d="M0,0 h0.729 v0.129 h0.121 l-0.016,0.032 C0.815,0.198,0.843,0.243,0.885,0.243 H1 v0.757 H0.271 v-0.086 l-0.121,0.057 v-0.214 c0,-0.032,-0.026,-0.057,-0.057,-0.057 H0 V0" />
        </clipPath>
        {/* -1 and -2 variants use mirrored/rotated versions of the same path */}
      </defs>
    </svg>
  )
}
```
Applied per-avatar so a grid of speakers doesn't repeat the same shape three times in a row:
```tsx
<img style={{ clipPath: `url(#${id}-${speakerIndex % 3})` }} {...} />
```

### Scattered Polaroid photo strip
Spotlight lays out 5 photos with alternating rotation for a scrapbook feel (`spotlight-ts/src/app/page.tsx`):
```tsx
function Photos() {
  let rotations = ['rotate-2', '-rotate-2', 'rotate-2', 'rotate-2', '-rotate-2']
  return (
    <div className="-my-4 flex justify-center gap-5 overflow-hidden py-4 sm:gap-8">
      {images.map((image, i) => (
        <div key={image.src} className={clsx('relative w-44 flex-none overflow-hidden rounded-xl bg-zinc-100 sm:w-72 sm:rounded-2xl', rotations[i % rotations.length])}>
          <div className="aspect-9/10">
            <Image src={image} alt="" className="absolute inset-0 h-full w-full object-cover" />
          </div>
        </div>
      ))}
    </div>
  )
}
```

### Native-video picture-in-picture with zero video library
Compass turns a playing, scrolled-off `<video>` into a floating bottom-right card using only an `IntersectionObserver` and CSS attribute selectors — no third-party player, no JS animation (`compass-ts/src/components/video-player.tsx`):
```tsx
export function Video({ className, ...props }: React.ComponentProps<'video'>) {
  let videoContainerRef = useRef<HTMLDivElement>(null)

  useEffect(() => {
    let videoContainer = videoContainerRef.current
    let observer = new IntersectionObserver(
      ([entry]) => {
        if (!entry.isIntersecting) videoContainer.setAttribute('data-offscreen', '')
        else videoContainer.removeAttribute('data-offscreen')
      },
      { threshold: 0.5 },
    )
    observer.observe(videoContainer)
    return () => observer.disconnect()
  }, [])

  return (
    <div ref={videoContainerRef} className="group aspect-video w-full rounded-2xl bg-gray-950">
      <video
        {...props}
        controls
        onPlay={(e) => e.currentTarget.setAttribute('data-playing', '')}
        onPause={(e) => {
          if (!videoContainerRef.current?.hasAttribute('data-offscreen')) e.currentTarget.removeAttribute('data-playing')
        }}
        className="aspect-video w-full rounded-2xl sm:group-data-offscreen:data-playing:fixed sm:group-data-offscreen:data-playing:right-4 sm:group-data-offscreen:data-playing:bottom-4 sm:group-data-offscreen:data-playing:z-10 sm:group-data-offscreen:data-playing:max-w-md sm:group-data-offscreen:data-playing:rounded-xl sm:group-data-offscreen:data-playing:shadow-lg"
      />
    </div>
  )
}
```

### Pure-CSS gradient-border hover
Syntax reveals a gradient card border on hover using layered backgrounds (padding-box content fill + border-box gradient) — no JS, no framer-motion (`syntax-ts/src/components/QuickLinks.tsx`):
```tsx
<div className="group relative rounded-xl border border-slate-200 dark:border-slate-800">
  <div className="absolute -inset-px rounded-xl border-2 border-transparent opacity-0 [background:linear-gradient(var(--quick-links-hover-bg,var(--color-sky-50)),var(--quick-links-hover-bg,var(--color-sky-50)))_padding-box,linear-gradient(to_top,var(--color-indigo-400),var(--color-cyan-400),var(--color-sky-500))_border-box] group-hover:opacity-100 dark:[--quick-links-hover-bg:var(--color-slate-800)]" />
  <div className="relative overflow-hidden rounded-xl p-6">{/* content */}</div>
</div>
```

### One CSS variable driving three layout behaviors
Oatmeal's `--scroll-padding-top` — see full code under Shape & spacing tokens above (anchor-scroll offset, sticky-nav height, sticky-table-header position, all from one property).

### Dual sun/moon icons with a pre-hydration fallback
Spotlight renders both icons and toggles visibility with `dark:hidden`/`dark:block`, plus a raw `prefers-color-scheme` media-query fallback so the correct icon shows even before React hydrates and sets the theme class — avoiding the classic dark-mode icon flash (`spotlight-ts/src/components/Header.tsx`):
```tsx
function ThemeToggle() {
  let { resolvedTheme, setTheme } = useTheme()
  let otherTheme = resolvedTheme === 'dark' ? 'light' : 'dark'
  let [mounted, setMounted] = useState(false)
  useEffect(() => setMounted(true), [])

  return (
    <button
      type="button"
      aria-label={mounted ? `Switch to ${otherTheme} theme` : 'Toggle theme'}
      onClick={() => setTheme(otherTheme)}
      className="group rounded-full bg-white/90 px-3 py-2 shadow-lg ring-1 ring-zinc-900/5 backdrop-blur-sm dark:bg-zinc-800/90"
    >
      <SunIcon className="h-6 w-6 dark:hidden [@media(prefers-color-scheme:dark)]:fill-teal-50 [@media(prefers-color-scheme:dark)]:stroke-teal-500" />
      <MoonIcon className="hidden h-6 w-6 not-[@media_(prefers-color-scheme:dark)]:fill-teal-400/10 not-[@media_(prefers-color-scheme:dark)]:stroke-teal-500 dark:block" />
    </button>
  )
}
```

## Accessibility patterns

- **`aria-label` on icon-only controls is close to universal** — nav toggles, theme toggles, social links, and play/pause buttons carry labels in nearly every template studied. Treat this as the baseline to match, not an extra. Spotlight's `ThemeToggle` above is a good example: the label itself changes based on mount state to avoid a hydration mismatch.
- **`useReducedMotion` is the exception, not the rule** — only Studio implements it (code above under Motion). If you're taking animation from any other template in this set, add the guard.
- **Three different accessibility architectures for interactive chrome**: most templates build dialogs/disclosures on Headless UI (`Dialog`, `Popover`, `Disclosure`) and manage ARIA state via React. Oatmeal delegates entirely to native HTML — a real `<dialog>` element plus `@tailwindplus/elements`' Commands API (`command="show-modal"`, `commandfor`) — relying on the browser's built-in focus-trapping and Escape-to-close instead of custom JS:
```tsx
<button command="show-modal" commandfor="mobile-menu" aria-label="Toggle menu">…</button>
<dialog id="mobile-menu" className="backdrop:bg-transparent">
  {/* panel content */}
</dialog>
```
Native delegation needs the least code but the least visual control; building on Headless UI needs more code but full custom visuals with correct semantics; react-aria/react-stately (used by Transmit's audio scrubber, not shown here) sits in between — full custom visuals, still correct semantics, more implementation work than either.
- Primer is the most consistent template about `aria-label`/`aria-labelledby` on sections and dynamic CTAs — worth using as the reference if auditing another project's labeling coverage.

## Picking a visual direction

- **Want restraint / let typography and whitespace carry the design** → studio's approach (monochrome, one variable font, disciplined scroll-reveal) or protocol's (no custom font, single accent color).
- **Want a warm, editorial, "crafted" feel** → radiant's gradient + hover-choreographed bento cards, or primer's heavier tinted shadows + large radii.
- **Want energetic, consumer-app motion** → pocket's patterns (marquees, spinning rings, morphing hover pill) — budget time to add the `useReducedMotion` guard it's missing.
- **Want a dark, ambient feel** → commit's star-field technique, or studio's `neutral-950` monochrome base.
- **Want a large, ready-made section library instead of designing from scratch** → oatmeal's 35 sections across 12 categories, plus 16 lower-level primitives.
- **Building a persistent-nav app shell (not a scrolling marketing page)** → compass's three-layout-shell approach (auth/sidebar/centered) is the clearest reference for making distinct chrome coexist under one root layout.

## Per-template visual identity, at a glance

| Template | One-line visual identity |
|---|---|
| radiant | Warm pink-gold-purple gradient everywhere, left-aligned hero, hover-choreographed bento cards |
| salient | Plain blue accent, hand-drawn squiggle underlines, real Inter+Lexend pairing, no motion |
| studio | Monochrome dark, one variable font (width axis for display), most disciplined/accessible motion |
| protocol | No custom font, emerald accent, teal-lime gradient hero pattern, scroll-linked header fade |
| syntax | Sky-blue docs aesthetic, fake code-editor hero, pure-CSS gradient-border card hover, zero motion library |
| compass | Grayscale + blue focus rings only, native `<video>` chrome, CSS-driven picture-in-picture |
| primer | Blue accent, heavy tinted shadows, huge radii, numbered scroll-spy nav, asymmetric bento hero |
| commit | Near-black + sky accent, persistent twinkling star field, variable-weight Mona Sans |
| transmit | Neutral slate, react-aria audio scrubber, static gradient waveform, no motion library |
| pocket | Achromatic gray + cyan accent, heaviest motion of the set (marquees, spinning rings, morphing hover pill) |
| keynote | Blue/indigo, Inter+DM Sans pairing, puzzle-piece avatar clips, zero motion |
| spotlight | Zinc + teal accent, no custom font, scroll-driven shrinking-avatar header, scattered-photo strip |
| oatmeal | Bespoke olive palette, Instrument Serif + Inter, zero animation (native-HTML interactivity only), largest section catalog |
