# Goodrocket CSS

This is a design free CSS starting kit for writing modular, scalable, and maintainable CSS. 
The aim is to provide a solid structure and sensible defaults to jump start the 
development. 

It uses the best of Sass and avoids its dangerous features. So no things like `@extend` 
or deep nesting. It is based on BEM naming conventions and OOCSS approach.  
Quite a few things were inspired by the ITCSS architecture and its real-life incarnation 
&mdash; Inuitcss framework, which I extended and adapted to my development style.


## Structure

### Config

Here we have global variables that we'll be using throughout our code. 
Think typography settings, color palette, spacing units, breakpoints, z-index layers, and so on. 
Note that component-specific variables such as `$carousel-width` are defined in component 
partials. This way we keep this layer slim, and can easily find most variables alongside 
the code that uses them.

**Note:** Config layer should not output any CSS.


### Tools

Tools are mixins and functions that need to be available globally.
For example, it can be a clearfix mixin or a function to access 
colors from the global color palette map. Any mixin or function that 
does not need accessing globally belongs in the partial to which it relates. 
The Tools layer comes after the Config layer because a function or a mixin may 
require one of the global variables as a default parameter.

**Note:** Tools layer should not output any CSS.


### Generic

The Generic layer is the first one that actually produces any CSS. It houses
very high-level, far reaching styles so it affects a lot of the DOM. This
layer is seldom modified, and is usually the same across any projects you work on.
It contains things like Normalize.css, box-sizing rules, CSS resets, etc.
We can also include bare element styles in here to define how unclassed HTML elements
would look like (e.g., `<h1>-<h6>`, `<a>`, `<ul>`, `<table>`, and so on).
I prefer not to do so and style them as part of components.


### Objects

Objects are abstracted, design-free, reusable code patterns. They provide
structure for our templates and for our UI components. This can be, for example, 
a grid system, media object, ratio container, i.e. things that we tend to 
repeat again and again in different places. This is the first 
layer in which class-based selectors are used. This layer affects less of 
the DOM than the last layer and has a higher specificity because we bind on 
to classes.

**Note:** Object classes are prefixed with a `o-` to help us understand what
these classes do when we look at the markup (e.g. `.o-media {}`).


### Components

This is where the majority of the work happens after initial project set-up.
We still use only classes here, so the specificity doesn't increase.
However, this layer is more explicit than the Objects layer because we style 
recognisable pieces of UI here (e.g. `.c-modal {}`). We shouldn't
find any selectors with a lower specificity than a class in this layer.
At the same time, we should avoid increasing specificity by nesting, chaining, 
or qualifying the selectors. Aim to keep the specificity graph flat.

**Note:** Component classes are prefixed with a `c-` to help us understand
what these classes do when we look at the markup (e.g., `.c-btn {}`).


### Utilities

This is where we put our utility classes (e.g. `.u-hidden {}` or `.u-clearfix {}`). 
This layer comes last, so it beats all other layers. Utilities have the power to 
override anything in our markup. We can optionally choose to generate utility classes
with `!important` to make them really heavy-weight.

Also in the initial stages of development or in prototyping, we can speed things up
by relying on utility classes to build pages. This resembles functional CSS approach
promoted by frameworks like [Tachyons](https://github.com/tachyons-css/tachyons/) or 
[Basscss](https://github.com/basscss/basscss).

**Note:** Utility classes are prefixed with a `u-` to help us understand
what these classes do when we look at the markup (e.g. `.u-clearfix {}`)


## Recommended reading

- [CSS Guidelines](http://cssguidelin.es)
- [Sass Guidelines](https://sass-guidelin.es)

## Frequently Asked Questions

**So why Goodrocket?** 
Because a good rocket will take you really far really fast. And it won't explode in the process. 
I thought this was a good metaphor for a CSS starter kit. Hint: there is a sister project
[Goodrocket Jekyll](https://github.com/ivanbabko/goodrocket-jekyll). It is a scaffolding for CMS and build system. Check it out.

**Why another boilerplate?**
I find it hard to use CSS frameworks or other people's boilerplates "as is". I have to understand fully what 
is under the hood to trust the system, and when I figure things out, I start disagreeing and customizing. 
So I figured I'll just create my own opinionated starting kit and find peace.
