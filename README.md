# Goodrocket CSS

This is a design free CSS starting kit for writing modular, maintainable, and scalable CSS. 
The aim is to provide a solid structure and sensible defaults to jump start the UI development
and then be able to maintain the project without going nuts.

<a href="https://github.com/ivanbabko/goodrocket-css/archive/master.zip">
  <img src=".hero.png" alt="Goodrocket CSS"/>
</a>

---

Based on Sass preprocessor, BEM naming conventions, and OOCSS approach. A few 
things were inspired by the ITCSS architecture and Inuitcss framework, which I extended and adapted to my development style.



## Structure

### Config

Here we have global variables that we'll be using throughout our code. Think typography settings, 
color palette, spacing units, breakpoints, z-index layers, and so on. Component-specific 
variables such as `$carousel-width` are defined in component partials. This way we keep this layer 
slim, and can easily find most variables alongside the code that uses them.

**Note:** Config layer should not output any CSS.

```scss
@import "config/config.typography"; // Typography variables: type scale, type faces, line heights
@import "config/config.colors"; // Project color palette defined as a Sass map
@import "config/config.spacing"; // Global spacing units to avoid magic numbers in the code
@import "config/config.responsive"; // Global breakpoints and settings for responsive utility classes
@import "config/config.z-index"; // Z-index layers defined as a Sass map for better maintainability
@import "config/config.misc"; // Misc global variables (border radius, default transition, etc.)
```

### Tools

Tools are mixins and functions that need to be available globally because we'll be using them
repeatedly throughout our code. For example, it can be a clearfix mixin or a function to access 
colors from the global color palette map. Any mixin or function that does not need accessing 
globally belongs in the partial to which it relates. The Tools layer comes after the Config layer 
because a function or a mixin may require one of the global variables as a default parameter.

**Note:** Tools layer should not output any CSS.


```scss
@import "tools/tools.font-size"; // Function to access global $type-scale map.
@import "tools/tools.font-face"; // Implement cross-browser @font-face with the greatest of ease.
@import "tools/tools.color"; // Function to access global $color-palette map.
@import "tools/tools.z-index"; // Function to access global $z-layers map.
@import "tools/tools.clearfix"; // Mixin for micro clearfix.
@import "tools/tools.hide"; // Mixin for accessibly hiding elements.
@import "tools/tools.mq"; // Sass MQ: Mixin for writing media queries.
```

### Generic

The Generic layer is the first one that actually produces any CSS. It houses
high-level, far reaching styles. It contains things like Normalize.css, box-sizing 
rules, CSS reset, etc. We can also include bare HTML element styles in here to define how 
unclassed HTML elements would look like (e.g., `<h1>`, `<a>`, `<ul>`, `<table>`, and so on).
However it is strongly suggested to style these elements as part of our UI components 
using classes (more on that below).

```scss
@import "generic/generic.box-sizing"; // Better default `box-sizing`.
@import "generic/generic.reset"; // Combination of reset and normalize.
@import "generic/generic.page"; // Page level setup.
@import "generic/generic.font-face"; // @font-face declarations.
```

### Objects

Objects are abstracted, design-free, reusable code patterns. They provide
structure for our templates and for our UI components. This can be, for example, 
a grid system, media object, ratio container. This is the first 
layer in which class-based selectors are used. This layer affects less of 
the DOM than the last layer and has a higher specificity because we bind on 
to classes.

**Note:** Object classes are prefixed with a `o-` to help us understand what
these classes do when we look at the markup (e.g. `<div class="o-media">`).

```scss
@import "objects/objects.container"; // Page constraint object.
@import "objects/objects.grid"; // Generic layout module. Used together with width utilities.
@import "objects/objects.media"; // Image- and text-like content side by side.
@import "objects/objects.flag"; // Table-layout-based advancement on the Media object.
@import "objects/objects.block"; // Image-on-top-of-text object.
@import "objects/objects.ratio"; // A wrapper for maintaining aspect ratio of content.
@import "objects/objects.table"; // Classes for manipulating `table`s.
@import "objects/objects.pack"; // Pack items into available horizontal space.
```

### Components

This is where the majority of the work happens after initial project set-up.
We still use **only** classes here, so the specificity doesn't increase. 
However Components layer is more explicit than Objects layer because we're styling 
recognisable pieces of UI (e.g. `.c-modal {}`). 

Component classes can be combined with Object classes in the markup 
to take advantage of already available abstractions in the Objects layer. 
For example, `<div class="o-media c-comment">` means we're extending media
object and creating its specific implementation &mdash; a comment block.

**Note:** Component classes are prefixed with a `c-` to help us understand
what these classes do when we look at the markup (e.g., `<a class="c-btn">`).

```scss
@import "components/components.icon"; // SVG icon styling
@import ...
...
```

### Utilities

This is where we put our utility classes (e.g. `.u-hide {}` or `.u-clearfix {}`).
Utilities layer beats all other layers, and has the power to override anything that has gone
before it. It contains very explicit selectors (e.g. `.u-h1 {font-size: 48px}`). 

With utilities we abstract and bind to classes commonly used styling properties 
(e.g., margins, paddings, font-sizes, widths). Utilities define these properties once in 
our stylesheet based on global variables in Config layer. This lets us apply these properties 
in markup without having to write any additional CSS code.

Our UI components become really flexible and reusable because we can easily modify every instance
using utility classes. This includes modifications necessary to adapt to different screen
sizes. Take a look at the example below. 

```html
<div class="o-grid">

  <!-- This grid item occupies 50% of space. It becomes 33% on desktop -->
  <div class="o-grid__item  u-1/2  u-1/3@desktop"> 
  
    <!-- This instance of the component has small bottom margin. It becomes large on desktop -->
    <div class="c-component  u-margin-bottom-s  u-margin-bottom-l@desktop">
    
      <!-- This heading is 24px (.u-h3). On desktop it becomes 36px (.u-h2) -->
      <h1 class="c-component__heading  u-h3  u-h2@desktop">Heading</h1>
     
    </div> 
    
  </div>
  
</div> 
```

This resembles functional CSS approach promoted by frameworks like 
[Tachyons](https://github.com/tachyons-css/tachyons/) or [Basscss](https://github.com/basscss/basscss),
although we are not compiling our UI components solely from utility classes. We utilize them to handle exceptions for often overridden properties.

**Note:** Utility classes are prefixed with a `u-` to help us understand
what these classes do when we look at the markup (e.g. `<div class="u-clearfix">`)

```scss
@import "utilities/utilities.clearfix"; // Bind the clearfix mixin onto a utility class.
@import "utilities/utilities.widths"; // Width helper classes.
@import "utilities/utilities.font-sizes"; // Reassigning text sizes to helper classes.
@import "utilities/utilities.spacing"; // Spacing classes to nudge bits of the DOM around.
@import "utilities/utilities.hide"; // Helper classes to hide content.
@import "utilities/utilities.text-align"; // Helper classes to align text horizontally.
@import "utilities/utilities.bfc"; // Utility classes for to manipulate block formatting context.
```

## Recommended reading

- [BEM 101](https://css-tricks.com/bem-101)
- [CSS Guidelines](http://cssguidelin.es)
- [Sass Guidelines](https://sass-guidelin.es)

## Frequently Asked Questions

**So why Goodrocket?** 
I thought this was a good metaphor for a modular CSS boilerplate with focus on maintainability and scalability. 
Because a good rocket will take you really far really fast. And it won't explode in the process. Also rockets are made of modules.

Hint: there is a sister project [Goodrocket Jekyll](https://github.com/ivanbabko/goodrocket-jekyll). 
It is a scaffolding for CMS and build system. Check it out.

**Why another boilerplate?**
I find it hard to use CSS frameworks or other people's boilerplates "as is". I have to understand fully what 
is under the hood to trust the system, and when I figure things out, I start disagreeing and customizing. 
So I figured I'll just create my own opinionated starting kit and find peace.
