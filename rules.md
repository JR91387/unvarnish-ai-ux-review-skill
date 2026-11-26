# Unvarnish rules

These are suggestions, not law. Each line: **rule**: what to check → the usual fix.
Drawn from widely shared UI tips, vibe-coded-app fixes, UX laws, niche CSS hacks and practical design tips. *(interpreted)* = no single clear source; this is one reasonable reading.
Process items from the lists live in SKILL.md, not here: write a brief, give AI a reference, design system first, the Impeccable skill (`pbakaus/impeccable`, optional companion, not installed).

---

## Phase 1: obvious, mechanical, low taste
Do these first. Most are token-level or one-line CSS/HTML.

### Identity and basics
- **Tab title + favicon**: generic "Vite App"/"React App" title or default icon → real page title per route, real favicon.
- **Custom 404**: default server/framework 404 → branded page with a way back (search, home, main task).
- **Get your own domain**: `*.vercel.app` / `*.pages.dev` on something public → flag only; don't change it.

### Color and shape
- **Kill purple gradients**: default violet→pink/blue gradients, gradient text, glows → solid surfaces; gradients only where they mean something.
- **One accent color**: several competing brand colors → one accent for action and focus; the rest neutral.
- **One corner radius**: mixed radii on similar things → one radius scale (e.g. 6/12) applied by role.
- **Concentric radius**: nested rounded boxes look off → inner radius = outer radius − padding.
- **Alpha borders**: solid gray borders that clash on tinted or dark surfaces → borders in `rgb(0 0 0 / .1)` (or white/alpha in dark mode); image outlines the same.

### Spacing and layout
- **Use a spacing scale**: random px values → a 4/8-based scale used everywhere.
- **More space above headings**: heading equally spaced between sections → space above > space below (it belongs to what follows).
- **More padding on the sides**: horizontal padding equals vertical in buttons/chips → sides ≈ 2× top/bottom.
- **Safe area insets**: content under the notch/home bar → `env(safe-area-inset-*)` on fixed bars and the page.
- **Dynamic viewport**: `100vh` jumping on mobile → `100dvh` / `svh`.

### Type
- **Left-align long text**: centered paragraphs → left-align anything over two lines.
- **Keep body text regular**: body in medium/semibold → regular (400); weight is for hierarchy.
- **Tight headline line height**: big headings at 1.5 line height → 1.05–1.2.
- **Balanced headlines**: one-word orphan lines → `text-wrap: balance` on headings, `pretty` on paragraphs.
- **Use sentence case**: Title Case Or ALL CAPS TRACKED labels → sentence case.
- **Tabular numbers**: digits jitter in counters/tables → `font-variant-numeric: tabular-nums`.
- **Right-align numbers**: numeric columns left-aligned → right-aligned with matching decimals.

### Controls and forms
- **Real icons, not emojis**: emoji as UI icons → one icon set (Lucide, Phosphor, Heroicons, or what the app already uses).
- **Labels, not placeholders**: placeholder as the only label → visible label above; placeholder for an example only.
- **Say what buttons do**: "Submit", "OK", "Continue" → verb + object ("Save changes", "Send invite").
- **Keep Delete far from Save**: destructive next to primary → separate them, style as destructive, confirm or undo.
- **Hit slop**: small tap targets → ≥44×44 px (24 px minimum per WCAG 2.2) via padding or a pseudo-element.
- **One-time code autofill**: OTP field without hints → `autocomplete="one-time-code"`, `inputmode="numeric"`.

### States
- **Proper loading states / skeleton loaders**: blank screen or spinner on a whole page → skeletons shaped like the content.
- **Design empty states**: "No data" → say what goes here, why it's empty, and **add a CTA** for the first action.
- **Real error messages**: "Something went wrong" → what happened, what to do, keep the user's input.

### Accessibility floor
- Text contrast AA (4.5:1 body, 3:1 large and UI), a visible focus ring on everything focusable, keyboard reachable, `prefers-reduced-motion` respected, alt text on meaningful images.

---

## Phase 2: taste and behavior
These need judgment against the brief. Offer them; don't force them.

### 2a. Look
- **Fewer cards, more room**: everything boxed in cards → remove boxes; let space and alignment group things.
- **Font that isn't Inter**: default Inter/system everywhere → a typeface that fits the brief (one family, or a display + text pair).
- **Rewrite the copy**: hype ("Unlock", "Seamless", "Elevate", ✨ badges), filler taglines → plain, specific words about what it does.
- **Use real photos**: stock or AI images, blob illustrations → real product or people shots, or none.
- **Tint your grays blue**: dead neutral grays → grays with a slight hue (blue, or the brand hue).
- **One hue per color**: shades of "blue" drifting toward purple or teal → each palette ramp keeps one hue.
- **OKLCH palettes**: uneven-looking ramps from HSL → build ramps in `oklch()` for even lightness steps.
- **OKLCH gradients**: muddy gray middles in gradients → `linear-gradient(in oklch, …)`.
- **Tint text on color**: pure white/gray text on colored surfaces → a very light or dark tint of that surface color.
- **Layer your shadows**: one big blurry shadow → 2–3 stacked shadows (tight + soft), low opacity.
- **Dark mode depth**: shadows in dark mode → elevation via lighter surfaces.
- **Mute colors in dark mode**: full-saturation colors glowing on dark → lower chroma/lightness.
- **Matched icon weight**: icon stroke heavier/lighter than the text next to it → match stroke to font weight.
- **Continuous corners**: hard circular corners on large surfaces → squircle (`corner-shape: squircle` where supported, else SVG mask). Optional.
- **Masked edges**: scroll areas cut off hard → `mask-image` fade at overflow edges.
- **Multiply blending**: white-background product images on tinted surfaces → `mix-blend-mode: multiply`.
- **BlurHash placeholders**: images pop in from blank → BlurHash/ThumbHash or a dominant-color placeholder.
- **Align on the baseline**: mixed-size text in a row aligned by box → `align-items: baseline`.
- **Text fills half the button** *(interpreted)*: label looks tiny or cramped → label line height ≈ half the button height (e.g. 20 px line in a 40 px button).
- **Gap = small text's height** *(interpreted)*: icon-to-label and tight related gaps are random → gap ≈ the font size of the smaller text.
- **Bigger landing page CTAs**: primary CTA the same size as nav links → larger, high contrast, one per view.

### 2b. Feel (interaction and engineering)
- **Meaningful animations**: decorative bounce/pulse → motion only to show cause, change or place; 150–250 ms.
- **Interruptible animations**: animations that must finish before input works → CSS transitions or springs that retarget mid-flight.
- **View transitions**: hard cuts between related views → View Transitions API for shared elements.
- **Blurred icon swaps** *(interpreted)*: icon state changes hard-cut (copy→check) → crossfade with brief blur + scale.
- **Optimistic updates**: UI waits for the server on likes/toggles → update immediately, roll back on error.
- **Undo toasts**: confirm dialogs for reversible actions → act at once, offer Undo for ~5 s.
- **Idempotency keys**: double-click = double charge/post → disable on submit + idempotency key on the request.
- **Stale-while-revalidate**: spinner on every revisit → show cached data, refresh in the background.
- **Pre-rendering**: slow next-page loads on obvious paths → prefetch/prerender likely next pages (Speculation Rules, router prefetch).
- **Command menu**: power-user app with deep nav → ⌘K/Ctrl+K palette.
- **Shortcut hints**: shortcuts exist but are hidden → show them in tooltips and menus.
- **Amazon safe triangle**: hover submenus close when the mouse cuts diagonally → menu-aim / safe-triangle delay.
- **Swipe-to-dismiss sheets**: mobile bottom sheets that need a close button only → drag down to dismiss.
- **Predictive back**: Android back exits without a preview → support the predictive back gesture (Android 13+); on the web, keep history and state correct.

---

## Phase 3: laws and abstract considerations
Use these to judge layout, flow and priorities. Each one: what to look for.

- **Hick's law**: too many choices at once slows decisions → cut options, group them, reveal more on demand.
- **Fitts's law**: important targets small or far → make them big and close to where the pointer or thumb already is.
- **Minimize target distance**: steps that bounce the eye or cursor across the screen → put the next action next to the last one.
- **Jakob's law**: invented patterns for common jobs → use conventions people know from other products.
- **Law of proximity**: related things far apart, unrelated things close → spacing shows grouping.
- **Law of similarity**: things that look alike but act differently (or the reverse) → same look = same behavior.
- **Uniform connectedness**: grouping only by color or space where a container would be clearer → a shared container or line for strong groups.
- **Law of Prägnanz**: complex shapes and layouts → simplest form that reads at a glance.
- **Miller's law**: long unchunked lists or codes → chunk into groups (about 4–7).
- **Serial position effect**: key items buried in the middle → most important first and last.
- **Von Restorff effect**: everything emphasized → one thing stands out: the primary action.
- **Doherty threshold**: responses >400 ms with no feedback → feedback within 400 ms (skeleton, optimistic update).
- **Peak-end rule**: a flat or bad ending (after save, checkout, onboarding) → design the peak moment and the ending.
- **Zeigarnik effect**: unfinished tasks forgotten → show progress and "continue where you left off".
- **Tesler's law**: complexity pushed onto the user → the system absorbs it (smart defaults, inference).
- **Postel's law**: strict input formats (phones, dates) → accept loose input, output clean and consistent.
- **Parkinson's law**: open-ended tasks drag → set scope and time cues (autofill, defaults, short forms).
- **Occam's razor**: features or steps that don't earn their place → remove them.
- **Pareto principle**: polish spread evenly → spend effort on the 20% of screens and flows used 80% of the time.
