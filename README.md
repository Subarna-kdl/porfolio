<div align="center">

<img src="images/logo.png" alt="Subarna logo" width="140">

# Subarna's Portfolio

**A dark, pink-accented personal portfolio site, built from scratch with plain HTML, CSS and a little JavaScript.**

[https://clanky.me/porfolio/]() · [School website I built](https://gssjs.netlify.app/) · [GitHub](https://github.com/Subarna-kdl)

</div>

---

## What is this?

I'm Subarna Kandel, a class 12 student at Gorkha Secondary School in Tulsipur, Nepal. This is my personal portfolio website. I wanted one place to show who I am, what I can build, and how people can contact me.

The page has five sections:

| Section | What it does |
|---------|--------------|
| **Home** | Big hero text with my name and "Full Stack Developer" |
| **About** | My photo, a short intro, and skill bars (HTML, CSS, JavaScript, Python) |
| **Services** | Three cards: Web Design, UI/UX Design, App Design |
| **Portfolio** | My school website project, with a hover overlay and link |
| **Contact** | Email, social icons, and a working contact form |

---

## Features

- **Dark theme** (`#080808`) with a hot-pink accent (`#ff004f`)
- **Animated nav links**: a pink underline grows in when you hover
- **Smooth scrolling** between sections (`scroll-behavior: smooth`)
- **Responsive layout**: on screens under 600px the navbar becomes a slide-in side menu (hamburger icon)
- **Hover effects**: service cards lift up and turn pink; portfolio images zoom and show an overlay
- **Back-to-top button** that fades in after you scroll 100px
- **Contact form that saves messages to Google Sheets** using Google Apps Script, so I don't need my own backend
- **Font Awesome icons** for services and social links

---

## Built with

- **HTML5** for the structure
- **CSS3** with Flexbox, Grid, transitions and media queries
- **Vanilla JavaScript** for the mobile menu, the scroll-to-top button and the form
- **Font Awesome** for icons
- **Google Apps Script + Google Sheets** as the contact form "backend"

---

## Project structure

```
porfolio/
├── index.html      # all the page content + the JavaScript
├── style.css       # all the styling
└── images/
    ├── logo.png    # navbar logo
    ├── logo2.png   # favicon
    ├── subarna.png # my photo in the About section
    └── scl.jpg     # screenshot of the school website project
```

---

## How to run it

No build step, no install.

```bash
git clone https://github.com/Subarna-kdl/porfolio.git
cd porfolio
```

Then open `index.html` in your browser. (Or use the Live Server extension in VS Code.)

> The contact form needs internet access, because it sends data to a Google Apps Script URL.

---

## How the cool parts work

### Mobile side menu
On small screens the `<ul>` menu is positioned `fixed` and pushed off the right edge with `right: -200px`. The hamburger icon calls `openmenu()`, which sets `right: 0`, and the X icon calls `closemenu()` to push it back. A CSS `transition: right 0.5s` makes it slide.

### Contact form → Google Sheets
The form's submit event is intercepted with `e.preventDefault()`. Then `fetch()` sends the form data to a Google Apps Script web app, which appends a row to a Google Sheet. After it succeeds, the page shows "Message sent successfully" for 5 seconds and clears the form.

### Skill bars
Each skill is a `.progress` box with an inner `<span>` whose CSS `width` is the percentage (HTML 90%, CSS 80%, JavaScript 35%, Python 69%).

### Back-to-top button
A scroll listener checks `window.pageYOffset > 100` and toggles an `active` class that changes the button from invisible (`opacity: 0`) to visible.

---

## Devlog

> Draft notes on the problems I ran into and how I solved them. Each one is based on how the code in this repo actually works.

### 1. The hamburger menu had to disappear on desktop
- **What went wrong:** I put two Font Awesome icons in the navbar, a hamburger (`fa-bars`) and an X (`fa-times`). On a laptop they showed up next to the normal links, which looked broken.
- **How I fixed it:** Both icons use the class `fas`, so I hid them everywhere with `nav .fas { display: none; }` and only turned them back on inside `@media (max-width: 600px)`. The X has its own rule that pins it to the top-left corner of the menu panel.

### 2. Making the menu slide in from the side
- **What went wrong:** On a phone the five links took up the whole header and pushed the hero text down.
- **How I fixed it:** In the media query I made the `<ul>` `position: fixed`, 200px wide, full height, and moved it off screen with `right: -200px`. Two tiny JavaScript functions change that to `right: 0` (`openmenu()`) and back (`closemenu()`). Adding `transition: right 0.5s` made it slide instead of jump.

### 3. Getting the contact form to work without a backend
- **What went wrong:** A plain HTML form needs a server to receive the data, and I don't have one.
- **How I fixed it:** I used a Google Apps Script web app connected to a Google Sheet. In JavaScript I stop the normal submit with `e.preventDefault()`, send the data with `fetch()` and `FormData`, then show "Message sent successfully" for 5 seconds with `setTimeout` and clear the form with `form.reset()`.
- **Still open:** If the request fails, the visitor sees nothing. The error only goes to the console. I want to show an error message on the page.

### 4. The hover overlay on my project image
- **What went wrong:** I wanted a pink panel to rise up over the school website screenshot, but it showed up in the wrong place or covered other content.
- **How I fixed it:** The overlay (`.layer`) is `position: absolute` with `height: 0`, so it only lines up properly because its parent `.work` is `position: relative`. On `.work:hover`, the height changes to `100%`, and `overflow: hidden` keeps it inside the rounded corners. The image zooms with `transform: scale(1.1)` at the same time.

### 5. Skill bars
- **What went wrong:** Each bar needs a different width, and I didn't know a clean way to do that.
- **How I fixed it:** For now I made four copies of the same CSS (`.language`, `.language2`, `.language3`, `.language4`), each with its own `width`. It works, but it's a lot of repeated code.
- **Next time:** Use one `.skill` class and set the width with an inline CSS variable like `style="--width: 90%"`.

### 6. A back-to-top button that doesn't get in the way
- **What went wrong:** If the button is always visible, it sits on top of the hero section. If I just make it transparent, it can still be clicked by accident.
- **How I fixed it:** The button starts with `opacity: 0` and `pointer-events: none`. A scroll listener adds an `active` class once `window.pageYOffset` is above 100, which makes it visible, clickable, and moves it up slightly.

### 7. Bugs I found when I reread my own code
- The "Skills" tab calls `opentab()`, but I never wrote that function. I copied the tab layout and only kept one tab, so clicking it causes an error.
- My Services and Portfolio headings use `sub-titile` in the HTML but `.sub-title` in the CSS. One typo meant the style never applied. I now check class names against the CSS when something looks unstyled.
- I asked for the Poppins font but never imported it, so the site uses the browser's default font.

### What I learned
- Flexbox and Grid make layouts much easier than I expected.
- `position: relative` on a parent is what makes `position: absolute` children behave.
- Small typos in class names cause big "why isn't my CSS working?" moments. Opening DevTools and checking whether the rule is applied saves time.
- A form can work without writing a backend if I connect it to a service like Google Sheets.
---

## Known issues / things I want to fix next

These are real bugs I found while reviewing my own code:

- [ ] **Skills tab:** the "Skills" tab calls `opentab('skills')`, but that function isn't defined yet, so clicking it throws a JavaScript error.
- [ ] **Section headings:** the Services and Portfolio headings use `class="sub-titile"` in the HTML, but the CSS is written for `.sub-title`, so they don't get the big heading style.
- [ ] **Font:** the CSS asks for `Poppins`, but I never import it (no Google Fonts link), so the browser falls back to a default sans-serif.
- [ ] **Favicon:** the `<link>` uses `rel="Website icon"`; it should be `rel="icon"`.
- [ ] **HTML structure:** there are extra closing `</div>` tags near the Portfolio section.
- [ ] **CSS typo:** `border-radius: 505` on the logo is missing a unit (`px`).
- [ ] **Skill-bar CSS:** the four skill bars repeat nearly the same CSS block. I could use one class and an inline `--width` variable.
- [ ] **Typos in text:** "studen", "Sercives", "ebsites", "tailores", "friendxly".
- [ ] **GitHub icon:** its color (`#222`) is very dark on a white circle, and the Services "Learn More" links go nowhere (`href="#"`).
- [ ] Add more projects to the Portfolio section.

---

## Credits

- Icons: [Font Awesome](https://fontawesome.com/)
- Contact form idea: Google Apps Script → Google Sheets
- Everything else: me 🙂

---

## Contact

- GitHub: [@Subarna-kdl](https://github.com/Subarna-kdl)
- Email: via the contact form on the site


