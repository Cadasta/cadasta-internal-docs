# Cadasta Styles & Assets

In an effort to simplify branding across Cadasta applications, commonly used assets are placed in the AWS S3 Bucket titled `assets.cadasta.org`. They are then available at `https://assets.cadasta.org/<FILENAME>`.

## Images

### Logos

#### Large, transparent background

![](https://assets.cadasta.org/logo/Color_Logo/Rectangular/logo_color_200x71/logo_color_200x71.png)

```
https://assets.cadasta.org/logo/Color_Logo/Rectangular/logo_color_200x71/logo_color_200x71.png
```

## Color Palette

### Background Colors

<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(255, 255, 255)"></div>
  <figcaption>
    HEX <code>#ffffff</code>
  </figcaption>
</figure>
<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(242, 244, 247)"></div>
  <figcaption>
    HEX <code>#f2f4f7</code>
  </figcaption>
</figure>
<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(46, 81, 163)"></div>
  <figcaption>
    HEX <code>#2e51a3</code>
  </figcaption>
</figure>
<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(229, 229, 229)"></div>
  <figcaption>
    HEX <code>#e5e5e5</code>
  </figcaption>
</figure>

### Text Colors

<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(89, 90, 92)"></div>
  <figcaption>
    HEX <code>#595a5c</code>
  </figcaption>
</figure>
<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(46, 81, 163)"></div>
  <figcaption>
    HEX <code>#2e51a3</code>
  </figcaption>
</figure>
<figure class="swatch-wrap">
  <div class="color-box" style="background: #0e305e none repeat scroll 0% "></div>
  <figcaption>
    HEX <code>#0e305e</code>
  </figcaption>
</figure>
<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(0, 0, 0)"></div>
  <figcaption>
    HEX <code>#000000</code>
  </figcaption>
</figure>
<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(238, 238, 238)"></div>
  <figcaption>
    HEX <code>#eeeeee</code>
  </figcaption>
</figure>
<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(255, 255, 255)"></div>
  <figcaption>
    HEX <code>#ffffff</code>
  </figcaption>
</figure>

### Highlight Colors

<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(0, 178, 136)"></div>
  <figcaption>
    HEX <code>#00B288</code>
  </figcaption>
</figure>
<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(46, 81, 163)"></div>
  <figcaption>
    HEX <code>#2e51a3</code>
  </figcaption>
</figure>
<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(237, 170, 0)"></div>
  <figcaption>
    HEX <code>#edaa00</code>
  </figcaption>
</figure>

### Button Color

<figure class="swatch-wrap">
  <div class="color-box" style="background: rgb(0, 178, 136)"></div>
  <figcaption>
  HEX <code>#00B288</code>
  </figcaption>
</figure>

## Fonts and Usage

The selection of typefaces and the arrangement of them can be as important as the use of color, images or graphics in creating a brand. These are the fonts that should be used in all Cadasta materials.

### Steelfish Regular

<h3><span style="font-family: Steelfish-Regular;">
ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890?&gt;&lt;!@#$%^&amp;*()_+
</span></h3>

For use only with headlines and subheads.
Use all caps (consider utilizing `text-transform: uppercase;` in the CSS).

```css
@font-face {
  font-family: Steelfish-Regular;
  src: url("https://assets.cadasta.org/fonts/steelfish-rg.ttf");
}

font-family: Steelfish-Regular;
text-transform: uppercase;
```

### Roboto Black

<span style="font-family: Roboto; font-weight: 900;">
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890?&gt;&lt;!@#$%^&amp;*()_+
</span>

For use only with subheads or for emphasis.

```html
<link
  href="https://fonts.googleapis.com/css?family=Roboto:900"
  rel="stylesheet"
/>
```

Or

```css
@import url("https://fonts.googleapis.com/css?family=Roboto:900");
```

```css
font-family: "Roboto", sans-serif;
font-weight: 900;
```

### Roboto Light

<span style="font-family: Roboto; font-weight: 300;">
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890?&gt;&lt;!@#$%^&amp;*()_+
</span>

For use in larger bodies of type, such as paragraphs.

```html
<link
  href="https://fonts.googleapis.com/css?family=Roboto:300"
  rel="stylesheet"
/>
```

Or

```css
@import url("https://fonts.googleapis.com/css?family=Roboto:300");
```

```css
font-family: "Roboto", sans-serif;
font-weight: 900;
```

### Roboto Light Italic

<span style="font-family: Roboto; font-weight: 300; font-style: italic;">
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890?&gt;&lt;!@#$%^&amp;*()_+
</span>

For use in larger bodies of type, such as paragraphs.

```html
<link
  href="https://fonts.googleapis.com/css?family=Roboto:300i"
  rel="stylesheet"
/>
```

Or

```css
@import url("https://fonts.googleapis.com/css?family=Roboto:300i");
```

```css
font-family: "Roboto", sans-serif;
font-weight: 300;
font-style: italic;
```
