# Development plan: Syntaxwear home page

A sneaker e-commerce site built **only with HTML (HyperText Markup Language) and CSS (Cascading Style Sheets)**, using a **mobile-first** approach. JS (JavaScript) comes in a later phase.

This plan was built from the description of the 5 sections. Adjust colors, fonts and proportions by looking at the mockup. Anything already done in the repository is marked with ✅.

> The text shown to visitors (menu links, titles, buttons) stays in Portuguese, because the site is for Brazilian customers. Code, class names, file names and comments are written in English.

---

## 1. Folder and file structure

```
ecommerce-syntaxwear/
├── index.html            ✅ single page
├── README.md             ✅
├── PLAN.md               ✅ this plan
├── css/
│   ├── reset.css         ✅ (empty) removes browser default styles
│   ├── tokens.css        ✅ (empty) variables: colors, fonts, spacing
│   ├── style.css         ✅ (empty) phone styles (the base)
│   └── responsive.css    ✅ (empty) tablet and desktop adjustments
└── assets/
    ├── img/              ✅ product photos, lifestyle photos, hero background
    ├── icons/            ✅ logo and SVG icons
    ├── video/            ✅ video for the grid card
    └── fonts/            (optional) only if you host the fonts yourself
```

**Why split it this way:** each file has a single job. When something breaks, you know where to look.

**Important convention for mobile-first:**
- `style.css` holds **only the phone layout**.
- `responsive.css` holds **only `min-width` media queries** ("from X pixels wide and up, do this").

That way the phone is the base, and larger screens only add or change rules.

**Suggested breakpoints:**
- **Tablet:** from 768px.
- **Desktop:** from 1024px.
- **Wide screens (optional):** from 1280px.

**Image names:** use lowercase, no accents and no spaces, for example `casual-sneaker-white.webp`. Spaces and accents break links on some servers.

---

## 2. Preparation phase

### 2.1 CSS reset (`reset.css`)
**What it is:** each browser applies its own margins, heading sizes and list styles. The reset removes those differences so you start from zero.

**What to include:**
- **`box-sizing: border-box` on every element.** It makes a box's width include its padding and border. Without it, a 300px box with 20px of padding takes up 340px, and that kind of mistake is what breaks layouts.
- **Remove the default margins** of `body`, headings, paragraphs and lists.
- **Remove the bullets from lists** used as menus.
- **Images and videos with `display: block` and `max-width: 100%`.** That way they are never wider than their container.
- **Buttons and form fields inheriting the page font.** By default they don't.

**Done when:** the unstyled page looks "raw", with no odd margins and with images fitting the screen.

### 2.2 CSS variables (`tokens.css`)
**What it is:** variables (also called *custom properties*) are names that store values. If the brand's black changes, you change it in one place and the whole page updates.

**Define them in the `:root` selector:**
- **Colors:** background, main text, secondary text, brand accent color, button and hover colors, and border color. Take the exact values from the mockup.
- **Fonts:** the heading family, the body text family and the weights used (for example, 400 and 700).
- **Font sizes:** a scale of 5 or 6 sizes, from small to extra large, in **rem**. The rem is relative to the font size the user chose in their browser, so it respects people who need larger text.
- **Spacing:** a fixed scale, such as 4, 8, 16, 24, 32, 48 and 64px, converted to rem. Always using the same values makes the layout look consistent.
- **Others:** border radius, maximum content width (for example, 1200px) and transition duration.

**Done when:** no color, font or spacing value appears "loose" in the other CSS files. Everything uses the variables.

### 2.3 Choosing and loading fonts
- Find out which fonts the mockup uses. In Figma, just click on the text.
- **Use at most two families** (one for headings and one for body text) and **few weights**. Each weight is one more file to download.
- Load them from Google Fonts, placing the `<link>` in the `<head>` **before** your CSS files. Include `display=swap` in the URL: with it, the text shows up in a fallback font while the real font loads, instead of staying invisible.
- Always list a generic fallback font at the end, such as `sans-serif`.

### 2.4 Global base (start of `style.css`)
- Set the `body` font, color and background using the variables.
- Create a **container** class: maximum width, centered, with side padding. It keeps content from touching the edge of the phone screen or stretching too wide on a monitor.
- Create a **"screen reader only"** utility class (usually called `visually-hidden`). It hides text visually but keeps it available to screen reader users. You will use it on the icons.
- Pick a naming pattern for classes. Suggestion: **BEM (Block Element Modifier)**, for example `card`, `card__title` and `card--featured`. The name itself tells you where the class belongs.

**Done when:** the page text already shows the right font, color and background.

---

## 3. Section phases

### Phase 3.1: Menu

**Goal:** the top bar with the logo, the 5 links (Masculino, Feminino, Outlet, Nossas lojas, Sobre) and the account, help and cart icons.

**Semantic tags:**
- `header` for the whole bar.
- `nav` with an `aria-label`, for example "Menu principal". ARIA stands for *Accessible Rich Internet Applications*: attributes that give extra information to screen readers.
- `ul`, `li` and `a` for the links. A menu is a list of links.
- The logo should be an `a` pointing to the home page, with an `img` inside. The `img` alternative text should be "Syntaxwear – página inicial".
- The icons should also be `a` elements (or `button` in the JS phase). Use SVG (*Scalable Vector Graphics*, vector images that don't lose quality when scaled) plus hidden text with the `visually-hidden` class, such as "Carrinho". An icon alone has no name for a screen reader.

> ⚠️ `index.html` currently has `<h1>SyntaxWear</h1>` inside the `header`. The `h1` should be the page's main title, which here is the hero title. Replace the logo with an `a` containing an `img` and save the `h1` for phase 3.2.

**Layout technique: Flexbox.**
- **Why:** the menu is a single row (one dimension) with three groups: logo, links and icons. Flexbox is made for distributing items in one direction.
- Use `justify-content: space-between` to push the groups to the edges and `align-items: center` to align them vertically.
- Use `gap` to space the links instead of margins.

**Responsive behavior:**
- **Phone:** show the logo and the icons. The 5 links don't fit side by side, so they go inside a **hamburger menu**. There is an HTML-only solution: the `details` and `summary` elements. The `summary` is the "open menu" button, and the rest of the content appears when it's clicked. It already works with the keyboard and screen readers, with no JS at all. The three-line icon goes inside the `summary`.
  - *Alternative:* the "hidden checkbox with a `label`" trick also works, but it's worse for accessibility. Prefer `details`.
  - *JS phase:* close the menu when clicking outside it and animate the opening.
- **Desktop (from 1024px):** hide the `summary` and show the links in a row, between the logo and the icons.
- **Optional:** `position: sticky` with `top: 0` keeps the menu stuck to the top while scrolling, with CSS only.

**Done when…**
- [ ] the logo links to the home page and has alternative text;
- [ ] on the phone, the menu opens and closes through `details`, including with Tab and Enter;
- [ ] on desktop, the 5 links appear in a row;
- [ ] every icon has a name that the screen reader can read;
- [ ] nothing overflows the screen width at 320px.

---

### Phase 3.2: Hero

**Goal:** a large area with a background photo, the title "Transforme qualquer passo em presença." and the "Ver modelos" and "Comprar" buttons.

**Semantic tags:**
- `section` with an `aria-labelledby` pointing to the title.
- `h1` with the title. It's the **only** `h1` on the page.
- `a` for the buttons, because they lead to other pages. Use `button` only for actions on the same page, such as submitting a form.

**Layout technique: column Flexbox.**
- **Why:** the content is a stack (title on top, buttons below) that needs to be aligned inside a tall box. Flexbox with `flex-direction: column` solves it in a few lines.
- The two buttons sit side by side with Flexbox and `gap`.
- **Background image:** use `background-image` with `background-size: cover` (the image covers the whole area without distortion) and `background-position` to choose which part of the photo stays visible.
- **Contrast:** place a dark gradient over the photo, in the same `background-image` property. Without it, white text disappears over the light parts of the photo.
- **Height:** use a minimum height (`min-height`), never a fixed height. With a fixed height, the text overflows when the screen is narrow and the title wraps onto several lines.

**Responsive behavior:**
- **Phone:** smaller text and buttons stacked, each at 100% width. Wide buttons are easier to tap with a finger.
- **Tablet and desktop:** larger title and buttons side by side. The text is aligned where the mockup shows.
- **Optional:** use `clamp()` on the title size so it grows gradually with the screen, without a media query.
- **Optional:** a smaller version of the background photo for phones, swapped in the media query. It saves mobile data.

**Done when…**
- [ ] the title is readable over any part of the photo;
- [ ] the buttons have a touch area of at least 44 × 44px;
- [ ] the buttons change appearance on hover and on keyboard focus;
- [ ] the section doesn't cut off the text at any width.

---

### Phase 3.3: Featured (4 category cards)

**Goal:** 4 cards (Casual, Esporte, Moderno and Futurista), each with an image and a name, linking to the category.

**Semantic tags:**
- `section` with an `h2` title. If the mockup doesn't show a title, use an `h2` hidden with `visually-hidden`, because every section needs a title for screen readers.
- `ul` and `li` for the cards, since it's a list of 4 items.
- Each card is an `a` wrapping an `img` and an `h3` with the category name. That way the whole card is clickable.

**Layout technique: CSS Grid.**
- **Why:** the cards form columns and rows that need to be the same size. Grid controls both directions at once.
- Use `aspect-ratio` on the images (for example, 3/4) with `object-fit: cover`. That way all cards keep the same proportions, even if the photos have different sizes.

**Responsive behavior:**
- **Phone:** 2 columns (a 2 × 2 grid). An alternative is a row that scrolls sideways, with `overflow-x: auto` and `scroll-snap-type`. *Scroll snap* makes each card "snap" into place while scrolling, like a carousel, with CSS only. Pick whichever is closest to the mockup.
- **Tablet:** 2 larger columns, or 4 if they fit.
- **Desktop:** 4 columns in a single row.

**Done when…**
- [ ] the 4 cards have the same height and the same image proportions;
- [ ] the whole card is clickable and shows a visible focus;
- [ ] each image has alternative text, or `alt=""` if the written name already describes the image (see phase 4).

---

### Phase 3.4: Bento grid

**Goal:** a grid of blocks in different sizes ("bento box" style) mixing product photos, lifestyle photos and a video card.

**Semantic tags:**
- `section` with an `h2`.
- `figure` for each image block, with a `figcaption` if there is a visible caption.
- Product blocks that lead to a page are `a` elements.
- `video` for the video card.

**Layout technique: CSS Grid with `grid-template-areas`.**
- **Why:** in a bento layout, some blocks span 2 columns or 2 rows. `grid-template-areas` lets you "draw" the layout with names, like a map. To change the layout for each screen size, you just redraw the map in the media query, without touching the HTML.
- Give each block an area name (for example, `video`, `product1`, `lifestyle1`) with `grid-area`.
- Images fill their block with `width: 100%`, `height: 100%` and `object-fit: cover`.

**Video without JS:**
- The `video` element works with HTML only, using the `controls` (shows the browser's play and pause buttons), `muted`, `playsinline` (on iPhone, plays inside the page instead of going full screen) and `poster` (image shown before playing) attributes.
- If the mockup shows the video playing on its own, there are `autoplay` and `loop` attributes. But automatically moving video bothers some people and can't be paused without controls. Recommendation: use `controls` for now and leave any custom play button, and the logic to respect the "reduce motion" preference, for the **JS phase**.
- Compress the video (a few MB, MP4 format). It's the heaviest file on the page.

**Responsive behavior:**
- **Phone:** 1 column, or 2 columns with some blocks spanning the full width. Blocks stack on top of each other.
- **Tablet:** 2 columns, with the video spanning both.
- **Desktop:** the mockup layout, for example 4 columns with some blocks spanning 2 × 2.
- Use `grid-auto-rows` with a fixed size or with `minmax()`. That way rows have a predictable height and blocks don't get squashed.

**Done when…**
- [ ] the desktop layout matches the mockup;
- [ ] on the phone, nothing is squeezed and there's no sideways scrolling;
- [ ] the video plays and pauses with its controls and has a `poster`;
- [ ] images below the first screen use `loading="lazy"`, so they only load when the user scrolls to them.

---

### Phase 3.5: Footer

**Goal:** newsletter, social media, link columns and copyright.

**Semantic tags:**
- `footer` wrapping everything.
- **Newsletter:** a `form` with a `label` tied to an `input type="email"` and a `button type="submit"`.
  - `type="email"` plus the `required` attribute make the browser validate the email by itself, with no JS. On phones, it also opens a keyboard with "@".
  - Don't use only the `placeholder` as a label, because it disappears as soon as the person starts typing. If the mockup shows no label, use a `label` with the `visually-hidden` class.
  - Actually sending the form belongs to the **JS or back-end phase**.
- **Social media:** a `ul` of icon links, each with a hidden name ("Instagram da Syntaxwear").
- **Link columns:** each column is a `nav` with an `aria-label`, or a `div` with a heading (`h2` or `h3`) and a `ul`.
- **Copyright:** a `p` with the `small` tag.

**Layout technique:**
- **CSS Grid for the link columns,** because they form a grid that changes its number of columns with the screen.
- **Flexbox for the newsletter row** (field and button) and for the social icons, because both are a single row.

**Responsive behavior:**
- **Phone:** everything stacked (newsletter, social, columns and copyright), with the link columns in 1 or 2 columns.
  - *Optional:* each column can become a `details` and `summary` (an "accordion" that opens and closes), with HTML only.
- **Tablet:** link columns in 2 or 3 columns.
- **Desktop:** newsletter on one side and columns on the other, as in the mockup.

**Done when…**
- [ ] the email field has a `label` and rejects an invalid email on submit;
- [ ] all social icons have a name;
- [ ] the footer links have enough contrast, which is often a problem in dark footers with gray text.

---

## 4. Final phase: quality

### 4.1 Accessibility
Follow WCAG (*Web Content Accessibility Guidelines*), the international guidelines for web accessibility. The main points for this project:

- **Alternative text (`alt`) on images:**
  - For a product photo, describe what matters: "Tênis casual branco com sola bege".
  - For a decorative image, or one that repeats the text next to it, use `alt=""` (empty). The screen reader then skips the image.
  - Never use "imagem" or "foto" as alternative text. The screen reader already announces that it's an image.
- **Contrast:** normal text needs a ratio of at least **4.5:1** against its background, and large text **3:1**. Test it with the WebAIM Contrast Checker or the browser DevTools.
- **Visible focus:** navigate the page using only the Tab key. You must always see where you are. **Never remove the `outline`** without adding another focus style in its place. Use `:focus-visible` to show focus only to keyboard users.
- **Headings:**
  - One `h1`, then `h2` for sections and `h3` inside them, without skipping levels.
  - Test with the HeadingsMap extension.
- **Language:** `lang="pt-BR"` ✅ is already in the HTML.
- **Motion:** if you use transitions, turn them off inside the `prefers-reduced-motion: reduce` media query. It respects people who set their system to reduce animations.

### 4.2 Testing on different screen sizes
- Use the **DevTools responsive mode** (F12, then the phone icon). Test at widths **320, 375, 768, 1024, 1280 and 1440px**.
- At each width, check:
  - whether a **horizontal scrollbar** appears (that's always a bug);
  - whether any text is cut off;
  - whether any image is distorted.
- **200% zoom:** the page must still be usable.
- **Real phone:** if you can, open the page on your phone. With the Live Server extension for VS Code, you can reach it through your computer's IP address on the same Wi-Fi network.
- Open the page in at least two browsers, such as Chrome and Firefox.

### 4.3 W3C Validator
- W3C (*World Wide Web Consortium*) is the organization that sets web standards.
- Validate the HTML at validator.w3.org, in the "Validate by File Upload" tab.
- Validate the CSS at jigsaw.w3.org/css-validator.
- Fix every **error**. Read each **warning** and understand it, because not every warning needs fixing.

### 4.4 Performance (bonus)
- Convert photos to **WebP**, a format lighter than JPG and PNG at the same quality.
- Resize photos to the size they actually appear at. Don't serve a 4000px photo in a 300px card.
- Set `width` and `height` on every `img`. The browser then reserves the space before the image loads, and the content doesn't "jump" while loading. That jump is called CLS (*Cumulative Layout Shift*).
- Run **Lighthouse**, found in DevTools under the Lighthouse tab. It scores accessibility and performance.

---

## 5. Common beginner mistakes and how to avoid them

| Mistake | Why it's bad | How to avoid it |
|---|---|---|
| Starting with desktop and "squeezing" it later | Leads to dozens of fixes and media queries fighting each other | Build each section at 375px first, and only then open `responsive.css` |
| Fixed height (`height`) on boxes with text | Text overflows when the screen gets narrower | Use `min-height` or let the height be automatic |
| Everything in `px` | Ignores the font size the user chose | Use `rem` for fonts and spacing |
| `div` for everything | Screen readers and Google can't understand the page | Ask "what is this?" and pick the tag by its meaning |
| Several `h1`s or skipped levels | Breaks the page structure for people navigating by headings | One `h1`, then `h2` and `h3` in order |
| Picking a tag by its visual size | `h3` isn't a "small heading", it's a "subheading of an `h2`" | Pick the tag by hierarchy and the size with CSS |
| Icon without a name | The screen reader only says "link" | Use text with `visually-hidden` or an `aria-label` |
| Removing the focus outline | Keyboard users get lost on the page | Style `:focus-visible` instead of removing it |
| Using `width: 100vw` | Creates a horizontal scrollbar, because `100vw` includes the scrollbar width | Use `width: 100%` |
| Positioning with `position: absolute` or "magic" margins | Breaks at any other screen size | Use Flexbox, Grid and `gap` for layout. Keep `absolute` for small overlays only, such as a badge |
| Loose colors and spacing in the CSS | The design becomes inconsistent and hard to change | Always use the variables from `tokens.css` |
| Using `!important` to "win" a style fight | Snowballs into more `!important` | Understand specificity and prefer simple classes, with no IDs in the CSS |
| Huge images | The page gets slow, especially on 4G | Resize photos, use WebP and `loading="lazy"` |
| Testing only on your own monitor | Bugs only show up on the user's phone | Test at the phase 4.2 widths after each finished section, not just at the end |
| Huge commits at the end | Hard to go back if something breaks | Commit after each finished step, for example "Add header markup" and then "Style header for mobile" |

---

## Summary of the execution order
1. Preparation: reset, tokens, fonts and global base.
2. For each section, in order (Menu, Hero, Featured, Grid and Footer):
   1. Write the semantic HTML, without CSS, and check that the "raw" page makes sense.
   2. Write the phone CSS in `style.css`.
   3. Write the tablet and desktop CSS in `responsive.css`.
   4. Go through the "done when…" checklist and commit.
3. Final phase: accessibility, testing, validation and performance.
