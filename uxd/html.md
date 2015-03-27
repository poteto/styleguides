# HTML

Read and follow the
[HTML section of Mark Otto’s (@mdo) code guide]
(http://codeguide.co/#html).

In addition to @mdo’s Code Guide:

## Indentation and Line Breaks

Break to a new line if the tag contains another element.

Bad:

    <p>This is a <a href="#">link</a>.</p>

Good:

    <p>
      This is a
      <a href="#">link</a>.
    </p>

The period has to be right after the anchor tag in this case because we don’t want an
[extra space]
(http://stackoverflow.com/questions/588356/why-does-the-browser-renders-a-newline-as-space)
when the browser renders it.

This also makes sense:

    <h2>June 16<sup>th</sup></h2>

because there shouldn’t be a space or possibility of line break
between the date and its
[ordinal indicator]
(http://en.wikipedia.org/wiki/Ordinal_indicator).

## Comments

* Pseudo code loops and conditionals to make it easier for
  engineers.

* Add TODO’s for placeholder links, images and copy that needs to be
  updated.

      {{! TODO update link}}
      {{link-to "Let’s talk" "" class="cta l-testimonial__cta"}}

* Explain anything that may be confusing.

## Escaping characters

* [Use escapes for `&lt;`, `&gt;`, `&amp;`]
(http://www.w3.org/International/questions/qa-escapes#use).

* No need to escape for
[smartquotes]
(http://smartquotesforsmartpeople.com/).

## QA

1. Check html in
   [WAVE’s outliner plugin]
   (https://chrome.google.com/webstore/detail/wave-evaluation-tool/jbbplnpkjmmeebjpijfedlgcdilocofh?hl=en-US)
   or your favorite outliner. Does it makes sense?
1. Test with screenreader (VoiceOver and
   [NVDA]
   (http://www.nvaccess.org/)).
1. Make sure
   [forms are semantic and accessible]
   (http://slides.com/amandacheung/styling-forms-semantically-and-accessibly).

## Common Patterns

1. [Cites and blockquotes]
   (http://html5doctor.com/cite-and-blockquote-reloaded/)
1. [Figures and figcpations]
   (http://html5doctor.com/the-figure-figcaption-elements/)
1. Navigation
   No need to do `<nav>` with `<ul>` with `<li>` and then the `<a>`’s.

        <nav class="nav">
          <a class="nav__link" href="/about">About</a>
          <a class="nav__link" href="/team">Team</a>
          <a class="nav__link" href="/contact">Contact</a>
        </nav>
