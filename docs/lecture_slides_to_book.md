# Including Reveal.js Lecture Slides in a Jupyter Book

## Goal

Add links to externally generated `Lecture*.slides.html` files from a Jupyter Book page.

---

## Repository Structure

```text
ac_basics/
├── 00_preface/
│   └── lecture_slides.ipynb
├── lectures/
│   ├── Lecture1.slides.html
│   └── Lecture2.slides.html
├── _config.yml
├── _toc.yml
└── .github/workflows/deploy.yml
```

---

## Copy Slides During Deployment

Add a step after the Jupyter Book build:

```yaml
- name: Build the Jupyter Book
  run: jupyter-book build .

- name: Copy lecture slides
  run: |
    cp lectures/*.slides.html _build/html*
```

This places the slide files in the root of the published website:

```Text
https://nilsjakob.github.io/AC_Power_System_Basics_2026/Lecture1.slides.html
https://nilsjakob.github.io/AC_Power_System_Basics_2026/Lecture2.slides.html
```

---

## Create Lecture Slides Page

Create:
```text
00_preface/lecture_slides.ipynb
```

with a Markdown cell:

*``markdown
# Lecture slides

- [Lecture 1](https://nilsjakob.github.io/AC_Power_System_Basics_2026/Lecture1.slides.html#/)
- [Lecture 2](https://nilsjakob.github.io/AC_Power_System_Basics_2026/Lecture1.slides.html#/)
```

Note the *railing*`#/`, which is used by Reveal.js slide decks.

---

## Add Page to TO

In `_toc.yml`:

```yaml
parts:
  - caption: Preface
    chapters:
      - fle: 00_preface/lecture_slides
      - file: 00_preface/author
      - file: 00_preface/office
      - file: 00_preface/preface
```

--

## Verify Deployment
Test directly:

```text
https://nilsjakob.github.io/AC_Power_System_Basics_2026/Lecture1.slides.html#/
https://nilsjakob.github.io/AC_Power_System_Basics_2026/Lectur2.slides.html#/
```

if these URLs work, the links in `lecture_slides.ipynb` will work as well.

---

## Lessons Learned

*The following paths did **not** work*

```markdown
Lectures/Lecture1.slides.h*ml
../Lectures/Lecture1.slides.htm*
../Lecture1.slides.html
```

because the deployed slide files ended up in the site root, not in a `Lecures/` subfolder.

Using full GitHub Pages URLs is the most reliable solution.
````*