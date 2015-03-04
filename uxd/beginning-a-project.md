# Beginning a Project

## Questions to Ask Client

* Is this project going to be responsive? If yes, what are the min and
  max px we should test for?
* What browsers should we support?
* Are there specific devices we should support?
* Do we have to prepare for translation to other languages?
* How do you want us to prepare the deliverables? (Zip file, GitHub, etc.)
* If the project has already begun, can we use
  [BEM]
  (https://github.com/dockyard/styleguides/blob/master/uxd/class-naming-conventions.md#bem-naming-conventions)
  naming conventions?

## Questions to Ask Designer

* Do we have rights to all the images being used in the mockup?
* Does the client have licenses to all the fonts being used? 

## Example File Structure

    stylesheets
      modules/
        buttons.scss
        footer.scss
        header.scss
      app.scss
      base.scss
      layout.scss
      load.scss
      type.scss

## APP.SCSS

    //= require load
    //= require base
    //= require_tree

If we need to import the stylesheets, order should be alphabetical
unless the stylesheet must be imported in a particular order.

    @import "load";
    @import "base";
    @import "layout";
    @import "type";

    @import "modules/buttons";
    @import "modules/header";
    @import "modules/footer";

## BASE.SCSS

Follows
[SMACSS' Base Rules]
(https://smacss.com/book/type-base). Defines the default styling
in all occurrences. Does not include any class or ID selectors.

We use modified versions of the
[Meyer reset]
(http://meyerweb.com/eric/tools/css/reset/).
If you are sure that certain elements will not be used in the project,
they may be deleted. 

    /* http://meyerweb.com/eric/tools/css/reset/ 
       v2.0 | 20110126
       License: none (public domain)
    */

    html, body, div, span, applet, object, iframe,
    h1, h2, h3, h4, h5, h6, p, blockquote, pre,
    a, abbr, acronym, address, big, cite, code,
    del, dfn, em, img, ins, kbd, q, s, samp,
    small, strike, strong, sub, sup, tt, var,
    b, u, i, center,
    dl, dt, dd, ol, ul, li,
    fieldset, form, label, legend,
    table, caption, tbody, tfoot, thead, tr, th, td,
    article, aside, canvas, details, embed, 
    figure, figcaption, footer, header, hgroup, 
    menu, nav, output, ruby, section, summary,
    time, mark, audio, video {
      margin: 0;
      padding: 0;
      border: 0;
      font-size: 100%;
      font: inherit;
      vertical-align: baseline;
    }
    /* HTML5 display-role reset for older browsers */
    article, aside, details, figcaption, figure, 
    footer, header, hgroup, menu, nav, section {
      display: block;
    }
    body {
      line-height: 1;
    }
    ol, ul {
      list-style: none;
    }
    blockquote, q {
      quotes: none;
    }
    blockquote:before, blockquote:after,
    q:before, q:after {
      content: '';
      content: none;
    }
    table {
      border-collapse: collapse;
      border-spacing: 0;
    }

## LAYOUT.SCSS 

This does not closely follow
[SMACSS' Layout Rules]
(https://smacss.com/book/type-layout).
We use `layout.scss` for wraps, grids and columns. Here is a
[JS Bin]
(http://jsbin.com/tiyome/3/edit?html,css,output) that illustrates some
common patterns. 

`.l-relative` is used for wrapping `div`&rsquo;s that do not need any
styling other than `position: relative`.

    .l-relative {
      position: relative;
    }

`.l-overflow` is used for wrapping `div`&rsquo;s that do not need any
styling other than `overflow: auto`.

    .l-overflow {
      overflow: auto;
    }

If there are recurring shared wrap sizes, something like `.l-wrap--s`
and `.l-wrap--b` would make sense. If not recurring, `.l-header` would
be better and would not belong in `layout.scss`, but in `header.scss`.

    .l-wrap--s,
    .l-wrap--b {
      margin-right: auto;
      margin-left: auto;
      width: 90%;
    }

    .l-wrap--s {
      max-width: 640px;
    }

    .l-wrap--b {
      max-width: 1020px;
    }

## LOAD.SCSS

Includes variables, placeholders (%extends), and mixins. Keep in mind:
[@extends can be dangerous]
(http://csswizardry.com/2014/11/when-to-use-extend-when-to-use-a-mixin/).
Here is a sample `load.scss`.

    // COLORS

    $white: #fff;
    $black: #000;



    // FONT FAMILIES

    $sentinel: 'Sentinel SSm A', 'Sentinel SSm B', serif;
    $whitney: 'Whitney SSm A', 'Whitney SSm B', sans-serif;



    // FONT WEIGHTS

    $light: 300;
    $book: 400;
    $medium: 500;
    $bold: 700;



    // PLACEHOLDERS

    %border-box {
      -moz-box-sizing: border-box;
      box-sizing: border-box;
    }

    %font-smoothing {
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
    }

    %appearance-none {
      -webkit-appearance: none;
      -moz-appearance: none;
      appearance: none;
    }



    // MIXINS

    @mixin transition-duration($transition-duration) {
      -webkit-transition-duration: $transition-duration;
      transition-duration: $transition-duration;
    }

    @mixin transition-delay($transition-delay) {
      -webkit-transition-delay: $transition-delay;
      transition-delay: $transition-delay;
    }

    @mixin transition-property($transition-property...) {
      -webkit-transition-property: $transition-property;
      transition-property: $transition-property;
    }

    @mixin blur($blur) {
      -webkit-filter: blur($blur);
      -moz-filter: blur($blur);
      -ms-filter: blur($blur);
      filter: blur($blur);
    }

    @mixin columns($columns, $column-gap) {
      -webkit-columns: $columns;
      -moz-columns: $columns;
      columns: $columns;
      -webkit-column-gap: $column-gap;
      -moz-column-gap: $column-gap;
      column-gap: $column-gap;
    }

The transform mixin required parentheses to be included for `$expression`.
Example: `@include transform(rotate, "(45deg)");`.

    @mixin transform($function, $expression) {
      -webkit-transform: #{$function}#{$expression};
      -ms-transform: #{$function}#{$expression};
      transform: #{$function}#{$expression};
    }

The calc mixin requires the `$expression` to be formatted like a string.
Example: `@include calc(width, "100% - 200px", 75%);`.

    @mixin calc($property, $expression, $fallback) {
      #{$property}: $fallback;
      #{$property}: -webkit-calc(#{$expression});
      #{$property}: -moz-calc(#{$expression});
      #{$property}: calc(#{$expression});
    }

Make sure to double check the necessary vendor prefixes with
[caniuse.com]
(http://caniuse.com/).

## TYPE.SCSS

    i {
      font-style: italic;
    }

    b {
      font-weight: $bold;
    }

`.t-display` is used for the shared display font style. Most likely a
page heading or section heading.

    .t-display {
      font-weight: $bold;
      @media (max-width: 1199px) {
        font-size: 24px;
      }
      @media (min-width: 1200px) {
        font-size: 36px;
      }
    }

`.t-text` is used for the shared text font style. Most likely the styles
used for blocks of text on a blog or static pages.

    .t-text {
      font-weight: $light;
      @media (max-width: 1199px) {
        font-size: 16px;
      }
      @media (min-width: 1200px) {
        font-size: 18px;
      }
    }

`.t-link` is used for shared text link styles. Most likely this is a
link that appears within `.t-text`.

    .t-link {
      border-bottom: 1px solid $white;
      text-decoration: none;
    }
