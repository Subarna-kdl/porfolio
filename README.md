<div align="center">

<img src="images/logo.png" alt="Subarna logo" width="140">

# Subarna's Portfolio

**A dark, pink-accented personal portfolio built from scratch with plain HTML, CSS and a little JavaScript.**

![HTML5](https://img.shields.io/badge/HTML5-e34f26?style=flat-square&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572b6?style=flat-square&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-f7df1e?style=flat-square&logo=javascript&logoColor=black)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34a853?style=flat-square&logo=googlesheets&logoColor=white)

[**Live site**](https://clanky.me/porfolio/) · [School website I built](https://gssjs.netlify.app/) · [GitHub](https://github.com/Subarna-kdl)

</div>

<!--
  SCREENSHOTS: on GitHub, edit this file and paste images straight in here.
  Good ones to add: the hero section, a services card on hover,
  the portfolio overlay, and the mobile side menu.
-->

---

## Contents

[About](#about-this-project) · [Features](#features) · [Built with](#built-with) · [Run it](#run-it-locally) · [How it works](#how-the-tricky-parts-work) · [Devlog](#devlog) · [Known issues](#known-issues) · [Credits](#credits)

---

## About this project

I'm Subarna Kandel, a student at Gorkha Secondary School in Tulsipur, Nepal. I built this portfolio to have one place that shows who I am, what I can make, and how to reach me.

The page is a single scrolling site with five sections:

| Section | What it does |
|---------|--------------|
| **Home** | Big hero text with my name and "Full Stack Developer" |
| **About** | My photo, a short intro, and skill bars for HTML, CSS, JavaScript and Python |
| **Services** | Three cards: Web Design, UI/UX Design, App Design |
| **Portfolio** | My school website project, with a hover overlay and a link to it |
| **Contact** | Email, social icons, and a working contact form |

---

## Features

- **Dark theme** (`#080808`) with a hot-pink accent (`#ff004f`)
- **Animated nav links:** a pink underline grows in when you hover
- **Smooth scrolling** between sections
- **Responsive layout:** under 600px the navbar turns into a slide-in side menu
- **Hover effects:** service cards lift and turn pink, and portfolio images zoom and reveal an overlay
- **Back-to-top button** that fades in after you scroll down
- **Contact form that saves to Google Sheets** through Google Apps Script, so I didn't need my own backend
- **Font Awesome icons** for services and social links

---

## Built with

| Tool | Used for |
|------|----------|
| HTML5 | Page structure |
| CSS3 | Flexbox, Grid, transitions, media queries |
| Vanilla JavaScript | Mobile menu, back-to-top button, form submit |
| Font Awesome | Icons |
| Google Apps Script + Google Sheets | The contact form "backend" |

<details>
<summary><b>Project structure</b></summary>

```
porfolio/
├── index.html      # page content + JavaScript
├── style.css       # all the styling
└── images/
    ├── logo.png    # navbar logo
    ├── logo2.png   # favicon
    ├── subarna.png # my photo in the About section
    └── scl.jpg     # screenshot of the school website project
```

</details>

---

## Run it locally

There is no build step and nothing to install.

```bash
git clone https://github.com/Subarna-kdl/porfolio.git
cd porfolio
```

Open `index.html` in your browser, or use the Live Server extension in VS Code.

> The contact form needs an internet connection because it sends data to a Google Apps Script URL.

---

## How the tricky parts work

<details>
<summary><b>Mobile side menu</b></summary>

<br>

On small screens the menu `<ul>` is `position: fixed` and pushed off the right edge with `right: -200px`. The hamburger icon calls `openmenu()`, which sets `right: 0`. The X icon calls `closemenu()` to push it back. `transition: right 0.5s` makes it slide instead of jump.

</details>

<details>
<summary><b>Contact form → Google Sheets</b></summary>

<br>

The form's submit event is intercepted with `e.preventDefault()`. Then `fetch()` sends the form data to a Google Apps Script web app, which adds a row to a Google Sheet. On success the page shows "Message sent successfully" for 5 seconds and clears the form.

</details>

<details>
<summary><b>Skill bars</b></summary>

<br>

Each skill is a `.progress` box with an inner `<span>` whose CSS `width` is the percentage: HTML 90%, CSS 80%, Python 69%, JavaScript 35%.

</details>

<details>
<summary><b>Back-to-top button</b></summary>

<br>

A scroll listener checks `window.pageYOffset > 100` and toggles an `active` class. That class changes the button from invisible (`opacity: 0`) to visible.

</details>

---

## Devlog

Problems I ran into while building this, and how I solved them.

### 1. The hamburger menu showed up on desktop

**Problem:** I added two Font Awesome icons to the navbar, a hamburger (`fa-bars`) and an X (`fa-times`). On a laptop they appeared next to the normal links, which looked broken.

**Fix:** Both icons share the class `fas`, so I hid them everywhere with `nav .fas { display: none; }` and only turned them back on inside `@media (max-width: 600px)`. The X gets its own rule that pins it to the top-left corner of the menu panel.

### 2. Making the menu slide in from the side

**Problem:** On a phone, five links took up the whole header and pushed the hero text down.

**Fix:** In the media query, the `<ul>` became `position: fixed`, 200px wide, full height, and sits off-screen at `right: -200px`. Two small JavaScript functions move it to `right: 0` and back. Adding `transition: right 0.5s` made it slide.

### 3. A contact form with no backend

**Problem:** A normal HTML form needs a server to receive the data, and I don't have one.

**Fix:** I connected the form to a Google Apps Script web app that writes into a Google Sheet. In JavaScript I stop the normal submit, send the data with `fetch()` and `FormData`, show a success message with `setTimeout`, then call `form.reset()`.

**Still open:** If the request fails, the visitor sees nothing, because the error only goes to the console. I want to show an error message on the page.

### 4. The hover overlay on my project image

**Problem:** I wanted a pink panel to rise over the school website screenshot, but it appeared in the wrong place and covered other content.

**Fix:** The overlay (`.layer`) is `position: absolute` with `height: 0`. It only lines up because its parent `.work` is `position: relative`. On `.work:hover` the height becomes `100%`, and `overflow: hidden` keeps it inside the rounded corners. The image zooms with `transform: scale(1.1)` at the same time.

### 5. Skill bars with different widths

**Problem:** Every bar needs a different width, and I didn't know a clean way to do that.

**Fix (for now):** I made four copies of the same CSS (`.language` to `.language4`), each with its own width. It works, but it's a lot of repeated code.

**Next time:** One `.skill` class, with the width set by an inline CSS variable like `style="--width: 90%"`.

### 6. A back-to-top button that stays out of the way

**Problem:** An always-visible button sits on top of the hero section. A button that is only transparent can still be clicked by accident.

**Fix:** The button starts with `opacity: 0` and `pointer-events: none`. Once the page is scrolled past 100px, JavaScript adds the `active` class, which makes it visible, clickable, and moves it up a little.

### 7. Bugs I found when I reread my own code

- The "Skills" tab calls `opentab()`, but I never wrote that function. I copied a tab layout and kept only one tab.
- The Services and Portfolio headings use `sub-titile` in the HTML but `.sub-title` in the CSS. One typo meant the style never applied.
- I asked for the Poppins font but never imported it, so the site uses the browser's default font.

### What I learned

- Flexbox and Grid make layouts much easier than I expected.
- `position: relative` on a parent is what makes `position: absolute` children behave.
- A tiny typo in a class name can cause a big "why isn't my CSS working?" moment. Checking the element in DevTools shows quickly whether a rule is being applied.
- A form can work without writing a backend if I connect it to a service like Google Sheets.

---

## Known issues

Bugs I found while reviewing my own code, and what I want to fix next:

- [ ] **Skills tab:** `opentab('skills')` isn't defined, so clicking the tab throws a JavaScript error.
- [ ] **Section headings:** `class="sub-titile"` in the HTML doesn't match `.sub-title` in the CSS.
- [ ] **Font:** Poppins is requested in the CSS but never imported.
- [ ] **Favicon:** `rel="Website icon"` should be `rel="icon"`.
- [ ] **HTML structure:** there are extra closing `</div>` tags near the Portfolio section.
- [ ] **CSS typo:** `border-radius: 505` on the logo is missing its unit (`px`).
- [ ] **Repeated CSS:** the four skill bars share almost identical rules.
- [ ] **Text typos:** "studen", "Sercives", "ebsites", "tailores", "friendxly".
- [ ] **Small details:** the GitHub icon (`#222`) is very dark on a white circle, and the "Learn More" links go nowhere (`href="#"`).

---

## Credits

- Icons: [Font Awesome](https://fontawesome.com/)
- Contact form approach: Google Apps Script → Google Sheets
- Everything else: me 🙂

---

## Contact

- GitHub: [@Subarna-kdl](https://github.com/Subarna-kdl)
- Email: use the contact form on the live site
