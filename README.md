# Reponsive Design and Grid-based design
Yesterday, we had an introduction to HTML and CSS, two languages that together allow us to create structured documents and give them any appearance we want. Today we're going to take a look at a few techniques to make creating this content easier.

## Responsive Design
Throughout the life of the web, the content delivered over the medium has evolved. But so have the devices used to access this content. In the 1990s the web was accessed at work, using an old 640x480 CRT monitor and a mouse (if lucky) + keyboard combo. These days,  your web page can find itself anywhere, from a user's smart watch's touchscreen all the way up to their 72" home cinema display.

It would be impossible to create one different and specialized experience for each device that exists out there. However, using some responsive design techniques, we can create web pages that will look and act well on a wide variety of devices.

As we'll see, CSS can help us with our responsive design adventure by providing some flexible sizing options like `%` and and `em`s, as well as media queries.

Many of the websites you visit daily are already great demonstrations of responsive design. Here is CNN's website displayed at different screen widths:

### CNN's largest view
![cnn largest](https://s13.postimg.org/u90yznn47/Screen_Shot_2016_10_31_at_16_10_37.png)

### CNN's medium view
![cnn medium](https://s13.postimg.org/kag0d6don/Screen_Shot_2016_10_31_at_16_10_51.png)

### CNN's tablet view
![cnn tablet](https://s13.postimg.org/rcdxzdhaf/Screen_Shot_2016_10_31_at_16_11_09.png)

### CNN's mobile view
![cnn mobile](https://s13.postimg.org/s0msibg07/Screen_Shot_2016_10_31_at_16_11_20.png)

The site [mediaqueri.es](http://mediaqueri.es/) showcases some really awesome responsive web sites.

## Grid-based design
Most of the websites you'll see online these days are created using a grid-based design. The main idea of a grid-based design is to look at a website as a series of rows of content. In the most common grids, each row is subdivided into 12 columns. A portion of the page can take anywhere from 1 up to 12 of these columns.

The number 12 was not chosen at random: 12 divides equally in two parts as 2x6, three parts as 3x4, four parts as 4x3, and six parts as 6x2, as well as one big part of 1x12. Having a set of CSS classes that allow us to create rows and column-based content cells makes it even easier to integrate designs.

## Frameworks implementing grid-based designs
Many CSS frameworks exist that implement grid-based classes as part of the framework. Two of the most popular ones are [Twitter's Bootstrap](http://getbootstrap.com/) and [Zurb's Foundation](http://foundation.zurb.com/). Rather than focus on these frameworks which you can learn on your own, we'll concentrate on creating our own CSS grid framework.

![example grid](https://s18.postimg.org/610cifqh5/Screen_Shot_2016_10_31_at_16_15_31.png)

To do this, we already have part of the information. We know how to lay elements out horizontally, using `display: flex` on their parent. We also know how to make an element take a flexible width using `%`. Since we'll have 12 subdivisions per row, we will need CSS classes to describe one column's width all the way up to 12 columns.

After creating our grid, we'll easily be able to use it to create most of the designs provided to us. For example, if a design shows three equally-sized boxes next to each other, then we know we'll need one row, and three boxes that span four columns each.

One of the main properties of a responsive grid that we don't know how to take care of yet is stacking columns. You can see this if you look at any responsive site in your browser, and start resizing your browser's window: elements that were once displayed next to each other end up stacked on top of each other at lower screen sizes. This means that the CSS `display: flex` that we use for those elements is somehow "conditional" to the screen's width. We can achieve this with the use of CSS Media Queries.

## Media Queries
Media queries are the `if/else` of CSS. They allow us to use some CSS rules only if certain aspects of the device are there. For example, we can use a `print` media query to create styles that only apply when our document is printed:

```css
@media print {
    img {
        /* Let's save some ink by not printing images! */
        display: none;
    }
}
```

If we had written this style targetting `<img>` elements, but without the media query, then no images would ever be displayed on our page. Here, we are adding this CSS **conditionally**, only if we are on a printed medium.

Most often, we're going to create our conditional CSS based on the device's screen width. Media queries allow us to do that by using the `min-width` query. This query will activate the CSS inside of it only if the device's screen satisfies a minimum width. For example:

```html
<p class="only-big-screen">You are on a big screen</p>
```

```css
.only-big-screen {
    display: none;
}
@media only screen and (min-width: 1024px) {
    .only-big-screen {
        display: block;
    }
}
```

Here, we are setting the class of `only-big-screen` to be hidden with `display: none` by default. Then, using a media query, we're saying that the class will be display block, but **only if** the width of the device's screen is 1000px or more.

The `1024px` here is commonly referred to as a "breakpoint". It's the point where things will start changing in our design. Rather than having media queries all over the place with different `min-width`s, we usually start by defining a set of breakpoints. In our case, this will help us create three separate grids:

* one grid for "small" devices having a width of less than **640px**. This will be most mobile phones.
* one grid for "medium" devices having a width of **640px-1023px**. This will be most tablets.
* one final grid for "large" devices, having a width of **1024px** or more.

Moreover, our grid will be implemented using a **[mobile-first](http://bradfrost.com/blog/web/mobile-first-responsive-web-design/)** approach: this is a technique that will help us integrate our designs starting at the mobile device, and adding details as we go.

## Media queries and width :warning:
When the first smartphones arrived on the market, not a single web page was built to display well on them. The first iPhone had a resolution of 320x480 pixels, and most web pages are meant to be displayed at 960px or more.

What the iPhone -- any many other smart phone browsers -- does to display most web pages correctly on its 320px-wide screen is the following:

1. Create a virtual screen that is 960px wide
2. Render the web page as if it was being displayed at 960px
3. Scale down the web page by 3 so that it fits in the view of the phone
4. Allow the user to use their fingers to scale up or down the content of the page

Not only does this look really ugly and reduces the usability of many sites, it also messes with media queries. By default, even a phone with 320px screen width will act as if the width was 960px. To fix this, a `<meta>` tag in the `<head>` of our document allows us to set the effective width to the same as the screen width, and is **necessary to make our media queries work correctly**:

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0">
```

This HTML tag should be added in the `<head>` of ALL the pages on which you want to use a responsive design. This version of the tag will still allow the user to pinch-zoom, in case some of your content is too small on their device. If you are certain of what you are doing and want to disable pinch-zoom, you can also set the `maximum-scale` to `1.0`:

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0">
```

This is mostly done for web applications like the mobile versions of Facebook or Gmail.

## It's not all about the media queries...
The subject of responsive design is often mentioned alongside media queries. It's true that media queries provide a great if/else way of dealing with different devices, but there are many other ways to do so.

### Using `vw` and `vh` for responsive design
The `vw` and `vh` CSS units stand for "1% of viewport's width" and "1% of viewport's height" respectively. Creating a container that takes the whole height of the window is as simple as giving it a `height: 100vh`. This requires no media queries and will respond to the screen size.

`vw` is also often used as a `font-size` unit when creating headings. Since the font size becomes a percentage of the viewport width, the heading will scale as the device gets smaller. Use this sparingly.

### Using percentages instead of pixel values
Pixel values are the enemy number one of responsive design. Saying that a box should be `400px` wide means it won't display correctly on many mobile phones.

A mix of `%`-based widths and the usage of `max-width` rather than `width` will improve the responsiveness of your site, without any media queries.

### Be careful with the `:hover` effects :warning:
While adding CSS effects on hover can be interesting, remember that mobile users cannot hover! If any functionality is hidden behind a hover, it may not work properly on a mobile device.

### Creating fixed aspect-ratio boxes
Sometimes, we care less about the size of a box than its aspect ratio. The best example of this is creating a responsive video embed from YouTube or Vimeo. There exists a commonly-known [aspect ratio trick using `padding-bottom`](http://www.goldenapplewebdesign.com/responsive-aspect-ratios-with-pure-css/). This uses no media queries and will respond to the screen size by adjusting the height of the container appropriately.

### Using flexbox for responsiveness
Using Flexbox can make it easy to implement responsive features without needing to use media queries. By using a mix of fixed-width elements and `flex` elements, we can create a layout that responds to the device size without being conditional.

A common example is having a fixed-size avatar of 40x40 px, and needing to display some text next to the avatar, using "all the remaining space". This can be achieved with:

```html
<div class="comment-box">
  <img class="avatar" src="http://placekitten.com/g/40/40">
  <p class="comment-text">Lorem ipsum blabla</p>
</div>
```

```css
.comment-box {
    display: flex;
}
.avatar {
    width: 40px;
    height: 40px;
    margin-right: 10px;
}
.comment-text {
    flex: 1; /* Take up all the remaining space after the 40px + margin of the avatar */
}
```

The above examples are only a few of the many ways to create responsive designs without the use of media queries or a grid. Knowing when to use grid-based design and media queries takes practice, as there are always many ways of doing things.

With that in mind, let's start building our twelve-column grid!

## Creating a non-responsive CSS grid
The first iteration of our CSS grid will use a row with a maximum width of 1000px, and twelve column classes named `col-1` up to `col-12`. Here is the CSS for our grid:

```css
/* Box-sizing fix! */
html {
  box-sizing: border-box;
}
*, *::before, *::after {
  box-sizing: inherit;
}

.row {
    max-width: 1000px;
    margin: 0 auto;
    display: flex; /* This will allow our row to display columns horizontally */
    flex-wrap: wrap; /* This will become necessary to allow our flex boxes to stack in responsive mode */
}
/* If an element needs all twelve columns, then its width is 100%. Easy. */
.col-12 {
    width: 100%;
}
/* If an element needs 11 of 12 columns, its width is 11/12 * 100% */
.col-11 {
    width: 91.666%;
}
.col-10 {
    width: 83.333%;
}
.col-9 {
    width: 75%;
}
.col-8 {
    width: 66.666%;
}
.col-7 {
    width: 58.333%;
}
/* This is the class we'd use if we want two 50/50 elements next to each other */
.col-6 {
    width: 50%;
}
.col-5 {
    width: 41.666%;
}
/* This is the class we'd use if we want three elements next to each other */
.col-4 {
    width: 33.333%;
}
/* This is the class we'd use if we want four elements next to each other */
.col-3 {
    width: 25%;
}
.col-2 {
    width: 16.666%;
}
/* We'll almost NEVER use this */
.col-1 {
    width: 8.333%;
}
```

And here's an example of using our grid:

```html
    <header>
      <div class="row">
        <div class="col-12">
          <h1>This title should take up the full twelve columns</h1>
        </div>
      </div>
    </header>
    <section>
      <div class="row">
        <div class="col-4">
          <h2>Selling point #1</h2>
          <p>Each of the <strong>col-4</strong> divs takes 4 columns, for a total of 12 columns!</p>
        </div>
        <div class="col-4">
          <h2>Selling point #2</h2>
          <p>Each of the <strong>col-4</strong> divs takes 4 columns, for a total of 12 columns!</p>
        </div>
        <div class="col-4">
          <h2>Selling point #3</h2>
          <p>Each of the <strong>col-4</strong> divs takes 4 columns, for a total of 12 columns!</p>
        </div>
      </div>
    </section>
    <section>
      <div class="row">
        <div class="col-6">
          <h2>Major update!</h2>
          <img src="http://placekitten.com/g/301/301">
          <p>This paragraph is part of a sub-section that is six columns wide!</p>
        </div>
        <div class="col-6">
          <h2>Major update!</h2>
          <img src="http://placekitten.com/g/299/299">
          <p>This paragraph is part of a sub-section that is six columns wide!</p>
        </div>
      </div>
    </section>
    <section>
      <div class="row">
        <div class="col-4">
          <img src="http://placekitten.com/g/200/200">
        </div>
        <div class="col-8">
          <h2>This is a title for the image on the left!</h2>
          <p>This content is part of a sub-section that takes 8 columns, or two thirds of the total space.</p>
        </div>
      </div>
    </section>

```

Which gives the following output in the browser:
![grid-based design](https://s21.postimg.org/3sa6jr7yv/grid_based.png)

All this without having to add extra CSS outside of the grid. Here's what the same output looks like with a twelve-column grid overlayed on top. We can clearly see that our divs are ligning up to the columns of the grid as we specified.

![grid-based design with overlay](https://s21.postimg.org/k20cmnimv/grid_overlay.png)

One major flaw of this grid is that it's not responsive. If we resize our browser to the size of a tablet or mobile, things start to break down:

![non-responsive grid on mobile](https://s12.postimg.org/k7imifw0d/Screen_Shot_2016_10_31_at_17_06_22.png)

First off, images are overflowing past their containers. To fix this, let's add back the `responsive-img` utility class to our CSS grid framework, and put it on all the images that need to be responsive:

```css
.responsive-img {
    max-width: 100%; /* don't be wider than your parent container, image */
}
```

Still though, the main flaw here is that the browser is still trying to layout our content horizontally, when at lower screen sizes this same content should stack instead. In the next section, we'll use media queries to solve this problem.

## Making a responsive grid
We're going to create our responsive grid based on a mobile-first approach. Rather than having one set of `col-XX` classes, we will have three sets: `col-small-XX`, `col-medium-XX` and `col-large-XX`. The `*small*` classes will apply to ALL devices, the `*medium*` classes will apply to medium AND LARGER devices, and the `*large*` classes will only apply to large devices. This will allow us to create responsive designs by starting from the mobile version, working our way up and adding classes only where needed.

Here's what the general structure of our grid will be:

```css
/* Utility classes like responsive-img and others */

/* Classes that apply to all device sizes */

@media only screen and (min-width: 641px) {
    /* Classes that apply to medium and larger devices */
}

@media only screen and (min-width: 1025px) {
    /* Classes that apply to large devices only */
}
```

First off, start by renaming all the `col-XX` classes you already have to `col-small-XX`. If we use the `col-small-6` on a div, it means we need it to be 50% width on ALL devices. If we use the class `col-small-6 col-medium-4`, then it means we wan the div to be 50% width on small devices, and 33% width on medium and larger devices.

With this in mind, create a set of `col-medium-XX` classes that have the same percentages as the small ones. Add those classes in the 641px+ media query.

Then, create a set of `col-large-XX` classes that have the same percentages as the other ones, but add them to the 1025px+ media query.

You'll end up with a CSS that looks like this, your grid system!

```css
/* Box-sizing fix! */
html {
  box-sizing: border-box;
}
*, *::before, *::after {
  box-sizing: inherit;
}

/* Utility classes */
.responsive-img {
  max-width: 100%;
}
.text-center {
    text-align: center;
}

/* Grid system! */
.row {
    max-width: 1000px;
    margin: 0 auto;
    display: flex; /* This will allow our row to display columns horizontally */
    flex-wrap: wrap; /* This will become necessary to allow our flex boxes to stack in responsive mode */
}

/* Small and up columns */

/* If an element needs all twelve columns, then its width is 100%. Easy. */
.col-small-12 {
    width: 100%;
}
/* If an element needs 11 of 12 columns, its width is 11/12 * 100% */
.col-small-11 {
    width: 91.666%;
}
.col-small-10 {
    width: 83.333%;
}
.col-small-9 {
    width: 75%;
}
.col-small-8 {
    width: 66.666%;
}
.col-small-7 {
    width: 58.333%;
}
/* This is the class we'd use if we want two 50/50 elements next to each other */
.col-small-6 {
    width: 50%;
}
.col-small-5 {
    width: 41.666%;
}
/* This is the class we'd use if we want three elements next to each other */
.col-small-4 {
    width: 33.333%;
}
/* This is the class we'd use if we want four elements next to each other */
.col-small-3 {
    width: 25%;
}
.col-small-2 {
    width: 16.666%;
}
/* We'll almost NEVER use this */
.col-small-1 {
    width: 8.333%;
}

/* Medium and up screen sizes */
@media only screen and (min-width: 641px) {
  .col-medium-12 {
      width: 100%;
  }
  /* If an element needs 11 of 12 columns, its width is 11/12 * 100% */
  .col-medium-11 {
      width: 91.666%;
  }
  .col-medium-10 {
      width: 83.333%;
  }
  .col-medium-9 {
      width: 75%;
  }
  .col-medium-8 {
      width: 66.666%;
  }
  .col-medium-7 {
      width: 58.333%;
  }
  /* This is the class we'd use if we want two 50/50 elements next to each other */
  .col-medium-6 {
      width: 50%;
  }
  .col-medium-5 {
      width: 41.666%;
  }
  /* This is the class we'd use if we want three elements next to each other */
  .col-medium-4 {
      width: 33.333%;
  }
  /* This is the class we'd use if we want four elements next to each other */
  .col-medium-3 {
      width: 25%;
  }
  .col-medium-2 {
      width: 16.666%;
  }
  /* We'll almost NEVER use this */
  .col-medium-1 {
      width: 8.333%;
  }

}




/* Large screen sizes */
@media only screen and (min-width: 1025px) {
  .col-large-12 {
      width: 100%;
  }
  /* If an element needs 11 of 12 columns, its width is 11/12 * 100% */
  .col-large-11 {
      width: 91.666%;
  }
  .col-large-10 {
      width: 83.333%;
  }
  .col-large-9 {
      width: 75%;
  }
  .col-large-8 {
      width: 66.666%;
  }
  .col-large-7 {
      width: 58.333%;
  }
  /* This is the class we'd use if we want two 50/50 elements next to each other */
  .col-large-6 {
      width: 50%;
  }
  .col-large-5 {
      width: 41.666%;
  }
  /* This is the class we'd use if we want three elements next to each other */
  .col-large-4 {
      width: 33.333%;
  }
  /* This is the class we'd use if we want four elements next to each other */
  .col-large-3 {
      width: 25%;
  }
  .col-large-2 {
      width: 16.666%;
  }
  /* We'll almost NEVER use this */
  .col-large-1 {
      width: 8.333%;
  }

}
```

Here's the same example HTML from above, but using the new responsive grid:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
    <link rel="stylesheet" href="grid.css">
  </head>
  <body>
    <header>
      <div class="row">
        <div class="col-small-12">
          <h1>This title should take up the full twelve columns</h1>
        </div>
      </div>
    </header>
    <section>
      <div class="row">
        <div class="col-medium-4">
          <h2>Selling point #1</h2>
          <p>Each of the <strong>col-medium-4</strong> divs takes 4 columns, for a total of 12 columns! This happens both on tablet and large screens due to the mobile-first way our classes are defined.</p>
        </div>
        <div class="col-medium-4">
          <h2>Selling point #2</h2>
          <p>Each of the <strong>col-medium-4</strong> divs takes 4 columns, for a total of 12 columns! This happens both on tablet and large screens due to the mobile-first way our classes are defined.</p>
        </div>
        <div class="col-medium-4">
          <h2>Selling point #3</h2>
          <p>Each of the <strong>col-medium-4</strong> divs takes 4 columns, for a total of 12 columns! This happens both on tablet and large screens due to the mobile-first way our classes are defined.</p>
        </div>
      </div>
    </section>
    <section>
      <div class="row">
        <div class="col-medium-6">
          <h2>Major update!</h2>
          <img class="responsive-img" src="http://placekitten.com/g/301/301">
          <p>This paragraph is part of a sub-section that is six columns wide on tablet and desktop!</p>
        </div>
        <div class="col-medium-6">
          <h2>Major update!</h2>
          <img class="responsive-img" src="http://placekitten.com/g/299/299">
          <p>This paragraph is part of a sub-section that is six columns wide on tablet and desktop!</p>
        </div>
      </div>
    </section>
    <section>
      <div class="row">
        <div class="col-medium-3 col-large-4">
          <img class="responsive-img" src="http://placekitten.com/g/200/200">
        </div>
        <div class="col-medium-9 col-large-8">
          <h2>This is a title for the image on the left!</h2>
          <p>This content is part of a sub-section. It takes 9 columns on tablet and 8 columns on large devices. On mobile, everything will stack</p>
        </div>
      </div>
    </section>
  </body>
</html>
```

Here's how this HTML looks given different screen sizes:

### Our website on mobile: everything stacks!
![mobile grid](https://s13.postimg.org/korb8fj8n/Screen_Shot_2016_10_31_at_17_48_52.png)

### Our website on tablet and up: elements line up horizontally
![tablet grid](https://s13.postimg.org/t5qtjcnxj/Screen_Shot_2016_10_31_at_17_49_13.png)

## Using our responsive grid
To use the responsive grid, all we have to do is put it in a file called `grid.css` and include it with our new project where we want to use it.

When comes the time to add our own project-specific CSS, we will do it in a **new file**, and not in the `grid.css` itself. This file will be included **after** our own CSS.

If we need to write our own media queries to hide/show some elements based on the screen size, we will **always use the same breakpoints as our grid system**. This means that, at most, there should be two different media queries in our project CSS: one for 641px+ screen sizes and one for 1025px+ screen sizes. All the styles that are outside a media query should apply to all of mobile, tablet and desktop/laptop devices. Styles in the 641px+ media query should apply to tablet and desktop, and styles in the 1025px+ media query should apply to desktop/laptop devices only.

## Other media queries
Note that while the `min-width` media query is the one used most often, there exist other media queries. For example one can use a `max-width` media query to *only* display something on a smaller screen. However, this would go against the mobile-first approach: add the mobile style without a media query, and hide anything you want to hide using the medium or large media queries.

Another example of media query would be the `orientation`. Sometimes we want to display something differently if the device is in `portrait` or `landscape` mode. `portrait` simply means that the height of the device screen is greater than the width, and landscape means that the width of the device screen is greater than the height.

## Existing responsive frameworks: Bootstrap and Foundation
As we mentioned earlier, there exist tons of responsive css frameworks out there. Googling for "responsive css framework" could cause a stack overflow on Google's servers from the amount of results :)

No matter the framework, they all work the same way. Usually, they have a set of utility classes, a responsive 12-column grid system, and some extra styles for forms, typography, images, etc. Finally most of the frameworks will include some dynamic functionality, often using jQuery in the process.

As part of your discovery of CSS frameworks, you'll be replicating yesterday's "Template 00" design using your own CSS framework, and later trying to adapt it to Bootstrap.

# Practice using responsive CSS frameworks to integrate designs

## Exercise 1
Using the responsive CSS grid you created in this workshop, you will replicate the [Template 00 design from yesterday's workshop](https://github.com/decodemtl/html-css-101#template-00). **However**, rather than simply replicating the fixed design like it is, you will first have to **figure out how this design translates to mobile and tablet devices**.

In an ideal world, the designer you are working with will provide you two or three different mockups of the project: at least one for small screens and one for large screens. When this is not the case, you will most often get a design for the large desktop/laptop screens. It will be up to you to determine what the design should look like on a mobile device: stacking elements, hiding some unnecessary content, etc.

For this exercise, don't forget to write your styles with a **mobile first** approach. Start by doing the mobile version, and only add styles for tablet or desktop, as much as possible. If something can be done with one of the two `min-width` media queries you have access to, then do it this way.

Here are the responsive elements we want to have:

1. On mobile devices, the top header content should take the full width of the screen. This should be taken care of by the `.row` element with its `max-width` of `1000px`. On mobile, the width of ~300px should make the top header shrink by default, without any extra work from your part.
2. For the selling points section, let's add one more item to make it four total. On mobile, the selling points will stack on top of each other. However, the icon image will be displayed to the left of the title + text of the selling point. On tablet, you will have two selling points per row. On desktop all four selling points on the same row.
3. For the "get notified" and vimeo vid section, we want them displayed next to each other on tablet and desktop. On mobile, the two sections should stack, but **with a twist**: the video should appear *before* the "get notified" block, **without changing or duplicating any HTML**. To do this, you can leverage the [Flexbox `order` property](https://css-tricks.com/almanac/properties/o/order/).
4. On mobile, the links in the footer nagivation should stack. **Do not use** the grid to do this! It's not made for displaying tiny items like icons next to each other, but mainly for big blocks of content. Use media queries and your own CSS to achieve this.

## Exercise 2
For this exercise, you're also going to replicate Template 00. However, rather than doing it with your own CSS framework, you are going to do it with the Bootstrap framework. To start, go back to the way your HTML looked before starting exercise 1.

Then, include the Bootstrap CSS from the [Bootstrap CDN](http://getbootstrap.com/getting-started/#download-cdn). Make sure to only include the first `<link>` tag, as we won't be using the bootstrap theme. We won't be using Bootstrap's JavaScript either.

Read about the [Bootstrap Grid System](http://getbootstrap.com/css/#grid). It works similar to ours but has four sizes instead of our three. We'll simply ignore the `md` sizing and use `xs` as our small, `sm` as our medium and `lg` as our large.

After adding the Bootstrap CSS to your HTML document, use the Bootstrap Grid to re-implement the requirements of Exercise 1.

## Exercise 3: Responsive block-grid
In this exercise, we're going to augment our responsive grid by adding a responsive block grid!

A block grid's most common use case is to create a photo gallery. Even though a photo gallery looks like a grid-based module, there is a difference: we are not thinking in terms of rows and columns, but rather in terms of a list of items displaying the same number of items per row.

Since a block grid displays the same number of items on each row, we will only need to add a class name to the parent element in our block grid. This element will almost always be a `<ul>`. Then, the child `<li>` elements will be automatically sized using the appropriate CSS.

Here is the CSS for a non-responsive block-grid:

```css
ul.block-grid {
  display: flex; /* display elements next to each other */
  flex-wrap: wrap;
  list-style: none; /* disable bullets on the LI items */
  padding-left: 0; /* reset the browser extra padding on UL elements */
}

ul.block-grid li {
  padding: 5px; /* Adding a 10px padding on a 100% width would not work if it wasn't for box-sizing: border-box! */
}

ul.block-grid-2 > li {
  width: 50%;
}
ul.block-grid-3 > li {
  width: 33.333%;
}
ul.block-grid-4 > li {
  width: 25%;
}
.block-grid-5 > li {
  width: 20%;
}
.block-grid-6 > li {
  width: 16.666%;
}
```

We'll stop at six elements per row for our grid, should be more than enough. Here's how to use the block grid to create a photo gallery with four elements per row. Notice that the `<ul>` element has **two class names**: one for the general block grid styles, and one to say we want four elements per row:

```html
<ul class="block-grid block-grid-4">
  <li>
    <img class="responsive-img" src="http://placekitten.com/g/400/400">
    <p>Kitten #1</p>
  </li>
  <li>
    <img class="responsive-img" src="http://placekitten.com/g/400/400">
    <p>Kitten #2</p>
  </li>
  <li>
    <img class="responsive-img" src="http://placekitten.com/g/400/400">
    <p>Kitten #3</p>
  </li>
  <li>
    <img class="responsive-img" src="http://placekitten.com/g/400/400">
    <p>Kitten #4</p>
  </li>
  <li>
    <img class="responsive-img" src="http://placekitten.com/g/400/400">
    <p>Kitten #5</p>
  </li>
  <li>
    <img class="responsive-img" src="http://placekitten.com/g/400/400">
    <p>Kitten #6</p>
  </li>
</ul>
```

![block grid](https://s13.postimg.org/meigewv2v/Screen_Shot_2016_10_31_at_23_29_11.png)

Once we write this HTML for the block grid, we can go on to add our own styles to it. Again, we're not going to write our custom styles in the `grid.css` file but rather in our own stylesheet.

Let's start by adding an extra class name to our `<ul>`, `"kittens-gallery"`. This way we can add styles to it without needing to target our responsive framework's class names. It's almost always a bad idea to target the classes of a framework. Much better to add our own class names to the markup.

Let's add the following styles:

```css
.kittens-gallery {
  justify-content: center; /* if there are less elements on the last row, center them instead of sending them to the left */
  text-align: center; /* This will center the caption text below each image */
}

.kittens-gallery img {
  padding: 5px;
  border: 1px solid #999;
  margin-bottom: 5px;
}

.kittens-gallery p {
  font-weight: bold;
  margin: 0;
}
```

Much better:
![styled block grid](https://s13.postimg.org/oddxeq4p3/Screen_Shot_2016_10_31_at_23_31_13.png)

One problem is that this block grid is not responsive. Ideally, we'd like to say: "give me a two-element block grid on mobile, three-element block grid on tablet and four-element block grid on desktop".

To do this, we need to create a responsive block grid. Rather than having one set of `block-grid-*` classes, we'll have a set of `block-grid-small-*`, `block-grid-medium-*` and `block-grid-large-*`. They will work the same as the responsive twelve-column grid, following a mobile-first approach.

Following the pattern you used to create your responsive grid, do the following:

1. Rename the `block-grid-*` classes to `block-grid-small-*`
2. Add a medium version of the block grid in the 641px+ media query
3. Add a large version of the block grid in the 1025px+ media query

Once your block grid is created, use it to modify the Kittens Gallery example above. To do this, all you'll have to do is change the class names associated to the `<ul>` element. You'll need one class name per screen size. The spec is:

* On mobile -- small screen -- we want two kittens per row
* On tablet -- medium screen -- we want three kittens per row
* On desktop -- large screen -- we want four kittens per row

Once you have done that, we'll make one final set of changes by adding some **custom CSS**:

* On tablet AND desktop, we want the font size of the gallery captions to be 1.2x the current font size. We'll need to add this style rule in a media query targeting medium and larger screen sizes
* As we scale things up and down, notice that some of our styles are still pixel-based. While this is perfectly fine sometimes, we can do better here. Revisit the `.kittens-gallery img` styles from above, and change the pixel values for padding and margin to `em`s instead.
* On mobile devices, we don't want to display any border around the images. Since we're using a **mobile-first** approach, note that the styles that add the border should be setup in a media query instead.

Congratulations, you now have a responsive block grid added to your CSS framework, and you know how to use it. Note that Bootstrap and Foundation both have block grids that work approximately the same way as yours.

## Keep practicing!
If you got this far, now is not the time to stop! Yesterday's workshop gave you three templates to integrate. Using your newly built CSS framework, go back to the second and third templates and implement them using your responsive CSS framework and custom CSS.
