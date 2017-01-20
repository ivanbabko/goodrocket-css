# Goodrocket CSS

This is a design free CSS starting kit for writing modular, maintainable, and scalable CSS. 
The aim is to provide a solid structure and sensible defaults to jump start the UI development
and then be able to maintain the project without going nuts.

It uses the best of Sass and avoids its dangerous features. So no things like `@extend` 
or deep nesting. It is based on BEM naming conventions and OOCSS approach. A few 
things were inspired by the ITCSS architecture and its real-life incarnation 
&mdash; Inuitcss framework, which I extended and adapted to my development style.


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
@import "tools/tools.fs"; // Function to access global $type-scale map.
@import "tools/tools.c"; // Function to access global $color-palette map.
@import "tools/tools.z"; // Function to access global $z-layers map.
@import "tools/tools.clearfix"; // Mixin for micro clearfix.
@import "tools/tools.hide-visually"; // Mixin for accessibly hiding elements.
@import "tools/tools.mq"; // Sass MQ: Mixin for writing media queries.
```

### Generic

The Generic layer is the first one that actually produces any CSS. It houses
high-level, far reaching styles. It contains things like Normalize.css, box-sizing 
rules, CSS resets, etc. We can also include bare element styles in here to define how 
unclassed HTML elements would look like (e.g., `<h1>-<h6>`, `<a>`, `<ul>`, `<table>`, and so on).
I prefer not to do so because there is a high chance that we will be overwriting these
default element styles a lot. I suggest styling these elements as part of our UI
components using classes (more on that below).

```scss
@import "generic/generic.box-sizing"; // Better default `box-sizing`.
@import "generic/generic.reset"; // Combination of reset and normalize.
@import "generic/generic.page"; // Page level setup.
```

### Objects

Objects are abstracted, design-free, reusable code patterns. They provide
structure for our templates and for our UI components. This can be, for example, 
a grid system, media object, ratio container, i.e. things that we tend to 
repeat again and again in our code for different UI elements. This is the first 
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
Here we style recognisable pieces of UI (e.g. `.c-modal {}`). We still use 
only classes here, so the specificity doesn't increase. We shouldn't find any selectors 
with a lower specificity than a class in this layer. At the same time, we should
aim not to increase the specificity. Avoid unnecessary nesting, chaining, or 
qualifying the selectors. Keep it flat.

Component classes can be combined with Object classes in the markup 
to take advantage of already available abstractions in the Objects layer. 
For example, `<div class="o-media c-comment">` means we're extending media
object and creating its specific implementation &mdash; a comment block.

**Note:** Component classes are prefixed with a `c-` to help us understand
what these classes do when we look at the markup (e.g., `<a class="c-btn">`).

```scss
@import "components/components.icon"; // SVG icon styling
@import "components/components.button"; // Buttons!
...
```

### Utilities

This is where we put our utility classes (e.g. `.u-hide {}` or `.u-margin-bottom-xl {}`).

Utilities play important role in Goodrocket CSS. With utilities we abstract and bind 
to classes commonly used styling properties. These are spacing properties (margin and padding), 
typography properties (font-sizes, line-heights), widths, and so on. Utilities define these 
properties once in our stylesheet based on global variables in Config layer. This lets us 
apply these properties in markup without having to write any additional CSS code.

Our UI components become really flexible and reusable because we can easily modify every instance
using utility classes. This includes modifications necessary to adapt to different screen
sizes. Take a look at the example below. 

```html
<div class="o-grid">

  <!-- This grid item occupies 50% of space. It becomes 33% on desktop -->
  <div class="o-grid__item  u-1/2  u-1/3@desktop"> 
  
    <!-- This component has small bottom margin. It becomes large on desktop -->
    <div class="c-component  u-margin-bottom-s  u-margin-bottom-l@desktop">
    
      <!-- This heading is 24px (.u-h3). On desktop it becomes 36px (.u-h2) -->
      <h1 class="c-component__heading  u-h3  u-h2@desktop">Heading</h1>
     
    </div> 
    
  </div>
  
</div> 
```

This resembles functional CSS approach promoted by frameworks like 
[Tachyons](https://github.com/tachyons-css/tachyons/) or [Basscss](https://github.com/basscss/basscss),
although we are not compiling our UI components solely from utility classes. 
We utilize them only as an abstraction for commonly used and often overridden properties.

Utilities layer goes last in our source order, because it is to beat all other layers. We can 
optionally choose to generate utility classes with `!important` to increase specificity
in case our build system doesn't output CSS in specific order.

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
