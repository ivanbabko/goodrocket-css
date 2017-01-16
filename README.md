# Goodrocket CSS

This is a design free CSS starting kit for writing modular, maintainable, and scalable CSS. 
The aim is to provide a solid structure and sensible defaults to jump start the UI development
and then be able to maintain the project without losing our minds.

It uses the best of Sass and avoids its dangerous features. So no things like `@extend` 
or deep nesting. It is based on BEM naming conventions and OOCSS approach. Quite a few 
things were inspired by the ITCSS architecture and its real-life incarnation 
&mdash; Inuitcss framework, which I extended and adapted to my development style.


## Structure

### Config

Here we have global variables that we'll be using throughout our code. Think typography settings, 
color palette, spacing units, breakpoints, z-index layers, and so on. Component-specific 
variables such as `$carousel-width` are defined in component partials. This way we keep this layer 
slim, and can easily find most variables alongside the code that uses them.

**Note:** Config layer should not output any CSS.


### Tools

Tools are mixins and functions that need to be available globally because we'll be using them
repeatedly throughout our code. For example, it can be a clearfix mixin or a function to access 
colors from the global color palette map. Any mixin or function that does not need accessing 
globally belongs in the partial to which it relates. The Tools layer comes after the Config layer 
because a function or a mixin may require one of the global variables as a default parameter.

**Note:** Tools layer should not output any CSS.


### Generic

The Generic layer is the first one that actually produces any CSS. It houses
very high-level, far reaching styles so it affects a lot of the DOM.
It contains things like Normalize.css, box-sizing rules, CSS resets, etc.

We can also include bare element styles in here to define how unclassed HTML elements
would look like (e.g., `<h1>-<h6>`, `<a>`, `<ul>`, `<table>`, and so on).
I prefer not to do so because there is a high chance that these default element 
styles will be overwritten most of the time.


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


### Components

This is where the majority of the work happens after initial project set-up.
Here we style recognisable pieces of UI (e.g. `.c-modal {}`). We still use 
only classes here, so the specificity doesn't increase. We shouldn't find any selectors 
with a lower specificity than a class in this layer. At the same time, we should
aim not to increase the specificity. Avoid unnecessary nesting, chaining, or 
qualifying the selectors. 

**Note:** Component classes are prefixed with a `c-` to help us understand
what these classes do when we look at the markup (e.g., `<a class="c-btn">`).
Component classes can be combined with Object classes in the markup 
to take advantage of already available abstractions in the Objects layer. 
For example, `<div class="o-media c-comment">` means we're extending media
object and creating its specific implementation &mdash; a comment block.


### Utilities

This is where we put our utility classes (e.g. `.u-hidden {}` or `.u-clearfix {}`). 
This layer comes last, so it beats all other layers. Utilities have the power to 
override anything in our markup. We can optionally choose to generate utility classes
with `!important` to make them really heavy-weight.

In the initial stages of development or in prototyping, we can speed things up
by relying on utility classes (together with object classes) to build pages. This 
resembles functional CSS approach promoted by frameworks like 
[Tachyons](https://github.com/tachyons-css/tachyons/) or [Basscss](https://github.com/basscss/basscss).

**Note:** Utility classes are prefixed with a `u-` to help us understand
what these classes do when we look at the markup (e.g. `<div class="u-clearfix">`)


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
